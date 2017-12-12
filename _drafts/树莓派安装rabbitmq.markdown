
sudo apt-get install rabbitmq-server

如果启动报这个错误：epmd error for host “demo”: timeout ，那么只需要修改一下hosts文件，增加你的主机名，注意，比如你的主机叫 demo.woplus，在hosts中配了 127.0.0.1 demo.woplus 也不行，你需要在hosts中再加一个 127.0.0.1 demo。

第一件事要创建用户，因为缺省的guest/guest用户只能在本地登录，所以先用命令行创建一个admin/admin123，并让他成为管理员。
sudo rabbitmqctl add_user admin 112358132134
sudo rabbitmqctl set_user_tags admin administrator

然后，我们启用WEB管理。
sudo rabbitmq-plugins enable rabbitmq_management


http://192.168.0.152:15672/ 默认端口15672



sudo rabbitmqctl  add_user rabbitmq rabbitmq
同时还要配置权限，允许从外面访问
sudo rabbitmqctl set_permissions -p / rabbitmq ".*" ".*" ".*"　
