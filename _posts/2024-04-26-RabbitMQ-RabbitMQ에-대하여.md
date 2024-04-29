---
title: RabbitMQ - RabbitMQ에 대하여
author: kyoungsu0717
date: 2024-04-26
categories: [ RabbitMQ ]
tags: [ RabbitMQ, Message Broker ]
image:
  path: /assets/img/20240426/thumbnail.png
  alt: RabbitMQ.
---

## 📝 RabbitMQ - RabbitMQ 에 관하여

Redis 를 공부하면서 Redis 의 Mesaage Queuing 을 공부하다가 Kafka 와 RabbitMQ 에 대해 알게 되었다.
내가 일하고 있는 회사에서 Kafka 를 사용하기에는 시스템 대비 Learning Curve 및 데이터 사용량이 적기도 하고 RabbitMQ 가 조금더 적합해보여서 정리해보려고 한다.

<br>

---

<br>

## 🚩 RabbitMQ 란?

> AMQP를 구현하여 서버간 메세지(데이터)를 전달해주는 오픈 소스 메시지 브로커 소프트웨어 ( 메시지 지향 미들웨어 )
{: .prompt-info }

<br>

### 📝 AMQP ( Advanced Message Queuing Protocol) 란?

> 메시지 지향 미들웨어를 위한 개방형 표준 응용 계층 프로토콜
{: .prompt-info }

<br>

## 🚩 RabbitMQ 구성

![](/assets/img/20240426/image01.png)

### 📝 1. Message

- 서버간 처리하고자 하는 메세지(데이터)

### 📝 2. Producer

- Message 를 송신하는 주체
- Message 를 Consumer 에게 전달하기 위해 Message 를 Exchange 에 발행한다.
- Queue 에 직접 접근하지 않고, 항상 Exchange 를 통해 접근한다.

### 📝 3. Exchange

- Producer 에게 전달받은 메세지(데이터)를 Queue 에게 전달해주는 Router 역할
- Exchange 는 Message 를 어떤 Queue 에 추가할지, 버려야할지 등 규칙에 의해 결정한다.
- Exchange 의 규칙 : Fanout, Direct, Topic, Headers

### 📝 4. Bindings

- Exchange 와 Queue 의 관계
- Binding 과정이 있어야 Exchange 에서 Queue 로 메세지(데이터)가 이동한다.
- 통상적으로 Exchange 가 어느 Queue 에 Binding 할지 정의해야한다.

### 📝 5. Queues

- Consumer 에게 메세지(데이터)를 전달하는 역할
- In-Memory or Disk 에 Message 를 보관
- Queue 는 이름으로 구분된다.
  &#8251; 같은 이름과 같은 설정으로 Queue 를 생성하면 에러 없이 기존 Queue 에 연결되지만, 같은 이름과 다른 설정으로 Queue 를 생성하려고 시도하면 에러가 발생하니 주의해야 한다.

### 📝 6. Consumer

- Message 를 수신하는 주체
- Queue 에 직접 접근하여 메세지(데이터)를 가져온다.

<br>

## 🚩 Exchange 종류

|   종류    |                                 설명                                  |         특징          |
|:-------:|:-------------------------------------------------------------------:|:-------------------:|
| Direct  |             Routing Key 가 정확히 일치하는 Queue 에게 Message 전송              | Unicast / Multicast |
|  Topic  |             Routing Key 의 패턴이 일치하는 Queue 에게 Message 전송              |      Multicast      |
| Headers | ( Key:Value ) 로 이루어진 Header 값 을 <br/>기준으로 일치하는 Queue 에게 Message 전송	 |      Multicast      |
| Fanout  |             해당 Exchange 에 등록 된 모든 Queue 에게 Message 전송	              |      Broadcast      |

### 📝 1. Direct ( Unicast / 1:1 or 1:N )

![](/assets/img/20240426/image02.png)
<figcaption style="text-align:center; font-size:15px; color:#808080; margin-top:40px">
  "Exchange Type = Direct Example 01"
</figcaption>

![](/assets/img/20240426/image03.png)
<figcaption style="text-align:center; font-size:15px; color:#808080; margin-top:40px">
  "Exchange Type = Direct Example 02"
</figcaption>

- RabbitMQ 에서 사용되는 Exchange 의 Default Option 이다.
- RabbitMq 에서 생성되는 Queue 가 자동으로 Binding 되고, Queue 의 이름이 routing key 로 지정된다.
- 하나의 Queue 에 여러개의 routing key 를 지정할 수 있다. ( Ex. Exchange Type = Direct Example 01 )
- 여러개의 Queue 에 동일한 routing key 를 지정할 수 있다. ( Ex. Exchange Type = Direct Example 02 )

### 📝 2. Topic ( Multicast / 1:N )

![](/assets/img/20240426/image04.png)
<figcaption style="text-align:center; font-size:15px; color:#808080; margin-top:40px">
  "Exchange Type = Topic Example"
</figcaption>

- routing key 의 패턴이 일치하는 Queue 에 메세지(데이터)를 전달한다.
- Direct 보다 좀 더 유연하게 정의해서 메세지(데이터)를 전송할 수 있다.
- 규칙
  - \* : 하나의 단어를 의미한다.
  - \# : 0개 이상의 단어를 의미한다.
- 예시
  - routing Key : quick.orange.male.rabbit
    - 메세지(데이터)를 받는 Queue 가 없다.
  - routing Key : quick.orange.fox
    - Q1만 메세지(데이터)를 받는다.
  - routing Key : lazy.orange.male.rabbit
    - Q2만 메세지(데이터)를 받는다.

### 📝 3. Headers ( Multicast / 1:N )

![](/assets/img/20240426/image05.png)
<figcaption style="text-align:center; font-size:15px; color:#808080; margin-top:40px">
  "Exchange Type = Headers Example"
</figcaption>

- key-value 로 정의된 헤더에 의해 메세지를 Queue 에 전달하는 방법.
- Producer 가 정의하는 Header 의 key-value 와 Consumer 가 정의한 argument 의 key-value가 일치하면 Binding 된다.
- x-match : all

  - Producer 가 정의하는 Header 의 key-value 와 Consumer 가 정의한 argument 의 key-value 가 정확히 일치해야 Binding 된다.
- x-match : any

  - Producer 가 정의하는 Header 의 key-value 와 Consumer 가 정의한 argument 의 key-value 중 하나라도 일치하면 Binding 된다.

### 📝 4. Fanout ( Broadcast / 1:ALL )

![](/assets/img/20240426/image06.png)
<figcaption style="text-align:center; font-size:15px; color:#808080; margin-top:40px">
  "Exchange Type = Fanout Example"
</figcaption>

- 사진과 같이 Exchange 에서 메세지(데이터)를 거르지 않고 Binding 되어 있는 모든 Queue 에 메세지(데이터)를 전송한다.

<br>

## 🚩 RabbitMQ & Spring Boot 연동

### 🔨 1. Pom.xml

```xml
<!--        RabbitMQ Dependency-->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-amqp</artifactId>
</dependency>
```

### 🔨 2. application.yml

```yaml
spring:
  rabbitmq:
    host: localhost // RabbitMQ host ip
    port: 5672 // RabbitMQ port
    username: guest // RabbitMQ 웹 관리 콘솔 아이디
    password: guest // RabbitMQ 웹 관리 콘솔 비밀번호
    queue:
      name: sample-queue // 사용할 queue 이름
    exchange:
      name: sample-exchange // 사용할 exchange 이름
    routing:
      key: key

```

### 🔨 3. RabbitMqConfig Class

```java
import lombok.RequiredArgsConstructor;
import org.springframework.amqp.core.Binding;
import org.springframework.amqp.core.BindingBuilder;
import org.springframework.amqp.core.DirectExchange;
import org.springframework.amqp.core.Queue;
import org.springframework.amqp.rabbit.connection.CachingConnectionFactory;
import org.springframework.amqp.rabbit.connection.ConnectionFactory;
import org.springframework.amqp.rabbit.core.RabbitTemplate;
import org.springframework.amqp.support.converter.Jackson2JsonMessageConverter;
import org.springframework.amqp.support.converter.MessageConverter;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@RequiredArgsConstructor
@Configuration
public class RabbitMQConfig {
  @Va

  @Value("${spring.rabbitmq.queue.name}")
  private String queueName;

  @Value("${spring.rabbitmq.exchange.name}")
  private String exchangeName;

  @Value("${spring.rabbitmq.routing.key}")
  private String routingKey;

  @Value("${spring.rabbitmq.host}")
  private String host;

  @Value("${spring.rabbitmq.port}")
  private int port;

  @Value("${spring.rabbitmq.username}")
  private String userName;

  @Value("${spring.rabbitmq.password}")
  private String password;

  // org.springframework.amqp.core.Queue
  @Bean
  public Queue queue() {
    return new Queue(queueName);
  }

  /**
   * 지정된 Exchange 이름으로 Direct Exchange Bean 을 생성
   */
  @Bean
  public DirectExchange directExchange() {
    return new DirectExchange(exchangeName);
  }

  /**
   * 주어진 Queue 와 Exchange 을 Binding 하고 Routing Key 을 이용하여 Binding Bean 생성
   * Exchange 에 Queue 을 등록
   **/
  @Bean
  public Binding binding(Queue queue, DirectExchange exchange) {
    return BindingBuilder.bind(queue).to(exchange).with(routingKey);
  }

  /**
   * RabbitMQ 연동을 위한 ConnectionFactory 빈을 생성하여 반환
   **/
  @Bean
  public CachingConnectionFactory connectionFactory() {
    CachingConnectionFactory connectionFactory = new CachingConnectionFactory();
    connectionFactory.setHost(this.host);
    connectionFactory.setPort(this.port);
    connectionFactory.setUsername(this.userName);
    connectionFactory.setPassword(this.password);
    return connectionFactory;
  }

  /**
   * RabbitTemplate
   * ConnectionFactory 로 연결 후 실제 작업을 위한 Template
   */
  @Bean
  public RabbitTemplate rabbitTemplate(ConnectionFactory connectionFactory) {
    RabbitTemplate rabbitTemplate = new RabbitTemplate(connectionFactory);
    rabbitTemplate.setMessageConverter(jackson2JsonMessageConverter());
    return rabbitTemplate;
  }

  /**
   * 직렬화(메세지를 JSON 으로 변환하는 Message Converter)
   */
  @Bean
  public MessageConverter jackson2JsonMessageConverter() {
    return new Jackson2JsonMessageConverter();
  }
}
```

### 🔨 4. RabbitMqService Class

```java

@RequiredArgsConstructor
@Service
public class RabbitMqService {

  @Value("${spring.rabbitmq.queue.name}")
  private String queueName;

  @Value("${spring.rabbitmq.exchange.name}")
  private String exchangeName;

  @Value("${spring.rabbitmq.routing.key}")
  private String routingKey;

  private final RabbitTemplate rabbitTemplate;

  /**
   * 1. Queue 로 메세지를 발행
   * 2. Producer 역할 -> Direct Exchange 전략
   **/
  public void sendMessage(MessageDto messageDto) {
    log.info("messagge send: {}", messageDto.toString());
    this.rabbitTemplate.convertAndSend(exchangeName, routingKey, messageDto);
  }

  /**
   * 1. Queue 에서 메세지를 구독
   **/
  @RabbitListener(queues = "${rabbitmq.queue.name}")
  public void receiveMessage(MessageDto messageDto) {
    log.info("Received Message : {}", messageDto.toString());
  }
}
```

### 🔨 5. RabbitMqService Class

```java

@RequiredArgsConstructor
@RestController
public class RabbitMqController {
  private final RabbitMqService rabbitMqService;

  @PostMapping("/send/message")
  public ResponseEntity<String> sendMessage(
    @RequestBody MessageDto messageDto
  ) {
    this.rabbitMqService.sendMessage(messageDto);
    return ResponseEntity.ok("Message sent to RabbitMQ");
  }
}
```


