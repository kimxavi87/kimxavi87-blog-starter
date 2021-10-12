---
title: Using @PageableDefault in webflux 
date: "2021-10-13"
---

- WebFlux Controller에 Paging 적용하려고 하니

```java
@RequiredArgsConstructor
@RestController
public class PlayerController {

    private final MemberRepository memberRepository;

    @GetMapping("/members")
    public List<Member> requestMembers(@PageableDefault(size = 10) Pageable pageable) {
        return memberRepository.findAll(pageable);
    }
}
```

- 아래와 같이 에러 발생

```java
No primary or default constructor found for interface org.springframework.data.domain.Pageable
```

- WebfluxConfig 추가 필요

```java
@Configuration
@ConditionalOnClass(EnableWebFlux.class)
@ConditionalOnWebApplication(type = ConditionalOnWebApplication.Type.REACTIVE)
public class WebfluxConfig implements WebFluxConfigurer {
    @Override
    public void configureArgumentResolvers(ArgumentResolverConfigurer configurer) {
        configurer.addCustomResolver(new ReactivePageableHandlerMethodArgumentResolver());
    }
}
```

[https://stackoverflow.com/questions/50730446/resolving-pageable-in-webflux](https://stackoverflow.com/questions/50730446/resolving-pageable-in-webflux)