workerman-thrift-resque
=========

配置运行环境
----------

[配置教程](http://www.workerman.net/install)

安装Redis
----------

[安装教程](http://redis.cn/download.html)

安装使用Thrift
----------

[在线文档](http://thrift.apache.org/docs/)

创建项目
----------

Composer
```sh
composer create-project --prefer-dist tumi/workerman-thrift-resque:dev-master
```

定义作业处理类
----------

普通作业
```php
namespace App\Resque\Job;

class Demo
{
    public function perform()
    {
        \Workerman\Worker::log($this->args['str']);
    }
}
```

启动停止
----------

启动
```sh
php start.php start -d
```

重启
```sh
php start.php restart
```

平滑重启  
```sh
php start.php reload
```

查看状态
```sh
php start.php status
```

停止
```sh
php start.php stop
```

管理队列（PHP）
----------

添加作业
```php
Resque::setBackend('127.0.0.1:6379');

$args = ['str' => 'This is a test!'];
$id = Resque::enqueue('default', 'Demo', $args);
```

删除作业
```php
Resque::setBackend('127.0.0.1:6379');

Resque::dequeue('default', ['Demo']);

Resque::dequeue('default', ['Demo' => $id]);

Resque::dequeue('default', ['Demo' => ['str' => 'This is a test!']]);

Resque::dequeue('default', ['Demo1', 'Demo2']);
```

查询状态
```php
Resque::setBackend('127.0.0.1:6379');
// Resque::enqueue('default', 'Demo', $args, true); 添加作业时

$id = '';
$status = new Resque_Job_Status($id);
$status->get();
```

通过Thrift RPC（参考client.php）
