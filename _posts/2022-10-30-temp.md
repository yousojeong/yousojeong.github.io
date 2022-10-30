---
title : "임시 파일" 
# title : 포스트의 제목을 큰 따옴표로 적는다
# 타이틀을 적지 않을 시 마크다운파일 .md 파일명으로 타이틀이 업로드 된다

excerpt : "Temp"
# 글 목록에서 보이는 글 소개 내용

categories :
    - temp
    # 카테고리

tags : 
    - [CV]
    # 태그 : 카테고리 보다 세부적, [] 대괄호 안에서 , 콤마로 구분하여 작성
    # 카테고리는 sub url이 붙는 페이지가 있지만, 태그는 없음

post-order : 1 
    # 같은 카테고리 내 정렬 순서

toc : true
    # Table Of Contents : 포스트의 헤더만 보여주는 목차를 사용할지 여부

toc_sticky : true # toc 고정유무, 고정 시 우측 상단에 고정

toc_icon : true # toc 아이콘

toc_label : "제목" # toc 제목

date : 2022-10-30
    # 최초 작성일자
    # 별도 정렬 순서가 없으면 이 값으로 정렬됨, 파일명에 기록되어있다면 생략 가능

last_modified_at : 2022-10-30
    # 마지막 수정일자

sidebar:
  nav: "docs"
---

<!-- 
front-matter 머릿말
머릿말이 있어야함 Jekyll 은 해당 문서를 블로그의 페이지로 인식하며, 없을 시 문서는 무시된다
포스트의 화면을 일관되게 구성하며, 글로벌 변수 또는 사용자 정의 변수를 선언해서 사용할 수도 있다

_config.yml 파일에 default 로 지정된 변수와 전역변수

defaults:  # 페이지의 머리말에 선언하지 않아도 아래 내용들은 기본값으로 적용됨.
  - scope:
      path: ""          # 모든 경로의 페이지에 적용
      type: posts       # 포스트 타입의 페이지에 적용
    values:
      layout: single    # single 레이아웃을 적용
      author_profile: true  # 내 프로필을 사이드바에 표시함
      read_time: false      # '한 포스트를 다 읽는데 n분 걸림' 이라는 문구를 붙여주는 완독 추측 시간
      comments: true        # 댓글 기능 활성화
      share: true       # 소셜 공유 기능 활성화
      related: true     # 관련 포스트 추천 활성화
      toc: true         # 현재 페이지의 목차 보기 활성화
      sidebar:          # (내 커스텀 변수) 블로그 목차 보기
        nav: main-sidebar   # (내 커스텀 변수) /_data/navigation.yml에 main-sidebar의 내용을 정의
# (내 커스텀 기능) 구글 드라이브에서 이미지 가져오기 url 접두어
gdrive_url_prefix: "https://drive.google.com/uc?export=view&id="

명시적으로 표현, 페이지 별로 다르게 적용하는 것은 페이지 자체 머릿말에 작성
---
layout: single                                 # 페이지에 single 레이아웃을 적용
title: "[Git Page Jekyll Blog] - [8] 첫번째 포스트 게시하기"  # 페이지 타이틀
post-order: 8                                  # (내 커스텀 변수) 같은 카테고리 내 정렬 순서
date: "2020-11-17 21:19:00 +0900"              # 최초 포스팅 날짜. 별도 정렬 순서가 없으면 이 값으로 정렬됨. 파일명에 기록되어있다면 생략 가능.
last_modified_at: "2020-11-17 21:19:00 +0900"  # 마지막 수정 날짜.
---
 -->
<!-- 머릿말 영역 끝 -->

<!-- 
서버 가동

github.io 폴더 
bundle exec jekyll serve
localhost:4000
 -->

<!-- 포스트 영역 시작 -->

# MMaction2
- 설치 완료, 도커 사용, 데모영상 돌아가는 것 확인 이후
- Dataset : kinetics-400 , Model : TSN 
- falldown 학습 도전
- falldown dataset 준비 : 유튜브에서 영상 추출, 넘어지는 장면만 편집 1~3초 정도 분량

## falldown 학습 과정
1. falldown 단독학습
2. kinetics-400에 falldown 추가 학습
---
### falldown 단독 학습
- 학습, 테스트 데모 돌렸을 때 결과가 음수로 나옴
- 라벨, csv, 영상 등등 다 falldown만 단독으로 넣은 상태
- 그럼 키네틱400에 falldown 하나를 추가해서 401개를 돌리면 어떻게 될지 시도

---

### kinetics-400 에 falldown 추가 학습
1. falldown 관련 영상 및 csv 파일 추가
2. DOC
3. "파일리스트 만들기" 1차 : 실패
4. "파일리스트 만들기" 2차 : 그럭저럭 성공
5. Train
6. tsn_r50_video_1x1x8_100e_kinetics400_rgb_data2 : RuntimeError: CUDA out of memory
7. tsn_r50_video_320p_1x1x3_100e_kinetics400_rgb_data2 :FileNotFoundError: VideoDataset 
8. falldown 파일에 load 시켜서 사용하는 방식
9. 결과 & 다음 단계
10. data2파일에 로드시키기


---

#### 1. falldown 관련 영상 및 csv 파일 추가
- 동영상 추가
  - `/data/videos_train 에 dataset-jtod/videos_train/falldown` 추가
  - `/data/videos_val 에 dataset-jotd/videos_val/falldown` 추가

- annotations 수정
  - `/data/annotations 아래의 kinetics_test.csv, kinetics_train.csv, kinetics_val.csv` 에 
    ```
    <kinetics_test.csv 맨 아래에 추가>
    youtube_id,time_start,time_end,split
    falldown001,1,3,test
    falldown002,1,2,test
    falldown003,1,3,test
    falldown004,1,2,test
    falldown005,1,2,test
    falldown006,1,2,test
    falldown007,1,2,test
    falldown008,1,2,test
    falldown009,1,2,test
    falldown010,1,3,test
    ```
    ```
    <kinetics_train.csv 맨 아래에 추가>
    label,youtube_id,time_start,time_end,split,is_cc
    falldown,falldown001,1,3,train,0
    falldown,falldown002,1,2,train,0
    falldown,falldown003,1,3,train,0
    falldown,falldown004,1,2,train,0
    falldown,falldown005,1,2,train,0
    falldown,falldown006,1,2,train,0
    falldown,falldown007,1,2,train,0
    falldown,falldown008,1,2,train,0
    falldown,falldown009,1,2,train,0
    falldown,falldown010,1,3,train,0
    ```
    ```
    <kinetics_val.csv 맨 아래에 추가>
    label,youtube_id,time_start,time_end,split,is_cc
    falldown,falldown001,1,3,val,0
    falldown,falldown002,1,2,val,0
    falldown,falldown003,1,3,val,0
    falldown,falldown004,1,2,val,0
    falldown,falldown005,1,2,val,0
    falldown,falldown006,1,2,val,0
    falldown,falldown007,1,2,val,0
    falldown,falldown008,1,2,val,0
    falldown,falldown009,1,2,val,0
    falldown,falldown010,1,3,val,0
    ```
    ![image](https://user-images.githubusercontent.com/64599187/197743507-f3f52941-8085-47d1-8d66-7b6a1dea12c5.png)
    ![image](https://user-images.githubusercontent.com/64599187/197743828-a7bd12f1-5d18-4ebc-9db6-f5d4b6d7a53b.png)
    ![image](https://user-images.githubusercontent.com/64599187/197744063-1932de42-5df7-41e3-927f-0fdcae11cef5.png)

---

#### 2. DOC 등 준비
- DOC
  - [generate-file-list](https://mmaction2.readthedocs.io/en/latest/data_preparation.html#generate-file-list)
  - [kinetics](https://github.com/open-mmlab/mmaction2/blob/master/tools/data/kinetics/README.md)
  ![image](https://user-images.githubusercontent.com/64599187/197742372-3e48f433-e251-47fa-8418-b013548715a5.png)

- data : mmdatction2 여기에 키네틱은 심볼릭이라 도커에서 사용 불가
- data2 : hdd-mount 여기에 있는 키네틱 폴더 사용예정

---

#### 3. "파일리스트 만들기" 1차시도 == 실패
- 원인 예상 : -- bash 파일 만들고 했어야 -- 2차시도로 넘어감
- 1차시도 사용 코드 
    ```
    # parse_file_list.py 에서 data 경로를 data2로 바꾸기 위해 복사본 생성
    root@6f488173946c:/mmaction2/tools/data# cp parse_file_list.py parse_file_list_data2.py


    # <parse_file_list_data2.py>
    # 경로 수정 data 에서 data2로 변경
    train_file = f'data2/{dataset}/annotations/kinetics_train.csv'
    val_file = f'data2/{dataset}/annotations/kinetics_val.csv'
    test_file = f'data2/{dataset}/annotations/kinetics_test.csv'


    # 주석 변경  ValueError: could not convert string to float: 'test' 에러 발생
    video = f'{x[0]}_{int(x[1]):06d}_{int(x[2]):06d}' # 원래 주석 상태
    # video = f'{x[1]}_{int(float(x[2])):06d}_{int(float(x[3])):06d}' # 주석 처리함


    # build_file_list_data2.py 생성
    root@6f488173946c:/mmaction2/tools/data# cp build_file_list.py build_file_list_data2.py
    # 경로 변경 from tools.data.parse_file_list_data2 import ...


    # 실행 및 에러
    python tools/data/build_file_list_data2.py kinetics400 /data2/kinetics400 --format videos --out-root-path /data2/kinetics400

    Traceback (most recent call last):
    File "tools/data/build_file_list_data2.py", line 269, in <module>
    main()
    File "tools/data/build_file_list_data2.py", line 220, in main
    assert len(splits) == args.num_split
    AssertionError
    ```

---

#### 4. "파일리스트 만들기" 2차시도
- 1차시도 때 `generate_videos..` 파일을 만들어야 할 것으로 예상함
- `generate_videos_filelist_data2` 생성
    ```
    root@6f488173946c:/mmaction2/tools/data/kinetics# cp generate_videos_filelist.sh generate_videos_filelist_data2.sh
    root@6f488173946c:/mmaction2/tools/data/kinetics# ls
    README.md                       download_videos.sh            generate_rawframes_filelist.sh        label_map_k600.txt
    README_zh-CN.md                 environment.yml               generate_videos_filelist.sh           label_map_k700.txt
    download.py                     extract_frames.sh             generate_videos_filelist_data2.sh     rename_classnames.sh
    download_annotations.sh         extract_rgb_frames.sh         generate_videos_filelist_falldown.sh
    download_backup_annotations.sh  extract_rgb_frames_opencv.sh  label_map_k400.txt
    ```
- 비디오 넣고
- csv 변경
- bash generate_videos_filelist_data2.sh kinetics400 실행
    ```
    root@6f488173946c:/mmaction2/tools/data/kinetics# bash generate_videos_filelist_data2.sh kinetics400
    We are processing kinetics400
    1 1
    args :  Namespace(dataset='kinetics400', flow_x_prefix='flow_x_', flow_y_prefix='flow_y_', format='videos', level=2, num_split=1, out_root_path='data2/', output_format='txt', rgb_prefix='img_', seed=None, shuffle=True, src_folder='data2/kinetics400/videos_train/', subset='train')
    data2/kinetics400
    Train filelist for video generated.
    1 1
    args :  Namespace(dataset='kinetics400', flow_x_prefix='flow_x_', flow_y_prefix='flow_y_', format='videos', level=2, num_split=1, out_root_path='data2/', output_format='txt', rgb_prefix='img_', seed=None, shuffle=True, src_folder='data2/kinetics400/videos_val/', subset='val')
    data2/kinetics400
    Val filelist for video generated.
    ```
- 결과
    - kinetics400_train_list_videos.txt : 목록 나옴 falldown 검색 하면 나옴
    ![image](https://user-images.githubusercontent.com/64599187/198351355-6ed81685-e9ad-4b52-858a-83c502cb99a0.png)
    - kinetics400_val_list_videos.txt : falldown 관련만 나오는 상태
    ![image](https://user-images.githubusercontent.com/64599187/198351411-3d445c72-1764-4e9e-af32-bda077a4f45d.png)

    - val_list 가 왜 비어서 나오는지 고민
        - 이전에 내가 아닌 교수님이 혼자서 돌리셨던 hdd-mount 내의 kinetics400 폴더에 보면 그건 rawframe 으로 돌린거긴 하지만 여기도 kinetics400_train_list_rawframes.txt 는 목록이 나왔지만

        ![image](https://user-images.githubusercontent.com/64599187/198354469-168d62b0-1557-44c1-9dbe-ab3847b531da.png)

        - kinetics400_val_list_rawframes.txt 는 목록이 나오지 않는 것을 확인함

        ![image](https://user-images.githubusercontent.com/64599187/198354398-35a6f640-c72a-489c-8762-49e23987c5a0.png)

        - 기존의 다른 키네틱 csv 리스트와 내가 넣은 falldown csv 리스트와 다른 점은 나는 유튜브 아이디 값을 셋 다 동일하게 맞춰 썼지만 기존은 다 다르게 사용했다는 점
        - 에러가 나는 부분은
            - len(splits) 의 값은 1로 나오고 
    args.num_split 의 값이 3으로 나와 맞지 않아서 나오는 문제

            - 예상되는 의심스러운 부분도 있고, 기존의 리스트에도 값이 비어있다는 것을 고려했을 때 
            - 현재 상태로 train 시키는 것도 나쁘지 않다고 생각됨
            ```
            root@6f488173946c:/mmaction2# python tools/data/build_file_list_data2.py --format videos kinetics400 data2/kinetics400/videos_val
            1 3
            args :  Namespace(dataset='kinetics400', flow_x_prefix='flow_x_', flow_y_prefix='flow_y_', format='videos', level=2, num_split=3, out_root_path='data2/', output_format='txt', rgb_prefix='img_', seed=None, shuffle=False, src_folder='data2/kinetics400/videos_val', subset='train')
            Traceback (most recent call last):
                File "tools/data/build_file_list_data2.py", line 274, in <module>
                main()
                File "tools/data/build_file_list_data2.py", line 223, in main
                assert len(splits) == args.num_split
            AssertionError
            ```

  - 사용한 파일
    - build_file_list_data2.py
    - parse_file_list_data2.py
    - generate_videos_filelist_data2.sh 
    ```
    root@6f488173946c:/mmaction2/tools/data# ls    
    build_file_list_data2.py            
    parse_file_list_data2.py 

    root@6f488173946c:/mmaction2/tools/data/kinetics# ls
    generate_videos_filelist_data2.sh 
    label_map_k400.txt
    ```
  - 
    기존 키네틱에 falldown 추가 (label_map_k400.txt 에 falldown 추가)
    - 파일 리스트 준비는 됐고 돌릴 때 
    - 400개에서 401개로만 높여주면 되는 상태
    ![image](https://user-images.githubusercontent.com/64599187/198355877-82ad43b0-e5ea-4340-9801-0fc12bf67aad.png)

---

#### 5. TSN Train
- [DOC](https://mmaction2.readthedocs.io/en/latest/recognition_models.html#id90)

- tsn_r50_video_1x1x8_100e_kinetics400_rgb_data2.py 파일 생성
    ```
    root@6f488173946c:/mmaction2/configs/recognition/tsn# ls             tsn_r50_video_1x1x8_100e_kinetics400_rgb_data2.py
    ```
- falldown 추가해서 class 400에서 401로 변경 tsn_r50_data2.py 파일 생성
    ```
    root@6f488173946c:/mmaction2/configs/_base_/models# cp tsn_r50.py tsn_r50_data2.py
    root@6f488173946c:/mmaction2/configs/_base_/models# ls
    audioonly_r50.py  c3d_sports1m_pretrained.py  slowfast_r50.py  tpn_slowonly_r50.py  tsm_r50.py        tsn_r50_data2.py
    ```
- tsn_r50_data2.py 내용 수정
    ```
    _base_ = [
    '../../_base_/models/tsn_r50_data2.py', '../../_base_/schedules/sgd_100e.py',
    '../../_base_/default_runtime.py'
    ]
    ```
- tarin 코드 돌리기 
    - [train 옵션에 대한 부분](https://mmaction2.readthedocs.io/en/latest/getting_started.html#training-setting)
    - [tsn train 의 부분](https://mmaction2.readthedocs.io/en/latest/recognition_models.html#id90)

---

#### 6. tsn_r50_video_1x1x8_100e_kinetics400_rgb_data2
- 기존에 제공된 코드를 변경하여 사용
    ```
    python tools/train.py configs/recognition/tsn/tsn_r50_1x1x3_100e_kinetics400_rgb.py \
    --work-dir work_dirs/tsn_r50_1x1x3_100e_kinetics400_rgb \
    --validate --seed 0 --deterministic
    
    root@6f488173946c:/mmaction2# python tools/train.py configs/recognition/tsn/tsn_r50_video_1x1x8_100e_kinetics400_rgb_data2.py --work-dir /mmaction2/data2/kinetics400/train/ --validate --seed 0 --deterministic
    ```
- 에러 발생 
    ```
    Traceback (most recent call last):
    File "tools/train.py", line 222, in <module>
        main()
    File "tools/train.py", line 218, in main
        meta=meta)
    File "/mmaction2/mmaction/apis/train.py", line 92, in train_model
        num_gpus=len(cfg.gpu_ids),
    File "/opt/conda/lib/python3.7/site-packages/mmcv/utils/config.py", line 507, in __getattr__
        return getattr(self._cfg_dict, name)
    File "/opt/conda/lib/python3.7/site-packages/mmcv/utils/config.py", line 48, in __getattr__
        raise ex
    AttributeError: 'ConfigDict' object has no attribute 'gpu_ids'
    ```  
- --gpu-ids 옵션 넣어서 재시도
    ```
    root@6f488173946c:/mmaction2# root@6f488173946c:/mmaction2# python tools/train.py configs/recognition/tsn/tsn_r50_video_1x1x8_100e_kinetics400_rgb_data2.py --work-dir /mmaction2/data2/kinetics400/train/ --validate --gpu-ids 0  --seed 0 --deterministic  
    ```
- gpu 옵션 넣은 결과 에러
    ```
    root@6f488173946c:/mmaction2# python tools/train.py configs/recognition/tsn/tsn_r50_video_1x1x8_100e_kinetics400_rgb_data2.py --work-dir /mmaction2/data2/kinetics400/train/ --validate --gpu-ids 0  --seed 0 --deterministic            
    /mmaction2/mmaction/utils/setup_env.py:33: UserWarning: Setting OMP_NUM_THREADS environment variable for each process to be 1 in default, to avoid your system being overloaded, please further tune the variable for optimal performance in your application as needed.
    f'Setting OMP_NUM_THREADS environment variable for each process '
    /mmaction2/mmaction/utils/setup_env.py:43: UserWarning: Setting MKL_NUM_THREADS environment variable for each process to be 1 in default, to avoid your system being overloaded, please further tune the variable for optimal performance in your application as needed.
    f'Setting MKL_NUM_THREADS environment variable for each process '
    tools/train.py:113: UserWarning: The Args `gpu_ids` and `gpus` are only used in non-distributed mode and we highly encourage you to use distributed mode, i.e., launch training with dist_train.sh. The two args will be deperacted.
    'The Args `gpu_ids` and `gpus` are only used in non-distributed '
    tools/train.py:119: UserWarning: Non-distributed training can only use 1 gpu now. We will use the 1st one in gpu_ids. 
    'Non-distributed training can only use 1 gpu now. We will '
    2022-10-27 17:47:44,059 - mmaction - INFO - Environment info:
    ------------------------------------------------------------
    중간 생략              
    -------------------- 
    2022-10-27 17:47:48,212 - mmaction - INFO - workflow: [('train', 1)], max: 100 epochs
    2022-10-27 17:47:48,212 - mmaction - INFO - Checkpoints will be saved to /mmaction2/data2/kinetics400/train by HardDiskBackend.
    Traceback (most recent call last):
    File "tools/train.py", line 222, in <module>
        main()
    File "tools/train.py", line 218, in main
        meta=meta)
    File "/mmaction2/mmaction/apis/train.py", line 232, in train_model
        runner.run(data_loaders, cfg.workflow, cfg.total_epochs, **runner_kwargs)
    File "/opt/conda/lib/python3.7/site-packages/mmcv/runner/epoch_based_runner.py", line 127, in run
        epoch_runner(data_loaders[i], **kwargs)
    File "/opt/conda/lib/python3.7/site-packages/mmcv/runner/epoch_based_runner.py", line 50, in train
        self.run_iter(data_batch, train_mode=True, **kwargs)
    File "/opt/conda/lib/python3.7/site-packages/mmcv/runner/epoch_based_runner.py", line 30, in run_iter
        **kwargs)
    File "/opt/conda/lib/python3.7/site-packages/mmcv/parallel/data_parallel.py", line 67, in train_step
        return self.module.train_step(*inputs[0], **kwargs[0])
    File "/mmaction2/mmaction/models/recognizers/base.py", line 307, in train_step
        losses = self(imgs, label, return_loss=True, **aux_info)
    File "/opt/conda/lib/python3.7/site-packages/torch/nn/modules/module.py", line 722, in _call_impl
        result = self.forward(*input, **kwargs)
    File "/mmaction2/mmaction/models/recognizers/base.py", line 269, in forward
        return self.forward_train(imgs, label, **kwargs)
    File "/mmaction2/mmaction/models/recognizers/recognizer2d.py", line 23, in forward_train
        x = self.extract_feat(imgs)
    File "/opt/conda/lib/python3.7/site-packages/mmcv/runner/fp16_utils.py", line 98, in new_func
        return old_func(*args, **kwargs)
    File "/mmaction2/mmaction/models/recognizers/base.py", line 170, in extract_feat
        x = self.backbone(imgs)
    File "/opt/conda/lib/python3.7/site-packages/torch/nn/modules/module.py", line 722, in _call_impl
        result = self.forward(*input, **kwargs)
    File "/mmaction2/mmaction/models/backbones/resnet.py", line 546, in forward
        x = res_layer(x)
    File "/opt/conda/lib/python3.7/site-packages/torch/nn/modules/module.py", line 722, in _call_impl
        result = self.forward(*input, **kwargs)
    File "/opt/conda/lib/python3.7/site-packages/torch/nn/modules/container.py", line 117, in forward
        input = module(input)
    File "/opt/conda/lib/python3.7/site-packages/torch/nn/modules/module.py", line 722, in _call_impl
        result = self.forward(*input, **kwargs)
    File "/mmaction2/mmaction/models/backbones/resnet.py", line 215, in forward
        out = _inner_forward(x)
    File "/mmaction2/mmaction/models/backbones/resnet.py", line 208, in _inner_forward
        out = out + identity
    RuntimeError: CUDA out of memory. Tried to allocate 784.00 MiB (GPU 0; 7.93 GiB total capacity; 6.17 GiB already allocated; 653.25 MiB free; 6.20 GiB reserved in total by PyTorch)
    root@6f488173946c:/mmaction2# 
    ```

---

#### 7. tsn_r50_video_320p_1x1x3_100e_kinetics400_rgb_data2
- config 파일을 변경해보기로
- 위에서 시도한건 노란색 새로 만드는건 주황색
![image](https://user-images.githubusercontent.com/64599187/198379683-bcaee519-d020-4dea-b1ab-190f1509b08b.png)

- tsn_r50_video_320p_1x1x3_100e_kinetics400_rgb_data2.py 생성
    ```
    root@6f488173946c:/mmaction2/configs/recognition/tsn# cp tsn_r50_video_320p_1x1x3_100e_kinetics400_rgb.py tsn_r50_video_320p_1x1x3_100e_kinetics400_rgb_data2.py

    root@6f488173946c:/mmaction2/configs/recognition/tsn# ls
    tsn_r50_video_1x1x8_100e_kinetics400_rgb_data2.py        tsn_r50_video_320p_1x1x3_100e_kinetics400_rgb_data2.py
    ```
- 위와 똑같이 tsn_r50_data2.py 사용함
- 실행 tsn_r50_video_320p_1x1x3_100e_kinetics400_rgb_data2.py
    ```
    root@6f488173946c:/mmaction2# python tools/train.py configs/recognition/tsn/tsn_r50_video_320p_1x1x3_100e_kinetics400_rgb_data2.py --work-dir /mmaction2/data2/kinetics400/train/ --validate --gpu-ids 0 --seed 0 --deterministic  
    
    /mmaction2/mmaction/utils/setup_env.py:33: UserWarning: Setting OMP_NUM_THREADS environment variable for each process to be 1 in default, to avoid your system being overloaded, please further tune the variable for optimal performance in your application as needed.
        f'Setting OMP_NUM_THREADS environment variable for each process '
    /mmaction2/mmaction/utils/setup_env.py:43: UserWarning: Setting MKL_NUM_THREADS environment variable for each process to be 1 in default, to avoid your system being overloaded, please further tune the variable for optimal performance in your application as needed.
        f'Setting MKL_NUM_THREADS environment variable for each process '
    tools/train.py:113: UserWarning: The Args `gpu_ids` and `gpus` are only used in non-distributed mode and we highly encourage you to use distributed mode, i.e., launch training with dist_train.sh. The two args will be deperacted.
        'The Args `gpu_ids` and `gpus` are only used in non-distributed '
    tools/train.py:119: UserWarning: Non-distributed training can only use 1 gpu now. We will use the 1st one in gpu_ids. 
        'Non-distributed training can only use 1 gpu now. We will '
    2022-10-27 19:28:37,459 - mmaction - INFO - Environment info:
    ------------------------------------------------------------
    중간 생략
    ------------------------------------------------------------

    2022-10-27 19:28:37,459 - mmaction - INFO - Distributed training: False
    2022-10-27 19:28:37,730 - mmaction - INFO - Config: model = dict(
        type='Recognizer2D',
        backbone=dict(
            type='ResNet',
            pretrained='torchvision://resnet50',
            depth=50,
            norm_eval=False),
        cls_head=dict(
            type='TSNHead',
            num_classes=400,
            in_channels=2048,
            spatial_type='avg',
            consensus=dict(type='AvgConsensus', dim=1),
            dropout_ratio=0.4,
            init_std=0.01),
        train_cfg=None,
        test_cfg=dict(average_clips=None))
    optimizer = dict(type='SGD', lr=0.01, momentum=0.9, weight_decay=0.0001)
    optimizer_config = dict(grad_clip=dict(max_norm=40, norm_type=2))
    lr_config = dict(policy='step', step=[40, 80])
    total_epochs = 100
    checkpoint_config = dict(interval=1)
    log_config = dict(interval=20, hooks=[dict(type='TextLoggerHook')])
    dist_params = dict(backend='nccl')
    log_level = 'INFO'
    load_from = None
    resume_from = None
    workflow = [('train', 1)]
    opencv_num_threads = 0
    mp_start_method = 'fork'
    dataset_type = 'VideoDataset'
    data_root = 'data/kinetics400/videos_train'
    data_root_val = 'data/kinetics400/videos_val'
    ann_file_train = 'data/kinetics400/kinetics400_train_list_videos.txt'
    ann_file_val = 'data/kinetics400/kinetics400_val_list_videos.txt'
    ann_file_test = 'data/kinetics400/kinetics400_val_list_videos.txt'
    img_norm_cfg = dict(
        mean=[123.675, 116.28, 103.53], std=[58.395, 57.12, 57.375], to_bgr=False)
    train_pipeline = [
        dict(type='DecordInit'),
        dict(type='SampleFrames', clip_len=1, frame_interval=1, num_clips=3),
        dict(type='DecordDecode'),
        dict(type='RandomResizedCrop'),
        dict(type='Resize', scale=(224, 224), keep_ratio=False),
        dict(type='Flip', flip_ratio=0.5),
        dict(
            type='Normalize',
            mean=[123.675, 116.28, 103.53],
            std=[58.395, 57.12, 57.375],
            to_bgr=False),
        dict(type='FormatShape', input_format='NCHW'),
        dict(type='Collect', keys=['imgs', 'label'], meta_keys=[]),
        dict(type='ToTensor', keys=['imgs', 'label'])
    ]
    val_pipeline = [
        dict(type='DecordInit'),
        dict(
            type='SampleFrames',
            clip_len=1,
            frame_interval=1,
            num_clips=3,
            test_mode=True),
        dict(type='DecordDecode'),
        dict(type='Resize', scale=(-1, 256)),
        dict(type='CenterCrop', crop_size=224),
        dict(
            type='Normalize',
            mean=[123.675, 116.28, 103.53],
            std=[58.395, 57.12, 57.375],
            to_bgr=False),
        dict(type='FormatShape', input_format='NCHW'),
        dict(type='Collect', keys=['imgs', 'label'], meta_keys=[]),
        dict(type='ToTensor', keys=['imgs'])
    ]
    test_pipeline = [
        dict(type='DecordInit'),
        dict(
            type='SampleFrames',
            clip_len=1,
            frame_interval=1,
            num_clips=25,
            test_mode=True),
        dict(type='DecordDecode'),
        dict(type='Resize', scale=(-1, 256)),
        dict(type='ThreeCrop', crop_size=256),
        dict(
            type='Normalize',
            mean=[123.675, 116.28, 103.53],
            std=[58.395, 57.12, 57.375],
            to_bgr=False),
        dict(type='FormatShape', input_format='NCHW'),
        dict(type='Collect', keys=['imgs', 'label'], meta_keys=[]),
        dict(type='ToTensor', keys=['imgs'])
    ]
    data = dict(
        videos_per_gpu=32,
        workers_per_gpu=2,
        test_dataloader=dict(videos_per_gpu=1),
        train=dict(
            type='VideoDataset',
            ann_file='data/kinetics400/kinetics400_train_list_videos.txt',
            data_prefix='data/kinetics400/videos_train',
            pipeline=[
                dict(type='DecordInit'),
                dict(
                    type='SampleFrames', clip_len=1, frame_interval=1,
                    num_clips=3),
                dict(type='DecordDecode'),
                dict(type='RandomResizedCrop'),
                dict(type='Resize', scale=(224, 224), keep_ratio=False),
                dict(type='Flip', flip_ratio=0.5),
                dict(
                    type='Normalize',
                    mean=[123.675, 116.28, 103.53],
                    std=[58.395, 57.12, 57.375],
                    to_bgr=False),
                dict(type='FormatShape', input_format='NCHW'),
                dict(type='Collect', keys=['imgs', 'label'], meta_keys=[]),
                dict(type='ToTensor', keys=['imgs', 'label'])
            ]),
        val=dict(
            type='VideoDataset',
            ann_file='data/kinetics400/kinetics400_val_list_videos.txt',
            data_prefix='data/kinetics400/videos_val',
            pipeline=[
                dict(type='DecordInit'),
                dict(
                    type='SampleFrames',
                    clip_len=1,
                    frame_interval=1,
                    num_clips=3,
                    test_mode=True),
                dict(type='DecordDecode'),
                dict(type='Resize', scale=(-1, 256)),
                dict(type='CenterCrop', crop_size=224),
                dict(
                    type='Normalize',
                    mean=[123.675, 116.28, 103.53],
                    std=[58.395, 57.12, 57.375],
                    to_bgr=False),
                dict(type='FormatShape', input_format='NCHW'),
                dict(type='Collect', keys=['imgs', 'label'], meta_keys=[]),
                dict(type='ToTensor', keys=['imgs'])
            ]),
        test=dict(
            type='VideoDataset',
            ann_file='data/kinetics400/kinetics400_val_list_videos.txt',
            data_prefix='data/kinetics400/videos_val',
            pipeline=[
                dict(type='DecordInit'),
                dict(
                    type='SampleFrames',
                    clip_len=1,
                    frame_interval=1,
                    num_clips=25,
                    test_mode=True),
                dict(type='DecordDecode'),
                dict(type='Resize', scale=(-1, 256)),
                dict(type='ThreeCrop', crop_size=256),
                dict(
                    type='Normalize',
                    mean=[123.675, 116.28, 103.53],
                    std=[58.395, 57.12, 57.375],
                    to_bgr=False),
                dict(type='FormatShape', input_format='NCHW'),
                dict(type='Collect', keys=['imgs', 'label'], meta_keys=[]),
                dict(type='ToTensor', keys=['imgs'])
            ]))
    evaluation = dict(
        interval=5, metrics=['top_k_accuracy', 'mean_class_accuracy'])
    work_dir = '/mmaction2/data2/kinetics400/train/'
    gpu_ids = [0]
    omnisource = False
    module_hooks = []

    2022-10-27 19:28:37,730 - mmaction - INFO - Set random seed to 0, deterministic: True
    load checkpoint from torchvision path: torchvision://resnet50
    2022-10-27 19:28:38,232 - mmaction - INFO - These parameters in pretrained checkpoint are not loaded: {'fc.weight', 'fc.bias'}
    Traceback (most recent call last):
        File "/opt/conda/lib/python3.7/site-packages/mmcv/utils/registry.py", line 52, in build_from_cfg
        return obj_cls(**args)
        File "/mmaction2/mmaction/datasets/video_dataset.py", line 40, in __init__
        super().__init__(ann_file, pipeline, start_index=start_index, **kwargs)
        File "/mmaction2/mmaction/datasets/base.py", line 89, in __init__
        self.video_infos = self.load_annotations()
        File "/mmaction2/mmaction/datasets/video_dataset.py", line 48, in load_annotations
        with open(self.ann_file, 'r') as fin:
    FileNotFoundError: [Errno 2] No such file or directory: 'data/kinetics400/kinetics400_train_list_videos.txt'

    During handling of the above exception, another exception occurred:

    Traceback (most recent call last):
        File "tools/train.py", line 222, in <module>
        main()
        File "tools/train.py", line 190, in main
        datasets = [build_dataset(cfg.data.train)]
        File "/mmaction2/mmaction/datasets/builder.py", line 40, in build_dataset
        dataset = build_from_cfg(cfg, DATASETS, default_args)
        File "/opt/conda/lib/python3.7/site-packages/mmcv/utils/registry.py", line 55, in build_from_cfg
        raise type(e)(f'{obj_cls.__name__}: {e}')
    FileNotFoundError: VideoDataset: [Errno 2] No such file or directory: 'data/kinetics400/kinetics400_train_list_videos.txt'
    ```
- 에러 발생  `FileNotFoundError: VideoDataset: [Errno 2] No such file or directory: 'data/kinetics400/kinetics400_train_list_videos.txt'`
- 경로가 data가 아니라 data2이어야 하는 문제

---

#### 8. falldown 파일에 load 시켜서 사용하는 방식
- 코드
- train.py
  ```
  root@6f488173946c:/mmaction2# python tools/train.py configs/recognition/tsn/tsn_r50_video_1x1x8_100e_kinetics400_rgb_falldown.py --work-dir /mmaction2/data/dataset-jtod/train/ --validate --gpu-ids 0 --seed 0 --deterministic
  ```
- tsn_r50_video_1x1x8_100e_kinetics400_rgb_falldown.py
- demo.py  : demo.mp4
  ```
  root@6f488173946c:/mmaction2# python demo/demo.py configs/recognition/tsn/tsn_r50_video_1x1x8_100e_kinetics400_rgb_falldown.py data/dataset-jtod/train/latest.pth demo/demo.mp4 tools/data/jtod/label_map_jtod_401.txt --out-filename data/demo_out_you3.mp4
  load checkpoint from local path: data/dataset-jtod/train/latest.pth
  time : 2.2921714782714844
  The top-5 labels with corresponding scores are:
  abseiling:  67.66443
  fixing hair:  0.40743375
  sharpening pencil:  0.3007607
  checking tires:  0.27560824
  carving pumpkin:  0.2534244
  Moviepy - Building video data/demo_out_you3.mp4.
  Moviepy - Writing video data/demo_out_you3.mp4

  Moviepy - Done !                                                                                                                                      
  Moviepy - video ready data/demo_out_you3.mp4
  ```
- demo.py : test1.mp4
  ```
  root@6f488173946c:/mmaction2# python demo/demo.py configs/recognition/tsn/tsn_r50_video_1x1x8_100e_kinetics400_rgb_falldown.py data/dataset-jtod/train/latest.pth data/dataset-jtod/test1.mp4 tools/data/jtod/label_map_jtod_401.txt --out-filename data/test_out_you3.mp4
  ```
  
- 생각을 잘못함 파일 리스트도 수정해야
  ```
  # 파일 리스트 생성
  root@6f488173946c:/mmaction2# bash generate_videos_filelist_falldown.sh dataset-jtod

  # 학습
  root@6f488173946c:/mmaction2# python tools/train.py configs/recognition/tsn/tsn_r50_video_1x1x8_100e_kinetics400_rgb_falldown.py --work-dir /mmaction2/data/dataset-jtod/train/ --validate --gpu-ids 0 --seed 0 --deterministic
  
  # 테스트 - 결과는 그닥 별로 다르지 않음
  root@6f488173946c:/mmaction2# python demo/demo.py configs/recognition/tsn/tsn_r50_video_1x1x8_100e_kinetics400_rgb_falldown.py data/dataset-jtod/train/latest.pth data/dataset-jtod/test1.mp4 tools/data/jtod/label_map_jtod_401.txt --out-filename data/test_out1.mp4
  ```

---

#### 9. 결과 & 다음 단계
- 위에는 data 와 연결해서 안에 비디오, csv, 파일리스트 다 falldown 만 관련된것만 넣고
- load .pth 시키고, 400개에서 classes 401개로 변경시키고, jtod_label_map_401 만 만들어서 사용했는데
- 결과가 좋지 않았음
- 원래 있던 영상도 알아보지 못하고, 같은 abseling 값만 반복적으로 모든 테스트 비디오의 1순위 결과 값으로 나오는 현상
- 이게 모델만 load 시킨다고 될 일이 아닌 것 같음

- 위는 jtod 하던거에 로드시키는 작업을 한거지만, 이번에는 data2 하던 거에 로드시키는 작업을 해서 영상 또한 포함시켜서 학습시키는 것을 도전
- 문제는 이전에 data2할 때 성공한게 아니라 (RuntimeError, data/ 디렉토리 찾는문제) 걸리는 게 있지만 일단 진행

---

#### 10. data2파일에 로드시키기
- 데이터 준비
  - 파일리스트 train 은 다 나오고, val 은 falldown 만 나오는 상태지만 어차피 학습은 train 이라고 생각하고 진행함
  - 비디오 videos_val, videos_train 에 falldown 추가 완료
  - 라벨 jtod_label_map_401 사용하면 되고 (124라인에 falldown 추가함, 이전에는 401에 추가했었는데 파일 리스트 마지막 번호가 라벨번호인것으로 추정, falldown 123으로 되어있음)
  - tsn_r50_video_1x1x8_100e_kinetics400_rgb_data2.py
  - jtod_runtime.py
  - tsn_r50_data2.py

- 코드
- 파일 리스트 생성은 이미 했으니까 (txt 파일 나온거)
- train 시키기
  ```
  root@6f488173946c:/mmaction2# python tools/train.py configs/recognition/tsn/tsn_r50_video_1x1x8_100e_kinetics400_rgb_data2.py --work-dir /mmaction2/data2/kinetics400/train/ --validate --gpu-ids 0 --seed 0 --deterministic

  RuntimeError: CUDA out of memory. Tried to allocate 784.00 MiB (GPU 0; 7.93 GiB total capacity; 6.17 GiB already allocated; 761.06 MiB free; 6.20 GiB reserved in total by PyTorch)


  401개로 두면
  size mismatch for cls_head.fc_cls.weight: copying a param with shape torch.Size([400, 2048]) from checkpoint, the shape in current model is torch.Size([401, 2048]).
  size mismatch for cls_head.fc_cls.bias: copying a param with shape torch.Size([400]) from checkpoint, the shape in current model is torch.Size([401]).

  같이 에러 나오고

  400개로 두면 안나오긴 하는데
  그래봤자 Runtime 에서 걸리는 건 똑같음

  sdg_100e.py 의 total_epochs 숫자 줄여봐도 똑같음
  ```
  


<br>
