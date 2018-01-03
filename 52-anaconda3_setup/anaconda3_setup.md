- [Anaconda3 Setup](#anaconda3-setup)
    - [System-wide vs Per-user](#system-wide-vs-per-user)
        - [System-wide 설치](#system-wide-%EC%84%A4%EC%B9%98)
        - [Per-user 설치](#per-user-%EC%84%A4%EC%B9%98)
    - [Anaconda3 다운로드](#anaconda3-%EB%8B%A4%EC%9A%B4%EB%A1%9C%EB%93%9C)
    - [Anaconda3 설치](#anaconda3-%EC%84%A4%EC%B9%98)
    - [환경변수 설정](#%ED%99%98%EA%B2%BD%EB%B3%80%EC%88%98-%EC%84%A4%EC%A0%95)
    - [설치 확인](#%EC%84%A4%EC%B9%98-%ED%99%95%EC%9D%B8)

Anaconda3 Setup
===============

이 섹션은 System-wide 설정을 다루므로, 서버 설치가 필요할 때마다 한 번씩 수행하면 된다.

System-wide vs Per-user
-----------------------

다른 이유가 없다면 System-wide 설치한다.
내 서버가 아니라서 sudo 권한이 없는 경우라면 그냥 Per-user 써야지 뭐...

### System-wide 설치

- 최초 설치 및 conda 유지보수 작업을 슈퍼유저 권한으로 해야 한다. 즉 유지보수가 관리자 책임이다.
- Anaconda 설치 인스턴스 하나를 모든 유저가 공유하므로 디스크 공간이 절약된다.
- 개별 유저는 서로 독립적으로 conda envs를 갖는다. (~/.conda/envs)
- 개별 유저는 슈퍼유저 권한 없이도 자신의 conda envs를 생성/관리/삭제할 수 있다.
- 개별 유저가 root env를 관리할 수 없다. 어짜피 가상환경 만들어서 써야 하므로 별 문제는 안 된다.

### Per-user 설치

- 최초 설치 및 conda 유지보수 작업을 개별 유저가 할 수 있다. 즉 유지보수가 개별 유저 책임이다.
- 유저마다 Anaconda 설치 인스턴스를 가지므로 디스크 공간이 낭비된다.
- 개별 유저는 서로 독립적으로 conda envs를 갖는다. (~/anaconda3/envs)
- 개별 유저는 슈퍼유저 권한 없이도 자신의 conda envs를 생성/관리/삭제할 수 있다.
- 개별 유저가 root env를 관리할 수 있다.
- 물론 System-wide 설치가 되어 있어도 Per-user 방식으로 추가로 설치할 수 있다.
  대신 디스크 용량을 많이 점유하면 욕먹겠지?

Anaconda3 다운로드
------------------

- <https://www.anaconda.com/download/#linux>
- Python3 리눅스 버전을 다운로드 해서 서버에 업로드 한다. 아니면 `wget`으로 직접 받아도 된다.

![](01.png)

```console
$ wget https://repo.continuum.io/archive/Anaconda3-5.0.1-Linux-x86_64.sh
--2017-12-28 17:39:29--  https://repo.continuum.io/archive/Anaconda3-5.0.1-Linux-x86_64.sh
Resolving repo.continuum.io (repo.continuum.io)... 104.16.18.10, 104.16.19.10, 2400:cb00:2048:1::6810:130a, ...
Connecting to repo.continuum.io (repo.continuum.io)|104.16.18.10|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 550796553 (525M) [application/x-sh]
Saving to: ‘Anaconda3-5.0.1-Linux-x86_64.sh’

Anaconda3-5.0.1-Lin 100%[===================>] 525.28M  5.74MB/s    in 92s

2017-12-28 17:41:01 (5.72 MB/s) - ‘Anaconda3-5.0.1-Linux-x86_64.sh’ saved [550796553/550796553]
```

Anaconda3 설치
--------------

- Per-user 로 설치하려면 sudo 없이 설치를 실행하고, 설치경로는 기본값을 쓰면 된다.

```console
$ sudo bash Anaconda3-5.0.1-Linux-x86_64.sh

Welcome to Anaconda3 5.0.1

In order to continue the installation process, please review the license
agreement.
Please, press ENTER to continue
>>>

Do you accept the license terms? [yes|no]
[no] >>> yes

Anaconda3 will now be installed into this location:
/home/admin_user/anaconda3

  - Press ENTER to confirm the location
  - Press CTRL-C to abort the installation
  - Or specify a different location below

[/home/koasing/anaconda3] >>> /opt/anaconda3
PREFIX=/opt/anaconda3
installing: python-3.6.3-hc9025b9_1 ...
Python 3.6.3 :: Anaconda, Inc.
... blahblah ... very many packages ...
installing: anaconda-5.0.1-py36hd30a520_1 ...
installation finished.
Do you wish the installer to prepend the Anaconda3 install location
to PATH in your /home/admin_user/.bashrc ? [yes|no]
[no] >>>

You may wish to edit your .bashrc to prepend the Anaconda3 install location to PATH:

export PATH=/opt/anaconda3/bin:$PATH

Thank you for installing Anaconda3!
```

환경변수 설정
-------------

- Anaconda를 기본값으로 사용하려는 유저는 `~/.bashrc` 파일에 다음 내용을 추가해 준다.
- 향후 추가될 사용자에게 기본값으로 적용하려면 `/etc/skel/.bashrc` 파일에 추가해준다.

```
# Anaconda3
export PATH=/opt/anaconda3/bin${PATH:+:${PATH}}
```

- PATH 추가를 하지 않은 경우, conda 사용할 때에는 전체 경로를 입력해서 작업해야 한다.
- 또는, conda 사용할 때에만 명시적으로 root 환경을 activate 하면 된다.
  `source /opt/anaconda3/bin/activate root`

설치 확인
---------

- 환경변수 설정이 반영되려면 한 번 로그아웃 한 뒤 다시 로그인 해야 한다.

```console
$ conda --version
conda 4.3.30
$ python --version
Python 3.6.3 :: Anaconda, Inc.
```
