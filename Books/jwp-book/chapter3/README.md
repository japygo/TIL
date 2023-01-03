# 3장 개발 환경 구축 및 웹 서버 실습 요구사항

## 3.2 로컬 개발 환경 구축
- [3 ~ 6장 실습에서 사용할 저장소](https://github.com/slipp/web-application-server)
- [6 ~ 12장 실습에서 사용할 저장소](https://github.com/slipp/jwp-basic)

## 3.3 원격 서버에 배포
- [리눅스 커맨드라인 완벽 입문서](http://www.yes24.com/Product/Goods/8208026)(윌리엄 E. 샤츠 주니어 저/이종우, 정영신 역, 비제이퍼블릭/2013년 1월)
- AWS에 대한 회원가입, 우분투 운영체제 설치, SSH를 통한 접근은 https://opentutorials.org/module/1946 문서를 참고해 진행할 수 있다.

### 3.3.1 요구사항
- 로컬 개발 환경에 설치한 HTTP 웹 서버를 물리적으로 떨어져 있는 원격 서버에 배포해 정상적으로 동작하는지 테스트한다.
- HTTP 웹 서버 배포 작업은 root 계정이 아닌 배포를 담당할 새로운 계정을 만들어 진행한다.

1. 도커로 우분투 실행
   ```shell
   docker -i -t ubuntu
   ```

2. jwp 사용자 계정 생성
   ```shell
   adduser jwp
   ```

3. 사용자 계정에 sudo 권한 주기
   - sudo 설치
   ```shell
   apt-get update
   apt-get install sudo
   ```
   - vim 설치
   ```shell
   apt-get install vim
   vi ~/.vimrc
   ```
   ```text
   set autoindent
   set smartindent
   set cindent
   set number
   set showmatch
   set tabstop=3
   set shiftwidth=3
   syntax enable
   syntax on
   ```
   - sudo 권한 주기
   ```shell
   visudo -f /etc/sudoers
   ```
   ```text
   # User privilege specification
   jwp  ALL=(ALL:ALL) ALL
   ```

4. java 설치
   ```shell
   apt-get install openjdk-8-jdk
   
   java -version
   javac -version
   
   vi ~/.bashrc
   ```
   ```text
   export JAVA_HOME=$(dirname $(dirname $(dirname $(readlink -f $(which java)))))
   export PATH=$PATH:$JAVA_HOME/bin
   ```
   
5. maven 설치
   ```shell
   apt-get install maven
   
   mvn -version
   ```
   
6. git 설치
   ```shell
   apt-get install git
   
   git --version
   ```
   
7. git clone
   ```shell
   git clone https://github.com/japygo/web-application-server.git
   ```
   
8. 빌드 후 실행
   ```shell
   cd web-application-server
   mvn clean package
   java -classpath target/classes:target/dependency/* webserver.WebServer &
   ```

9. curl 설치
   ```shell
   apt-get install curl
   
   curl http://localhost:8080
   ```
   
10. 외부에서 접속하기
   ```shell
   docker commit [컨테이너명] [새로 만들 이미지명]:[태그]
   docker run -it -p 8080:8080 [새로 만든 이미지명]:[태그]
   ```