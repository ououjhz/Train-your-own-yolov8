我的实验环境:
    python: 3.8.19
    torch: 2.1.6+cu118
    torchvision: 0.16.0+cu118
    timm: 0.9.8
    mmcv: 2.1.0
    mmengine: 0.9.0
    ultralytics: 8.0.178

 额外需要的包安装命令:
        pip install timm==0.9.8 thop efficientnet_pytorch==0.7.1 einops grad-cam==1.4.8 dill==0.3.6 albumentations==1.3.1 pytorch_wavelets==1.3.0 -i https://pypi.tuna.tsinghua.edu.cn/simple
    其他运行的时候缺什么就装什么即可

# 一些文件说明
1. train.py
    训练模型的脚本
2. main_profile.py
    输出模型和模型每一层的参数,计算量的脚本
3. detect.py
    推理的脚本
4. track.py
    跟踪推理的脚本
5. test_yaml.py
    用来测试所有yaml是否能正常运行的脚本
6. heatmap.py  
    生成热力图的脚本
7. get_FPS.py
    计算模型储存大小、模型推理时间、FPS的脚本


# 训练方法
1. 在mydata.yaml中配置数据集路径
2. 选择模型，模型配置文件都在ultralytics/cfg/models/v8中，比如使用yolov8模型，即H:\ultralytics-main\ultralytics\cfg\models\v8\yolov8.yaml
3. 命令行参数和超参数设置存放在ultralytics/cfg/default.yaml,也可以直接在train.py中设置，然后运行train.py即可
4. 训练结果自动保存在runs/detect/train下

# 关于改进方法
以加入动态蛇形卷积DySnakeConv
(论文地址：https://arxiv.org/abs/2307.08388
代码地址：https://github.com/YaoleiQi/DSCNet)
为例:
1. 在ultralytics/cfg/models/v8文件夹下新建一个 yolov8-C2f-DySnakeConv.yaml 
2. 在ultralytics/nn/extra_modules文件夹下新建dynamic_snake_conv.py,将源代码添加到 py 文件
3. 将 DySnakeConv、DSConv、C2f_DySnakeConv 这个类的名字加入到 ultralytics/nn/tasks.py 中注册
(其他改进，比如添加注意力机制，将源代码加入ultralytics/nn/extra_modules/attention.py,并进行注册即可)
4. 修改 yolov8-C2f-DySnakeConv.yaml ，使用上述模块构建网络 
