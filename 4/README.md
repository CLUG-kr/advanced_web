# 가상환경 & 장고 프로젝트 생성
```
$ cd  # 홈 디렉토리로 이동
$ sudo apt-get update  # 패키지 리스트 업데이트
$ sudo apt-get install virtualenv  # virtualenv 설치
$ virtualenv -p python3 venv  # 가상환경 생성
$ source venv/bin/activate  # 가상환경 실행
(venv)$ pip install django==2.2.14  # 가상환경에 장고 설치
(venv)$ django-admin startproject test_proj  # 테스트 프로젝트 생성
```

# gunicorn 실행해보기
```
(venv)$ cd  # 홈 디렉토리로 이동
(venv)$ pip install gunicorn  # 가상환경에 구니콘 설치
(venv)$ cd test_proj  # 프로젝트 디렉토리 들어가기
(venv) ~/test_proj$ gunicorn --bind 0.0.0.0:8000 test_proj.wsgi:applicationj  # 구니콘 프로세스 시작
```
실행후에 브라우저에서 접속이 안되면 보안그룹 확인!!
Not Allowed hosts 에러 발생시에 settings.py 확인 

# systemd 사용해보기 
## 서비스 등록 스크립트 생성
`/etc/systemd/system/gunicorn.service` 파일을 아래와 같은 내용으로 생성한다.
```
[Unit]
Description=gunicorn daemon
After=network.target

[Service]
User=ubuntu
Group=www-data
WorkingDirectory=/home/ubuntu/test_proj
ExecStart=/home/ubuntu/venv/bin/gunicorn \
        --workers 3 \
        --bind unix:/home/ubuntu/gunicorn.sock \
        test_proj.wsgi:application

[Install]
WantedBy=multi-user.target
```
## Gunicorn 서비스 등록
```
$ sudo systemctl enable gunicorn  # 심볼릭링크 파일 생성
$ sudo systemctl start gunicorn  # 데몬 프로세스 시작
```

# Nginx
## Nginx 설치하기
```
$ sudo apt-get install nginx
```
apt를 사용하여 설치하고 브라우저에 접속해서 디폴트 메시지 확인
## Nginx에 gunicorn 리버스 프록시 컨피그 추가

`/etc/nginx/sites-available/gunicorn` 파일 생성
```
server {
        listen 80;
        server_name YOUR_IP;
        
        location / {
                include proxy_params;
                proxy_pass http://unix:/home/ubuntu/gunicorn.sock;
        }
}
```
## 사이트 추가
```
sudo ln -s /etc/nginx/sites-available/gunicorn /etc/nginx/sites-enabled
```
## Nginx 문법 검사 & Nginx 데몬 재시작
```
sudo nginx -t
sudo systemctl restart nginx
```
