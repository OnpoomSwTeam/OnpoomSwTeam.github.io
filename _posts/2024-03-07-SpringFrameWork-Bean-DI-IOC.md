---
title: Spring - IOC, DI, Bean
author: eogud6780
date: 2024-03-07
categories: [Spring]
tags: [Spring, IOC, DI, Bean]
---

# 😁DI(Dependency Injection)

## 의존관계 주입

- 객체 간의 의존 관계를 외부에서 결정하고 주입하는 디자인패턴중 하나이다.
  DI를 통해 객체는 직접 자신이 필요로 하는 의존성을 생성하거나 참조하는것이 아닌
  외부에서 주입된 의존성을 사용함. 이를통해 결합도를 낮추고 유연성 향상이 목표

- 필름카메라와 필름이 한군데에서 의존하고있다면 결합도가 높은 일회용카메라 (X) - 별로임
- 필름카메라에 필름을 주입하여 의존하고있다면 결합도가 낮은 필름교체용카메라 (O) - 굿

---

> ### 1.생성자 주입 (🤞주로사용 )

- 생성자 호출시점에 딱 1번만 호출되는 것이 보장됩니다.
  생성자 주입은 객체를 생성할 때 딱 1번만 호출되므로 이후에 호출되는 일이 없음
  따라서 불변하게 설계할 수 있다.
- 생성자 주입을 활용할 경우 값의 불변을 보장할 수 있으므로 final을 활용가능
  생성자를 통해 의존성을 설정하고 변경할 일이 없으므로 final으로 안전하게 불변을 보장

```java
@Service
@RequiredArgsConstructor   <-- Lombok으로 스프링에서 DI(의존성 주입)의 방법 중에 생성자 주입을 자동으로 설정해주는 어노테이션
public class CommonServiceImpl implements ICommonService {
	private final SpeakerInfoRepository speakerInfoRepository;
}
```

> ### 2.setter(수정자) 주입

- 대부분의 의존관계 주입은 한번 일어나면 애플리케이션 종료시점까지 의존관계를 변경할 일이 거의 없다.
  하지만 setter는 언제든 변경되게 할 위험이 있음 -수정자 주입을 사용하면, setXxx 메서드를 public으로 열어두어야 함.
  즉 언제 어디서든 변경이 가능하다는 말임

```java
public  class  ExampleCase{
    	private  ChocolateService  chocolateService;
    	private  DrinkService  drinkService;

        @Autowired
    	public void  setChocolateService(ChocolateService  chocolateService){
    		this.chocolateService = chocolateService;
    	}

    	@Autowired
    	public void  setDrinkService(DrinkService  drinkService){
    		this.drinkService = drinkService;
    	}

    }
```

> ### 3.필드 주입

- 코드가 간결하지만, 외부에서 변경하기 힘들다. 프레임워크에 의존적이고 객체지향적으로 좋지 않다.

```java
@Component
public class OrderServiceImpl implements OrderService {

    @Autowired
    private final MemberRepository memberRepository;

    @Autowired
    private final DiscountPolicy discountPolicy;

}
```

> ### 4.method 주입

```java
@Component
public class OrderServiceImpl implements OrderService {

    private MemberRepository memberRepository;
    private DiscountPolicy discountPolicy;

    @Autowired
    public void init(MemberRepository memberRepository, DiscountPolicy discountPolicy){
        this.memberRepository = memberRepository;
        this.discountPolicy = discountPolicy;
    }
```

---

# 😁IOC(Inversion of Control)

## 제어의 역전

- 보통 일반적으로 개발자가 원하는대로 직접호출하고 제어하는 스타일로 개발을 하는데
  제어의 역전의 개념은 프레임워크같은게 내코드를 대신호출해주는것임

- ex) 스프링프레임워크 에서 빈을 등록하면 IOC 컨테이너에 저장하고 대신관리해주는 것
  개발자가 하는게 아닌 프레임워크가 대신해주므로 제어의 역전이 일어났다고 볼수있음.

---

> #### 스프링을 사용하지않은 일반 자바코드에서의 객체 생성과 의존성 주입 예시

```java
public class OrderService {
    private OrderServiceImPl orderServiceImpl;
    public OrderService() {
        this.orderServiceImpl = new OrderServiceImPl();
    }
 }
```

-위 코드에서 OrderService 클래스는 OrderServiceImPl 클래스에 의존하고 있다.
그리고 OrderService의 생성자에서 직접 OrderServiceImPl 객체를 생성하고 있다.
이러한 경우, OrderService는 OrderServiceImPl 강하게 결합되어 있어 유연성이 떨어지게된다.

> #### 스프링을 사용하여 제어의 역전이 일어난 예시

```java

public class OrderService {
    private final OrderDAO orderDAO;
    public OrderService(OrderDAO orderDAO) {
        this.orderDAO = orderDAO;
    }
}
```

- 위 코드에서는 OrderService클래스가 OrderDAO를 주입받아 사용하고있기 때문에 의존성이 약하며
  결합도가 낮아 유연해진다.

---

# 😁Bean

## -Spring IoC 컨테이너가 관리하는 자바 객체

> #### 스프링 Bean은 왜사용하는가❓

- 가장 큰 이유는 스프링 간 객체가 의존관계를 관리하도록 하는 것에 가장 큰 목적이 있다. 객체가 의존관계를 등록할 때 스프링 컨테이너에서 해당하는 빈을 찾고, 그 빈과 의존성을 만든다.

> #### Bean을 등록하는 방법

- xml에 직접 등록하기
- @Bean 어노테이션 이용하기
- @Component, @Controller, @Service, @Repository 어노테이션을 이용하기

> #### 1.xml에 직접 등록하기

```java
<bean id="helloService" class="com.example.myapp.di.HelloService"/>
<bean id="helloController" class="com.example.myapp.di.HelloController" p:helloService-ref="helloService"</bean>
```

> #### 2.@Bean 어노테이션 사용하기

```java
@Configurable
@ComponentScan(basePackages= {"com.example.myapp"})
@ImportResource(value= {"classpath:application-config.xml"})
public class AppConfig {
		@Bean
		public IHelloService helloService() {
			return new HelloService();
		}

		@Bean
		public HelloController helloController() {
			HelloController controller = new HelloController();
			controller.setHelloService(helloService());
			return controller;
		}
}
```

> #### 3. @Component, @Controller, @Service, @Repository 어노테이션을 이용

```java
@Controller
public class EmpController {
	private IEmpService empService;

	@Autowired
	public EmpController(IEmpService empService) {
		this.empService = empService;
	}

	void printInfo() {
		int count = empService.getEmpCount(50);
		System.out.println("사원의 수 : " + count);
	}
}
```
