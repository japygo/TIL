# 10장 새로운 MVC 프레임워크 구현을 통한 점진적 개선

## 10.1 MVC 프레임워크 요구사항 3단계

### 10.1.1 요구사항

클래스패스로 설정 되어 있는 클래스 중에 `@Controller` 애노테이션이 설정되어 있는 클래스를 찾기 위한 목적으로
[reflections](https://github.com/ronmamo/reflections) 라이브러리를 활용할 수 있다.
이 라이브러리를 활용해 `@Controller` 애노테이션이 설정되어 있는 클래스를 찾은 후 `@RequestMapping` 설정에 따라 요청 URL과 메소드를 연결하도록 구현할 수 있다.

### 10.1.2 자바 리플렉션

#### 10.1.2.1 자바 리플렉션 API 활용해 클래스 정보 출력하기

자바 리플렉션을 활용해 Question 클래스의 모든 필드, 생성자, 메소드 정보를 출력한다.

#### 10.1.2.2 "test"로 시작하는 메소드 실행하기

#### 10.1.2.3 @MyTest 애노테이션으로 설정된 메소드 실행하기

#### 10.1.2.4 생성자가 있는 클래스의 인스턴스 생성하기

#### 10.1.2.5 private 필드에 접근하기

## 10.2 MVC 프레임워크 구현 3단계

### 10.2.1 @Controller 애노테이션 설정 클래스 스캔

### 10.2.2 @RequestMapping 애노테이션 설정을 활용한 매핑

### 10.2.3 클라이언트 요청에 해당하는 HandlerExecution 반환

### 10.2.4 DispatcherServlet과 AnnotationHandlerMapping 통합

## 10.3 인터페이스가 다른 경우 확장성 있는 설계

## 10.4 배포 자동화를 위한 쉘 스크립트 개선

### 10.4.4 동영상을 통한 배포/원복

- https://youtu.be/UqocnEIX-mA: 심볼릭 링크를 활용해 원복이 가능한 구조를 설계하고 이를 쉘 스크립트를 구현하는 과정

```shell
#!/bin/bash

REPOSITORIES_DIR=~/repositories/jwp-basic
TOMCAT_HOME=~/tomcat
RELEASE_DIR=~/releases/jwp-basic

cd $REPOSITORIES_DIR
pwd
git pull
mvn clean package

C_TIME=$(date +%s) 

mv $REPOSITORIES_DIR/target/jwp-basic $RELEASE_DIR/$C_TIME
echo "deploy source $RELEASE_DIR/$C_TIME directory"

$TOMCAT_HOME/bin/shutdown.sh

rm -rf $TOMCAT_HOME/webapps/ROOT
ln -s $RELEASE_DIR/$C_TIME $TOMCAT_HOME/webapps/ROOT

$TOMCAT_HOME/bin/startup.sh

tail -500f $TOMCAT_HOME/logs/catalina.out
```

- https://youtu.be/7OSzN16FqCw: 서비스 배포 후 장애가 발생하는 경우 이전 버전으로 원복하는 스크립트를 작성하는 과정

```shell
#!/bin/bash

RELEASES_DIR=~/releases/jwp-basic
TOMCAT_HOME=~/tomcat

RELEASES=$(ls -1tr $RELEASES_DIR)
echo "releases : $RELEASES"
REVISIONS=(${RELEASES//\n/})

if [ "${#REVISIONS[@]}" -lt 2 ]; then
  echo "release source length more than 2"
else
  echo "rollback directory : ${REVISIONS[1]}"

  $TOMCAT_HOME/bin/shutdown.sh

  rm -rf $TOMCAT_HOME/webapps/ROOT
  ln -s $RELEASES_DIR/${REVISIONS[1]} $TOMCAT_HOME/webapps/ROOT

  $TOMCAT_HOME/bin/startup.sh

  tail -500f $TOMCAT_HOME/logs/catalina.out
fi
```
