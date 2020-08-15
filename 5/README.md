## 도메인 등록
- https://xn--220b31d95hq8o.xn--3e0b707e/ 회원가입 진행
- 인증서 발급 탭 클릭후 자신이 원하는 도메인 검색
### 도메인 등록 후 IP 와 Domain 연결됐는지 확인
```
$ssh -i your_key.pem ubuntu@your_domain
```

# ca 인증서 다운로드 후 crt 파일 생성
### scp를 통한 파일 서버 업로드
```
$ scp -i your_key.pem your_domain.zip ubuntu@your_domain:
```
### 웹 서버 crt 파일 생성
```
 $ ssh -i your_key.pem ubuntu@your_domain
 $ sudo apt-get update
 $ sudo apt-get install nginx
 $ sudo apt-get install unzip
 $ unzip your_domain.zip
 $ cat certificate.crt > your_doamin.crt
 $ cat private.key >> your_domain.crt     <하이픈 다섯개 중간에 띄워쓰기>
 $ cd /etc/nginx/sites-available


# Nginx
### Nginx 설치
```
$sudo apt-get install
```

### nginx config 파일 생성
```
$ sudo vi your_doamain
ssl_certificate /home/ubuntu/your_domain.crt;
ssl_certificate_key /home/ubuntu/private.key;
server {
        listen  80;
        server_name www.co-coding.kro.kr co-coding.kro.kr;
        return  301 https://$host$request_uri;
}
server {
        listen 443;
        server_name www.co-coding.kro.kr co-coding.kro.kr;
        ssl on;
        root /var/www/html;
        index index.html index.htm index.nginx-debian.html;
        server_name _;
        location / {
                try_files $uri $uri/ =404;
        }
}
 $ sudo nginx -t
 $ sudo systemctl restart nginx
 ```
  

## Nginx 설치
- sudo apt-get install nginx

## ca인증서

- 우리 public key >, >>
- 우리 상위 도메인에대한 퍼블릭키 (ca_bundle)  ->> concat
-nginx config

