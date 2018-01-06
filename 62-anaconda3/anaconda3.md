- [Conda를 사용한 Python 가상환경 사용하기](#conda%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%9C-python-%EA%B0%80%EC%83%81%ED%99%98%EA%B2%BD-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0)
    - [Python Virtualenv](#python-virtualenv)
    - [Conda 가상환경 관리](#conda-%EA%B0%80%EC%83%81%ED%99%98%EA%B2%BD-%EA%B4%80%EB%A6%AC)
        - [가상환경 생성](#%EA%B0%80%EC%83%81%ED%99%98%EA%B2%BD-%EC%83%9D%EC%84%B1)
        - [가상환경 복사](#%EA%B0%80%EC%83%81%ED%99%98%EA%B2%BD-%EB%B3%B5%EC%82%AC)
        - [가상환경 활성화 및 파이썬 경로 찾기](#%EA%B0%80%EC%83%81%ED%99%98%EA%B2%BD-%ED%99%9C%EC%84%B1%ED%99%94-%EB%B0%8F-%ED%8C%8C%EC%9D%B4%EC%8D%AC-%EA%B2%BD%EB%A1%9C-%EC%B0%BE%EA%B8%B0)
        - [현재 생성된 가상환경을 확인하기](#%ED%98%84%EC%9E%AC-%EC%83%9D%EC%84%B1%EB%90%9C-%EA%B0%80%EC%83%81%ED%99%98%EA%B2%BD%EC%9D%84-%ED%99%95%EC%9D%B8%ED%95%98%EA%B8%B0)
        - [가상환경을 다른 컴퓨터로 이전하려는 경우](#%EA%B0%80%EC%83%81%ED%99%98%EA%B2%BD%EC%9D%84-%EB%8B%A4%EB%A5%B8-%EC%BB%B4%ED%93%A8%ED%84%B0%EB%A1%9C-%EC%9D%B4%EC%A0%84%ED%95%98%EB%A0%A4%EB%8A%94-%EA%B2%BD%EC%9A%B0)
        - [가상환경 삭제](#%EA%B0%80%EC%83%81%ED%99%98%EA%B2%BD-%EC%82%AD%EC%A0%9C)
    - [패키지 관리하기](#%ED%8C%A8%ED%82%A4%EC%A7%80-%EA%B4%80%EB%A6%AC%ED%95%98%EA%B8%B0)
        - [새 패키지 설치](#%EC%83%88-%ED%8C%A8%ED%82%A4%EC%A7%80-%EC%84%A4%EC%B9%98)
        - [패키지 업데이트](#%ED%8C%A8%ED%82%A4%EC%A7%80-%EC%97%85%EB%8D%B0%EC%9D%B4%ED%8A%B8)
    - [텐서플로 설치](#%ED%85%90%EC%84%9C%ED%94%8C%EB%A1%9C-%EC%84%A4%EC%B9%98)

Conda를 사용한 Python 가상환경 사용하기
=======================================

Python Virtualenv
-----------------

- 파이썬은 다양한 라이브러리를 지원한다. 그런데 여기엔 의존성 문제가 있다.
- 예를들어 오래된 Project1 은 Tensorflow 1.1 기반으로 개발이 진행되어 왔고,
  Project2 는 Tensorflow 1.4 기반으로 개발이 진행되었다고 해 보자.
  문제는 TF1.1과 TF1.4의 데이터는 상호 호환이 안 된다.
- 프로젝트 넘어갈 때마다 TF를 삭제하고 버전에 맞게 재설치 하면서 써야 한다.

---

- 이럴때 가상 환경을 사용한다. 아예 파이썬 설치를 분리하는 것이다.
- 파이썬은 자체적으로 독립된 설치 인스턴스를 지원하므로, 최소한의 라이브러리만 복사하면 된다.
- 이걸 자동화 해 주는 것이 Python Virtualenv 이며, Conda는 Virtualenv 관리 프로그램이다.

---

- Virtualenv를 사용하면 디스크 용량이 낭비되는 것은 사실이지만, 하드디스크 가격은 꽤 저렴하다.
- 서로 다른 프로젝트에서 라이브러리 버전 충돌을 막을 수 있으며, 여기에서 얻는 이득이 더 크다.
- 웬만하면 개별 프로젝트 마다 Virtualenv 만들어서 쓰자...

Conda 가상환경 관리
-------------------

### 가상환경 생성

- 사용하려는 라이브러리 목록을 인자로 기재해 준다.
- 특정 라이브러리를 사용하려면 라이브러리=버전 식으로 기재
- 가상환경 이름은 알아보기 쉬운 것으로 권장한다

```console
$ conda create --name py35-tf-gpu python=3.5
... blahblah ...
```


### 현재 생성된 가상환경을 확인하기

- 현재 활성화(activate)된 환경에 * 이 붙는다.

```console
$ conda env list
# conda environments:
#
py35-tf-gpu              /home/koasing/.conda/envs/py35-tf-gpu
root                  *  /opt/anaconda3
```

### 가상환경 활성화 및 파이썬 경로 찾기

- 파이썬 경로는 PyCharm 설정에 필요하므로 따로 적어 둘 것.

```console
$ source activate py35-tf-gpu
$ which python
/home/koasing/.conda/envs/py35-tf-gpu/bin/python
$ python --version
Python 3.5.4 :: Anaconda, Inc.
$ source deactivate
$ which python
/opt/anaconda3/bin/python
$ python --version
Python 3.6.3 :: Anaconda, Inc.
```

### 새 패키지 설치

- 패키지를 설치할 환경을 미리 활성화 하고 진행한다.
  아니면 `--name` 옵션으로 환경 이름을 명시해서 설치할 수도 있다.

**참고** conda install tensorflow 로 설치하면 TF1.3 이 설치되므로 주의할 것.

```console
$ source activate new_env_name
$ conda install tensorflow-gpu
... blahblah ...
```

### 가상환경 복사 (Optional)

- pip로 설치한 패키지도 자동으로 처리해 준다.
- 동일한 사용자 내에서 복사하는 방법이다. 다른 사용자나 다른 서버로 넘길거면 아래 참조.

```console
$ conda --clone old_env_name --name new_env_name
... blahblah ...
```


### 가상환경을 다른 컴퓨터로 이전하려는 경우 (Optional)

- pip로 설치한 패키지도 자동으로 처리해 준다.
- 첫 번째 컴퓨터에서는 가상환경을 yml 파일로 내보낸다.

```console
$ conda env export --name py35-tf-gpu --file export.yml
```

- 내보낸 파일을 두 번째 컴퓨터로 복사하고, 새 환경을 만들어 준다.
- `conda env create` 임에 주의할 것. 새 환경을 만들때와 명령어가 약간 다르다.

```console
$ conda env create --name tensorflow-gpu --file export.yml
... blahblah ...
```

### 가상환경 삭제

```console
$ conda env remove --name old_env_name
... blahblah ...
```

패키지 관리하기
---------------


### 패키지 업데이트

- 패키지 이름을 늘어두거나, `--all` 옵션을 주어 전체 패키지를 다 업데이트 할 수 있다.
- 개발환경이 고정(freeze) 되었다면 가급적 패키지 버전 업데이트는 하지 않는게 좋다.
- 크리티컬 이슈로 패키지 버전을 엎어야 하는 상황이라면, 새 환경을 복사해서 작업하는게 유리하겠지?

```console
$ conda update --all
... blahblah ...
```

예제) Tensorflow 설치
-------------

```console
$ source activate py35-tf-gpu
$ pip install --ignore-installed tensorflow-gpu
... blahblah ... many packages ...
```

```pycon
Python 3.5.4 |Anaconda, Inc.| (default, Nov 20 2017, 18:44:38)
[GCC 7.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import tensorflow as tf
>>> tf.__version__
'1.4.1'
>>> h = tf.constant('Hello TF!')
>>> sess = tf.Session()
2017-12-18 18:52:47.531043: I tensorflow/core/platform/cpu_feature_guard.cc:137] Your CPU supports instructions that this TensorFlow binary was not compiled to use: SSE4.1 SSE4.2 AVX
2017-12-18 18:52:47.695491: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:892] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero
2017-12-18 18:52:47.695893: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1030] Found device 0 with properties:
name: GeForce GTX 1080 Ti major: 6 minor: 1 memoryClockRate(GHz): 1.721
pciBusID: 0000:01:00.0
totalMemory: 10.91GiB freeMemory: 10.72GiB
2017-12-18 18:52:47.695915: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1120] Creating TensorFlow device (/device:GPU:0) -> (device: 0, name: GeForce GTX 1080 Ti, pci bus id: 0000:01:00.0, compute capability: 6.1)
>>> print(sess.run(h))
b'Hello TF!'
```


예제) PyTorch 설치
-------------

```console
$ source activate py36-torch
$ conda install pytorch torchvision cuda90 -c pytorch
... blahblah ... many packages ...
```

-----
