# Mac 설치

날짜: 2021년 1월 18일 → 2021년 1월 24일
번호: 2
태그: Mac, 라라벨, 콤포저

### 1. Composer 설치

```bash
# 1단계 install
## 방법1. brew로 설치
## 이방법이 간단하고 버전관리도 잘해줘서 이걸로 설치했다
brew install composer

## 방법2. script로 설치 
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === '756890a4488ce9024fc62c56153228907f1545c228516cbf63f885e036d37e9a59d27d63f46af1d4d07ee0f76181c7d3') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"

# 2단계 PATH 설정
# oh my zsh설치 시 .zshrc에 PATH 설정(나의 경우 oh my zsh 설치했고 .zshrc에 설정했다.
# 설치 안 했을 때 .bash_profile에 PATH 설정

# /Users/YOUR-PATH/.zshrc를 vi로 연다
cd ~
vi .zshrc

# composer 환경설정 경로인 /Users/YOUR-PATH/.composer/vendor/bin을 입력한다.
export PATH="$PATH:$HOME/.composer/vendor/bin"
source .zshrc

# composer 버전 확인
composer -V 

# composer 설치완료
```

### 2. php 설치

```bash
# 1단계 install
# composer와 마찬가지로 brew로 설치했다.
brew install php

# 2단계 PATH 설정
# PATH를 설정하지 않았을때, osx에서는 기본 설정이 되있는데 이것을 이용하면 composer, valet 설정에서 문제가 생겼다.
# 아래 이미지와 같은 문제가 발생했다.

# /Users/YOUR-PATH/.zshrc를 vi로 연다
cd ~
vi .zshrc

# brew에서 설치한 php 설치경로 /opt/homebrew/Cellar/php/8.0.1_1/bin를 입력한다.
export PATH="/opt/homebrew/Cellar/php/8.0.1_1/bin:$PATH"
# zshrc에서 변경한 정보를 적용한다.
source .zshrc

# php 버전확인
php -v

# php 설치완료
```

2단계에서 일어난 에러 이미지

![Mac%20%E1%84%89%E1%85%A5%E1%86%AF%E1%84%8E%E1%85%B5%2061aa1da61ead4434889d516a9a2ddcf6/_2021-01-20__11.04.09.png](Mac%20%E1%84%89%E1%85%A5%E1%86%AF%E1%84%8E%E1%85%B5%2061aa1da61ead4434889d516a9a2ddcf6/_2021-01-20__11.04.09.png)

### 3. Laravel 설치

```bash
# 1단계 laravel프로젝트 create
# 설치한 composer create-project명령어로 laravel project를 생성한다.
composer create-project laravel/laravel blog

cd blog

# php artisan명령어로 server를 실행
# 127.0.0.1:8000을 브라우저에서 실행하면 해당 프로젝트를 실행한다.
php artisan serve

# Global Composer 종속성으로 설치
# 이 명령어를 실행하면 laravel new example-app과 같은 laravel명령어를 실행할 수 있다
composer global require laravel/installer
# composer create-project laravel/laravel blog = laravel new blog가 실행된다
laravel new blog
```

### 4. valet 설치

```bash
# 필자는 이부분이 가장 힘들었다 어떤문제가 있는지 밖으로 잘 안보여줘서 처음 설치해보는 입장에선 혼란의 도가니였다.
# composer로 valet정보를 다운로드..? -> valet install -> ~/.composer/vendor/laravel/valet/에 valet을 설치한다.
composer global require laravel/valet
valet install

# valet으로 php버전을 변경할 수 있다
valet use php

# 아래의 명령어들은 응용프로그램을 포함하는 컴퓨터의 디렉토리를 nginx에 포함한다
# park -> 명령어를 실행한 폴더의 하위 프로젝트는 모두 .test로 사이트를 띄울 수 있다 ex) http://blog.test
cd project 상위폴더
valet park

# project별로 symbolic link를 만들고 싶을때 사용한다. 사용시 ~/.config/valet/Sites/ 아래에 symbolic link를 생성한다. 
cd project
valet link

# 그 외 valet 명령어
## valet을 시작, 재시작, 종료한다.
valet start, restart, stop

## valet park를 실행한 폴더를 조회한다.
valet parked 

## valet link로 생성한 link list를 보여준다.
valet links

## valet park를 실행한 폴더정보를 삭제한다.
valet forget

## valet link로 생성한 link를 삭제한다.
valet unlink

## 프로젝트를 TLS적용 시킨다.
valet secure [app-name]

## 프로젝트 TLS적용 해제한다.
valet unsecure [app-name]
```