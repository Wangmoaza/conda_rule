conda와 함께하는 슬기로운 서버생활
============================

## Python 기본 규칙

#### 1.Python은 기본적으로 conda 배포판을 사용한다.
#### 2. Python package를 설치 시 conda environment를 사용한다.

---

### Table of Contents

0. 소개
1. Python은 기본적으로 conda 배포판을 사용한다.
2.  Python package를 설치 시 conda environment를 사용한다.
    1. 가상환경 생성
    2. 가상환경 실행
    3. 가상환경 관리
    4. 가상환경에서 pip 사용하기
    5. 가상환경에 패키지 설치
        1. bioconda를 이용한 패키지 설치
    6. 가상환경 제거
3. Quick Start
4. 맺으며

---
## 0. 소개

#### Anaconda
Conda is an open source package management system and environment management system that runs on Windows, macOS and Linux. Conda quickly installs, runs and updates packages and their dependencies. Conda easily creates, saves, loads and switches between environments on your local computer. It was created for Python programs, but it can package and distribute software for any language.

Conda as a package manager helps you find and install packages. If you need a package that requires a different version of Python, you do not need to switch to a different environment manager, because conda is also an environment manager. With just a few commands, you can set up a totally separate environment to run that different version of Python, while continuing to run your usual version of Python in your normal environment.

From: https://conda.io/docs/index.html

#### 가상환경이란?

![virtual environment](https://dojang.io/pluginfile.php/5584/mod_page/content/2/048003.png)

From: https://dojang.io/mod/page/view.php?id=1168

---
## 1. Python은 기본적으로 conda 배포판을 사용한다.

서버에 기본 파이썬은 conda 버전으로 되어있을 것입니다.
현재 어떤 파이썬을 사용하는지는 아래 커맨드를 이용해 확인할 수 있습니다.

```shell
$ which python
/home/omics/miniconda2/bin/python

$ python
Python 2.7.15 |Anaconda, Inc.| (default, Nov 13 2018, 23:04:45)
[GCC 7.3.0] on linux2
Type "help", "copyright", "credits" or "license" for more information.
```

만약 파이썬이 linux 배포판으로 되어있을 경우 다음의 조치를 취한 후 다시 확인해주시길 바랍니다.

```shell
$ source ~/.bashrc
```

## 2. Python package를 설치 시 conda environment를 사용한다.

conda virtual environment는 격리된 독립적인 파이썬 가상환경을 제공합니다. 이는 여러 프로젝트를 진행할 때 패키지 버전이 다른 문제점을 해결해줍니다. 가상 환경은 기본적으로 자신의 개발 환경을 보호한다는 개념이므로 새로운 패키지를 설치할 때는 꼭 가상환경 내에서 해주시길 바랍니다.

### 2-1. 가상환경 생성
conda envrionment를 만들 때는

```shell
$ conda create --name [name]
```

특정 파이썬 버전을 지정해주고 싶을 때는
```shell
$ conda create -n [name] python=3.4
```

패키지 생성 시점에서 특정 패키지를 함께 깔아주고 싶을 때는
```shell
$ conda create -n [name] scipy numpy
```

특정 버전의 패키지를 깔고 싶을 때는

```shell
$ conda create -n [name] scipy=0.15.0
```

특정 버전의 파이썬과 패키지를 동시에 깔 때는

```shell
$ conda create -n [name] python=3.4 scipy=0.15.0 astroid babel
```

TIP: 패키지간 dependency가 다를 수 있으므로 여러 패키지를 설치할 때는 하나씩 설치하는 것 보다 한꺼번에 설치하는 것이 좋습니다.

### 2-2. 가상환경 실행

conda environment를 활성화 시킬 때는

```shell
$ conda activate [name]
```

자신의 가상환경이 활성화 되어있으면 커맨드창이 다음과 같이 뜰 것입니다.

```shell
(name) $
```

가상환경을 비활성화 시키고 싶으면

```shell
(name) $ conda deactivate
```

### 2-3. 가상환경 관리

현재 존재하는 모든 가상환경을 보려면

```shell
$ conda info --env
# conda environments:
#
base                  *  /home/haeun/miniconda2
cmappy                   /home/haeun/miniconda2/envs/cmappy

```
현재 사용하는 가상환경에 * 표시가 되어있습니다.

가상환경 내에 설치되어있는 모든 패키지들을 확인하려면

```shell
$ conda list -n [name]
```

가상 환경이 활성화되어있다면

```shell
(name) $ conda list
```

### 2-4. 가상환경에서 pip 사용하기

여태까지 파이썬 패키지를 설치할 때는 pyPI를 많이 사용하셨을 것입니다. 대부분 패키지들은 conda를 이용하여 깔 수 있지만 conda에 원하는 패키지가 올라와있지 않은 경우는 pip을 사용해야 합니다. conda와 pip을 섞어 사용하는 것은 권장되는 사항은 아니나 위와 같은 불가피한 경우는 가상환경 내에 pip을 설치하신 후에 이용해주시길 바랍니다. 만약 system pip을 사용하게 되면 pip을 이용한 패키지 설치는 가상환경의 파이썬이 아닌 system 파이썬에 깔리게 됩니다.

```shell
$ conda install -n [name] pip
$ conda activate [name]
(name) $ pip <pip_subcommand>
```

현재 어떤 pip을 사용하고있는지는 다음 커맨드로 확인할 수 있습니다.
```shell
$ type pip
pip is /home/omics/miniconda2/bin/pip

# 가상환경이 활성화되어있는 상태라면
(name) $ type pip
pip is /home/omics/miniconda2/envs/name/bin/pip
```

아래는 [anaconda blog](https://www.anaconda.com/blog/developer-blog/using-pip-in-a-conda-environment/)에서 명시해준 best practice checklsit입니다.

> #### Best Practices Checklist
> *Use pip only after conda*
> * install as many requirements as possible with conda, then use pip
> * pip should be run with –upgrade-strategy only-if-needed (the default)
> * Do use pip with the –user argument, avoid all “users” installs
> *Use conda environments for isolation*
> * create a conda environment to isolate any changes pip makes
> * environments take up little space thanks to hard links
> * care should be taken to avoid running pip in the “root” environment
> *Recreate the environment if changes are needed*
> * once pip has been used conda will be unaware of the changes
> * to install additional conda packages it is best to recreate the environment
> *Store conda and pip requirements in text files*
> * package requirements can be passed to conda via the –file argument
> * pip accepts a list of Python packages with -r or –requirements
> * conda env will export or create environments based on a file with conda and pip requirements


### 2-5. 가상환경에 package 설치

2-1에서 언급한 것 처럼 가상환경을 생성하는 시점에서 패키지를 함께 설치하는 것이 권장되지만 가상환경을 사용하는 중간에 패캐지를 새로 설치하고 싶을 때도 많이 있습니다.

가상 환경 밖에서 설치하는 방법

```shell
$ conda install -n [name] scipy
```

가상환경을 실행시킨 후 설치하는 방법

```shell
$ conda activate name
(name) $ conda install scipy
```


#### 2-5-1. bioconda를 이용한 패키지 설치

conda의 기본 채널 이외에 다양한 채널들이 존재하는데 이 중 하나가 bioconda입니다.
우리가 사용하는 다양한 bioinformatics tool들이 bioconda에 있다면 이를 이용해 설치할 수 있습니다.
conda를 이용해 설치를 하면 PATH에 따로 경로를 올려놓지 않아도 가상환경 내에서 바로 실행이 가능합니다.
이 방법이 항상 최선은 아니지만 대중적으로 이용되고 관리가 잘 되어있는 tool들은 conda로 설치하는 것이 사용하기 편할 것입니다.
몇몇 패키지들은 특정 파이썬 버전에 dependent하기 때문에 확인 후 설치 바랍니다.

최초 1회 다양한 채널을 추가해놓고 바로 설치를 하는 방법

```shell
$ conda config --add channels conda-forge
$ conda config --add channels defaults
$ conda config --add channels r
$ conda config --add channels bioconda
```

```shell
$ conda install bowtie2
$ conda install bedtools
```

그때 그때 채널을 지정해놓고 설치를 하는 방법

```shell
$ conda install -c bioconda picard
```


conda로 설치할 수 있는 tool들 예시

```
$ conda install cutadapt
$ conda install meme
$ conda install fastqc
$ conda install fastx_toolkit
$ conda install bowtie
$ conda install bowtie2
$ conda install prinseq
$ conda install circos
$ conda install tophat
$ conda install cufflinks
$ conda install samtools
$ conda install bedtools
$ conda install sra-tools
$ conda install trimmomatic
$ conda install paralyzer
$ conda install star
$ conda install rsem
$ conda install bwa
$ conda install picard
$ conda install subread
$ conda install entrez-direct
$ conda install art
$ conda install macs2
$ conda install htseq
$ conda install rseqc
```


### 2-6. 가상환경 제거

```shell
$ conda remove --name [name] --all
```

---

## 3. Quick Start

```shell

# haeun이라는 python2 가상환경을 만든다.
$ conda create -n [haeun] python=2.7

# 가상환경을 실행한다.
$ conda activate haeun

#가상환경이 실행 됐다.
(haeun) $

# scipy 패키지를 설치한다.
(haeun) $ conda install scipy

# bioconda에 있는 bedtools 패키지를 설치한다.
(haeun) $ conda install -c bioconda bedtools

# 가상환경에서 빠져 나온다.
(haeun) $ conda deactivate

# 빠져나왔다.
$

# conda 버전에 따라 이렇게 보일 수도 있다.
(base) $

```
---
## 맺으며

위에 쓰여있는 내용 conda documentation을 참고하여 만들었습니다. 기본적인 조작법만 포함되어있으므로 conda에 대해 더 깊게 알고싶으신 분은 conda의 [User guide](https://conda.io/docs/user-guide/index.html)를 참고해주시길 바랍니다. 특히 두 섹션을 [Getting started with conda](https://conda.io/docs/user-guide/getting-started.html)와 [Managing environments](https://conda.io/docs/user-guide/tasks/manage-environments.html)를 추천드립니다. [여기](https://conda.io/docs/_downloads/conda-cheatsheet.pdf)를 누르시면 cheatsheet를 다운받으실 수 있습니다.

서로 서로 배려하는 슬기로운 서버생활 되시길 바랍니다~.~
