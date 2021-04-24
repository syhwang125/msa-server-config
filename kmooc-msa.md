# msa
microservice


k-mooc 마이크로서비스 설계 및 구현 강좌

Week1. Microservice 개념과 특성
1-1. Biz민첨성과 아키텍처 요건
1-2. 마이크로서비스 vs 모노리스
1-3. MSA 특징(1)
1-4. MSA 특징(2)

Week2. Microservice Outer Architecture
2-1. 마이크로서비스 outer/inner 아키텍처 의미
  1) MSA 내외부 아키텍처
	ㄴ MSA outer 아키텍처 : infra, platform, app 간의 관계를 정의하는 것으로 
	                        마이크로서비스가 운영되는 환경을 정의하는 과정 (유연성, 확장성) 
	ㄴ MSA inner 아키텍처 : 실제로 비즈니스에서 실행되는 각각의 마이크로서비스 내 구조 정의 
	
   2) 아키텍처 
      가) Application : 
	     ㄴ Channel : Mobile, Browser
		 ㄴ Core Services : Front End Service (Mobile BFF, Web BFF 등 ), Back-End Service (가입승인MS, 사용자/조직MS, 역할/권한MS, 게시판MS 등) 
		 ㄴ Data : RDB(maria), MDB(Reids), Legacy(Oracle)  
	   나) platform : 
	     ㄴ Platform services
	        devops-pipeline(svn, zenkins, sonarqube) , 협업환경(confluence,jira), 
	        message bus(rabbitmq, kafka), centralized logging(ELK), 라이브러리(nexus), 
	     ㄴ MSA Based Services 
	        API G/W(zuul), monitoring(hystrix), conf(config server), service discovery(eureka) 
	다) infrastructure : vm, bare metal, openshift 

   3) Microservice Architecture Pattern 
      어떤 문제영역에 대한 검증되고 정의된 유용한 해법 = pattern 
	  
2-2. 인프라, 플랫폼, 데브옵스 환경
    1) 인프라 영역 
	   클라우드 환경 -> 자동화된 기능으로 제공 
	   시스템 자원구성 할당시 기존 베어메탈 장비나 가상머신vm , 클라우드 iaas 를 선택할지 결정	   
	   
	2) 플랫폼 영역 
       vm : server, host OS 위에 hypervisor, guest os 가 올라감
       container : server, host 위에 bin/lib 등 탑재됨. 작은 서비스를 패키지하고 배포하는데 적합 
       컨테이너 관리 기술 = 컨테이너 오케스트레이션 = kubernetes 
         ㄴ CPU, Memroy 임계치가 초과될 경우 scale out 설정 가능
	   appl.infra영역 - devops 
       자동화된 배포 - CI(Continuous Integration,지속적 통합) 
       개발자 -> 형상관리서버에 check-in -> 빌드도구에서 check out해서 빌드, 테스트 	   
	  
	   자동으로 통합하고 테스트하고 레포트로 남기는 활동 = CI(Continuous Integration,지속적 통합) 
	   실행환경에 자동으로 배포하는 것 = CD (Continuous Deployment, 지속적 배포)
       형상관리(소스저장소)의 빌드된 실행파일을 실행환경에 배포 	  
	   
	   CI/CD 구성방법을 파이프라인이라고 함 
	     ㄴ 통합 및 배포까지 일련의 프로세스를 하나로 연계하여 자동화하고 시각화된 프로세를 구축하는 것
		 ㄴ 배포전에 UI테스트, 통합테스트, 배포승인 프로세스 등을 넣어 추가 설계 가능 
		 
2-3. 마이크로서비스 플랫폼(기반서비스-1)
    빠른 실패, 피드백 
    1) MSA History 
	   XP 방법론(1999) -> Agile (2001) -> 2010년 넷플릭스는 AWS에 거대한 시스템을 올렸으나 DB스토리지 고장으로 서비스에 큰 장애를 겪음 
	   -> 모노리식시스템에서 마이크로서비스기반의 시스템으로 전환 -> 넷플릭스의 시스템운영어려움 -> 여러서비스 도구 개발 -> 오픈소스로 공개 -> Netflix OSS 
	   -> hystrix, ribbon, eureka (2012)-> docker (2013) -> spring boot(2013) -> microservice define (마틴파울러, 2014) -> kubernetes(2014)
	2) Application 영역 - 개발프레임워크 
     넷플릭스OSS : netflix가 마이크로 서비스를 개발운영하면서 생긴 노하우를 공개 (zuul, eureka, hystrix, ribbon 등) 
      패턴 : API게이트웨이, 서비스디스커버리, 모니터링, 트레이싱 
     마이크로서비스를 개발하기 위한 기본 프레임워크 = spring boot + spirng cloud 

	3) Application 영역 - 기반서비스 DevOps 환경 
       config server, hystrix(monitoring), zipkin(trace), zuul 
       github, 

	   ※ 파이프라인 : 통합 및 배포까지 일련의 프로세스를 하나로 연계하여 자동화하고 시각화된 프로세스를 구축하는 것 
	   
    4) Application 영역 - API Gateway 
       서비스 라우팅 기능 수행, 로드밸런싱 처리, 전/후 필터링  
	   (내부라이브러리 라우팅-zuul, 로드밸런싱  - ribbon 사용) 
	   
	5) Application 영역 - BFF(Backend For Front-end)    
	    API 게이트웨이를 하나로 두지 않고 Frontend 의 유형에 따라 각각 두는 패턴 
		예) web, mobile app, 3rd party app 각각에 대해 gateway 를 둠 
		
	6) Application 영역	- Service discovery (zuul 과 ribbon 이 수행)	
	    서비스가 올라가는 컨테이너의 IP정보가 고정되어 있지 않기 때문에 서비스 이름과 IP정보를 매핑하여 보관할 저장소가 필요 
		
		서비스클라이언트 -> 라우터(레지스트리서비스) -> 서비스인스턴스A(ip:port), B(ip:port), C(ip:port) 등
   		=> 서비스레지스트리에 등록
		
	7) Application 영역 - Service registry 
        ※ 서비스레지스트리 
	       서비스매핑데이터베이스로서 처음 서비스가 등록될때 위치정보가 저장, 서비스 종료시 위치정보 삭제
	       모든 기반 서비스들의 주소도 같이 보관
		   
	   service discovery (service register / eureka) 
	     ㄴ 다수의 인스턴스가 하나의 서비스이름으로 등록될떄 다수의 IP, 포트정보가 매핑
	  
	8) Application 영역 -  Externalized configuration 
	   코드에서 사용하는 환경설정정보는 코드와 완전히 분리되어 관리해야 함
	   예) DB연결정보, 리소스정보, 호스트명 등 
	   
	
2-4. 마이크로서비스 플랫폼(기반서비스-2)
    1) Application 영역 - 인증, 인가 (oAUth) 
	   권한이 확인되면 access token 발급
	2) Application 영역 - 장애격리 (circuit breaker) 
       상황에 따라 서비스를 동적으로 증가시켜 과부하/오류상황시에도 지속가능한 서비스가 가능하도록 관리 
	    연속실패횟수가 임계값을 초과하면 회로 차단기가 작동
		
	3) Application 영역 - 장애격리 (fallback처리) 
	    장애격리 후 대체처리 수행 
		
	4) Application 영역 - 모니터링 
	   원격시스템이나 서비스를 호출하는 구간을 격리해 관리, 모니터링 해주는 라이브러리 (hystrix) 	   
    
	5) Application 영역 - 모니터링 대시보드 (hystrix dashboard) 
    
    6) Application 영역 - Log aggregation, Exception tracking 
       Elastic Search  : 분석엔진 (로그중앙관리저장소) 
       Logstash : 서버쪽 로그집합기 
       Kibana : 대시보드  
	   
	   logstash -> redis -> elastic -> kibana 
	   
	7) Application 영역 - Distributed tracing  (zipkin) 
	    장애포인트 확인, 지연구간 호출빈도 
		
		
#########################################################

Week3. Microservice Inner Architecture	

3-1. Application Architecture와 Java Enterprise 발전흐름

외부 아키텍처 : 아키텍처를 이루는 구성요소와의 관계를 설명
내부 아키텍처 : 어플리케이션의 내부 구조를 정의하는 활동 
※ 어플리케이션 아키텍처 - 동작하는 공통 메커니즘을 정의하는 것

(1) Web Application 의 구조 
정적 컨텐츠의 응답 <-> 
                          WEB서버 (요청전송) <-> Applciation서버 (처리) <-> DB서버    
동적 컨텐츠의 응답 <->  

위에서 Web서버와 Appl.서버간의 I/F를 어플리케이션 아키텍처 영역에 해당함

(2) Java 엔터프라이즈 발전 흐름
HTML (웹서버에서 HTML언어로 정적 서비스만 제공) , CGI (데이터를 가공하여 동적 컨텐츠 제공, perl 등의 언어로 저장하여 세션 관리 기능이 없음)
-> 이러한 문제를 해결하기 위해서 Servlet(로직), JSP(화면), EJB(엔터프라이즈 업무처리를 위한 컴포넌트 모델)등이 나옴 
-> 개선하기 위해 Spring 등장 
   ※ Spring특징
   POJO(Plain Old Java Object) 컨테이너 : 로직을 순수하게 표현하는 것을 중요시함
   프레임워크에 의존하지 않는 일반 Object의 생명주기 관리 
   Object간의 의존관계 
   -> 기존 EJB 등 Biz로직 처리등 유지보수 및 확장성이 어렵다는 단점을 극복하려는 시도였음
   
   ※ POJO 
     비즈니스를 객체모델로 표현
	 기술에 의존적인 부분들을 분리하려고 노력함 (Transaction처리, 에러, 로깅,데이터 처리 등을 객체모델)
	 AOP, DI 기술 활용 

   ## spring 이 java 진영의 표준 프레임워크가 됨	

3-2. Application Architecture : Layered Architecture 

(1) Tier와 Layer 구분
tier : 물리층 (물리적장비, 서버 컴퓨터 등)
layer : 논리층 (Tier 내부의 논리적 분할) 

클라이언트층 (PC, 스마트폰) - 중간층(Applicaton서버)  - EIS층 (데이터베이스, 레거시스템)  
Layer 로 표현하면 Presentation - Business Logic - Data Access Layer 

(2) Layered Architecture 
  설계자들이 복잡한 시스템을 분리할때 흔히 사용하는 패턴 중 하나 
  상위는 하위를 호출함
  하위의 여러 Layer를 알 필요없이 바로 밑에 근접한 Layer를 활용힘
  다양한 서비스를 이용함 
  
  Presentation -> Business Logic -> Data Access Layer 
  
  각 Layer 의 표준화, 각 Layer의 모듈화 
  -> 상의 Layer가 하위 Layer의 변경에 영향을 받지 않게 함   
  -> 어플리케이션이 쉽게 변경되거나 확장될 수 있게 함 
  -> 하위 계층은 상위를 알수 없게 해야 함 
  
  
  1 tier - User Interface 
  2 tier - Presentation 
         | Application
		 | Domain Model       
		 | Persistence 
  3 tier -  Data           		 
  
  ※ Layer는 Tier로 묶임. Tier 단위로 하나의 장비를 구성함 
  
  Presentation Layer : 클라이언트층에 있는 Presentation / 서버층의 Presentation  
  Business Logic Layer : 비즈니스, 도메인 
  Data Access Layer : 인티그레이션, 퍼시스턴스 
  
(3) Spring에서 사용되는 구조 

    Presentation Layer   : Controller        : 화면표현처리, 이벤트처리, 세션관리 기능 제공
    Business Logic Layer : Service - Domain  : Service (Controller에 의해 호출되며 도메인 로직을 호출, 특정 업무처리 흐름제어) 
                                               Domain ( Biz Logic 실행에 주요개념 및 그 구체적인 로직을 담고 있음. POJO로 구성) 
	Data Access Layer    : DAO               : 서비스가 처리한 결과를 받아 데이터로 저장하는 역할 수행
	
(4) 각 Layer간의 호출 원칙 
    각 Layer는 높은 응집력을 갖춘 외부와 Layer간의 낮은 결합도를 갖도록 설계 
    Layer사이의 호출은 인터페이스를 통해 호출하는 거이 바람직함 
    구현 클래스에 직접 의존하지 않음으로써 Object사이에 약한 결합을 유지함 
	
3-3. Application Architecture : Hexagonal Architecture (헥사고널 아키텍처) 

(1) Layer간의 의존성	

클라이언트 -> Presentation Layer (컨트롤러) -> Business Logic Layer (서비스 Impl -> Domain) -> Data Access Layer (DAOImpl) -> 저장소 

데이터베이스를 교체한다면 Biz Logic에 영향을 줌

# 헥사고널 
ㄴ biz logic 처리영역과 결과 처리영역으로 구분됨. -> 비즈니스 로직에 영향을 미치지 않는 것이 가장 좋은 설계이다. 
1) biz logic 처리 영역 : 어플리케이션에서 제일 중요한 영역 (브라우저에 표현하는 기술, 데이터베이스에 저장하는 기술) 
2) biz logic 결과를 처리 영역 (결과를 사용자에게 보여주는 부분, 결과를 저장하는 부분) 

GUI, 설명도(웹UI,모바일UI) -> 구현처리 -> 영속화, 다른 시스템 으로 구성될때 
차이점을 흡수하여 구현처리(presentation layer, data access layer + biz Logic Layer) 중에서 Biz logic 층이 영향을 받지 않도록 하는 것이 중요함
-> 이것이 헥사고널 아키텍처의 핵심임   

Hexagonal Architecture ?
" 다양한 이질적 클라이언트가 동등한 지위에서 시스템과 상호작용하도록 하는 대칭성을 만드는 스타일의 아키텍처 " (Port Adaptor Pattern) 

외부영역과 내부영역으로 나뉘며, 외부영역과는 어댑터를 통해 인터페이스함
ㅇ 외부영역 : 이질적 클라이언트가 입력을 보낼 수 있음. 영속데이터를 갖고 있거나 내부영역의 결과를 DB에 저장. 
              다른 위치로 전송하는 메시지 메커니즘 제공. 연계되는 타임마다 이에 특화된 어댑터 존재. API를 통해 내부영역과 이어짐
ㅇ DIP (Dependency Inversion Principle) 의존관계의 역전원칙 
   : 의존하려면 잘 변경되지 않는 부분에 의존해야 한다는 원칙 
   -> 다른 Layer에 가장 큰 영향을 줄 것 같은 위치에 있는 Layer의 의존방향을 바꿔 적용함 
   예) Biz Logic Layer 에서는 특정DB에 특화되지 않은 일반화된 Data처리 Interface (IData) 정의하고,
       Data Access Layer 에서는 OracleImpl, MariaImpl 과 같은 어댑터 정의 
       	   

		                Mobile, Browser (XML,JSON,HTTP)                    Oracle DB   Maria DB       
                                |		                                     |             | 
   외부영역 (Presentation층 controller)          외부영역(Data Access층, OracleImpl , MariaImpl ) 
                                  |                             |
        내부영역 (Biz Logic층 / 서비스-서비스Impl - 도메인 - IData )


3-4. 

(1) 각 Layer별 구조 설계 및 고려사항 
1) Presentaion Layer 

 MVC패턴 (Model View Controller로 이루어진 패턴) 
<---  client   ----->|<----------- Application Server  ---------------------->|<-- Enterprise Server  -->| 
 browser -> (request) -> Servlet (Controller) -> JSP (View) -> Java Bean (Model) ->       DB  

 최근에는 웹,모바일,태블릿,인터넷, PC, 셋톱박스 등 다양한 인터페이스 등장으로 다양한 화면플랫폼 등장 
 예) thymeleaf, react, angularjs, vuejs 등 
 
<---  client   ----->|<-- Front-end Microservice -->|<----------------- Back-End Biz MicroService  --------------------------------------->| 
 browser -> (request) -> Servlet -> View -> Model -> Controller (presentation) -> Service <-> Domain (Bisuness Logic) -> Dao (Data Access)       

  MVC구조의 Presentation Layer 존재                            Business Logic층의 API를 제공하기 위한 Presentation Layer존재  
   
  servlet 에서 REST API를 이용해서 Back-end의 controller 에 전달 
  
2) Front-End Microservice 장/단점
   ㅇ 장점 : Back-End Biz Microservice를 개발운영하는 경우 마이크로서비스 단위로 자율적 배포, 독립성 
   ㅇ 단점 : Front-end microservice는 한덩어리. 모노리스시스템의 단점이 나타남
 
 -> front-end microservice도 각 back-end biz microservice처럼 독립적으로 분해되어 서비스를 제공할 수 있도록 분해되는 방식 등장 
    이러한 방식을 " composite UI " 라고함
	
3) Server-side Page Fragment Composition 방식 
	서버에는 각 조각을 모으기 위한 간단한 웹프레임서비스가 동작 
	각 조작 영역은 비즈니스를 구성하는 한쌍의 Front-end, Back-end microservice에 의해 제공됨
	
2) Business Logic  층의 역할 
presentation층 (컨트롤러) 
    |
Business Logic 층 (서비스 Impl -> 도메인) 	
    |
Data Access 층 (DAOImpl) 

※ Business Logic 층
  - 특정 업무의 도메인 개념을 표현한 도메인과 Biz흐름 처리를 수행하는 서비스로 구성
  - 개발 운영시 기능추가와 변경은 로직변경을 통해 이루어짐 
  - 비즈니스 민첩성을 좌우할 핵심 레이어
 
3) Transaction Script 패턴 
   트랜잭션 단위로 모든 처리 흐름을 혼자 처리하는 방식
   ㅇ 서비스 : 모든 비즈니스 처리를 수행함 
   ㅇ 도메인 : 행위가 없고 데이터 처리 결과를 담고 있는 데이터 홀더의 역할 수행 
             서비스 클래스를 처리하기 위한 데이터 또는 그 결과를 담는 용도로 활용 (DTO역할) 
			 
-----------------------       ----------------
| Transaction Script  |       |    DTO-A     | 
| 행위()              |   ->  |    속성()    |
| 행위()              |       |    속성()    |
-----------------------       ---------------- 
	서비스                       도메인  
	
    ※ 이 패턴은 사용하기 쉬우나, 트랜잭션 증가에 따라 비슷한 처리 흐름이 중복되는 한계가 있음	
	    단순한 입출력 구조의 쉬운 업무처리를 위한 마이크로서비스 내부 구조로 활용하면 유용함
	
4) Domain Model 패턴 
  ㅇ 서비스 : 모든 Biz흐름을 처리하지 않고 도메인 클래스의 Biz개념에 따라 처리 위임
  ㅇ 도메인 : 객체 모델의 형태. 자체적으로 Biz행위 처리. 여러개의 Domain Model 결과를 모아 로직처리를 하는 경우에만 활용 
 
-----------------------       ----------------       ------------         ------------
| 서비스                      | 어그리게잇   |      | 엔티티    |         | VO        |       
| 속성()              |   ->  |    속성()    |  ->  |  속성()   |   ->    | 속성()    |       
| 행위()              |       |    행위()    |      |  행위()   |         | 행위()    |       
-----------------------       ----------------      -------------         -------------
	서비스                    <-----------       도메인              -----------------> 
	
	※ Domain Model패턴은 복잡한 비즈니스 로직을 정리 후 핵심 서비스로 활용하는 경우에 효율적임 

5) Business Logic 층의 Architecture 고려사항 
  ㅇ 명시적 Transaction : 컨트롤러 -> 서비스 -> DAO -> DB
  ㅇ 선언적 Transaction : 컨트롤러 -> 트랜잭션처리(프레임워크제공) -> 서비스 -> DAO -> DB  
  ※ 선언적 Transaction 을 사용하는 경우 Transaction 코드 기입 불필요 -> 코드의 간결성 및 Biz로직에만 집중 가능  
  
6) Data Access 층의 아키텍처 고려사항 
   Hexagonal Architecture 적용 
   
   Data Access 층은 어떤 저장소가 오더라도 Biz Logic 층에 영향을 적게 받도록 하는 것이 중요함 
   데이터 처리 메커니즘 검토 필요 (OR Mapping, SQL Mapping) 
     ㄴ OR Mapping : Spring, Spring JPA, 하이버네이트 기술 활용 
	 ㄴ SQL Mapping : Mybatis 프레임워크 활용 
	 
7) Data Access 층의 역할 
   ㅇ SQL Mapping 
      데이터 모델링을 통해 관계형 테이블 작성 후 Biz Logic을 처리할때 유요함
      Biz Logic Layer에 Trasaction Script 패턴을 적용하는 경우 적합한 방식 
	  개발자가 직접 SQL 작성, 쿼리 튜닝 등이 필요한 경우 
	  
   ㅇ OR Mapping 
      Biz Logic에 필요한 Object가 무엇인지 먼저 고려함 
      -> 비즈니스를 처리하는 흐름 구현 
      -> 저장소 결정 및 Object에 Mapping하여 데이터 처리 
	  -> 객체지향 중심 Domain Model의 Entity를 추출하는 방식임 
	  ※ OR Mapper가 SQL문을 자동으로 생성 
	  저장소를 다른 관계형 DB 또는 NO-SQL 저장소로 쉽게 변경 가능한 경우 유용함 

## 일반적으로 
 Biz Logic Layer가 Trasaction Script 구조로 설계된 경우 -> 데이터 모델링을 먼저 수행하는 방식으로 진행 
 Biz Logic Layer가 Domain Model 구조로 설계된 경우 -> 도메인 객체 모델링을 먼저 수행하는 방식으로 진행 

 
		
#########################################################

Week4. Microservice Inner Architecture	
목표 ) 마이크로서비스간의 통신을 위한 기법인 이벤트 주도 아키텍처 이해
       마이크로서비스간의 트랜잭션 처리를 위한 기법인 SAGA, CQRS패턴 이해
       마이크로서비스 아키텍처가 어떻게 도메인 주도설계와 연계되는지 이해 
	   
4-1: 메시징 기반 Event Driven Architecture
1) REST API 
   클라이언트에서 서버쪽에 존재하는 마이크로서비스를 호출할때의 기본 통신 방법
   다양한 클라이언트 채널 연계나 외부에 API의 공개를 원활하게 하기 위한 방법으로 API G/W를 사용함

   - 동기식호출(sync방식) : 요청을 하면 바로 응답이 오는 방식 
     호출받은 마이크로서비스에서 장애가 온다면? 메시징 기반 이벤트 주도 아키텍처(비동기방식)
   
   - 비동기식 호출(Asynchronous ) 
     메시지를 보낸 다음 응답을 기다리지 않고 일을 처리
	 동기식처럼 완결성 보장 못함
	 Apache Kafka, ActiveMQ, RabbitMQ 같은 메시지 브로커 사용 (producer , consumer 가 메시지 브로커에 연결) 
	 메시지 브로커는 메시지 처리 규모에 따라 확장 가능 
	 통신하는 서비스들이 물리적으로 동일한 시스템에 위치할 필요없음
	 프로세스를 서로 공유할 필요없음
	 같은 시간 동시에 동작할 필요없음
	 클라우드 시스템에서 활용하기 좋음 
	 
   ※ 이벤트주도아키텍처 (Event Driven Architecture) 
   이벤트를 생산하는 모듈과 이벤트에 대응하는 모듈을 분리하고 상호 독립적으로 동작하여 병렬처리를 촉진함
   발신자와 수신자를 장소와 시간에서 쉽게 분리 가능
   느슨한 결합으로 인해 확장성 및 수정 가능성에 많은 이점 제공 
	 
4-2: Microservice 트랜잭션 처리 - SAGA 패턴
   1) 서비스별 데이터베이스 
      각각의 마이크로서비스는 각자의 비즈니스 처리를 위한 영구 데이터를 소유함 
	  자신이 소유한 데이터는 다른 서비스에 직접 호출하지 않고 자신의 API를 통해서만 액세스 가능 
	  
	  -> 여러 서비스간에 데이터 일관성을 유지하는 방법 
	     과거 전통적인 방법으로 2PC 기법 사용 
		 
      -> SAGA 패턴 
         각 서비스의 로컬 트랜잭션을 순차적으로 처리하는 패턴
         각 로컬 트랜잭션은 자신의 데이터베이스를 업데이트 한 후 SAGA 내에 있는 다음 로컬 트랜잭션을 트리거하는 메시지 또는 이벤트를 게시하는 방법  
		 
	  -> 보상 트랜잭션 
         어떤 서비스에서 트랜잭션 처리에 실패할 경우, 
         서비스의 앞선 다른 서비스에서 처리된 트랜잭션을 되돌리게 하는 트랜잭션
		 
		 ※ 정리하면, SAGA 패턴 는
    		일관성 유지가 필요한 트랜잭션을 모두 묶어 "하나의 트랜잭션"으로 처리하지 않고, 
		    각각의 "로컬 트랜잭션으로 분리"하여 순차적으로 처리하는 방법임
		    그러다가 트랜잭션이 실패한 경우 이전 로컬 트랜잭션이 작성한 변경사항을 취소하는 일련의 보상트랜잭션을 통해 전체의 일관성을 유지함 
			 
   2) SAGA - Choreography (메시지 이벤트 )
      주문처리시 고객의 신용한도 정보에 따라 최종 주문을 승인하는 업무 
      예) 주문처리가 시작되면 Order Service는 주문을 생성하고 이벤트 게시 및 트랜잭션종료 
	      그 다음엔 Customer서비스가 주문생성 이벤트 확인 뒤 신용한도 조회, 충족되면 신용승인, 이벤트 게시 
		  신용승인 이벤트 확인 후 주문승인처리 수행하는 등 3개의 로컬 트랜잭션으로 처리됨 
		  
   3) Orchestrator SAGA 패턴 (이벤트를 사용하지 않고 동기식 API사용) 
      이벤트를 통해서가 아닌 Orchestration이 직접 처리하는 방식 
	  예) 주문서비스가 주문을 생성하고 createOrderSaga 클래스를 생성함. CreateOrderSaga는 고객 서비스에 승인한도를 조회하고 바로 응답 (동기식API사용)
	  
	  http://microservices.io/patterns/data/saga.html 
	  

4-3: 명령과 조회의 역할 분리 - CQRS 패턴
   마이크로서비스 아키텍처에서 많이 사용되는 패턴 CQRS 
   1) 명령과 조회의 역할 분리 - CQRS패턴 (Command Query Responsibility Segregation)
      - 명령 조회 책임 분리
	  - 기존의 일반적 개념이었던 동일한 저장소에 데이터를 넣고, 입력/수정/삭제/조회하는 방식
	  - 사용자의 비즈니스 요청 
	   ㅇ시스템 상태를 변경하는 명령
	   ㅇ시스템의 상태를 가져오는 명령 (입력/수정/삭제 보다 조회가 더 많으므로 분리하는게 맞음) 
	   -> 쓰기와 조회의 전략을 각각 분리하면 쓰기 시스템의 부하를 줄이고 대기 시간을 줄이는 등 엄청난 이점을 가져옴
	   https://docs.microsoft.com/en-us/previous-versions/msp-n-p/dn568103(v=pandp.10) 
   
   마이크로서비스를 명령과 쿼리 측면으로 나눔 
   ① Command Side : Client 의 Commands 를 통해서 Create update delete 처리 수행 명령 API 
                     ->  Command Processor 쓰기 Logic (Java) -> CUD 저장소 (RDB) 
   ② 이벤트 기반 Loosely-Couples :  MQ Event 저장 및 발행 (pub/sub) 	  
   ③ Query Side : Client 의 QUery 를 통해 조회 API -> 구체화된 View 읽기 Logic (node.js 등) -> View 저장소 (MongoDB, ElasticSearch 등 noSQL DB) 
 
    ※ 읽기 시점과 쓰기 시점의 데이터가 다르기 때문에 데이터를 쓰면서 저장한 내역을 MQ로 이벤트 발생
	   -> 조회서비스는 이벤트를 가져와 저장소에 최신상태로 동기화 함 -> 이러한 과정을 '결과적 일관성(Eventual Considency)' 이라고 함
	
    2) 이벤트 소싱 
	   관계형 모델로 변환하지 않고 이벤트 자체를 순차적으로 포함하여 저장 
	   모든 쓰기를 이벤트로 전환해서 별도의 이벤트 스트림으로 스트림저장소에 저장하는 방식 
	   
    3) 이벤트 소싱 저장소 
	   - 추가만 가능하여 계속 이벤트들이 쌓이게 만들고 필요한 데이터를 구체화시키는 시점에서는 그때까지 축적된 데이터를 바탕으로 작성
	   - 각각의 이벤트는 한가지 액티비티에만 집중하게 되어 아무리 복잡한 Biz Logic이라 하더라도 간단하게 만들 수 있음
	   
	※ 쓰기 퍼포먼스 측면에서 강점을 지니며 CQRS와 같이 자주 사용 
	
   
4-4: MSA와 도메인 주도 설계
   1) 서비스를 제공하는 각 서비스간의 관계 	  
      서비스가 독립적이고 대체 가능하도록 느슨하고 유연하게 구조화되어 있어야 함
	  서비스 내부의 구조도 시스템의 유연성과 확장성을 높이기 위해 기술과 비즈니스 로직을 분리하여 구축하는 것이 바람직함 
      * 추천도서 : 에릭에빈스의 도메인 주도 설계 (2011)   
   2) 이벤트 주도 아키텍처와 전략적 설계 
      ㅇ 이벤트 주도 설계 
         - 마이크로서비스의 관계를 느슨하게 만드는 중요한 기법
	  ㅇ 이벤트 주도 마이크로서비스의 구현 
	     - 시스템 운영방식 및 데이터, 서비스간의 상호작용 파악이 중요함 
		 - 비슷한 비즈니스 기능과 데이터가 응집성있게 하나의 서비스로 도출하는 것이 중요함 
      * 추천도서 : 도메인주도설계핵심, 에이콘, 2017 		 
   3) 도메인 주도 설계의 전술적 설계 기법
      - 헥사고널 아키텍처의 내부영역에 존재하는 기술
      - 독립적인 도메인 모델을 설계, 개발할 수 있는 설계 도구 
      - 도메인 모델은 비즈니스 도메인의 개념을 잘 표현한 객체지향모델 

		
#########################################################

Week5. 도메인 주도 설계 

5. 도메인 주도 설계의 이해 				
  
5-1. 도메인 주도설계란 무엇인가?
ㅇ 도메인 주도 설계 : 도메인의 가치를 최우선시하는 모델링 기법 
   - 개발자들도 분석과 설계자와 함께 회의에 참석하고
   - 코드를 구현하기에 앞서 도메인과 도메인 모델의 완전한 이해 강조
   - 코드와 모델의 밀접하게 관련시켜 코드에 도메인의 의미가 녹아있게 함 
   - 소프트웨어 설계시 비즈니스 상 중요도에 따라 도메인으로 나누고 
  
ㅇ 도메인주도 설계 
   1) 전략적 설계 (  마이크로서비스를 식별하는 역할)
   2) 전술적 설계 (식별된 마이크로서비스의 내부구조 상세정의, 비즈니스의 고유한 활동 모델링) 

ㅇ 전략적 설계 
    1) 명확히 식별된 Boundned Context 내부를 모델링하기 위한 유비쿼터스 언어를 정의 
	    (유비쿼터스언어 : 소프트웨어 개발팀에서의 공통언어) 
	2) 이 공통의 언어를 활용해서 비즈니스를 분석하고 핵심이 되는 개념을 식별
	3) 식별된 개념들을 분석해서 Bounded Context를 식별 
	4) 식별된 Bounded Context 간의 매핑관계를 정의해서 Context map 작성
	5) Bounded Context 로의 분할에 따른 효과를 분석해서 서비스의 분할 및 통합을 검토 
	-> 후보 마이크로 서비스를 도출함

ㅇ 도출된 마이크로서비스 -> 전술적 설계 (서비스의 내부구조 상세 정의) -> 최종 코드로 구현 
   * 전술적 설계 : 비즈니스의 고유한 활동을 정확하게 모델링하는 설계 패턴과 방법들을 의미함 
     (도메인모델과 모듈, 서비스 인터페이스와 API , 프로트엔드의 설계)

ㅇ 용어 정의
   - 모듈 : 도메인 객체를 담는 컨테이너의 역할을 수행하는 것
   - API : http URI로 표현된 리소스에 대한 동작이나 범위를 정의한 것으로 '무엇을 어떻게 한다'로 정의된 API=REST API 
   - 도메인 모델 : 비즈니스의 핵심 개념을 도메인 객체로 표현한 것. UML의 Class Diagram의 형태 
   
ㅇ 사례 : SK 마이크로서비스 개발 프로세스 (도메인주도설계 기반의 'SW개발 방법론'을 개발하여 사내 표준으로 사용)
   
   1) 전략적 설계 : 핵심개념 설계 -> 바운디드컨텍스트식별 -> 마이크로서비스 후보 식별 -> 
   
   2) 전술적 설계                                   3) 서비스 개발           
           도메인모델설계            -> 도메인 모델 개발             -> 구현모델 검토      
		         |                             |                              |
        ->  도메인 모듈 설계         -> 도메인 모듈 개발                단위테스트
		         |                              |                             |
		-> 마이크로 서비스 개발      ->  마이크로서비스 개발             API 테스트
   
* 추천도서 : 에릭 에반스, 도메인주도설계    
             반버논, 도메인주도설계 핵심 
			 최범균 DD Start!  
   
5-2. 전략적 설계 바운디드컨텍스트와 유비쿼터스언어

ㅇ 전략적 설계 : 비즈니스 상 전략적으로 중요한 것을 찾아 중요도에 따라 일을 나누는 방법
    - 바운디드컨텍스트 : 도메인의 주요 개념 정의, 도메인간의 경계
	- 유비쿼터스언어 : 프로젝트 내의 모든 팀원이 공통으로 사용하는 언어 
	
ㅇ 서브도메인 모델의 유형
   도메인은 여러개의 서브도메인으로 나뉘어지며 서브도메인 유형은 3가지 임
    - 서브도메인 : 전체 비즈니스 도메인의 하위영역으로 하나의 논리적 도메인 모델 (핵심/서브/일반 도메인으로 구분)
	- 서브도메인 유형 
	  1) 핵심서브도메인 : 다른 경쟁자와 차별화를 만들 수 있는 비즈니스 영역
	                      높은 우선순위를 갖는 전략적 투자 영역, 가장 큰 투자가 필요한 곳 
	  2) 지원서브도메인 : 맞춤개발이 필요한 영역. 핵심 서브도메인의 성공을 위한 중요한 영역 
      3) 일반서브도메인 : 기존 제품구매를 통해 바로 충족시킬수있는 영역. 핵심/지원 서비도메인이 할당된 팀에서 직접 구현 가능 
	  
ㅇ 바운디드컨텍스트	 
   - 동일한 맥락의 경계 또는 범위를 표현하는 것. 유비쿼터스 언어로 모델링 되는 것 
     동일한 맥락의 경계란? 그 범주 내에서 소프트웨어 모델의 각 컴포넌트가 특정한 의미를 갖고 그 특정한 일을 한다는 것 
   - 바운디드 컨텍스트 = 모델이 구현되는 곳 
     컨텍스트마다 각각 분리된 포인트의 산출물이 나옴
	 
ㅇ 서브도메인과 바운디드 컨텍스트의 관계 
   서브도메인과 바운디드 컨텍스트는 1:1의 관계로 매핑됨 
   예) 핵심도메인 : 바운디드컨텍스트 = 1:1 
   바운디드 컨텍스트의 명확한 개념과 모델을 잘 유지할 수 있도록 하기 위함 
 
ㅇ 유비쿼터스언어 
   - 제품책임자, 도메인전문가, 개발자간의 표준화된 언어
   - 도메인의 의도를 정확히 반영하고 도메인의 핵심 개념이 잘 표현되도록 정의
   - 사용자와 도메인을 이해하기 위해 의사소통할때와 설계 모델링을 진행할때/구현할때 
     소스의 class명, method명에도 사용되엉야 하는 언어임

   - 바운디드 컨텍스트별로 유비쿼터스 언어를 정의하여 사용해야 함 
     예) Sales 의 Customer =(Buyer) , Accounting 의 Customer = (Payer), Order의 Customer = (Receiver) 로 정의
	 
5-3. 전략적설계 (컨텍스트 매핑)
ㅇ 컨텍스트 매핑 : 핵심 도메인을 다른 바운디드 컨텍스트와 통합하는 것
    각 팀에 사용하는 컨텍스트를 매핑하기 위해서는 유비쿼터스 언어로 정의해야 함
ㅇ 컨텍스트 매핑 유형 
    1) 파트너십 
	2) 공유커널 : 바운디드 사이에 공통적인 모델을 공유하는 관계 
	  - 공유하는 모델 요소 서로 합의하에 만들어야 하며, 모델의 코드 및 빌드관리, 테스트는 어느 한팀에서 맡아 수행.
	  - 공유하는 부분은 다른 팀과 협의없이 변경 불가. 
	  - 서로 다른 구현 기술을 사용하는 경우 사용 불가
	  - 모델, 코드 및 데이터베이스 설계의 일부까지 포함
	3) 고객-공급자 유형 : 고객이 원하는 공급자가 제공해 주는 관계 
	  - 공급자는 고객이 원하는 것을 제공하므로 두 컨텍스트간의 관계를 주도. 
	    공유커널이 기술적으로 불가능하거나 두 컨텍스트의 분리된 모델을 갖는 것이 정당한 경우
	4) 준수자(confirmist) 유형 : 공급자의 모델을 그래도 따르는 방식의 관계 
       - 하류의 요구를 지원하지 못하는 경우 사용 
       - 확정된 모델과의 통합이 필요한 경우 
	     예) 아마존과 제휴한 판매자가 아마존 시스템에 통합하는 경우 	   
	5) 충돌방지계층(Anti-Corruption Layer) : 공급자와 모델과 고객 모델 사이에 번역 계층을 만드는 방법 (ACL) 
	   - 다른 시스템과 상호작용에 필요한 통신 메커니즘을 구현해서 상류모델의 변경없이
	     하위모델과 통합을 위한 데이터를 중간에서 양방향으로 변환해주는 영역을 구현하는 것	 
		 New System (MicroServive) <-->    API       <--> Legacy System  
		          Class                   Adapter            Class 
                                          Facade 
                                          Adapter 
                                         Translator 
                                           (ACL) 
										   
	6) 공개 호스트 서비스 (Open Host Service) : 바운디드 컨텍스트에 접근할 수 있는 인터페이스를 정의하는 방법 
	   - 바운디드 컨텍스트와 통합하고자 하는 모든 컨텍스트가 사용할 수 있도록 프로토콜이나 인터페이스를 공개 
	7) 공표된/발행된 언어 (published Language) : 바운디드 컨텍스트의 규모와 상관없이 사용과 번역을 위한 가능하게 하는 문서화된 정보교환언어를 제공하는 방법 
	   - 언어는 XMl스키마, JSON스키마 형식으로 정의
	   - 발행된 언어는 공개 호스트 서비스와 결합되어 사용
	8) 매핑안함 

5-4. 전술적 설계
     전략적 설계를 통해 도출된 마이크로서비스 내부 설계 방법 

ㅇ 도메인 모델의 표준 패턴 
ㅇ 계층형 아키텍처 
   각 계층은 계층별로 높은 응집성과 하위계층에 대한 의존 등 규칙 설정 

   1) 사용자인터페이스 계층: 사용자의 입력을 받아 애플리케이션 계층에 전달하는 기능 
   2) 애플리케이션 계층: 애플리케이션이 수행할 작업을 정의하고 조정하며, 하위 계층인 도메인 객체로 작업을 전달하는 역할 
   3) 도메인 : 비즈니스의 로직에 위치, 업무용 소프트웨어의 핵심으로 상위계층에서 전달받은 업무의 처리를 위한 업무정보와 업무규칙을 표현 
   4) 인프라스트럭처 : 데이터의 저장이나 메시지 시스템 등의 상위 계층을 지원하는 기능을 제공 
   
   사용자 인터페이스 -> 애플리케이션 : 계층의 모든 요소는 같은 계층 또는 하위 계층에서만 의존 
   애플리케이션 -> 도메인 -> 인프라스트럭처 : 상위로의 의존은 콜백 또는 옵저버 패턴 등을 활용한 간접적인 메커니즘을 반드시 활용 
   도메인 주도 설계에서는 도메인 계층을 분리하는 것이 매우 중요!! 도메인설계에만 집중!! 

ㅇ 전술적 설계
   1) 엔티티 (Entity) 
      - 소프트웨어의 상태가 변경되더라도 식별자를 통해 다른 객체와 식별되어야 할때 사용 
      - 생명주기 동안 형태/내용이 변경될 수 있지만, 연속성은 유지 
      -> 고유한 식별자와 변화 가능성 
	  -> 엔티티와 값 객체를 구분하는 중요한 차이점 
	  
   2) 값 객체 (Value Object) 
      - 개념적으로 식별성이 없고 단순히 값만을 갖고 있는 객체 
 	  - 상태를 변경할 수 없는 객체로 엔티티를 서술하고 수량화하거나 측정하는 속성으로 사용 
	  - 불변성이 유지될 수 있도록 모델링되어야 하며, 개별 속성이 수정/삭제되지 않고 전체 값 객체가 생성/삭제되는 방식으로 동작 

   3) 애그리게잇 (Aggregate) 
      - 업무상 관련있는 객체들을 모아 경계를 명확히 정의, 객체 간 관계를 복잡하지 않게 함으로써 생명주기 상의 
        전 단계에서도 도메인 객체의 무결성을 유지할 수 있게 해주는 패턴 
	  -> 데이터 변경의 단위로 다루는 연관된 객체의 묶음 
	  -> 하나의 트랜잭션은 오직 하나의 애그리게이션만을 수정하고 커밋한다는 원칙
	  ※ 애그리게잇은 트랜잭션의 일관성을 만드는 경계 (비동기 연동, 일정시간 데이터가 일치하지 않을 수 있지만 어느 시점이 되면 결과적 일관성 보장) 
	  
	  
	  - 1개 이상의 엔티티로 구성되며, 그 중 한 엔티티는 애그리게잇 루트로 정의되어 구성되며 각 객체 포함 가능 
	  - 트랜잭션의 제어가 데이터베이스에 커밋될때 한 애그리게잇 내의 모든 구성요소들은 비즈니스 규칙을 따르면서 일관성있게 처리된다는 것이 애그리게잇의 설계 규칙

   4) 저장소 (Repository) 
      - 애그리게잇의 개념적인 저장소, 필요한 객체에 접근하기 위한 진입점을 리파지토리  
	  - 도메인 계층의 데이터 저장을 위한 인터페이스를 구현하고, 실제 저장 기능은 인프라스트럭처 계층에 위치 
	  -> 도메인 계층은 데이터베이스 기술의 변경에도 추가적인 변경이 필요없게 되어 도메인 계층이 비즈니스를 처리하는 일에만 집중 
	
   5) 도메인 이벤트 (Domain Event) 
      예) 발행컨텍스트(애그리게잇, 엔티티의 집합체) -> 도메인 이벤트 발생 -> 구독컨텍스트(애그리게잇) 
      - 하나의 바운디드 컨텍스트 내부의 변경이 다른 바운디드 컨텍스트로 파생되는 작업이 필요한 경우 비동기 방식의 도메인 이벤트를 통해 명시적으로 구현 
	  -  메시지 큐를 사용한 비동기 방식으로 동작 (데이터 일관성 보장) 
	  - 각 바운디드 컨텍스트는 다른 바운디드 컨텍스트와의 의존도 낮음 
	  ※ 즉, 서로 다른 도메인이 섞이는 것을 방지할 수 있다는 것 
	  
	  
#########################################################

Week6. 마이크로서비스 도출을 위한 전략적 설계  

6-1. 전략적 설계의 정의 및 이벤트 스토밍 기법 소개 
ㅇ 전략적 설계 
   - 설계가 계속해서 커지게 되면 중요한 것과 덜 중요한 것을 찾아 분할 
   - 큰 도메인을 서브도메인으로 분리 : 핵심/지원/일반 
   - 서브도메인에서 바운디드컨텍스트를 도출하고 이때 서브도메인과 바운디드컨텍스트는 1:1 또는 1:N으로 도출 
   ※ 컨텍스트들간의 관계를 찾아 컨텍스트 맵을 그립 -> 최종적으로 마이크로서비스를 도출 
   
ㅇ 효과적인 설계에 대한 요구
   - Agile관리 프로세스 적요 -> 2~3주의 스프린트 -> 스프린트기간에서 상세한 설계와 구현, 테스트 진행방식 적용
     (도메인주도설계기법 적용 개발) 
   ※ 따라서 쉽고 빠른 설계를 위한 가속화 기법의 적용이 필요함 (이벤트 스토밍 기법) 
   
ㅇ 이벤트 스토밍 (Event Storming) 
   도메인주도설계 전문가 - 알베르토 브란톨리니 
   마이크로서비스 구현을 위해 필수적인 큰 도메인을 마이크로서비스로 분할하는 것을 쉽고 빠르게 할 수 있을까?   
   마이크로서비스를 도출하기 위한 시스템 안의 이벤트를 이해하는 도메인 전문가와 개발자들의 브레인스토밍 활동 
   - 이벤트 스토밍 수행 절차 
     1) 도메인 이벤트 찾아내기 (흐름을 순서대로 나열 -> 명확하지 않으면 모델링 공간에 표시) 
     2) 도메인 이벤트를 찾아내는 커맨드 정의 및 커맨드를 동작하게 하는 특정한 사용자나 역할을 식별 
     3) 도메인을 여러개의 서브도메인으로 분할 ( "커맨드" "도메인 이벤트" )
	 4) 바운디드 컨텍스트 식별 
   - 이벤트 스토밍 준비 
     1) 넓은 워크샵 공간 - 의견공유
     2) 적합한 사람들 - 모든 이해관계자
     3) 대형롤페이퍼, 스티커, 마크펜 	 
   
6-2. 실습 : 쇼핑몰 서비스 - Big Picture 그리기(1)
ㅇ 쇼핑몰 서비스 
   판매자가 쇼핑몰 서비스에 상품 등록, 구매자가 상품주문, 상품확정
   상품판매자 : 상품등록 -> 상품 발송
   상품구매자 : 회원가입 -> 상품구매 -> 배송조회 -> 제품수령 -> 구매확정
   
ㅇ 핵심프로세스를 찾기 위해 도메인 이벤트 
  시간의 흐름에 맞게 이벤트를 정의
  실행 프로세스 / 외부시스템 찾기
  핫 스팟 (hot spot) 찾기 

6-3. 실습 : 쇼핑몰 서비스 - Big Picture 그리기(2) 
ㅇ 이벤트 스토밍 (도메인의 이벤트 찾기 -> 커맨드 정의) 
   1) 커맨드 : 각 도메인 이벤트를 동작하게 하는 커맨드를 찾는 활동
     ㄴ 커맨드 이름(changed account) - 도메인 이벤트(request shipment) 
   - 커맨드와 도메인이벤트가 한 쌍이 되어 붙여짐. 
   - 커맨드를 동작하게 하는 사용자/역할을 찾는 활동 
      member(노랑 / 역할) / change Account (파랑 / 커맨드) / Account Changed (주황 / 이벤트) 
	  프로세스 (Validation check process / 보라) , 핫스팟 (Rules for re-register , 빨강)

ㅇ 서브 도메인 식별	  
   1) 도메인의 이벤트 찾기 
   2) 커맨드 정의 : 커맨드를 동작하게 하는 특정한 사용자나 역할을 식별 
   3) 큰그림&핵심 프로세스 도출 : 도메인을 여러개의 서브 도메인으로 분할 
   핵심서브도메인 (상품등록, 추천 등) / 일반 서브 도메인 (배송), 지원 서브 도메인 (회원가입, 반품, 구매후기 등) 
   
6-4. 실습 : 쇼핑몰 서비스의 이해 - 마이크로서비스 도출하기 
ㅇ 전략적 설계의 정의 및 이벤트 스토밍 기법 
    - 커맨드 / 도메인 이벤트 의 짝을 정의함 => Entity / Aggregate  (노랑??)
	이벤트 수행결과를 표현하는 데이터, 저장하는 데이터 
	
	예) changed account (account에 대한 변경요청) - account changed (변경이 수행됨) 
	    -> account entity/aggregate 로 정의될 수 있음 
	    register product - product registered -> product entity aggregate 

ㅇ 바운디드 컨텍스트 식별 		
    Account context 
    return context
    purchase context / payment service  

	이벤트를 통한 비동기 방식의 흐름 : 컨텍스트간의 불필요한 결헙, dependency 제거
    동기식 방식 흐름 
	
ㅇ 읽기 모델 (Read Model) 
   : 사용자의 커맨드를 동작시키기 위해 필요한 데이터를 식별하는 과정
ㅇ User Interface  
   
7-1차. Microservice 설계 및 구현
전략적설계 : 이벤트 스토밍 기법 ->  바운디드 컨텍스트 식별 -> 마이크로서비스도출
전략적설계를 통해 식별된 마이크로서비스의 내부구조를 상세하게 정의, 전술적 설계 (비즈니스 모델링)

ㅇ 전략적 설계. : 이벤트스토밍 기법 활용
   - 하나의 큰 비즈니스 도메인 -> 여러개의 서브 도메인
   - 바운디드컨텍스트 식별 및 컨텍스트매핑 -> 컨텍스트맵 그리기
   - 마이크로서비스 후보 식별

ㅇ 전술적 설계
- 전술적 설계를 통해 식별된 마이크로서비스의 내부상세설계
- 비즈니스의 고유한 활동을 정확하게 모델링하는 설계 패턴과 방법 의미
- 도메인 모델 및 모듈 등 정의 

ㅇ 마이크로서비스의 내부 구조  : 헥사고날 아키텍처 
     Client ->[  resource -> service interface -> (domain module > domain model) -> adapter  ] ->  다른 micro service, db 등 

* 헥사고날 아키텍처 : 애플리케이션의 내부, 즉 도메인 모델을 설계하고 UI,데이터베이스 등 기술과 도메인 모델을 분리하는 것 
      도메인 영여과 외부 기술영역을 완벽하게 분리

ㅇ 도메인 주도 설계에서의 도메인 모델 (예전의 개념모델과 유사)
고객, 제품책임자, 분석/설계자와 개발자 -> 도메인 모델 (도메인 개념/정보/규칙을 서로 공유하고 이해)

 1)CBD/MDA에서의 모델
    분석가/설계자 (PIM) -> 개발자 (PSM) -> 구현(Code) 과정에서 많은 도메인 지식들의 유실/변경으로 제품에 문제 발생 
  CBD(component based Development)
  MDA(Model Driven Architecture) 
  PIM(Platform Independent Model)
 PSM(Platform Specific Model) 

2)도메인모델 : 특정문제와 관련된 모든 주제의 개념모델이라고 정의

ㅇ 모델링 도구 : 기존 uml 도구 활용 가능 
    starUML2(무료), Visual Paradigm, Enterprise Architect(상용) 제품

ㅇ 도메인 모델의 표준 패턴 
   1) 엔티티 식별 : 변화가능성, 고유식별자
        개념적으로 필수적행동과 그 행동에서 필요로하는 특성만 식별 
       예) Account (계정) 
   2) 값 객체 (Value Object)
   상태를 변경할 수 없는 객체로 단순히 값만을 갖는 객체 
  고유한 식별성이 없음
   값의 형태로 속성을 비교함으로써 값 객체 비교
  Entity를 서술하거나 수량화하거나 측정하는데 사용
  값 객체는 상태를 변경할 수 없는 객체로 모델링
  개별 속성이 수정/삭제되지 않고 값 객체 전체가 생성/삭제되는 방식으로 동작
    예) Address 객체
   3) 표준타입 (Standard Type) 
   대상의 타입을 나타내는 서술적 객체 
    예) MembershipLevelType, MemberType 
       

7-2차. 도메인 모델링 따라하기 
 ㅇ 헥사고날 아키텍처 : 도메인모델 -> 도메인 모듈 -> 마이크로서비스 
 ㅇ 쇼핑몰 예제 : 도메인모델 패키지 구조 정의 
     ㄴ shopping mall 
          ㄴ domain model
              ㄴ account 
                   ㄴ account 
                   ㄴ model  (entity, 값객체, 표준타입 ; 객체식별 = 도메인모델설계)
                        ㄴ <entity> Account
                        ㄴ <value object> CreditCard
                        ㄴ MembershipLevelType    
                   ㄴ respository  (저장소 인터페이스) 
                        ㄴ 
                   ㄴ service  (서비스인터페이스, 인터페이스 구현체)
                        ㄴ 

ㅇ 도메인 모델 요소
ㅇ 실제 쇼핑몰 시스템의 account service를 도메인 모델링 설계
ㅇ 도메인 모델 안의 객체들을 트랜잭션 단위로 경계를 구분해야 하는데, 이를 Aggregate 라고 함

7-3차시. 도메인 모델링 따라하기#2 (에그리게잇 식별하기) 
ㅇ Aggregate이란 ? 
    - 데이터 변경의 단위, 트랜잭션의 단위가 되는 연관된 객체의 묶음
    - 도메인 모델을 간단,명확하게 구분해서 복잡성을 낮춰야함
    - 생명주기 전 단계에서 도메인의 무결성 유지가 필요함

#  즉,  데이터 변경의 단위로 다루는 연관된 객체의 묶음을 말함 
     Aggregate 는 1개 이상의 entity로 구성되며
     그 중 한 entity는 aggregate root로 정의하며 구성에는 값 객체 포함 가능 
    하나의 트랜잭션은 오직 하나의 Aggregate만을 수정하고 commit한다는 원칙 

ㅇ Aggregate 설계시 고려사항 
   1) 하나의 트랜잭션에서는 하나의 aggregate만 수정한다는 것 
       ㄴ aggregate 는 트랜잭션 일관성을 만드는 경계
   2) 애그리게잇은 하나의 일을 잘 수행할 수 있도록 작게 설계할 것
   3) SRP(Single Responsibility Principle) 단일책임의 원칙 
   4) 다른 애그리게잇은 식별자를 통해서만 참고해야함
   5) Aggregate의 갱신은 결과적 일관성을 통해한다는것

ㅇ Aggregate 식별 
   ㄴ model:Account (Entity, Aggregate Root) 
   ㄴmodel : Address (value Object) 
   ㄴmodel:MemberType (Enumeration , StandardType)
   ㄴmodel:MembershipLevelType (Enumeration , StandardType)
위 모델들중 account 가 aggregate Root 가 되며, 다른 aggregate 는 account aggregate 를 갱신하기 위해서는 account 를 통해서만 접근 가능 
Account 는 aggregate 안의 다른 모든 요소를 소유하고 있다고 볼 수 있음
각 aggregate 는 일관성있는 트랜잭션 경계를 형성함
트랜잭션 제어가 DB commit될때 한 aggreate내의 모든 구성요소는 반드시 비즈니스 규칙을 따르면서 일관성있게 처리됨

ㅇ 요약정리 
    ㄴ 도메인을 표현하는 객체 정의
    ㄴ 각 객체는 엔티티, 값객체, 표준타입으로 정의
    ㄴ 이 도메인 모델에서 aggregate 식별
    ㄴ aggregate 의 정의와 필요이유, 특징 
    ㄴ aggregate을 식별하는데 필요한 사항들 

7-4차시. 도메인 모델링 따라하기#3 (쇼핑몰 서비스 모델링) 
ㅇ 쇼핑몰서비스 업무흐름
    1) 구매자 상품구매 : 구매번호,구매자,수취자,상품이름,상품가격,구매수량
    2) 구매자 조회 : Account Service (회원이름,주소) 
    3) 상품조회 : Product Service(상품이름, 상품가격) 
    4) 구매내역발행 : Queue 
    5) 구매내역구독 : Delivery Service (구매번호, 배송상태) 
    6) 구매자는 배송현황 조회 

ㅇ 모델 패키지 구조 
    : Model, Repository, Service 로 구성 

ㅇ Account Domain Model 예
    - 주소(주소,우편번호) 
    - 계정 (주소, 이메일,아이디,멤버십레벨,멤버타입,이름)
    - 멤버십등급유형(VIP, Gold,Silver) 
    - 멤버유형(판매자,구매자) 

   1) 엔티티
   고유한 식별자와 변화가능성이라는 특징을 갖는 객체를 Entity로 식별 
   생명주기동안 형태나 내용이 변경될수있지만 연속성 유지되어야 함
   2) 값 객체
   식별성이 없는 경우 값 객체로 식별되며 VO 전체가 생성/삭제되는 방식 
   
model:Account
Service: AccountServie <<interface>>
Service : AccountLogic <service< 
Repository : AccountRepository <<interface>>
   
ㅇ 도메인 모듈이란 
도메인 모델을 담고 있는 java 패키지나 C#의 네임스페이스
   도메인 모듈의 설계는 서로 다른 모듈에 있는 객체 사이에 낮은 결합도를 유지하기 위해 사용
  도출된 마이크로서비스 단위로 모듈 정의

ㅇ 도메인 서비스
  Account 에서의 account service란 도메인 고유의 작업을 수행하는 오퍼레이션으로 aggregate에서 수행되지 못하는 동작이 필요한 경우 정의되어 사용 

ㅇ Product Domain Model 
   - 사이즈 ( S / M / L/ XL) <enumeratrion>> : 표준타입
   - 상품상세정보 (색상/사이즈) : 값객체 
   - 색상 (빨강/주황/노랑/파랑/보라) <<enumeration>> : 표준타입
   - 상품 (상품코드/상품명/가격/상품상세정보) : 엔티티

ㅇ Purchase Domain Model 
    - 구매자 (멤버십등급/이름) : value object
    - 구매 (구매자/구매번호/구매상품/구매수량/수취자/총구매금액) : aggregate root
    - 수취자(주소/수취자이름/우편번호) : value object
    - 구매품(가격/상품명) : value object
    - 신용카드(카드번호/유효기간) : value object

ㅇ Delivery Domain Model 
    - 수취자 (주소/이름/전화번호/우편번호) : vo
    - 배송 (배송상태/배송아이디/구매품/수취자) : entity : aggreagate root 
    - 배송품 (상품코드/상품이름) : vo
    - 배송상태 (준비중/배송중/배송완료) : 표준타입 

ㅇ 도메인 모델링 
    ㄴ 엔티티,값개체,표준타입 식별 후 aggregate 식별하는 도메인 모델 설계
    ㄴ service interface와 repository interface를 정의하는 도메인 모듈 설계
    ㄴ 각 마이크로서비스 동기/비동기 연동설계하는 마이크로서비스 설계 
 	
	
8-1차. Microservice 구현

8-2차. 마이크로서비스 구현 #1 - 스프링부트 프로젝트 생성 
표준 패키지 구조 
ㅇ application.sp 
  - config 
  - web : 다른 마이크로서비스에 제공할 API를 정의하는 곳 
ㅇ domain 
   - model : entity, value object, domain envent 정의하는 곳으로 도메인주도설계와 JPA와 같은 기술이 적용 
   - repository : 데이터 저장소를 관리하는 기능의 interface가 정의됨
ㅇ service : 비즈니스 로직의 흐름처리나 트랜잭션을 처리하는 코드가 구현됨

8-3차. 마이크로서비스 구현#2 - API테스트 및 데이터베이스 변경 
ㅇ Swagger : 프로젝트에서 지정한 URL들을 HTML화면에서 확인해주는 오픈소스 
   ㄴ Rest API를 만드는 경우 주소/파라미터가 변경되더라도 변경된 소스를 그대로 참조문서로 만들어줌
   
ㅇ JPA를 사용하는 경우
   ㄴ OR mapper가 쿼리를 자동 생성해 주기 때문에 저장소의 변경에도 쿼리 변경없음 

ㅇ H2DB 에서 MariaDB로 변경 

8-4차.마이크로서비스 구현#3 - 도메인 모델 구현 
ㅇ업무기능 추가 - 엔티티,값개체,표준타입를 구분
  Account, Address, membershipLevelType, memberType 
  - 엔티티 : Account (address, email, id, memberType 등) 
  - 값객체 : entity를 설명하는 객체로서 Embeddable 어노테이션 추가하여 정의함. id가 없으며 값전체가 객체로 선언됨 (Address 주소와 우편번호로 분리)
  - 표준타입 (enum type으로 선언) : 회원등급, 회원유형 구분 
 

8-5차.마이크로서비스 구현#3 - 서비스간 연계 구현  
ㅇ 쇼핑몰 서비스 업무 흐름 (구성도) 
구매자 상품 구매 -> PurchaseService (구매번호, 구매자, 수취자, 상품이름, 상품가격, 구매수량) 
구매자 조회 -> AccountService (회원이름, 주소) : 구매자를 조회할 수 있는 Rest API 추가 
상품 조회 -> ProductService (제품번호, 제품명, 내역)
구매내역 발행 -> Queue -> 구매내역 구독 
배송현황 조회 -> DeliveryService
