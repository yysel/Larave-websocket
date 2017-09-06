# Laravel-WebSocket  
为`Laravel`项目量身开WebSocke服务驱动器，可以让您快速部署websocket服务程序，该程序具有以下特点，

（1）开箱即用，配置简单

（2）丰富的api。

（3）强大的SocketManager进程管理控制

（4）全局通信组件，使WebSocket服务与Http服务完美结合

## 一、安装与配置： 

### （一）安装

```composer
    在命令行执行：composer require kitty/websocket
```

### （二）配置

​	修改/config/queue.php文件 在connections字段中添加一个元素（自定义队列驱动）如下：

```php
'connections'=>[
  	//....
    //添加以下代码
  	 'websocket' => [
            'driver' => 'websocket',
            'connection' => 'default',
            'queue' => 'default',
            'address'=>'192.168.1.114', //监听地址
            'port'=>'2000',        		//监听端口
            'console'=>true,			//是否开启控制台信息
        ],
]
```

### （三）添加服务提供者

​	修改``/config/app.php``文件下的'providers'数组内如下：

```php
'providers' => [
  		//...
         Kitty\WebSocket\Providers\WebSocketServiceProvider::class,
 ]
```



## 二、开启服务

```php
     php artisan socket:run
```

​	上述命令会开启一个常驻内存的socket进程 ，并监听上一步骤配置项的地址与端口，来处理websocket请求。



## 三、创建事件响应器

​	1、一旦服务启动，每个socket请求都将会被kitty/websocket响应到`\App\Job\WebSocketJob`类中，所以你可以在这个类中，按照你的业务逻辑来处理这些请求。

​	2、你可以通过命令`php artisan socket:make job` 来快速的在app/Jobs目录下创建事件处理类`WebSocketJob` ,`kitty/websocket`已经为你解决了继承关系，并生成了示例代码。



## 四、事件响应

​	如上所述，`kitty/websocket`的事件响应代码都放置在了app/Jobs/WebSocketJob.php中。共包括五种事件类型：

​	1、用户建立连接时，响应  login() 事件；

​	2、用户断开连接时，响应logout()事件；

​	3、用户发送消息时，响应massage()事件；

​	4、接收到管理进程(SocketManager)指令时，响应manager()事件；

​	5、timer()事件，用来响应定时任务

​	

## 五、进程管理（SoketManager）

​	进程管理是kitty/websocket的特色功能，提供了强大进程管理、进程监控的能力，除此之外，借助SoketManager还可以在项目任意一处代码中实现WebServer与SocketServer的通信。

```composer
    进入进程管理控制台：php artisan socket:manager
```



## 六、API

（一）Socket API 

​	1、

（二）Globle API

