### 生成结果示例

+ 斗破苍穹续写：

  ```
  萧炎对这威胁之话还是感到陌生，若非他灵魂力量同样不弱的话，恐怕早就忍不住的出手了。
  
  “呵呵，小家伙，既然你已经晋入了斗宗层次，那我也不再有半点留手，你体内的那种能量，应该也是处于天境后期吧？”萧炎一笑，在提醒了一声后，便是再度闭目养神。
  
  听得萧炎这话，一旁的薰儿眼眸也是微微眯了起来，她知道萧炎体内，有着一个古怪的内院存在，如今好不容易得到异火，却是根本就没有半点的可信性。
  
  “那便继续等，他们继续出手！”
  
  萧炎咬着牙，手印变动，体内斗气顿时在身体表面翻腾而起，旋即一个奇异的符文缓缓出现在其手掌上，漆黑的眸子中，闪烁着森寒之色。
  
  “萧炎哥哥，恭喜你了，萧族的事，交给我便好。”
  
  听得萧炎那般淡淡的话语，薰儿脸颊也是微变，缓缓的道。
  
  “我倒是希望你能走到今天的”萧炎笑了笑，他知道薰儿嘴中所说的那一句话，或许便是最好的事。
  
  “我知道你对自己迁移到你的身体有些不太满意吧？”薰儿眼波流转，却是并未否认。
  
  萧炎眉头微皱，刚欲说话，一旁的萧玄却是突然低声道：“我想，你应该是认为我们萧族会对我所说的话有着不小的一些芥蒂。”
  ```

+ 古诗生成：

  ```
  一身走四海，万事付悠悠。贫病不相弃，饥寒难自谋。风霜欺病骨，烟雨暗归愁。近日思归梦，空台南望留。
  
  天涯望不极，此夜又秋残。此处谁同赏，从来恨独难。月明风正凛，霜冷夜应寒。独倚栏干思，清光几处看。
  
  山色初晴水色鲜，幽禽啼唤雨余天。客心自觉空如梦，世事休论更可怜。老眼开时花自笑，残年厄闰草堪怜。一筇破衲同春睡，谁与浇愁入酒泉。
  
  清溪一曲绕茅堂，溪上秋风透短墙。草木扶疏迷远径，山川清澹接高阳。诗情到处成三叹，酒兴知时变一觞。我亦放怀非独乐，拟将闲事寄僧房。
  ```

+ 日常话题生成：

  ```
  我们快毕业了，那么接下来的事我们就来看看那些做这些工作的，有什么需求我们来写这些工作的需求我们，我们也可以在这里学到更多的知识，因为我们只是一个需要工作的人而已。这里也同样需要一个专业的工作室，而我们也需要更多的需要与之的工作环境，以及每个需求的工作。根据我们的喜好来看，企业的员工薪酬是高于其他需求的，所以企业的需求是高于本岗位的。我们的需求是高于本岗位的，所以我们需要对自己的工作有一个全新的认识。这里如果你。。。
  ```

### 介绍

实现了基于tranformer xl进行文本生成任务，代码基于https://github.com/kimiyoung/transformer-xl 。  也从这里看起，有基本了解，感谢他们的工作。主要改动在下面几个地方：

+ 原本的代码只有training 和 eval，增加了inference 部分，主要在train_gpu中增加了inference 函数以及相应函数的改变。
+ 增加了可视化每一层attention以及查看每个结果候选词的代码，在visualize_attention.py中。
+ 模型里面增加了inference，见model.py中的函数。

### requirements
python3;
tf >=1.12.0

### 使用

+ 以小说训练为例，其他同理，古诗对应shi_base_gpu， 日常话题对应zhihu_base_gpu（在 tf 目录下执行）

  先进行数据准备,具体参数设置在doupo_base_gpu中调节

  bash scripts/doupo_base_gpu.sh  train_data

  训练 ：

  bash scripts/doupo_base_gpu.sh train

  inference：

  bash scripts/doupo_base_gpu.sh inference  （注意在inference的时候记得修改train_gpu.py中第504行，改成你想inference的数据集名字）

### 引入新的训练数据训练（针对中文，若要训练英文，直接用tf下的old_vocabulary.py 替换vocabulary.py  ）

+ 首先在data目录下，建立新的文件夹，名字随意，然后将训练数据重命名为train.txt和valid.txt。
+ 在tf/scripts 目录下建立新的bash脚本，名字随意，内容可以先复制已存在的脚本，然后将路径名更改成上一步新建的路径，并修改其他相应的名字。
+ 在tf 下执行bash scripts/[新的bash脚本名字] train_data
+ 再执行 bash scripts/[新的bash脚本名字] train 进行训练
+ 最后执行  bash scripts/[新的bash脚本名字] inference 进行测试。

### 可视化每个head 每个layer 的attention， 
在train_gpu.py里inference 函数中对应的位置，打了todo标记，默认是head 10个 layer 16，若不同自行修改visualize_attention.py中的对应部分。
可视化效果如：
![image](https://github.com/GaoPeng97/transformer-xl-chinese/blob/master/tf/attention_pic/36住.png)
