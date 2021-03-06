# 内蒙古大学图书馆自动预约系统

为方便内蒙古大学的同学能够方便的预约到座位，现开发并完成图书馆自动预约系统。 通过使用本系统，可以帮助考研的同学节约时间， 更加专心的复习 (ง •\_•)ง。

## 安装须知

为了能够使项目解压就可使用，本项目的数据库使用 `Mysql` 远程数据库。不用担心数据库问题。

## 所用技术

服务器采用 `ASP.NET` 所写，前端采用传统 `Bootstrap` 框架，小图标采用 `iconfont`，数据库采用 `MySQL`

## 预约规则

为了能够适用不同同学的预约需求（单双周，小学期，双学位等等）本系统的预约采用规则式预约，用户可以插入预约规则。每条规则里有三个属性，起始时间，预约时长，循环间隔。

- 起始时间 - 这条规则生效的开始时间
- 预约时长 - 每次所要预约的持续时间
- 循环间隔 - 上一次预约距下次预约的间隔时间，例如每周一次循环间隔就是 `7`

## 任务队列

自动预约的实现采用任务队列完成，首先在项目启动时初始化任务队列，将所有有效用户的规则分析处理，选取最进的一条规则按时间插入任务队列中。之后每隔 `100ms` 检测一下队首，如果队首时间小于当前时间，则执行队首任务，执行完该任务后会根据接下来是否有任务来返回新的任务或 `null` 如果有新的任务则插入队列中。

当用户修改规则或者关闭开始开关，则更新当前用户的任务队列，不影响他人的任务。

## 软件目录

![软件目录](./READMEIMG/mulu.png)

- App_Code //源码文件夹
- iconfont //小图标
- images //图片
- javascript //js 目录
- style //css 目录
- \*.aspx //窗口
- packages.config //包配置文件
- Global.asax //全局入口
- Web.config //项目配置文件

![软件目录](./READMEIMG/mulu2.png)

## 系统登陆界面

![登陆界面](./READMEIMG/img1.png)

## 系统首页

![登陆界面](./READMEIMG/img2.png)

## 用户服务开关

显示当前服务（自动预约）是否开启，如果已经开启则显示 `已启动` 否则显示 `未启动`。

![登陆界面](./READMEIMG/img3.png)

## 预约规则设置

![登陆界面](./READMEIMG/img4.png)

## 数据库

数据库只需要一张表，五个字段，分别是：ID，用户名，密码，服务状态，预约的时间规则

时间规则是数组，用'?'分割每一条。每一条用 ',' 分割 时间，星期，预约间隔天数

```
-- ----------------------------
-- Table structure for User
-- ----------------------------
DROP TABLE IF EXISTS `User`;
CREATE TABLE `User` (
  `uid` int(11) NOT NULL AUTO_INCREMENT,
  `username` varchar(255) NOT NULL,
  `password` varchar(255) NOT NULL,
  `state` varchar(255) NOT NULL,
  `rule` varchar(255) NOT NULL,
  PRIMARY KEY (`uid`)
) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=utf8;
```

## 免责声明

在使用内蒙古大学图书馆自动预约系统（以下简称 IMU-LibARS ）前，请您务必仔细阅读并透彻理解本声明。 您可以选择不使用 IMU-LibARS ，但如果您使用 IMU-LibARS ，您的使用行为将被视为对本声明全部内容的认可。

- 用户可以自由选择是否使用本产品提供的服务。如果用户使用本产品所提供的服务，即表明用户信任该软件作者， 软件作者对任何原因在使用本产品中提供的软件时可能对用户自己或他人造成的任何形式的损失和伤害不承担责任。
- 除 IMU-LibARS 注明之服务条款外，其它因不当使用本系统而导致的任何意外、疏忽、诽谤、版权或 其他知识产权侵犯及其所造成的任何损失， IMU-LibARS 概不负责，亦不承担任何法律责任。
- 对于因不可抗力或因黑客攻击、通讯线路中断等 IMU-LibARS 不能控制的原因造成的网络服务中断或其他缺陷， 导致用户不能正常使用 IMU-LibARS ，IMU-LibARS 不承担任何责任，但将尽力减少因此给用户造成的损失或影响。
- 本系统不含有任何旨在破坏用户计算机数据和获取用户隐私信息的恶意代码，不含有任何跟踪、监视用户计算机的功能代码， 不会监控用户网上、网下的行为，不会收集用户使用其它软件、文档等个人信息，不会泄漏用户隐私。
- 本系统免费为内蒙古大学同学提供服务，任何单位或个人不得将本系统用于商业用途。
- 本网站相关声明版权及其修改权、更新权和最终解释权均属 IMU-LibARS 所有。
