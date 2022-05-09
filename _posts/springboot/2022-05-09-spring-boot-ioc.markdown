---
layout: post
title:  "Spring boot IoC"
categories: Java SpringBoot
---

### Bean
- Spring IoC Container 가 관리하는 object
- Spring Bean 저장소에 저장해놓고 사용
 
### Container
- bean 의 생성/관계/사용/생명주기 관리
- 등록된 bean 들은 시스템 전반적으로 사용 가능

### IoC (Inversion of Control)
- Spring에서 instance 생성과 object life cycle 을 spring container 가 담당
- 개발자 대신 framework 이 제어권을 가짐 

### DI (Dependency Injection)
- dependant 객체를 외부로부터 주입받는 방식
- 결합도를 줄이고 가독성을 높이고 유지 보수를 편하게 함
- spring container가 DI 를 수행
- singleton 으로 생성/관리됨
#### injection
- 외부에서 생성된 instance 를 interface 를 통해 전달 받음 
 - 결합도를 낮춤
 - 런타임시에 의존관계가 결정되기 때문에 유연한 구조를 가짐
#### injection 방식
- field injection
```java
@Controller
public class SampleController {
    @Autowired
    private SampleService sampleService;
}
``` 
- setter injection
```java
@Controller
public class SampleController {
    private SampleService sampleService;
 
    @Autowired
    public void setSampleService(SampleService sampleService) {
        this.sampleService = sampleService;
    }
}
```
- constructor injection
```java
@Controller
public class SampleController {
	private final SampleService sampleService;
	
	public SampleController(SampleService sampleService) {
	    this.sampleService = sampleService;	
    }
}
```