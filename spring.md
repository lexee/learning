## SpringApplication.run -> ConfigurableApplicationContext

+ ConfigurableApplicationContext context
  + ConfigurableListableBeanFactory
  + final AnnotatedBeanDefinitionReader reader
  + final ClassPathBeanDefinitionScanner scanner
+ ConfigurableEnvironment environment

> Step 1
>
> prepareContext(context, environment, listeners, applicationArguments, printedBanner);



> Step 2
>
> refreshContext(context);

+ ((AbstractApplicationContext)context).refresh()
  + prepareRefresh();
  + 

> Step 3
>
> afterRefresh(context, applicationArguments);

## Spring在VSCode Debug中配置运行时Args

使用`vmArgs`代替`args`携带Spring参数, 参考: [stackoverflow](https://stackoverflow.com/questions/54908243/adding-spring-arguments-to-vscode-debug-launch-json)

```
{
    "version": "0.2.0",
    "configurations": [
        {
            "type": "java",
            "name": "Debug Blog Rest",
            "request": "launch",
            "mainClass": "com.example.BlogRestApplication",
            "vmArgs": [
                "-Dspring.config.location=classpath:/application.yml,classpath:/application-secret.yml"
            ]
        }
    ]
}
```
