# JFinal Annotation Plugin
JFinal的基于注解的依赖注入插件，实现了 `@Resource`,  `@Service`, `@Repository` 注解

## 使用
- 可以将项目根目录下的jFinal-annotation-plugin.jar添加到自己的本地项目依赖中。  
可以将源码直接拷贝到自己的项目中
   
- 在jfinal的配置文件中添加全局拦截器: 
    ```java
      	/**
      	 * 配置全局拦截器
      	 */
      	public void configInterceptor(Interceptors me) {
      		me.addGlobalActionInterceptor(new InjectionInterceptor());
      	}
    ```
- 在jfinal配置文件中添加plugin, 参数为存放service 和 dao的报名,例如下
    ```java
        /**
         * 配置插件
         */
        public void configPlugin(Plugins me) {
            me.add(new AnnotationInjectionPlugin("com.jfinal.core.service","com.jfinal.core.dao"));
        }
    ```
    
- 分别在service和dao类上添加注解`@Service`和`@Repository`将类实例交给插件处理:  
    ```java
         @Repository
         public class BillDao {
         }
    ```
    ```java
        @Service
        public class BillService {
        
            @Resource
            private BillDao billDao;
        }
    ```
    ```java
        public class BillController {
            @Resource
            private BillService billService;
        }
        ```
- 通过`@Resource`将dao实例注入到Service中或者将Service实例注入到Controller中

**在`@Service`和`@Repository`的value参数中可以自定义要注入实例的名称, 如果不填默认名称为类的首字母小写**
**默认所有有插件管理的实例都是单例, 暂时不支持修改为每次请求创建一个新实例**
