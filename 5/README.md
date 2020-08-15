# 도메인 등록
- https://xn--220b31d95hq8o.xn--3e0b707e/ 회원가입 진행
- 인증서 발급 탭 클릭후 자신이 원하는 도메인 검색
### 도메인 등록 후 IP 와 Domain 연결됐는지 확인
```
$ssh -i YOUR_KEY.pem ubuntu@YOUR_DOMAIN
```

# ca 인증서 다운로드 후 crt 파일 생성
### scp를 통한 파일 서버 업로드
```
$ scp -i YOUR_KEY.pem YOUR_DOMAIN.zip ubuntu@YOUR_DOMAIN:
```
### 웹 서버 crt 파일 생성
```
 $ ssh -i YOUR_KEY.pem ubuntu@YOUR_DOMAIN
 $ sudo apt-get update
 $ sudo apt-get install unzip
 $ unzip YOUR_DOMAIN.zip
 $ cat certificate.crt > YOUR_DOMAIN.crt
 $ cat private.key >> YOUR_DOMAIN.crt     <하이픈 다섯개 중간에 줄바꿈>
```

# Nginx
### Nginx 설치
```
$sudo apt-get install nginx
```

### Nginx config 파일 생성
```
$ cd /etc/nginx/sites-available
# sudo vi your_damain
```
### Nginx config
```
ssl_certificate /home/ubuntu/your_domain.crt;
ssl_certificate_key /home/ubuntu/private.key;
server {
        listen  80;
        server_name www.YOUR_DOMAIN YOUR_DOMAIN;
        return  301 https://$host$request_uri;
}
server {
        listen 443;
        server_name www.YOUR_DOMAIN YOUR_DOMAIN;
        ssl on;
        root /var/www/html;
        index index.html index.htm index.nginx-debian.html;
        server_name _;
        location / {
                try_files $uri $uri/ =404;
        }
}
 ```
 ### 사이트 추가
 ```
 $ sudo ln -s /etc/nginx/sites-available/gunicorn /etc/nginx/sites-enabled
 ```
 
 ### Nginx 문법 검사 및 데몬 재시작
 ```
 $ sudo nginx -t
 $ sudo nginx systemctl restart nginx
 $ sudo nginx systemctl status nginx
 ```
 
