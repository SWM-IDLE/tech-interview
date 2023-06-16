# 스프링 프레임워크

- 자바를 이용해 엔터프라이즈급 개발을 편리하게 만들어주는
‘오픈소스 경량급 애플리케이션 프레임워크’
- **애플리케이션 개발에 필요한 기반을 제공해서 개발자가 비즈니스 로직 구현에만 집중**

## 제어 역전(IOC)

- 일반적인 자바 개발의 경우 개발자가 객체를 생성하고 사용하는 작업을 직접 제어함
- 제어 역전을 특징으로 하는 스프링은 **객체의 생명주기 관리를 외부에 위임**함
- 이때 외부는 **스프링 컨테이너 또는 IOC 컨테이너**를 의미함
- 객체 관리를 컨테이너에게 맡겨 제어권이 넘어간 것이 제어 역전이며 제어 역전을 통해 의존성 주입, 관점 지향 프로그래밍 등이 가능해짐

## 의존성 주입(DI)

- 제어 역전의 방법 중 하나로, **사용할 객체를 직접 생성하지 않고 외부 컨테이너가 생성한 객체를 주입 받아 사용**하는 방식
- @Autowired 어노테이션을 통해 의존성 주입 가능
- 의존성을 주입 받는 세가지 방법
    - **생성자**를 통한 의존성 주입
    - **필드 객체 선언**을 통한 의존성 주입
    - **setter 메서드**를 통한 의존성 주입
- 생성자를 통한 의존성 주입이 권장됨; 레퍼런스 객체(주소값) 없이는 객체를 초기화할 수 없게 설계할 수 있기 때문

## 관점 지향 프로그래밍(AOP)

- **핵심 기능과 부가 기능으로 구분**해 관점을 기준으로 묶어 개발하는 방식
    - 핵심 기능: 비즈니스 로직이 처리하려는 목적 기능
    - 부가 기능: 로깅, 트랜잭션 처리 등
- 여러 비즈니스 로직에서 **반복되는 부가 기능을 하나의 공통 로직으로 처리하도록 모듈화해 삽입**하는 방식
    - 컴파일 과정에서 삽입 하는 방식
    - 바이트코드를 메모리에 로드하는 과정에서 삽입하는 방식
    - 프락시 패턴을 이용한 방식 ← 스프링에서 활용

## 스프링 프레임워크의 다양한 모듈

<img width="716" alt="image" src="https://github.com/SWM-IDLE/tech-interview/assets/78717113/11809a45-c275-4838-9b3a-81fb7e3164e0">

# 스프링 프레임워크 vs 스프링 부트

- 필요한 모듈을 추가하다 설정이 복잡해지는 문제를 해결하기 위해 등장한 스프링부트

## 의존성 관리

- 스프링 프레임워크에서는 각 모듈의 의존성을 직접 설정, 호환되는 버전 명시 필요
- 따라서 버전을 올리는 상황에서 연관된 다른 라이브러리 버전도 고려해야 함
- 스프링 부트에서는 **spring-boot-starter 의존성**을 제공
    - **각 라이브러리의 기능과 관련해 자주 사용되고 서로 호환되는 모듈 조합** 제공
    - web, test, jdbc, security, data-jpa, cache 등의 라이브러리
    - 여러 라이브러리 사용 시 의존성이 겹칠 수 있음 → spring-boot-starter-parent가 검증된 조합을 제공함

## 자동 설정

- 스프링 프레임워크의 기능을 사용하기 위한 자동 설정을 지원함
- 애플리케이션에 **추가된 라이브러리를 실행하는 데 필요한 환경 설정**을 알아서 해줌
- 즉 개발에 필요한 의존성 추가 시 프레임워크가 이를 자동으로 관리해줌
    - ex) @SpringBootApplication

## 내장 WAS

- 스프링 부트의 각 웹 애플리케이션에는 내장 WAS가 존재함
    - ex) spring-boot-starter-web의 내장 톰캣

## 모니터링

- 스프링 부트 액추에이터라는 자체 모니터링 도구가 있음
    - 시스템이 사용하는 스레드, 메모리, 세션 등의 주요 요소 모니터링

# 스프링 부트의 동작 방식

- spring-boot-starter-web 모듈은 기본적으로 **톰캣을 사용하는 스프링 MVC 구조** 기반 동작

- **서블릿(Servlet): 클라이언트의 요청을 처리, 결과를 반환**하는 자바 웹 프로그래밍 기술
- **서블릿 컨테이너**: 서블릿 인스턴스를 생성하고 관리
    - **서블릿 객체를 생성, 초기화, 호출, 종료하는 생명주기 관리**
    - 서블릿 객체는 ***싱글톤 패턴***으로 관리됨
    - ***멀티 스레딩***을 지원함
- **톰캣이 WAS의 역할과 서블릿 컨테이너의 역할을 수행**하는 대표적인 컨테이너

<img width="740" alt="image" src="https://github.com/SWM-IDLE/tech-interview/assets/78717113/98e45645-b272-40eb-9c7e-3c09dab67890">

(1) DispatcherServlet으로 요청(HTTPServletRequset)이 들어오면 DispatcherServelet은 **핸들러 매핑을 통해 요청 URI에 매핑된 핸들러(컨트롤러)를 탐색**

(2) **핸들러 어댑터로 컨트롤러를 호출**

(3) 핸들러 어댑터에 컨트롤러의 응답이 돌아오면 **ModelAndView로 응답을 가공해 반환**

(4) 뷰 형식으로 리턴하는 컨트롤러를 사용할 때는 **뷰 리졸버를 통해 뷰를 받아 리턴**함

- 추가 자료
    
    <img width="571" alt="image" src="https://github.com/SWM-IDLE/tech-interview/assets/78717113/b95feddf-d73c-4d99-a493-624deee7926f">
    
    
    https://steady-coding.tistory.com/599
    

- **핸들러 매핑: 요청 정보를 기준으로 어떤 컨트롤러를 사용할지 선정**하는 인터페이스
    - BeanNameUrlHandlerMapping: 빈 이름을 URL로 사용하는 매핑 전략
    - ControllerClassNameHandlerMapping: URL과 일치하는 클래스 이름을 갖는 빈을 컨트롤러로 사용하는 전략
    - SimpleUrlHandlerMapping: URL 패턴에 매핑된 컨트롤러를 사용하는 전략
    - DefaultAnnotationHandlerMapping: 어노테이션으로 URL과 컨트롤러를 매핑
- View 사용 시 동작 방식

<img width="491" alt="image" src="https://github.com/SWM-IDLE/tech-interview/assets/78717113/5e3a1765-706d-43c9-b1c9-db86a99608fb">

- @RestController 사용 시 동작 방식

<img width="472" alt="image" src="https://github.com/SWM-IDLE/tech-interview/assets/78717113/d13f10fd-c2d0-4dfa-b90f-6f1cee57fc29">


- MessageConverter은 요청과 응답에 대해 Body 값을 변환하는 역할 수행 (스프링 부트에서 기본적으로 HttpMessageConverter 인터페이스를 빈으로 등록함)

# POJO 기반의 구성

- POJO(Plain Old Java Object): 다른 클래스나 인터페이스를 상속/구현 받아 메서드가 추가된 클래스가 아닌, 일반적으로 우리가 알고 있는 getter, setter 같이 기본적인 기능만 가진 자바 객체
- 즉 특정 기술에 종속되지 않는 순수한 자바 객체
- 트랜잭션, 보안 등 로우 레벨의 로직까지 작성해야하는 부담감을 없애고자 EJB(Enterprise Java Beans)가 만들어짐
- EJB를 통해 로우 레벨의 로직 개발에 대한 수고는 덜었지만, EJB를 상속/구현하게 되면서 가벼운 서비스 조차 무겁게 만들어지는 현상 발생 → 코드가 모두 EJB에 종속적
- JAVA의 기본 개념인 객체지향에 집중하고자 특정 클래스나 라이브러리에 종속되지 않는 POJO 구성으로 코드 작성

<aside>
💡 그럼 특정 기술규약과 환경에 종속되지 않으면 모두 POJO라고 말할 수 있는가? 많은 개발자가 크게 오해하는 것 중의 하나가 바로 이것이다. ...(중략)...

진정한 POJO란 객체지향적인 원리에 충실하면서, 환경과 기술에 종속되지 않고 필요에 따라 재활용될 수 있는 방식으로 설계된 오브젝트를 말한다.

.. 토비의 스프링에서 발췌

</aside>

- “분리됐지만 반드시 필요한 엔터프라이즈 서비스 기술을 POJO 방식으로 개발된 애플리케이션 핵심 로직을 담은 코드에 제공한다”는 것이 스프링의 가장 강력한 특징과 목표

<img width="603" alt="image" src="https://github.com/SWM-IDLE/tech-interview/assets/78717113/eb9634b1-6522-4d78-8940-e0a5235eab3a">

- 스프링의 주요 기술인 Ioc/DI, AOP, PSA는 애플리케이션을 POJO로 개발할 수 있게 해주는 가능 기술들이며, 이것들은 모두 스프링이 있기 전에도 여러 가지 형태로 시도됐고 발전하던 기술이라고 함

# PSA

- Portable Service Abstraction
- 추상화 계층을 사용하여 어떤 기술을 내부에 숨기고 개발자에게 편의성을 제공해주는 것을 서비스 추상화(Service Abstraction)라고 함
    - ex) @Transactional 어노테이션을 선언하는 것만으로 별도의 코드 추가 없이 트랜잭션 서비스 사용 → 내부적으로 트랜잭션 코드가 추상화되어 숨겨져있기 때문
- 하나의 추상화로 여러 서비스들을 묶어둔 것을 Spring에서 Portable Service Abstraction이라고 함
    - ex) JDBC, JPA(또는 mysql, mongoDB) 어떤 경우라도 @Transactional 사용 시 트랜잭션 유지
- 어떻게 가능할까?
    
    <img width="553" alt="image" src="https://github.com/SWM-IDLE/tech-interview/assets/78717113/ed2e721e-3bcd-4f27-9a52-54be1e11efe0">
    
    - Spring의 @Transactional은 각 TransactionManager를 각각 구현하고 있는 것이 아니라 최상위 PlatformTransactionManager을 이용하고, 필요한 TransactionManager를 DI로 주입 받아 사용하기 때문
- 즉 환경의 변화와 관계없이 일관된 방식으로 기술로의 접근 환경을 제공하는 추상화 구조
    - PSA = 잘 만든 인터페이스
    - ex) Spring Web MVC, Spring Transaction, Spring Cache

<aside>
💡 Spring은 이렇게 특정 기술에 직접적 영향을 받지 않게끔 객체를 POJO 기반으로 한번씩 더 추상화한 Layer를 갖고 있으며,

**이를통해 일관성있는 Service Abstraction(서비스 추상화)를 만들어 냅니다.**

덕분에 코드는 더 견고해지고 기술이 바뀌어도 유연하게 대처할 수 있게 됩니다.

</aside>

# 서블릿

- 서블릿은 웹 프로그래밍에서 클라이언트 요청을 처리하고, 처리 결과를 클라이언트에 전송하는 기술로써, 자바로 구현된 CGI(Common gateway Interface)
    - CGI: 별도로 제작된 웹 서버와 프로그램간의 교환 방식 — 어떠한 프로그래밍 언어로도 구현 가능
- 쉽게 말하자면 자바를 사용해서 웹을 만들기 위해 필요한 기술

<img width="568" alt="image" src="https://github.com/SWM-IDLE/tech-interview/assets/78717113/d93165b8-ecae-4eb6-b2a8-7f859c518629">

[https://velog.io/@oyoungsun/Spring-서블릿Servlet-이란](https://velog.io/@oyoungsun/Spring-%EC%84%9C%EB%B8%94%EB%A6%BFServlet-%EC%9D%B4%EB%9E%80)

- 서블릿의 특징
    - 클라이언트의 요청에 대해 동적으로 작동하는 웹 어플리케이션 컴포넌트
    - html을 사용해서 요청에 응답
    - java thread를 통해 동작함
    - mvc 패턴 중 controller로 이용됨
    - http 프로토콜 서비스를 지원하는 javax.servlet.httl.HttpServlet 클래스를 상속 받음
    - html 변경 시 servlet을 재 컴파일 해야 함
- 서블릿의 동작 방식
    
    <img width="578" alt="image" src="https://github.com/SWM-IDLE/tech-interview/assets/78717113/19f24c40-e409-40b3-b7e6-05f3c830ec80">
    
    [https://velog.io/@oyoungsun/Spring-서블릿Servlet-이란](https://velog.io/@oyoungsun/Spring-%EC%84%9C%EB%B8%94%EB%A6%BFServlet-%EC%9D%B4%EB%9E%80)
    
    1. 사용자가 url 클릭 시 http request를 서블릿 컨테이너로 전송
    2. 요청을 전송 받은 서블릿 컨테이너는 HttpServletRequest, HttlServletResponse 두 객체를 생성
    3. web.xml은 사용자가 요청한 url을 분석해 어느 서블릿으로 요청한 것인지 탐색
    4. 해당 서블릿에서 service 메서드를 호출한 후 클라이언트의 요청 종류(get, post)에 따라 doGet 또는 doPost 호출
    5. doGet, doPost 메서드는 동적 페이지를 생성한 후 HttpServletResponse 객체에 응답을 보냄
    6. 응답이 끝나면 HttpServletRequest, HttlServletResponse 두 객체를 소멸시킴
- 서블릿 컨테이너
    - 서블릿 컨테이너는 서블릿을 관리해주는 컨테이너 — 대표적으로 톰캣
    - 서블릿 컨테이너는 클라이언트의 요청을 받고, 응답할 수 있도록 웹 서버와 소켓을 만들어 통신함

# 레이어드 아키텍처

- 애플리케이션의 **컴포넌트를 유사 관심사 기준 레이어로 묶어 수평적으로 구성**한 구조
- **프레젠테이션 계층**
    - 애플리케이션의 최상단 계층으로, **클라이언트의 요청을 해석하고 응답**하는 역할
    - UI나 API를 제공
    - 별도의 **비즈니스 로직 포함X**, 비즈니스 계층으로 요청을 위임하고 받은 결과를 응답하는 역할만 수행
- **비즈니스 계층**
    - 애플리케이션이 제공하는 **기능을 정의하고 세부 작업을 수행하는 도메인 객체**를 통해 업무를 위임하는 역할을 수행함
    - DDD(Domain-Driven Design)기반의 아키텍처에서는 비즈니스 로직에 도메인이 포함되기도 하고, 별도로 도메인 계층을 두기도 함
- **데이터 접근 계층**
    - 데이터베이스에 접근하는 일련의 작업을 수행함

## 레이어드 아키텍처 기반 설계의 특징

- 각 레이어는 **가장 가까운 하위 레이어의 의존성을 주입** 받음
- 각 레이어는 관심사에 따라 묶여 있으며, 다른 레이어의 역할을 침범하지 않음
    - 각 컴포넌트의 역할이 명확하므로 **코드의 가독성과 기능 구현에 유리**함
    - **코드의 확장성**이 좋아짐
- 각 레이어가 독립적으로 작성되면 다른 레이어와의 의존성을 낮춰 **단위 테이스에 용이**함

## 스프링의 레이어드 아키텍처

<img width="565" alt="image" src="https://github.com/SWM-IDLE/tech-interview/assets/78717113/0da3051b-4c57-401d-8fb8-b9a92141f9e9">

- **프레젠테이션(유저 인터페이스) 계층**
    - 클라이언트와의 접점이 됨
    - 클라이언트로부터 데이터와 함께 요청을 받고 처리 결과를 응답으로 전달하는 역할
- **비즈니스(서비스) 계층**
    - 핵심 비즈니스 로직을 구현하는 영역
    - 트랜잭션 처리나 유효성 검사 등의 작업도 수행함
- **데이터 접근(영속) 계층**
    - 데이터베이스에 접근해야 하는 작업을 수행함
    - Spring Data JPA에서는 DAO 대신 레포지토리로 대체 가능

- 비즈니스 로직은 ***도메인 계층***에서 담당하는 것이 일반적임
