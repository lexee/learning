## BeanDefinition

>A BeanDefinition describes a bean instance, which has property values,
>
>constructor argument values, and further information supplied by
>
>concrete implementations.
>
>
>This is just a minimal interface: The main intention is to allow a
>
>{@link BeanFactoryPostProcessor} to introspect and modify property values
>
>and other bean meta data.

![](F:\学习笔记\images\bean-definition\BeanDefinition.png)



## BeanFactory

>

![](F:\学习笔记\images\bean-factory\DefaultListableBeanFactory.png)

第一层

+ ***ListableBeanFactory extends BeanFactory*** 

  >Extension of the {@link BeanFactory} interface to be implemented by bean factories
  >
  >that can enumerate all their bean instances, rather than attempting bean lookup
  >
  >by name one by one as requested by clients. BeanFactory implementations that
  >
  >preload all their bean definitions (such as XML-based factories) may implement
  >
  >this interface.

+ ***HierarchicalBeanFactory extends BeanFactory***

  > Sub-interface implemented by bean factories that can be part
  >
  > of a hierarchy.

+ ***AutowireCapableBeanFactory extends BeanFactory***

  > Extension of the {@link org.springframework.beans.factory.BeanFactory}
  >
  > interface to be implemented by bean factories that are capable of
  >
  > autowiring, provided that they want to expose this functionality for
  >
  > existing bean instances.

第二层

+ ***ConfigurableBeanFactory extends HierarchicalBeanFactory, SingletonBeanRegistry***

  > Configuration interface to be implemented by most bean factories. Provides
  >
  > facilities to configure a bean factory, in addition to the bean factory
  >
  > client methods in the {@link org.springframework.beans.factory.BeanFactory}
  >
  > interface.

第三层

+ ***ConfigurableListableBeanFactory extends ListableBeanFactory, AutowireCapableBeanFactory, ConfigurableBeanFactory***

  >Configuration interface to be implemented by most listable bean factories.
  >
  >In addition to {@link ConfigurableBeanFactory}, it provides facilities to
  >
  >analyze and modify bean definitions, and to pre-instantiate singletons.

第四层

+ ***DefaultListableBeanFactory extends AbstractAutowireCapableBeanFactory implements ConfigurableListableBeanFactory, BeanDefinitionRegistry, Serializable***

  >Spring's default implementation of the {@link ConfigurableListableBeanFactory}
  >
  >and {@link BeanDefinitionRegistry} interfaces: a full-fledged bean factory
  >
  >based on bean definition meta data, extensible through post-processors.

## Bean生命周期

+ **ScopedProxyMode.INTERFACES** or **ScopedProxyMode.INTERFACES** ?

[Which proxyMode should I choose?](https://stackoverflow.com/questions/21759684/interfaces-or-target-class-which-proxymode-should-i-choose/43013315)

[bean的作用域](https://blog.csdn.net/u013423085/article/details/82872533)