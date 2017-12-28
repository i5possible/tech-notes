## Swagger Configuration
### Spring Boot

#### Dependencies

```xml
    <properties>
        <swagger.version>2.7.0</swagger.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger2</artifactId>
            <version>${swagger.version}</version>
        </dependency>
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger-ui</artifactId>
            <version>${swagger.version}</version>
        </dependency>
    </dependencies>
```

#### Configuration

```java
package com.kyeegroup.autotest.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

/**
 * @author lianghong
 * @date 12/12/2017
 */
@Configuration
@EnableSwagger2
public class SwaggerConfig {
    @Bean
    public Docket api() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.kyeegroup.autotest.controller"))
                .paths(PathSelectors.any())
                .build();
    }

    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("Spring Boot中使用Swagger2构建RESTful APIs")
                .description("Spring Boot中使用Swagger2构建RESTful APIs")
                .termsOfServiceUrl("http://swagger.io/")
                .contact("程序猿")
                .version("1.0")
                .build();
    }
}
```

如上代码所示，通过@Configuration注解，让Spring来加载该类配置。再通过@EnableSwagger2注解来启用Swagger2。再通过createRestApi函数创建Docket的Bean之后，apiInfo()用来创建该Api的基本信息（这些基本信息会展现在文档页面中）。select()函数返回一个ApiSelectorBuilder实例用来控制哪些接口暴露给Swagger来展现，本例采用指定扫描的包路径来定义，Swagger会扫描该包下所有Controller定义的API，并产生文档内容（除了被@ApiIgnore指定的请求）。

```java
// 添加Controller的说明
@RestController 
@RequestMapping("/product") 
@Api(value="onlinestore", description="Operations pertaining to products in Online Store") 
public class ProductController { .  . . . }

// 忽略某个Controller
@SwaggerIgnore

// 添加某个方法的说明
@ApiOperation(value = "View a list of available products", response = Iterable.class)
@RequestMapping(value = "/list", method= RequestMethod.GET,produces = "application/json")
public Iterable list(Model model){
    Iterable productList = productService.listAllProducts();
    return productList;
}

// 添加对返回值类型的说明
@ApiOperation(value = "View a list of available products", response = Iterable.class)
@ApiResponses(value = {
        @ApiResponse(code = 200, message = "Successfully retrieved list"),
        @ApiResponse(code = 401, message = "You are not authorized to view the resource"),
        @ApiResponse(code = 403, message = "Accessing the resource you were trying to reach is forbidden"),
        @ApiResponse(code = 404, message = "The resource you were trying to reach is not found")
}
)
@RequestMapping(value = "/list", method= RequestMethod.GET, produces = "application/json")
public Iterable list(Model model){
    Iterable productList = productService.listAllProducts();
    return productList;
}

// 添加对实体类的说明
import io.swagger.annotations.ApiModelProperty;
import javax.persistence.*;
import java.math.BigDecimal;
import lombok.*;

@Entity
@Getter
@Setter
public class Product {
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @ApiModelProperty(notes = "The database generated product ID")
    private Integer id;
    @Version
    @ApiModelProperty(notes = "The auto-generated version of the product")
    private Integer version;
    @ApiModelProperty(notes = "The application-specific product ID")
    private String productId;
    @ApiModelProperty(notes = "The product description")
    private String description;
    @ApiModelProperty(notes = "The image URL of the product")
    private String imageUrl;
    @ApiModelProperty(notes = "The price of the product", required = true)
    private BigDecimal price;
}
```




### Spring MVC

#### Dependencies

```xml
    <properties>
        <swagger.version>2.7.0</swagger.version>
        <jackson.version>2.6.6</jackson.version>
    </properties>
    
    <dependencies>
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger2</artifactId>
            <version>${swagger.version}</version>
        </dependency>
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger-ui</artifactId>
            <version>${swagger.version}</version>
        </dependency>

        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>${jackson.version}</version>
        </dependency>
    </dependencies>
```

如果启动项目时出现com/fasterxml/jackson/databind/ObjectMapper类找不到，就需要jackson-databind。

#### Java Configuration

```java
/*
省略 package name
*/
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import springfox.documentation.service.ApiInfo;
import springfox.documentation.service.Contact;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

@Configuration //必须存在
@EnableSwagger2 //必须存在
@EnableWebMvc //必须存在
@ComponentScan(basePackages = {"org.blog.controller"}) //必须存在 扫描的API Controller package name 也可以直接扫描class (basePackageClasses)
public class WebAppConfig{
    @Bean
    public Docket api() {
        return new Docket(DocumentationType.SWAGGER_2)
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.xxl.job.admin"))
                .paths(PathSelectors.any())
                .build();
    }

    private ApiInfo apiInfo() {
        Contact contact = new Contact("周发扬", "https://cc520.me", "yangyang_666@icloud.com");
        return new ApiInfo("Blog前台API接口",//大标题 title
                "Blog前台API接口",//小标题
                "0.0.1",//版本
                "www.fangshuoit.com",//termsOfServiceUrl
                contact,//作者
                "Blog",//链接显示文字
                "https://cc520.me"//网站链接
        );
    }
}
```

#### Controller Under basePackage Scanned

```java
import io.swagger.annotations.*;

@Controller
@RequestMapping(value = "/v1/api")
public class HomeApiController{

    //这里使用POST @RequestBody必须使用POST才能接收，这里方便讲解
    @ApiOperation(value = "一个测试API", notes = "第一个测试API")
    @ResponseBody
    @RequestMapping(value = "/test/{path}", method = RequestMethod.POST)
    @ApiImplicitParams({
            @ApiImplicitParam(name = "blogArticleBeen", value = "文档对象", required = true, paramType = "body", dataType = "BlogArticleBeen"),
            @ApiImplicitParam(name = "path", value = "url上的数据", required = true, paramType = "path", dataType = "Long"),
            @ApiImplicitParam(name = "query", value = "query类型参数", required = true, paramType = "query", dataType = "String"),
            @ApiImplicitParam(name = "apiKey", value = "header中的数据", required = true, paramType = "header", dataType = "String")
    })
    public JSONResult test(@RequestBody BlogArticleBeen blogArticleBeen,
                           @PathVariable Long path,
                           String query,
                           @RequestHeader String apiKey,
                           PageInfoBeen pageInfoBeen){
        System.out.println("blogArticleBeen.getLastUpdateTime():"+blogArticleBeen.getLastUpdateTime());
        System.out.println("blogArticleBeen.getSorter():"+blogArticleBeen.getSorter());
        System.out.println("path:"+path);
        System.out.println("query:"+query);
        System.out.println("apiKey:"+apiKey);
        System.out.println("pageInfoBeen.getNowPage():"+pageInfoBeen.getNowPage());
        System.out.println("pageInfoBeen.getPageSize():"+pageInfoBeen.getPageSize());
        JSONResult jsonResult = new JSONResult();
        jsonResult.setMessage("success");
        jsonResult.setMessageCode(null);
        jsonResult.setCode(0);
        jsonResult.setBody(null);
        return jsonResult;
    }
}
```

#### Tell Spring To Scan Confinguration Class

spring-context.xml

```xml
<context:annotation-config/>
<context:component-scan base-package="com.xxl.job.admin.config"/>

<mvc:resources location="classpath:/META-INF/resources/" mapping="swagger-ui.html"/>
<mvc:resources location="classpath:/META-INF/resources/webjars/" mapping="/webjars/**"/>
```

#### Servlet Config

web.xml

```xml
<servlet>
    <servlet-name>dispatcher</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath*:spring/spring-mvc.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
    <servlet-name>dispatcher</servlet-name>
    <url-pattern>*.do</url-pattern>
</servlet-mapping>

<servlet-mapping>
    <servlet-name>dispatcher</servlet-name>
    <url-pattern>/v2/api-docs</url-pattern>
</servlet-mapping>
```

