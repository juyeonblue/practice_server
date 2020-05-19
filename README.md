## aws   
### Elastic Beanstalk   
###### Elastic Beanstalk을 활용하여 배포를 해보았어요^____^   

<br/>   

#### ⚒ 환경 설정 ⚒   

<hr/>   

###### 환경 설정이 이렇게,,, 이렇게 힘들 줄이야ㅜ   
###### 환경 설정을 하기 위해선 아래와 같은 복잡한 과정을 거쳐야 한다.   
##### AWS의 IAM에서 유저를 생성했다는 가정하에 아래와 같은 과정을 진행한다.   

```   
$ pip3 install awsebcli --upgrade --user     
```

##### 위와 같이 명령어 입력 후, 제대로 설치되었는지 확인하기   
```   
$ eb --version   
```   
<img width="500" alt="eb_version" src="https://user-images.githubusercontent.com/55732968/82357765-85b46d00-9a40-11ea-9fa8-72175a97a8a7.png">   

##### 위와 같은 버전이면 성공!!!   

<br/>   

#### django 버전 주의하기!   
```   
$ python3
<b-m venv myvenv
$ source myvenv/bin/activate
$ python3 -m pip install --upgrade pip
$ pip install django==2.1.1
$ django-admin startproject firstproject   
$ cd firstproject   
$ python3 manage.py startapp firstapp   
$ python3 manage.py migrate
```   

<br/>   

#### .gitignore파일 추가하기!   
```python   

# Created by https://www.gitignore.io/api/django
# Edit at https://www.gitignore.io/?templates=django

### Django ###
*.log
*.pot
*.pyc
__pycache__/
local_settings.py
db.sqlite3
media

# Elastic Beanstalk Files
.elasticbeanstalk/*
!.elasticbeanstalk/*.cfg.yml
!.elasticbeanstalk/*.global.yml
```
###### !.elasticbeanstalk/*.cfg.yml -> 이 파일은 절대 무시하면 안된다는 것.   
<br/>   

#### requirements.txt 파일과 .ebextensions 폴더 생성하기!   
```   
$ pip freeze > requirements.txt   
$ mkdir .ebextensions   
```   
<img width="169" alt="file" src="https://user-images.githubusercontent.com/55732968/82360026-b518a900-9a43-11ea-9749-42c4d4648434.png">   

###### ⚡️파일 위치 매우매우 중요‼️   

##### .ebextensions 폴더 내에 django.config라는 구성 파일을 추가한다.   
##### django.config 파일에 아래와 같이 적어준다.   
```   
option_settings:
    aws:elasticbeanstalk:container:python:
        WSGIPath: firstapp(앱 이름)/wsgi.py
```   
<br/>   

#### 📍배포 전 git에 push하기   

<br/>   

##### push후, 가상환경 끄기   
```python   
$ deactivate   

### eb init을 사용하여 초기화 과정 진행   
$ eb init -p python-3.6 first(어플리케이션 이름 아무거나)   

### IAM credential CSV파일에 있는 id랑 key를 입력해줍니다!   
### "Application first has been created." 화면에 뜨면 어플리게이션이 잘 생성되었다는 것   

$ eb create first(환경 이름 아무거나)   
### 어플리케이션 이름과 동일해도 상관없다.   
### "2020-05-19 16:18:24    INFO    Successfully launched environment: first" 환경이 잘 생성됨.   
```   

##### CName 확인하기   
```   
$ eb status   
```   
##### CName을 복사해서 settings.py의 ALLOWED HOST에 붙여넣기   
```python   
ALLOWED_HOSTS = ['여기에 CName']   
```   

<br/>   

#### 📍배포 전 git에 push하기   

<br/>   

#### 배포하기   
```python   
$ eb deploy   
### "2020-05-19 16:21:02    INFO    Environment update completed successfully." 환경이 잘 생성됨.   
```   

```   
$ eb open   
```   

<img width="400" alt="eb_open" src="https://user-images.githubusercontent.com/55732968/82362845-f0b57200-9a47-11ea-8c13-bde89a167e4d.png">   

##### 화면에 띄울 파일들을 생성하지 않았기 때문에 아무것도 보이지 않는다ㅎㅅㅎ   
   
<hr/>      

### 🔶 위의 과정을 거친 후, 본격적으로 화면에 띄울 파일들을 생성한다면?!   
   
##### 가상환경을 끈 상태에서 진행한다.   
##### pip을 사용하는 경우라면 가상환경 켠 후 진행 (다시 꼭 끄기!!)   

#### 📍배포 전 git에 push하기   

##### 아래 명령어 입력!   
```   
$ eb deploy   
$ eb open   
```




