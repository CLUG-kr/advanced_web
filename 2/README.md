# EC2 접속 방법
## Mac, Linux 
```
$ ssh -i your_key.pem ubuntu@your_ipv4_address
```
## 접속이 되지않는다면 접속 key에 대한 권한을 변경
```
$ sudo chmod 400 your_key.pem
```
## Windows 
### Putty
https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html

# Linux 기본 명령어 
### pwd 
현재 작업중인 디렉토리 정보 출력 
```
$ pwd
/home/ubuntu
```
### cd 
경로 이동 
절대 경로와 상대 경로로 이동 가능하다. 
```
$ cd /home/ubuntu/mydir
$ pwd
/home/ubuntu/mydir


$ cd ..
$ pwd
/home/itholic
```
### ls 
디렉토리 목록 확인 

```
$ ls
testfile1  testfile2  testfile3


$ ls -l
total 0
-rw-r--r-- 1 itholic 197121 0 11월  6 22:08 testfile1
-rw-r--r-- 1 itholic 197121 0 11월  6 22:08 testfile2
-rw-r--r-- 1 itholic 197121 0 11월  6 22:08 testfile3


$ ls -a
./  ../  testfile1  testfile2  testfile3


$ ls -al
total 4
drwxr-xr-x 1 itholic 197121 0 11월  6 22:08 ./
drwxr-xr-x 1 itholic 197121 0 11월  6 22:08 ../
-rw-r--r-- 1 itholic 197121 0 11월  6 22:08 testfile1
-rw-r--r-- 1 itholic 197121 0 11월  6 22:08 testfile2
-rw-r--r-- 1 itholic 197121 0 11월  6 22:08 testfile3
```
### cp 
파일 혹은 디렉토리를 복사 
디렉토리를 복사할때는 -r 옵션을 주어야함 
```
$ ls
testdir/  testfile


$ cp testfile1 testfile_cp
$ ls
testdir/  testfile  testfile_cp


$ cp -r testdir testdir_cp
$ ls
testdir/  testdir_cp/  testfile  testfile_cp
```
### mv 
파일 혹은 디렉토리 이동 

실제로 원하는 위치로 이동할때도 사용하지만, 이름을 변경하는 용도로도 사용한다. 

cp와는 달리 디렉토리를 이동할때도 별다른 옵션이 필요 없다. 
```
$ ls
testdir/  testfile


$ mv testfile testfile_mv
$ ls
testdir/  testfile_mv


$ mv testfile_mv testdir/
$ ls
testdir/


$ ls testdir/
testfile
```
### mkdir 
디렉토리 생성

-p 옵션을 주면 하위 디렉토리까지 한 번에 생성 가능 

아래 예제중 ls -R 옵션은 디렉토리의 하위목록까지 전부 보여주는 옵션인데, 

내 경우 실제로 많이 사용하진 않아서 ls 명령어에서 따로 설명하진 않았다. 

mkdir -p 옵션 예제에서 실제로 하위디렉토리가 생성되었다는 것을 보여주기 위해 사용하였다. 
```
$ ls
testfile


$ mkdir testdir
$ ls
testdir/  testfile


$ mkdir -p a/b/c/d/e/
$ ls -R a/
a/:
b/

a/b:
c/

a/b/c:
d/

a/b/c/d:
e/

a/b/c/d/e:
```
### rm 
파일이나 디렉토리를 삭제 

디렉토리를 삭제할때는 r 옵션을 주어야 한다. 

-f 옵션을 주면 사용자에게 삭제 여부를 묻지 않고 바로 삭제한다. 

디렉토리를 삭제할 때에는 하위 디렉토리까지 모두 삭제되므로 유의하자. 
```
$ ls
testdir/  testfile1  testfile2


$ rm -f testfile1
$ ls
testdir/  testfile2


$ rm -rf testdir/
$ ls
testfile2
```
# 장고 프로젝트를 위한 가상환경 생성/실행

```
$ sudo apt-get update
$ sudo apt-get install virtualenv
$ virtualenv -p python3 venv
$ source venv/bin/actiavate
```

# 장고 프로젝트 생성/실행

```
$ pip install django==2.2.13
$ django-admin startproject test_proj
$ cd test_proj
$ python manage.py runserver
```
