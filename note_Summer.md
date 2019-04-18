## tmux StarGAN_Norland

8月3日 StarGAN训练结果和预期不同
看code和论文，以及三天了，还没解决呜呜

用Norland的夏天冬天去训练CycleGAN和StarGAN
StarGAN：还是没学到季节转换每一行的图像是一样的， CycleGAN：ConnectionError: Error connecting to Visdom server 

```python3 -m visdom.server```
且要用python3来运行

春夏秋冬四季的图片数量
春1084: 夏4292 秋1085 冬535

8月16日
(1)得到每幅图像的属性list
类似于：attri_list_celeba.txt 步骤： 春夏秋冬分别获得attri_list.txt merge： 保证四个季节的图像数量差不多都是1080左右 重新ffmpeg了冬天的图像

(2)在本地显示tensorboard运行loss结果
参考博客 在本地运行： ssh -L 16006:127.0.0.1:6006 -p 252 timing@10.60.100.57 登录到二楼服务器后运行： tensorboard --logdir=logs_path bug：locale.Error: unsupported locale setting 原因：locale setting问题说明是转型问题，编码方式不统一导致的结果。 *解决办法： export LANGUAGE=en_US.UTF-8 export LC_ALL=en_US.UTF-8 最后，在本地访问地址：http://127.0.0.1:16006/

增大了训练iteration
200K变成1000k，模型仍然保存在stargan_norland_Aug17原来的model和logs等文件夹会被更新,但是训练iters仍然从0开始，原因：solver.py中202行和main.py中的82行resume_iters,default=None决定了iters开始于哪里。 解决办法：把main中的resume_iters的‘default=None’改成default=200000 event保存在： /home/timing/StarGAN_Norland/stargan_custom_Norland_Aug17/logs/events.out.tfevents.1534823736.4e290b8bddd6

## StarGAN训练出来的图像每列不是具有同一属性的

*原因是：图像在train和test时候没有按照要转换的属性划分：白天，晚上，冬季，夏季，雨季等*


**办法：把二楼的图像重新scp到我的mac上归类*
```scp -r -P 252 timing@10.60.100.57:/home/timing/git_repositories/StarGAN/data/RaFD/ /Users/zhanghui/Desktop/```

## 训练StarGAN时要改的参数  
 **Train_StarGAN_.bash里面数据位置，模型存储位置**
 **main.py里面继续训练的num_iters和resume_iter，默认分别是200000和None**
