# Jersey框架

## 注解

### @Produces

@Produces指定http响应的MIME类型，默认是*/*，表示任意的MIME类型。该注解的值是数组类型，支持多个MIME类型，可以使用MediaType来指定MIME类型。

可以加在类上、方法上。

@Produces("application/json")其作用是指定 `response` 的 `content-type` 为 `application/json`

### @Consumes

@Consumes指定http请求的MIME类型，默认是*/*，表示任意的MIME类型。该注解的值是数组类型，支持多个MIME类型，可以使用MediaType来指定MIME类型。可以加在类上、方法上。

@Consumes(MediaType.APPLICATION_JSON)

相较于spring的@RequestMapping系列注解，Jersey将请求路径和请求方法拆解开来，如请求方法有：

### @GET

### @POST

### @PUT

### @DELETE

### @Path

指定请求路径

### 例

```java
@Path("/user")
public class UserRes {    
    //创建用户
    @PUT
    @Path("")
    @Consumes(MediaType.APPLICATION_JSON)
    @Produces(MediaType.APPLICATION_JSON)
    public String add(String params) {
        return "";
    }
}

```

## 坑

当请求传入一个时间的参数（附在url后面）时，如 2021-01-22，jersey只会将这个参数看为String类型，并且只能使用String来接收，不能使用Timestamp之类的时间类型来接收

```java
@GET
@Path("test")
public void test(@QueryParam("time") String time){ // 请求会进入本接口
    sout(time);
}

@GET
@Path("test")
public void test(@QueryParam("time") Timestamp time){ // 请求不会进入本接口
    sout(time);
}
```

如果将时间参数封装在一个类中，并以请求体提交，则可以映射到Timestamp上

```java
public class Time{
    private Timestamp time;
    // getter、setter方法
}

@GET
@Path("test")
public void test(Time time){ // 请求会进入本接口, 并成功映射请求体中的数据
    sout(time.getTime());
}
```

要注意的是，封装类似乎不需要加任何注解就可以使用，但如果想接收拼接在url后的参数，必须使用 @QueryParam("xxx") 与参数名称对应。这是GET请求使用请求体传输数据。

