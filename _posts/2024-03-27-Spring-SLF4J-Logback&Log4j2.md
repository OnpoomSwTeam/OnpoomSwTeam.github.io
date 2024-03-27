---
title:  Spring - SlF4j / Logback & Log4j2
author: eogud6780
date: 2024-03-27 
categories: [Web] 
tags: [Spring]
image:
  path: /assets/img/20240327/log.png 
  alt: Logger
---

## 👀Log란 무엇인가?
> 로그는 소프트웨어 이벤트를 시스템의 상태 및 동작 정보를 시간 경과에따라 기록하는 것이다.
소프트웨어 개발 과정 혹은 개발 후 동작상태를 파악하여 문제 진단 및 해결에 도움을 준다.
운영과 관리에 도움을 주는 좋은 데이터가 될 수있다.
로그를 기록하는 행위를 로깅(logging)이라고 하며 JAVA에서는 다양한 로깅 라이브러리를 지원한다.
하지만 각각 로깅 라이브러리의 내부동작을 이해하지 못하면 성능상 이슈가 발생할 수 있기때문에
내부동작을 이해하는것이 중요하다.

#### 디버깅 이나 System.out.println이랑 비교했을때 로깅 라이브러리를 사용할시 장점
- 상황별 Log Level을 지정해 Level별로 로깅이 가능하다
- 프로그램 실행에 대한 흐름과 에러 확인가능
- sysout에 비해 파일,클라우드,DB 등 출력 위치 및 다양한 출력 형식 지원
- 모듈,파일,메소드,클래스별로 유연하게 메세지 출력 가능

#### 로깅 라이브러리종류
- java.util.logging.Logger
- Log4j,Log4j2
- LogBack
- SLF4j

-> 자바에서는 Log4j -> Logback -> Log4j2 시간 순으로 라이브러리가 개발됨

#### Log Level 별 일반적인 용도 ( 팀이나 회사의 규칙에 따를수도있음 )
---
- 💖Trace : 가장 상세한 로그 레벨로,코드의 흐름을 따라가며 디버깅 정보를 기록
- 🧡Debug : 디버깅을 위한 로그 레벨로, 프로그램의 상태 및 실행중에 발생하는 중요한 이벤트를 기록
- 💛Info : 일반적인 정보를 기록하는 로그 레벨로, 프로그램의 주요 이벤트 및 상태 변경을 기록 / 에러는 아니지만 주시해야 할것을 기록
- 💚Warning : 예외적인 상황을 기록하는 로그 레벨로, 잠재적인 문제 또는 예상이못한 동작을 기록 / 에러는 아니지만 예외상황인 경우를 기록
- 💙Error : 심각한 에러를 기록하는 로그 레벨로, 잘못된 동작을 기록
- 💜Fatal : 가장 심각한 로그 레벨로, 오류를 기록하고 프로그램의 중단 또는 비정상 종료를 알림

---
   
#### 🔥 System.out.println()을 로깅에 사용하면 안좋은 이유가 무엇일까? 
 - sysout이 내부적으로 사용하는 wirte()와 newLine()이 동기화 메서드이다
 - Blocking I/O이다  ( 해당 I/O가 발생하는 작업시간동안 cpu가 놀기되기 때문)
 - 즉 성능상 좋지 않다.
 

## 👀SLF4J ( Simple Logger Facade For Java)
> SLF4J는 java.util.logging, logback 및 log4j와 같은 다양한 로깅 프레임 워크에 대한 추상화(인터페이스) 역할을 하는 라이브러리입니다. logger의 추상체로써, 인터페이스 이므로 SLF4j 인터페이스를 사용해서 로깅하게 되면 구현체만 갈아 끼우면 logback이나 log4j등으로 마이그레이션 할 수 있습니다. 구현체로는 logback, log4j2 등이 있습니다.

![](https://velog.velcdn.com/images/eogud2/post/f5c2da38-c5e8-44d0-b885-553f381a1341/image.png)


#### SLF4J의 구성요소
SLF4J는 세가지의 구성요소를 갖는다.
- SLF4J API
	- SLF4J를 사용하기 위한 인터페이스를 제공
   	- slf4j-api-{version}.jar를 통해 사용
 	- 반드시 하나의 바인딩만을 사용해야한다 ( 아니면 에러남 )
    <br>
- SLF4J 바인딩
	- SLF4J 인터페이스를 로깅 구현체(logback 또는 log4j)와 연결하는 어댑터 역할
    <br>
- SLF4J Bridging Modules
	- 다른 로깅 API로 Logger 호출을 할 때, SLF4J 인터페이스로 연결(redirect)하여 SLF4J API가 대신 Logger를 처리할 수 있도록 하는 어댑터 역할의 라이브러리
  	- 다른 로깅 API ==> Bridge(redirect) ==> SLF4J API
    
### 📍LogBack 
- logback이란 log4j 이후에 나왔으며 log4j 보다 향상되고 가장 널리 사용되고 있는 Java 로깅 라이브러리 입니다.
- slf4j의 구현체로써 스프링부트의 기본 log로 사용되고 있으며 spring-boot-starter-web 안에 spring-boot-starter-logging의 logback이 기본적으로 내장되어있어 따로 추가해줄 필요없다.
- Automatic Reloading 기능을 제공하여 따로 재시작없이 설정 변경후 사용 가능하다

스프링부트에서의 사용예제
```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

class LogbackLogger {
    private static Logger logger = LoggerFactory.getLogger(LogbackLogger.class);

    void method() {
        logger.trace("Trace");
        logger.debug("Debug");
        logger.info("Info");
        logger.warn("Warn");
        logger.error("Error");
    }
}

```
### 📍Log4j2
- Log4j2는 Log4j를 보안한 라이브러리입니다. 그리고 Facade 패턴으로 구현되어 다른 Log 라이브러리들과 사용할 수 있습니다. 그 예로 LogBack의 앞에서 Facade 형태로 사용되어질 수 있습니다.
- Log4j2의 가장 눈에 띄는 기능은 **비동기 성능** 입니다 또한 LMAX 디스럽터를 활용 하는데 이 라이브러리로 인해 12배 강한 성능을 제공한다.
- 동일 환경에서 Log4j2는 초당 18,000,000개의 메세지를 기록할 수 있는 반면 Logback과 Log4j는 1초당 2,000,000 개 미만의 메세지를 기록 할 수 있다.
- 사용을 위해서는 spring-boot-starter-logging 모듈을 exclude하고 spring-boot-starter-logging-log4j2의 의존성을 주입해야 한다
- 멀티 쓰레드 환경에서 성능이 우수하다.


### 📍Log4j2 - AsyncLogger
- 공식문서에 의하면 스레드가 늘어남에 따라 더 좋은성능을 보이는 비동기로거 사용을 권장한다.
![](https://velog.velcdn.com/images/eogud2/post/50f8a7b4-468f-424f-824d-f90843c46a8f/image.png)

- 그러나 무조건 비동기 로거가 좋은 것은 아니다.

#### 장점
 - 동기 로거에 비해 6~68배 빠르다
 - 대용량 로깅에 적합하다 ( 비동기로 처리하기때문 )
 
#### 단점
- 처리 과정에 예외가 발생하여 로깅에 실패한경우 비동기로 처리하기 때문에 문제상황을 알리기 힘들다.
- 새로운 스레드를 계속 생성하므로 CPU 자원이 부족한 하드웨어에서는 성능이점이 없을 수 있다.

### 📍동기 + 비동기 방식을 사용하자 ( Async + Sync Logger )
```yml
Configuration:
	Loggers:
    Root:
      level: info
      AppenderRef:
        - ref: RollingFile_Appender
    AsyncLogger:
      name: asyncLogger
      additivity: false
      level: debug
      includeLocation: false
      AppenderRef:
        - ref: RollingFile_Appender
```

### 🙄무엇을 사용해야할까?
- 로깅해야 할 양이 많고 성능이 중요하다면 Log4j2를 사용하는 것이 좋다
- Logback,Log4j2 둘 다 SLF4j를 구현하고 있기 때문에 교체하기에 편리하다 
 -> ( 내부적인 코드를 변경하지않고 dependency만 교체해주면되기 때문)
- 비동기 로거는 순서가 중요하지 않고 방대한 로그를 작성해야 할때 사용하면 좋을것같다.
- 동기 로거는 코드의 동작 순서가 중요할때 ( 디버깅용 ) 사용하면 좋을것같다.


   	
  
