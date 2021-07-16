
# 五分钟系统：
```
    SLOW COUNT/慢速数量
        Triggered when the number of slow requests sent to the application exceeds the configured threshold.
        当发送到APP的速度慢的请求请求数量大于阈值（数量）时触发报警。
    SLOW RATE/慢速比例
        Triggered when the percentage(%) of slow requests sent to the application exceeds the configured threshold.
        当发送到APP的速度慢的请求比例大于阈值（比例）时触发报警。
    ERROR COUNT/错误数量
        Triggered when the number of failed requests sent to the application exceeds the configured threshold.
        当发送到APP的错误请求的数量大于阈值（数量）时触发报警。
    ERROR RATE/错误比例
        Triggered when the percentage(%) of failed requests sent to the application exceeds the configured threshold.
        当发送到APP的错误请求的比例大于阈值（比例）时触发报警。
    TOTAL COUNT/总数量
        Triggered when the number of all requests sent to the application exceeds the configured threshold.
        当发送到APP的所有请求数量大于阈值（数量）时触发报警。
    SLOW COUNT TO CALLEE/被调用的慢请求数量
        Triggered when the number of slow requests sent by the application exceeds the configured threshold.
            You must specify the domain or the address(ip, port) in the configuration UI's "Note..." box 
            ex) www.naver.com, 127.0.0.1:8080
    SLOW RATE TO CALLEE/被调用的慢请求比例
        Triggered when the percentage(%) of slow requests sent by the application exceeds the configured threshold.
            You must specify the domain or the address(ip, port) in the configuration UI's "Note..." box 
            ex) www.naver.com, 127.0.0.1:8080
    ERROR COUNT TO CALLEE/被调用的请求错误数
        Triggered when the number of failed requests sent by the application exceeds the configured threshold.
            You must specify the domain or the address(ip, port) in the configuration UI's "Note..." box 
            ex) www.naver.com, 127.0.0.1:8080
    ERROR RATE TO CALLEE/被调用的请求错误率
        Triggered when the percentage(%) of failed requests sent by the application exceeds the configured threshold.
            You must specify the domain or the address(ip, port) in the configuration UI's "Note..." box 
            ex) www.naver.com, 127.0.0.1:8080
    TOTAL COUNT TO CALLEE/被调用的总数量
        Triggered when the number of all requests sent by the application exceeds the configured threshold.
            You must specify the domain or the address(ip, port) in the configuration UI's "Note..." box 
            ex) www.naver.com, 127.0.0.1:8080
    HEAP USAGE RATE/堆使用率
        Triggered when the application's heap usage(%) exceeds the configured threshold.
        当监控的APP的堆使用率大于阈值（比例）时触发报警。
    JVM CPU USAGE RATE/JVM的CPU使用率
        Triggered when the application's CPU usage(%) exceeds the configured threshold.
        当监控APP的JVM CPU使用率大于阈值（比例）时触发报警。
    DATASOURCE CONNECTION USAGE RATE/数据源链接使用率
        Triggered when the application's DataSource connection usage(%) exceeds the configured threshold.
        当APP的数据连接池使用率大于阈值（比例）时触发报警。
```
