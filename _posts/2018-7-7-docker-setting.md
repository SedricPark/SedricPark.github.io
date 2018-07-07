---
categories: docker
---

### Docker 정리

#### 도커 명령어 정리(zsh에서 진행)

* docker 이미지 다운로드 받기

```python
# docker hub에서 이미지를 다운로드
docker pull ubuntu:16.04 
    
# shell에서 docker hub 이미지 검색
docker search ubuntu # docker search <이미지 이름> 

# 다운로드 받은 이미지 확인하기
docker images

# 이미지 삭제 하기
docker rmi ubuntu:16.04 # 이미지 이름이나 ID
    
# 모든 이미지 삭제 하기
docker rmi $(docker images -q)
```

* docker 이미지로 컨테이너 실행하기

```python
# 다운로드 받은 이미지를 가지고 컨테이너 실행
# 만약 이미지를 다운로드 받지 않았으면 자동으로 다운로드
# 옵션 -rm 도커 프로세스 종료후 컨테이너 삭제
# 옵션 -i, -t 실행된 bash에 입력 및 출력
# 옵변 --name은 실행될 컨테이너 이름을 지정
# /bin/bash 컨테이서 실행시 쉘 지정
docker run -rm -i -t --name ubuntu_01 ubuntu:16.04 /bin/bash
```

* docker 컨테이너 빠져 나오기 및 다시 들어가기

```python
# 컨테이서 실행 후 컨테이너 빠져 나오기
exit # 컨테이너 빠져 나오면서 컨테이너도 종료 및 삭제 됨

# 컨테이너 종료 시키지 않고 빠져나오고
단축키 : control + p + q 를 차례대로 누름

# 컨테이너로 다시 들어가기
# bash로 접속되며 엔터를 한번더 눌러야 들어 가진다
docker attach ubuntu_01 # --name에서 정한 컨테이너 이름. ID도 가능함
    
# 컨테이너 다시 들어가기
# 나중에 zsh을 설치 후에는 docker exec -it ubuntu_01 /usr/bin/zsh
docker exec -it ubuntu_01 /bin/bash # ubuntu_01은 --name에서 지정한 이름
```

* docker 컨테이너 관련 중요 명령어
  * 아래에서 컨테이너 이름말고 CONTAINER ID를 입력해도 됨

```python
# 실행중인 컨테이너 목록보기
docker ps

# 종료된 컨테이너 목록까지 보기
docker ps -a

# 특정 컨테이너 실행 시키기
docker start ubuntu_01 # --name에서 지정한 컨테이너 이름

# 특정 컨테이너 재시작 시키기
docker restart ubuntu_01 # --name에서 지정한 컨테이너 이름

# 특정 컨테이너 중지 시키기
docker stop ubuntu_01 # --name에서 지정한 컨테이너 이름

# 모든 컨테이너 중지 시키기
docker stop $(docker ps -a -q)

# 특정 컨테이너 삭제 하기
docker rm ubuntu_01 # 또는 CONTAINER ID

# 모든 컨테이너 삭제 하기
docker rm $(docker ps -a -q)
```



#### 기본 ubuntu:16.04에서 zsh, pyenv 설치, 파이썬 특정버전 설치까지 

> 맥 OS에서 zsh로 작업을 하였으며, 순서대로 해야 오류가 적었음

* 우분투 이미지로 컨테이너 실행
  * `docker run -i -t --name ubuntu_01 ubuntu:16.04  /bin/bash`
* 우분투 업데이트 실행
  * `apt-get update`
* pip 설치 하기 (pip 버전에 낮으면 업그레이드)
  * `apt-get install python-pip`
* vi 에디터와 git 설치
  * `apt-get install git vim`
* pyenv 설치 환경(의존성) 패키지 설치
  * 참고 : https://github.com/pyenv/pyenv/wiki/common-build-problems

```
apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev \
libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev xz-utils
```

* pyenv 설치하기
  * 설치하면 맨 마지막에 환경변수 추가해주어야 할 내용이 나옴

```
curl -L https://raw.githubusercontent.com/yyuu/pyenv-installer/master/bin/pyenv-installer | bash
```

* bash 환경변수 설정해주기

```python
# ~/.bash_profile 파일 설정 하기
vi ~/.bash_profile 

# 아래 내용을 작성하고 종료하기 / 경로는 환경에 따라 모두 다를 수 있음
export PATH="/root/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"

# bash_profile 마운트
source ~/.bash_profile
```

* pyenv에 특정 파이썬 버전 설치하기
  * `pyenv install 3.5.2`
* zsh 및 oh-my-zsh 설치하기

```python
# apt-get으로 zsh 설치하기
apt-get install zsh

# oh-my-zsh 설치하기
wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | zsh
```

* bash에서 zsh로 변경 하기
  * `chsh -s /usr/bin/zsh` : root 권한을 요청 할수도 있음
* zsh에 pyenv 환경변수 추가하기

```python
# ~/.zshrc 파일을 열어서 환경 변수 추가하기. 위에 bash 환경변수와 동일
vi ~/.zshrc

# 아래 내용 추가
export PATH="/root/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"

# 추가한 환경변수 적용 시키기
source ~/.zshrc
```

* 파이썬 가상환경 세팅하기

```python
# 메인으로 사용할 파이썬 버전을 선택해주기
pyenv version 
pyenv versions
pyenv global 3.5.2 # 파이썬 버전은 자기가 위에서 설치한 파이썬 버전중에 하나 선택

# 루트디렉토리에서 /srv 디렉토리로 이동해서 폴더 하나 생성
mkdir first_env

# 파이썬 가상환경 하나 생성하기
pyenv virtualenv 3.5.2 my_env # 3.5.2는 버전, my_env는 가상환경 별칭

# /srv 디렉토리에서 생성한 폴더로 이동
cd first_env

# first_env 폴더로 이동후, 이 폴더에 적용될 파이썬 가상환경 적용시키기
pyenv local my_env
```

#### Docker 제작한 컨테이너로 이미지 만들기

* 현재의 도커가 기본 이미지에서 어떻게 변경되었는지 확인
  * `docker diff ubuntu_01` : <ubuntu_01>는 컨테이너 이름
  * A: 추가된 파일, C: 변경된 파일, D:삭제된 파일 
* 선택한(생성한) 컨테이너로 이미지 만들기

```python
# 기본명령어 : docker commit <옵션> <컨테이너 이름> <이미지 이름>:<태그>
# 컨테이너 이름대신 ID를 작성해도 됨
# 옵션 -a 이미지를 작성한 작성자 이름 지정
# 옵션 -m 이미지 생성에 대한 메시지 작성

docker commit -a "imad<imad@king.com" -m "파이썬기본" ubuntu_01 pyubun:0.1
```

* 생성된 이미지로 컨테이너를 새로 만들기

```python
docker run -i -t --name pyubuntu pyubun:0.1 /usr/bin/zsh
```