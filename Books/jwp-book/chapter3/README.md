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

### 3.3.4 리눅스, 터미널과 친해지기
- [리눅스 커맨드라인 완벽 입문서](http://www.yes24.com/Product/Goods/8208026)(윌리엄 E. 샤츠 주니어 저/이종우, 정영신 역, 비제이퍼블릭/2013년 1월)
- [영어버전 무료 pdf](http://linuxcommand.org/tlcl.php)
- https://youtu.be/JbH-xzD7IkE
- [[특강] 크롬을 활용한 프론트엔드 디버깅](https://school.programmers.co.kr/learn/courses/7/7-%ED%8A%B9%EA%B0%95-%ED%81%AC%EB%A1%AC%EC%9D%84-%ED%99%9C%EC%9A%A9%ED%95%9C-%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C-%EB%94%94%EB%B2%84%EA%B9%85) : 넥스트에서 웹 UI 전공을 담당했던 윤지수님의 강의

## 3.4 웹 서버 실습

### 3.4.3 실습 요구사항

#### 3.4.3.1 요구사항 1 - index.html 응답하기
- http://localhost:8080/index.html 로 접속했을 때 webapp 디렉토리의 index.html 파일을 읽어 클라이언트에 응답한다.

#### 3.4.3.2 요구사항 2 - GET 방식으로 회원가입하기
- "회원가입" 메뉴를 클릭하면 http://localhost:8080/user/form.html 으로 이동하면서 회원가입 할 수 있다.
- 회원가입을 하면 다음과 같은 형태로 사용자가 입력한 값이 서버에 전달된다.  
/user/create?userId=javajigi&password=password&name=JaeSung&email=javajigi%40slipp.net

#### 3.4.3.3 요구사항 3 - POST 방식으로 회원가입하기
- http://localhost:8080/user/form.html 파일의 form 태그 method를 get에서 post로 수정한 후 회원가입이 정상적으로 동작하도록 구현한다.

#### 3.4.3.4 요구사항 4 - 302 status code 적용
- "회원가입"을 완료하면 /index.html 페이지로 이동하고 싶다.

#### 3.4.3.5 요구사항 5 - 로그인하기
- "로그인" 메뉴를 클릭하면 http://localhost:8080/user/login.html 으로 이동하고 로그인이 성공하면 /index.html로 이동하고, 로그인이 실패하면 /user/login_failed.html로 이동해야 한다.
- 로그인이 성공할 경우 요청 헤더의 Cookie 헤더 값이 logined=true, 로그인이 실패하면 Cookie 헤더 값이 logined=false로 전달되어야 한다.

#### 3.4.3.6 요구사항 6 - 사용자 목록 출력
- 접근하고 있는 사용자가 "로그인" 상태일 경우 http://localhost:8080/user/list 로 접근했을 때 사용자 목록을 출력한다.
- 만약 로그인하지 않은 상태라면 로그인 페이지(login.html)로 이동한다.

#### 3.4.3.7 요구사항 7 - css 지원하기

## 3.5 추가 학습 자료

### 3.5.1 Git과 GitHub

> https://github.com/honux77/practice/wiki/learngit 문서를 기반으로 작성한 내용이다.

#### 3.5.1.1 Git 학습을 시작하는 개발자에게 추천
- http://backlogtool.com/git-guide/kr: "누구나 쉽게 이해할 수 있는 Git 입문"
- http://rogerdudler.github.io/git-guide/index.ko.html: Git 설치, 기본 사용법에 대한 간편 안내서
- http://www.slideshare.net/ibare/dvcs-git: Git의 commit과 push의 개념잡기

#### 3.5.1.2 조금 익숙해졌을 때
- https://www.atlassian.com/git/tutorials

#### 3.5.1.3 직접 해보는 실습
- https://try.github.io/levels/1/challenges/1: Git 15분만에 배우는 실습
- http://pcottle.github.io/learnGitBranching: 브랜치 rebase 등을 배우는 실습

#### 3.5.1.4 동영상 강의
- http://opentutorials.org/course/1492: 생활 코딩 Git 강좌

#### 3.5.1.5 무료 이북
- http://dogfeet.github.io/articles/2012/progit.html

#### 3.5.1.6 추천하는 Git GUI 도구
- http://www.sourcetreeapp.com: Mac, 윈도우 모두에서 사용할 수 있는 GUI 도구

### 3.5.2 빌드 도구 메이븐
- https://slipp.net/wiki/pages/viewpage.action?pageId=10420233: "자바 세상의 빌드를 이끄는 메이븐" 책의 6장까지 공개하고 있다.
- http://youtu.be/Eg1Ebl_KNFg: 빌드 도구에 대한 초간단 설명, 이클립스에서 메이븐 디렉토리 구조의 프로젝트 생성, JUnit 라이브러리에 대한 의존성 추가, 메이븐 의존성 전이에 대해 설명한다.
- http://youtu.be/A8h1y-qXCbU: 이클립스 effective pom 탭을 통해 메이븐 부모 pom 설명, 메이븐 기본 명령어인 compile/test/package 페이즈(phase) 설명, 이클립스에서 메이븐 명령 실행을 다룬다.
- http://youtu.be/58yiJQU0xEY: 메이븐의 페이즈(phase)와 골(goal)과의 관계 설명, compiler 플러그인과 eclipse 플러그인 재정의 및 빌드, 이클립스에서 메이븐 골 실행 방법을 설명한다. 개발하고 있는 프로젝트의 디렉토리 구조를 변경하지 않으면서 메이븐을 적용할 수 있다. 메이븐을 적용할 경우 Github에 공유하던 많은 소스 코드를 공유하지 않아도 된다. 특히 이클립스 관련 설정과 jar 라이브러리를 공유하지 않아도 되는 것은 큰 장점이다.
- http://youtu.be/ovpVzUaQtSM: 메이븐이 적용되어 있지 않은 프로젝트에 메이븐을 적용하는 과정,  Github에서 jar 파일을 버전 관리하지 않도록 설정하는 과정을 다룬다.
- http://kwonnam.pe.kr/wiki/gradle: 그래들에 대한 학습


To be continued...