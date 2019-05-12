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
  1) mvn clean install 
  2) mvn spring-boot:run 
