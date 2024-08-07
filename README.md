- 👋 Hi, I’m @wwlloo
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
wwlloo/wwlloo is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
#include <ros/ros.h>
#include <cv_bridge/cv_bridge.h>
#include <sensor_msgs/image_encodings.h>
#include <opencv2/imgproc/imgproc.hpp>
#include <opencv2/highgui/highgui.hpp>

using namespace cv;
void Cam_RGB_Callback(const sensor_msgs::Image msg)
{
    cv_bridge::CvImagePtr cv_ptr;
    try
    {
        cv_ptr = cv_bridge::toCvCopy(msg, sensor_msgs::image_encodings::BGR8);

    }
    catch(const std::exception& e)
    {
        ROS_ERROR("cv_bridge exception: %s, e.what()");
        return;
    }
Mat imgOriginal = cv_ptr->image;
imshow("RGB", imgOriginal);
waitKey(1);

    
}
int main(int argc, char **argv)
{
    ros::init(argc, argv, "cv_image_node");
    ros::NodeHandle nh;
    ros::Subscriber rgb_sub = nh.subscribe("/kinect2/qhd/image_color_rect",1,Cam_RGB_Callback);
    namedWindow("RGB");
    ros::spin();

}



#include <ros/ros.h>
#include <opencv2/opencv.hpp>  // 引入OpenCV的核心功能
#include <opencv2/highgui/highgui.hpp>  // 引入OpenCV的图形用户界面功能

using namespace cv;

int main(int argc, char **argv)
{
    // 初始化ROS节点
    ros::init(argc, argv, "cv_camera_node");
    ros::NodeHandle nh;

    // 创建OpenCV VideoCapture对象来访问电脑摄像头
    VideoCapture cap(0);  // 0表示默认的摄像头

    if (!cap.isOpened())  // 检查摄像头是否成功打开
    {
        ROS_ERROR("Could not open camera");
        return -1;
    }

    // 创建一个窗口来显示视频流
    namedWindow("Camera Feed");

    // 循环捕获视频帧并显示
    while (ros::ok())
    {
        Mat frame;
        cap >> frame;  // 捕获一帧图像

        if (frame.empty())
        {
            ROS_ERROR("Captured empty frame");
            break;
        }

        imshow("Camera Feed", frame);  // 显示图像
        if (waitKey(30) >= 0)  // 等待30ms，处理窗口事件，退出时按键
            break;
    }

    // 释放VideoCapture对象和关闭窗口
    cap.release();
    destroyWindow("Camera Feed");

    return 0;
}

