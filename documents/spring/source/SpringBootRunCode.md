# spring-boot源码解读 -- 初始化和启动
```java
public SpringApplication(ResourceLoader resourceLoader, Class<?>... primarySources) {
		this.resourceLoader = resourceLoader;
		Assert.notNull(primarySources, "PrimarySources must not be null");
		this.primarySources = new LinkedHashSet<>(Arrays.asList(primarySources));

		//判断是否要嵌入web服务，如果需要是什么类型的，web服务分为两种类型：REACTIVE和	SERVELET-BASE
		this.webApplicationType = WebApplicationType.deduceFromClasspath();
		//初始化
		setInitializers((Collection) getSpringFactoriesInstances(
				ApplicationContextInitializer.class));
		setListeners((Collection) getSpringFactoriesInstances(ApplicationListener.class));
		this.mainApplicationClass = deduceMainApplicationClass();
	}
```

