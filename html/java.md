# Java中遇到的问题  
### Linux执行Jar包： no main manifest attribute  
问题原因:pom.xml配置的问题，maven插件配置错误    
> 解决办法:  

pom.xml中添加下面的配置
```
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <executable>true</executable>
                </configuration>
            </plugin>
        </plugins>
    </build>
```