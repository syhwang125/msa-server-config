# msa-server-config
  msa service configuration files repository

# eclipse maven project 생성 (parent - child )
1) parent 프로젝트를 만들기 위해 먼저 packaging 을 pom 형태로 생성 
2) maven module 생성하고 parent 를 1)에서 만든 프로젝트를 선택 (여러개 생성)
3) 1)번의 pom 파일에 module 추가됨 

# github 
  msa config 파일을 저장하기 위한 repository 생성 

# 프로젝트 
  1) spring-boot application 
  2) 각 프로젝트에서 /src/main/resources/application.yml 파일 생성 

# 실행하기 
   MINGW64:/t/eclipseworkspace4mes/sboot-servers/sboot-config-server
   MINGW64:/t/eclipseworkspace4mes/sboot-servers/sboot-eureka-server
   MINGW64:/t/eclipseworkspace4mes/sboot-servers/sboot-zuul-server
    /c/Apache/apache-maven-3.5.4/bin/mvn clean install
    /c/Apache/apache-maven-3.5.4/bin/mvn spring-boot:run
    
  1) mvn clean install 
  2) mvn spring-boot:run 

# 브라우저에서 확인 
http://config-server:8888/ke

http://config-server:8888/sample-api/vdi

http://config-server:8888/second-api/vdi

http://eureka-server:8761/

http://eureka-server:8761/actuator

http://eureka-server:8761/actuator/health

http://zuul-server:8080/actuator

http://zuul-server:8080/actuator/health

# 윈도우 열린 포트 확인 
$netstat -ano | grep :9999       // 해당 포트의 프로세스ID찾기 

C:\Users\Administrator>netstat -ano     // 현재 실행중인 프로세스, 포트확인 

C:\Users\Administrator>tasklist| findstr 10976   

java.exe                     10976 Console                    1    387,808 K

C:\Users\Administrator>taskkill /f /pid 10976

성공: 프로세스(PID 10976)가 종료되었습니다.
