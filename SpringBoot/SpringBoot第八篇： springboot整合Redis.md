## SpringBoot第八篇： springboot整合Redis

这篇文章主要介绍springboot整合redis，至于没有接触过redis的同学可以看下这篇文章：5分钟带你入门Redis。

## 引入依赖：

在pom文件中添加redis依赖：

```xml
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

## 配置数据源

```java
spring.redis.host=localhost
spring.redis.port=6379
#spring.redis.password=
spring.redis.database=1
spring.redis.pool.max-active=8
spring.redis.pool.max-wait=-1
spring.redis.pool.max-idle=500
spring.redis.pool.min-idle=0
spring.redis.timeout=0
```

如果你的redis有密码，配置下即可。经过上述两步的操作，你可以访问redis数据了。

## 数据访问层dao

通过redisTemplate来访问redis.

```java
@Repository
public class RedisDao {

    @Autowired
    private StringRedisTemplate template;

    public  void setKey(String key,String value){
        ValueOperations<String, String> ops = template.opsForValue();
        ops.set(key,value,1, TimeUnit.MINUTES);//1分钟过期
    }

    public String getValue(String key){
        ValueOperations<String, String> ops = this.template.opsForValue();
        return ops.get(key);
    }
}
```

## 单元测试

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class SpringbootRedisApplicationTests {

    public static Logger logger= LoggerFactory.getLogger(SpringbootRedisApplicationTests.class);
    @Test
    public void contextLoads() {
    }

    @Autowired
    RedisDao redisDao;
    @Test
    public void testRedis(){
        redisDao.setKey("name","forezp");
        redisDao.setKey("age","11");
        logger.info(redisDao.getValue("name"));
        logger.info(redisDao.getValue("age"));
    }
}
```

启动单元测试，你发现控制台打印了：

> forezp
>
> 11

单元测试通过；

源码下载：https://github.com/forezp/SpringBootLearning

## 参考资料

messaging-redis