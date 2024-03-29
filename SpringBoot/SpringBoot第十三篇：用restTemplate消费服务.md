## SpringBoot第十三篇：用restTemplate消费服务

## 构架工程

创建一个springboot工程，去消费RESTFUL的服务。这个服务是 http:///gturnquist-quoters.cfapps.io/api/random ，它会随机返回Json字符串。
在Spring项目中，它提供了一个非常简便的类，叫RestTemplate，它可以很简便的消费服务。

## 消费服务

通过RestTemplate消费服务，需要先context中注册一个RestTemplate bean。代码如下：

```java
@Bean
    public RestTemplate restTemplate(RestTemplateBuilder builder) {
        return builder.build();
    }

    @Bean
    public CommandLineRunner run(RestTemplate restTemplate) throws Exception {
        return args -> {
            String quote = restTemplate.getForObject(
                    "http://gturnquist-quoters.cfapps.io/api/random", String.class);
            log.info(quote.toString());
        };
    }
```

运行程序，控制台打印：

> {
>    “type”: “success”,
>    “value”: {
>        “id”: 6,
>        “quote”: “It embraces convention over configuration, providing an experience on par with frameworks that excel at early stage development, such as Ruby on Rails.”
>    }
> }
