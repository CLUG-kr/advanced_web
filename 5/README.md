## 도메인 등록
- https://xn--220b31d95hq8o.xn--3e0b707e/ 회원가입 진행
- 인증서 발급 탭 클릭후 자신이 원하는 도메인 검색
- 
- cname


## ca 인증서 다운로드 후
```
 $ scp -i your_key.pem your_domain.zip ubuntu@your_domain:
 $ ssh -i your_key.pem ubuntu@your_domain
 $ sudo apt-get update
 $ sudo apt-get install nginx
 $ sudo apt-get install unzip
 $ unzip your_domain.zip
 $ cat certificate.crt > your_doamin.crt
 $ cat private.key >> your_domain.crt     <하이픈 다섯개 중간에 띄워쓰기 끼륵끼륵>
 $ cd /etc/nginx/sites-available
 $ sudo vi your_doamain
 $ sudo nginx -t
 $ sudo systemctl restart nginx
 ```
 
 # nginx config
 ```
 
 ```
 
  

## Nginx 설치
- sudo apt-get install nginx

## ca인증서

- 우리 public key >, >>
- 우리 상위 도메인에대한 퍼블릭키 (ca_bundle)  ->> concat
-nginx config

