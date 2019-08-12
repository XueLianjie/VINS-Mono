VINS-Mono notes:
1.主要是class　Estimator, 这个class包括
1).相机外参、滑窗里的Ps、Ｖs、Ｒs、Bas、Bgs、class　IntegrationBase, FeatureManager, MotionEstimator, InitialEXRotation, 加速度观测量，角加速度观测量、
2).parameters 只有一个作用就是read parameters
3).feature_tracker是单独的一个node,负责接受raw image，然后进行图像特征提取和feature tracking，最后发布特征的topic。topic中包括feature的位置和feature的在像素图像上的速度。
4).condition_variable，用于状态同步的变量，想要修改该变量的进程需要先获取该变量的lock，修改完该变量后需要将该变量的lock进行释放。
5).vins_estimator中的feature_manager是对feature_tracker中pub出来的feature来进行管理。
6).主程序开启子线程，并进入process线程，等待measurement数据准备好后开始初始化和state estimator。主线程执行ros::spin，一直处理imu 和　image callback数据。两个线程之间采用condition_variable进行同步
当前线程会unlock　m_buf，等待measurement 数据。或者等待notify_one.
7).feature tracker会对原始图像进行处理，如果原始图像时间戳不连续或者时间戳逆序则进行estimator reset.
8).
