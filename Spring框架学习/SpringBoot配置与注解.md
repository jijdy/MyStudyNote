## SpringBoot的配置文件及格式
	
	1. 一般通过在resource文件夹下的application.Properties文件中配置
		* 配置格式直接使用=号赋值。
		~~~
			environments.dev.url=http://dev.bar.com
			environments.dev.name=Developer Setup
			environments.prod.url=http://foo.bar.com
			environments.prod.name=My Cool App
		~~~
	2. 或者通过yaml文件，即.yml文件中进行配置
		* 配置文件具有阶梯性
		* 在配置中若有比较多的赋值，则使用yml格式更优
		~~~
			environments:
				dev:
					url: http://dev.bar.com
					name: Developer Setup
				prod:
					url: http://foo.bar.com
					name: My Cool App
		~~~