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