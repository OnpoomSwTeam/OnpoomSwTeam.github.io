---
title: Redis - Spring 과 Redis 그리고 Message Queue
author: kyoungsu0717
date: 2024-04-05
categories: [ Redis ]
tags: [ Redis, Message Broker ]
image:
  path: /assets/img/20240405/thumbnail.png
  alt: Redis.
---

# 🚩 Spring 과 Redis 그리고 Message Queue

저번 Redis 의 포스팅에서 Local Cache 를 넘어 Global Cache 의 Redis 에 관해서 작성하였다. <br>
이번에는 Redis 에서 제공하는 Message Queue 기능에 대해서 작성해보려고 한다. <br>
우리 팀은 지금까지 데이터를 전달하는데 있어서 프로그램들이 같은 Local 에 존재하여도 상호간의 TCP/UDP 를 이용하여 데이터를 전달하였다. <br>
그러다보니 연결관리 및 데이터 편린화 방지 등 불필요한 공수가 발생하여서, 이러한 문제를 Redis 의 Message Queue 의 기능을 사용하여 개선해보려고 글을 작성하게 되었다. <br>

## 📝 Message Broker ( 메시지 브로커 ) 란 ?
Redis 의 Message Queue 기능을 알아보기 전에 Message Broker 에 대한 개념부터 알아야한다.

### ■ Message Broker 의 정의

> * 애플리케이션, 시스템 및 서비스가 서로 간에 통신하고 정보를 교환할 수 있도록 해주는 소프트웨어
{: .prompt-info }

### ■ Message Broker 의 구조

![](assets/img/20240405/Message Broker 구조.png)

### ■ Message Broker 의 특징

1. 프로그램 간의 느슨한 결합 ( Decoupling ) <br>
   * 프로그램 간의 직접적인 연결이 아닌 미들웨어를 통한 연결이므로 직접적인 프로그램 간의 연결률을 낮춰준다

2. 전송 데이터의 안전한 보관 ( Persistence and Reliability ) <br>
   * TCP/UDP 와 달리 수신하려는 프로그램이 정상적으로 실행되어 있지 않더라도 송신하려는 프로그램에서 미들웨어까지의 데이터 송신이 가능하다.

3. 비동기 커뮤니케이션 ( Asynchronous communication ) <br>
   * 송신 프로그램과 수신 프로그램이 서로의 응답 메세지를 기다릴 필요 없이 서로 독립적으로 운영되도록 설계할 수 있다.

4. 확장의 용이 ( Scalability ) <br>
   * 통신하려는 프로그램이 많아지면 기본적인 세션이 늘어나지만 Message Broker 를 사용하면 새로운 프로그램도 Message Broker 에 붙이기만 하면 되므로 확장이 용이하다. 

### ■ Message Broker 의 단점
1. 성능 병목 가능성
   * 아무래도 데이터들이 Message Broker 로 몰리기 때문에 성능 병목이 올 수 있지만 다행이도 요즘 Message Broker 들이 확장성 및 성능이 좋아서 해당 문제가 발생될 확률은 매우 적다.

2. 운영 복잡도 부가
   * 운영하고 관리해야될 프로그램이 하나 늘어나고 해당 프로그램이 핵심적인 요소를 차지하기에 운영요소가 증가함에는 어쩔수가 없다.

<br>
이상으로 Message Broker 에 대한 간단한 개념을 알아보았다. <br>
다음으로는 Redis 의 Message Queue 기능에 대해서 작성해보겠다.

## 📝 Redis - Message Queue ( Publish / Subscribe )  란 ?
> * Redis 에서 Global Cache & Queue 기능을 활용하여 구현된 최소화된 Message Broker
{: .prompt-info }

> * 다른 Message Broker 와 다르게 클라이언트에게 메세지를 전달한 후 바로 삭제된다.
{: .prompt-danger }

## 📝 Redis - Message Queue 구조
![](assets/img/20240405/Redis Pub-Sub 구조.png)

## 📝 Spring Boot & Redis - 기본 연동
이제부터 Spring Boot & Redis 의 기본적인 연동 방법을 작성해보려고한다.
Redis 를 사용하는데 있어서 Template 방식과 Repository 방식이 있는데 Repository 방식을 선택하여 Global Cache 를 사용할 것이다.

### 1. Pom.xml
```xml
<!--        Netty Dependency 포함-->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

### 2. application.yml
```yml
spring:
  redis:
    host: # Redis Server IP
    port: # Redis Server Port
    publisher:
      topic: #publisher topic
    subscriber:
      topic: #subscriber topic
```

### 3. Spring Boot Main Class
Spring Boot Main Class Annotation 에서 Redis Repository 를 인식할 수 있도록 설정한다.
```java
@SpringBootApplication
@ImportResource({"classpath:/egovframework/springmvc/dispatcher-servlet.xml", "classpath*:/egovframework/spring/context-*.xml"})
@Import(EgovBootInitialization.class)
@ComponentScan(basePackages = {"기본 Package 경로"})
@EnableRedisRepositories(basePackages = {"Redis Repository Package 경로"})
@EnableJpaAuditing
public class EgovBootApplication {

    public static void main(String[] args) {
        SpringApplication springApplication = new SpringApplication(EgovBootApplication.class);
        springApplication.setBannerMode(Banner.Mode.OFF);
        springApplication.setLogStartupInfo(false);
        springApplication.run(args);
    }

}
```

### 4. Domain Class
Spring - Data - JPA 와 동일하게 Repository 에서 사용할 Domain Class 를 생성한다.
```java
@Getter
@NoArgsConstructor(access = AccessLevel.PROTECTED)
@AllArgsConstructor
@Builder
@RedisHash(value = "user_auth_info")
@ToString
public class UserAuthInfo {
    @Id
    private String userId;

    @Indexed
    private String index;

    private String value;

    @TimeToLive
    private long ttl;
}
```

### 5. Repsitory Interface
Spring - Data - JPA 와 동일하게 Repository Interface 를 생성한다.

```java
public interface UserAuthInfoRepository extends CrudRepository<UserAuthInfo, String> {
  Optional<UserAuthInfo> findByIndex(String index);
}
```

* Repository 의 사용법은 Spring-Data-JPA 와 매우 흡사하나 원하는 필드로 검색을 하려면 해당 필드에 @Indexed Annotation 을 붙여줘야한다.
* Repository 에서 원하는 필드로 검색하는 Method 를 작성한다.

## 📝 Spring Boot & Redis - Message Queue 기능 구현
다음으로는 Redis 를 활용한 Message Queue 기능에 대한 구현이다.
간단하게 Pub / Sub 를 설정해주면 끝난다.

### 1. Publisher Class
Pub 는 어려운 것도 없다.
해당 Class 를 Service Layer 에서 DI 하여 사용하면 된다.
```java
@Component
@RequiredArgsConstructor
public class Publisher {
    private static final Logger syncLogger = LoggerFactory.getLogger(Publisher.class);
    private static final Logger asyncLogger = LoggerFactory.getLogger("asyncLogger");


    @Value("${spring.redis.publisher.topic}")
    private String topic;
    private final StringRedisTemplate redisTemplate;

    public void publish(String message) {
        redisTemplate.convertAndSend(topic, message);
    }

}
```

### 2. Subscriber Class
Sub 할 때 처리된 Handler Class 이다.
```java
@Component
@RequiredArgsConstructor
public class Subscriber implements MessageListener {
  private static final Logger syncLogger = LoggerFactory.getLogger(Subscriber.class);
  private static final Logger asyncLogger = LoggerFactory.getLogger("asyncLogger");

  private final ISubscriberService subscriberService;
  private final StringRedisTemplate redisTemplate;

  @Override
  public void onMessage(Message message, byte[] pattern) {
    String publishMessage = redisTemplate.getStringSerializer().deserialize(message.getBody());

    try {
      subscriberService.process(publishMessage);
    } catch (JsonProcessingException e) {
      throw new RuntimeException(e);
    }
  }
}
```

### 3. Subscriber Config Class
Sub 는 Message 를 수신하기 위하여 Adapter 와 Container 를 설정해야한다.
설정법은 아래와 같다.

```java
@Configuration
@RequiredArgsConstructor
public class SubscriberConfig {
    private static final Logger syncLogger = LoggerFactory.getLogger(UserInfoController.class);
    private static final Logger asyncLogger = LoggerFactory.getLogger("asyncLogger");


    @Value("${spring.redis.subscriber.topic}")
    private String topic;
    private final Subscriber subscriber;

    @Bean
    public MessageListenerAdapter messageListenerAdapter(Subscriber subscriber) {
        return new MessageListenerAdapter(subscriber);
    }

    @Bean
    public RedisMessageListenerContainer container(RedisConnectionFactory connectionFactory, MessageListenerAdapter messageListenerAdapter) {
        RedisMessageListenerContainer redisMessageListenerContainer = new RedisMessageListenerContainer();
        redisMessageListenerContainer.setConnectionFactory(connectionFactory);
        redisMessageListenerContainer.addMessageListener(messageListenerAdapter, new PatternTopic(topic));

        return redisMessageListenerContainer;
    }

}
```
# 🚩 마치며..
우리팀에서 활용하기 위한 Redis 의 관한 기본 지식은 모두 정리한 것 같다.
조금 더 심화한다면 Message Broker 의 심화 개념인 Kafka 와 RabbitMQ 에 대해서 정리해보는 것도 좋을 것 같다.
