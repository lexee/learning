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