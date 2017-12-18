# 大疆视频传输ROS tcpic

---
## 编译安装

这里将原先的大疆示例程序封装为ros package，需要注意得是大疆提供的header和so文件是c的，需要在头文件加入`extern c`表示编译的包是来自c的，才能编译通过。
```
cd ~/catkin_ws/src
git clone https://github.com/guanfuchen/dji_cam_transport
cd ~/catkin_ws
# 编译过程中缺少依赖库自行安装，该仓库依赖roscpp、rospy、std_msgs、image_transport、cv_bridge
catkin_make
source $YOUR_HOME/catkin_ws/devel/setup.bash
```

---
## 运行dji_cam_transport

运行dji_cam需要获得root权限，切换root权限并且执行```source $YOUR_HOME/catkin_ws/devel/setup.bash```，运行如下所示：

```bash
sudo su
# 生成buffer然后通过image_transport等依赖库转发topic
rosrun dji_cam_transport dji_cam_transport -g
# 本机查看image
rosrun image_view image_view image:=/camera/image
```

---
## 主从通信

参考如下，在从机上运行`image_view`即可：
- [ROS在多机器人上的使用](http://wiki.ros.org/cn/ROS/Tutorials/MultipleMachines)

---
## 一些问题

在非图形界面下无法生成图像，需要输入以下才能生成图像数据。

```bash
xinit&
export DISPLAY:=0
```

[使用 Manifold 获取 M100 上的 X3 摄像头数据](https://dl.djicdn.com/downloads/manifold/cn/Using_Manifold_to_Acquire_Data_from_X3_Camera_cn.pdf)

[使用Manifold 获取M100 上的X3 摄像头数据一硬件连接二使用](http://www.doc88.com/p-6751555878127.html)

但是获取图像数据需要获取root权限，而使用sudo rosrun则提示无法找到rosrun命令，所以切换到root用户中，并且将相应的ros环境source到root用户中，此时发现运行上述命令无法实现图像解码。猜想是由于自动登录非root用户如ubuntu将图形界面占据，那么接下来的思路便是将自动登录改为root，具体详细步骤参考如下链接，包括开启ssh相关的root登录等等。

```bash
sudo passwd root
sudo vim /etc/ssh/sshd_config
#注释#PermitRootLogin without-password并增加PermitRootLogin yes
sudo vim /usr/share/lightdm/lightdm.conf.d/50-ubuntu.conf
#修改如下所示
#[SeatDefaults]
#autologin-user=root
#user-session=ubuntu
#greeter-show-manual-login=true
sudo vim /root/.profile
#将mesg n替换成tty -s && mesg n
```
[How to Allow root to use SSH on Ubuntu 14.04](http://www.ehowstuff.com/how-to-allow-root-to-use-ssh-on-ubuntu-14-04/)

[linux ubuntu启用root用户登录](http://andy136566.iteye.com/blog/953731)

[ubuntu 16.04启用root用户登陆](https://zerlong.com/265.html)
