- [PyCharm 셋업 & 원격 실행](#pycharm-%EC%85%8B%EC%97%85-%EC%9B%90%EA%B2%A9-%EC%8B%A4%ED%96%89)
    - [PyCharm 설치](#pycharm-%EC%84%A4%EC%B9%98)
    - [DNN 서버 등록](#dnn-%EC%84%9C%EB%B2%84-%EB%93%B1%EB%A1%9D)
    - [Ptyhon 가상환경 설정](#ptyhon-%EA%B0%80%EC%83%81%ED%99%98%EA%B2%BD-%EC%84%A4%EC%A0%95)
    - [프로젝트 설정](#%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EC%84%A4%EC%A0%95)
        - [새 프로젝트](#%EC%83%88-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8)
        - [기존 프로젝트](#%EA%B8%B0%EC%A1%B4-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8)
    - [Tensorflow 테스트](#tensorflow-%ED%85%8C%EC%8A%A4%ED%8A%B8)

PyCharm 셋업 & 원격 실행
========================

PyCharm 설치
------------

- <https://www.jetbrains.com/pycharm/>
- Pro 버전을 다운받는다. 라이센스는 학교 이메일 주소로 학생 등록하면 1년단위로 무료 갱신된다.
- 설치 후 라이센스 등록까지 마치고 실행한다.
- 설치도중 JRE 설치여부를 물으면 같이 설치한다.
  PyCharm 자체는 Java로 제작되어서 실행하는데 Java 런타임을 필요로 한다.

DNN 서버 등록
-------------

이 과정은 서버마다 한 번씩 해 줘야 한다.

- Welcome 윈도우면 Configure - Settings. 프로젝트를 불러왔다면 File - Default Settings.
- Build, Execution, Deployment - Deployment 메뉴 진입
- 서버 리스트에서 + 눌러서 서버 추가하고, Connection 은 SFTP, 서버 주소 등을 입력한다.
- Test SFTP connection 눌러서 성공 뜨는지 확인하고 root path autodetection 해 준다.
- Web server는 그냥 비워두던지 IP주소를 입력해 준다. 이거 설정하면 주피터 편하게 쓸 수 있는데 귀찮다... 
- Mapping, Excluded Paths 탭에서는 설정이 안 떠야 정상이다. 이건 프로젝트별 설정이다.

Ptyhon 가상환경 설정
--------------------

이 과정은 서버에서 가상환경이 추가/변경될 때마다 해 줘야 한다.

- Welcome 윈도우면 Configure - Settings. 프로젝트를 불러왔다면 File - Default Settings.
- Project Interpreter 메뉴 진입
- 현재 추가되어 있는 가상환경 목록이 뜬다. 처음 설치시에는 <No interpreter> 뜰 것이다.
  리스트 누르고 Show all 선택하면 목록 창이 뜬다. 우측 + 누르고 Add Remote 선택
- 기본값으로 Deployment Configuration, 위에서 추가한 서버가 뜰 것이다. 안 뜨면 선택해 준다.
- Interpreter Path 는 `which` 로 찾아준 파이썬 실행파일 절대경로를 입력한다.
  activate 안 해도 실행파일 절대경로를 지정해 주면 가상환경 분리는 잘 작동한다.
- 이름 변경하려면 우측 연필 누르고 고치면 된다.
- 이제 창 닫아주면 설치된 패키지 목록이 뜰 것이다.

프로젝트 설정
-------------

### 새 프로젝트

- New Project
- Project interpreter 눌러서 숨겨진 항목들을 펼쳐준다.
- Existing interpreter 에서 위에서 추가한 가상환경을 선택해 준다.
  여기서 톱니바퀴 누르고 가상환경을 추가할 수도 있다.
- Remote project location 은 기본적으로 서버의 /tmp 밑으로 들어간다.
  본인의 홈디렉터리 밑으로 적절히 옮겨준다.
- PyCharm 특성상 전역 설정을 프로젝트별 설정으로 복사해 오기 때문에 좀 지저분해진다.
  다음 절차를 계속 수행해서 필요없는 설정을 지워준다.

### 기존 프로젝트

- File - Settings
- Build, Execution, Deployment - Deployment 메뉴 진입
- 서버 등록에서 추가해 준 서버가 보일 것이다. 선택하고 Mappings 진입
- Local path는 내 컴퓨터의 프로젝트 경로가 지정되어 있을 것이다.
- Deployment path 는 비어 있을 것이다. 서버에서 프로젝트 파일 저장할 경로를 지정해 준다.
- Web path... `/` 입력하고 넘어가자... 주피터 쓰려면 잘 지정해주면 된다.
- Project - Project Interpreter 메뉴 진입
- 인터프리터를 아까 추가해 준 원격서버로 지정해 준다. 아니면 바로 추가하던지.
- Path mappings에서 \<Project root\> -> 적절한 경로 가 떠야 한다.
- 서버에 설치된 패키지를 복사해 와서 PyCharm에서 분석하는 과정을 거쳐야 한다.
  시간이 몇 분에서 십수 분 걸리므로 기다릴 것. Tensorflow 용량이 좀 크다.

Tensorflow 테스트
-----------------

- Hello TF 작성하고 실행시켜 본다.
```python
import tensorflow as tf

h = tf.constant('Hello TF!')
sess = tf.Session()

print(sess.run(h))
```

```console
ssh://koasing@192.168.10.79:42202/home/koasing/.conda/envs/tf35-gpu/bin/python -u /home/koasing/dnn/tf_hello/tf_hello.py
2017-12-21 16:53:34.403127: I tensorflow/core/platform/cpu_feature_guard.cc:137] Your CPU supports instructions that this TensorFlow binary was not compiled to use: SSE4.1 SSE4.2 AVX
2017-12-21 16:53:35.044563: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:892] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero
2017-12-21 16:53:35.044968: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1030] Found device 0 with properties: 
name: GeForce GTX 1080 Ti major: 6 minor: 1 memoryClockRate(GHz): 1.721
pciBusID: 0000:01:00.0
totalMemory: 10.91GiB freeMemory: 10.75GiB
2017-12-21 16:53:35.044990: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1120] Creating TensorFlow device (/device:GPU:0) -> (device: 0, name: GeForce GTX 1080 Ti, pci bus id: 0000:01:00.0, compute capability: 6.1)
b'Hello TF!'

Process finished with exit code 0
```
