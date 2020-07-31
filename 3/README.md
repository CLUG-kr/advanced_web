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
(venv) ~/test_proj$ gunicorn --bind 0.0.0.0:8000 test_proj.wsgi:application django-admin startproject test_proj  # 구니콘 프로세스 시작
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
        --bind 0.0.0.0:8000 \
        test_proj.wsgi:application

[Install]
WantedBy=multi-user.target
```
## Gunicorn 서비스 등록
```
$ sudo systemctl enable gunicorn  # 심볼릭링크 파일 생성
$ sudo systemctl start gunicorn  # 데몬 프로세스 시작
```
## Gunicorn 서비스 구동 확인
```
$ systemctl status gunicorn
● gunicorn.service - gunicorn daemon
   Loaded: loaded (/etc/systemd/system/gunicorn.service; enabled; vendor preset: enabled)
   Active: active (running) since Fri 2020-07-31 16:09:14 UTC; 5s ago
 Main PID: 14569 (gunicorn)
    Tasks: 4 (limit: 1121)
   CGroup: /system.slice/gunicorn.service
           ├─14569 /home/ubuntu/venv/bin/python3 /home/ubuntu/venv/bin/gunicorn --workers 3 --bind 0.0.0.0:8000 test_proj.wsgi:application
           ├─14596 /home/ubuntu/venv/bin/python3 /home/ubuntu/venv/bin/gunicorn --workers 3 --bind 0.0.0.0:8000 test_proj.wsgi:application
           ├─14597 /home/ubuntu/venv/bin/python3 /home/ubuntu/venv/bin/gunicorn --workers 3 --bind 0.0.0.0:8000 test_proj.wsgi:application
           └─14598 /home/ubuntu/venv/bin/python3 /home/ubuntu/venv/bin/gunicorn --workers 3 --bind 0.0.0.0:8000 test_proj.wsgi:application

Jul 31 16:09:14 ip-172-31-46-208 systemd[1]: Started gunicorn daemon.
Jul 31 16:09:15 ip-172-31-46-208 gunicorn[14569]: [2020-07-31 16:09:15 +0000] [14569] [INFO] Starting gunicorn 20.0.4
Jul 31 16:09:15 ip-172-31-46-208 gunicorn[14569]: [2020-07-31 16:09:15 +0000] [14569] [INFO] Listening at: http://0.0.0.0:8000 (14569)
Jul 31 16:09:15 ip-172-31-46-208 gunicorn[14569]: [2020-07-31 16:09:15 +0000] [14569] [INFO] Using worker: sync
Jul 31 16:09:15 ip-172-31-46-208 gunicorn[14569]: [2020-07-31 16:09:15 +0000] [14596] [INFO] Booting worker with pid: 14596
Jul 31 16:09:15 ip-172-31-46-208 gunicorn[14569]: [2020-07-31 16:09:15 +0000] [14597] [INFO] Booting worker with pid: 14597
Jul 31 16:09:15 ip-172-31-46-208 gunicorn[14569]: [2020-07-31 16:09:15 +0000] [14598] [INFO] Booting worker with pid: 14598
$ sudo ps -ef | grep gunicorn
ubuntu   14569     1  0 16:09 ?        00:00:00 /home/ubuntu/venv/bin/python3 /home/ubuntu/venv/bin/gunicorn --workers 3 --bind 0.0.0.0:8000 test_proj.wsgi:application
ubuntu   14596 14569  0 16:09 ?        00:00:00 /home/ubuntu/venv/bin/python3 /home/ubuntu/venv/bin/gunicorn --workers 3 --bind 0.0.0.0:8000 test_proj.wsgi:application
ubuntu   14597 14569  0 16:09 ?        00:00:00 /home/ubuntu/venv/bin/python3 /home/ubuntu/venv/bin/gunicorn --workers 3 --bind 0.0.0.0:8000 test_proj.wsgi:application
ubuntu   14598 14569  0 16:09 ?        00:00:00 /home/ubuntu/venv/bin/python3 /home/ubuntu/venv/bin/gunicorn --workers 3 --bind 0.0.0.0:8000 test_proj.wsgi:application
ubuntu   14609  1700  0 16:10 pts/0    00:00:00 grep --color=auto gunicorn

```
만약 구동 실패시 에러 로그는 /var/log/syslog에서 확인할 수 있다. 
