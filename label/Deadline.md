### 20191015
1. NER实验全部结束; 
    1.1 疾病细分; 加几种实体、状态; 
        1.1 部分直接单独做; 
        1.2 其余部分: 1.词典 + 正则; 
    1.2** 评价标注; [晚上] 
        1.1 取出来所有的预测标签;  真实标签; 
        1.2 先检索B-,然后检索，如果后面是E-，则直接取出来span;   如果后面还是B-，则单独取出来; 然后再接着下一个B-；
        if 'label'

    1.3 Demo 优化; 
    1.4 词典对齐小类;
    1.5 Pipline 模型做,或者直接使用CNN，做状态以及子类分类任务;
        1.5.1 Bert_后面直接做一个分类任务,相当于多任务;   
        1.5.2 看看谷歌acl2019的做法; [明天] 
2. HotpotQA,WikihopQA数据各自20份用word标注; 



#### 20191017 周 
1. NER实验部分结束; 
2. HotpotQA; WikihopQA; 数据标注结束; 
3. 最起码 三个模型跑通,并且查看 代码以及 case_study 
4. 针对这两个数据集想自己的  idea； 
5. 毕设开题部分基本写完: 基于会话的阅读理解研究; 

#### 20191021 周 
1. CosMos 数据集的整理,以及之前别人的做法, idea; 
2. CommonSenseQA 数据集上总结,看看别人是怎么引入 外部知识，以及可以做的点，以及改进的地方;
突然觉得自己国庆节做的东西什么都没有用，现在还是要重新整理一下思路，然后再接着想方法; 
3. Social IQA 整理; 
4. TA TACO整理; 
5. TweetQA整理;  

#### 20191028 周 
1. 着手一两个数据集就开始做; 代码能力一定要更上去; 
    1.1 数据处理;
    1.2 数据统计; 
    1.3 模型部分: 1. 直接使用别人的组件; 2.自己尝试写; 


#### 20191016 
1. NER
    谷歌acl2019做法: lstm 隐层接出来然后单独做分类;
    数据处理: 
        1. 数据尽量不区分 子类-状态: 四种有状态; 疾病单独取子类; 
        2. 模型设计: if label==疾病: nn.Linear();  if label in [疾病、手术、检查、药物]
    评价: 
2. 整理Pytorch中 什么是pytorch?  笔记--各种切片等操作; 
字典{1:'ss','1':'ss}
w=[1:'ss',2:'tt']
a = []
[a.append(i) for i in w]
print(a)  ---> [1,2]

###### 20191017 
1. NER  chunyu[869份数据]
    1.1 重新用数据集做: test/dev/train
    1.2 NER这部分代码所有都重新写一遍; 之前的代码数据处理,以及等等都直接写在一起;  1.尽量高度封装; 2.尽量减少外部参数;    晚上:
    1.3 BiLSTM:作为baseline评价;  
    1.4 NER:2种模型放在一起,根据参数自己选用; if Model = BiLSTM_CRF;   if Model = Bert_CRF;  
    1.2 
    谷歌acl2019做法: lstm 隐层接出来然后单独做分类;
    数据处理: 
        1. 数据尽量不区分 子类-状态: 四种有状态; 疾病单独取子类; 
        2. 模型设计: if label==疾病: nn.Linear();  if label in [疾病、手术、检查、药物]
        3. 添加分类模型: 疾病细分子类,其他区分子类结果还是自己; 
        4. 主要是数据处理这块; 
2. MRC 
    2.1 HotpotQA -  WikihopQA 数据集分析各自15篇; 然后再重新看看两篇数据集文章; 
   [?] 2.2 DFGN代码调试通,得到结果;   
    2.3 今天文章; 

#### 20191018 ____ 20191021 
1. NER: Bert_NER 代码重新写;
    1.1 重写
    1.2 ACL2019方法写分类器; 
    1.3 数据处理这块重先;
{

}
2. 昨天文章的整理以及笔记; [1] 
3. 标注样例--各自15份  
4. DFGN              [1]


### 20191022 
1. 谷歌分类器那个; [下午]
	1.1 数据格式
		[
			{
				'句子': '医生: 今天感觉怎么样胸闷，做了心电图吗?',
				'Tokens':'医 生 : 今 天 感 觉 怎 么 样 胸 闷 ，做 了 心 电 图 吗 ?'
				'NER_Token_label':'O O O O O O O O O O B-疾病 E-疾病 ， O O B-检查 I-检查 E-检查 O O'
				'Class_State_label':[
										{'token':'胸闷','position':[11,13],'state':'Y','class':'胸闷'},
										{'token':'心电图','position':[16,19],'state':'U','class':'检查'}
									]
			}
		]
		问题: 两句话-三句话拼接在一起时候标签的位置又会发生变化; 遇到非法字符等位置偏移发生变化; 

	1.2 数据格式-全部封装在一起，等到最后使用时候再拆开;
		训练时候直接使用提前标注好的结果，测试的时候batch_size = 1,然后直接取对应的 *之前的第一句话或者整句话进行预测最终的结果; 
		Bert_Token 切分之后进一步把标签 合并-分类等操作;   state: Y-N-U-Q 	class: 43子类_8大类


2. hotpot-wikihop标注样例完成:[中午吃饭回来]
3. DFGN代码整理; Hotpot-Wiihop--GCN方法总结; [下午-晚上]



1. 数据:   春雨--录音  修改;  
2. 文章:   不懂就问;    

#### 20191022 最后给哲哥
1. 春雨-录音最后的模型给诗万; 
2. 实验最终结果的 excel
3. case_study分析; 
4. 对比CCKS的区别,做评测用; 
5. 句子分类模型使用诗万的结果进行测试; 
	5.1 自己写一个简单的bert的英文文本分类的接口直接进行测试; 
##### 20191023 
1. hotpotQA_train_question进一步分析; 文章尽可能再看一遍; 开源的先都下载下来; 

#### 20191024  
1. 毕设开题摘要; 
2. hotpotQA_train_question进一步分析; 文章尽可能再看一遍; 开源的先都下载下来; 
3. torch[]
4. 论文进一步整理;  wikihopQA--问题分析等; 
5. DFGN部署到21上面; 

#### 20191025 
1. TweetQA_CosMos 数据集整理--详细整理--数据分析; [上午] 
2. NER模型整理[] --形成直接给别人用的;  
3. NER实验结束[]    [下午]
4. DFGN代码过一遍; 
5. CosMos  Base_line 调试通,得到结果; 
6. QtoRef 同样;  

#### 20191026 
1. TweetQA整理
2. CosMos——Base_line
3. QuoRef

#### 20191027之前
1. wikihopQA-HotpotQA  的分析[数据+之前文章]结束; 整理想法; 
2. 下周直接看最新的数据集; 
##### 20191028周
1. 项目
   1.1 一篇关于 症状  为中心的  KBQA 的文章; 
   1.2 调研自动解决构建  以前构建的知识图谱中  相应实关系的方法; 
   1.3 看看之前的方法是否可以扩展到  其他科室; 

2. 阅读理解
   2.1 Multi-hop QA 整理完成:  HOtpotQA-wikihopQA数据分析--> 文章笔记[分析]
	* 多想想还能有什么方法去做,真的是现在能做的基本都已经做完了; 
	* github模块更新一下; 这几篇文章的总结和模型图; 该空的地方都空出来;    

3. 毕设开题结束: 

#### 20191028-29-30
1. 开题报告-结束发送
1.DFGN代码-[]-掌握GCN的构建和使用;  
3.tweetqa-quoref:数据集的整理
4.MRC.json: Multi-hop写完吧，也算是一个念想; 
5. 速度一定要快;  动态图-超图-动态超图; 好像没有看的必要了; 

6. 明天开会PPT以及排练; 

### 20191031 
1. TweetQA-CosMos整理到PPT,MRC
2. MRC文件进行分类处理; 
3. python-10大排序算法,以及动图gif [下午吃饭完了,给写python文件知识点]
4. ATOMIC文章,以及现在都是怎么用的?

### 20191104  20191105 
1. TweetQA整理;  [1]   
2. MRC进行分类处理。  [1]  回宿舍整理   
3. python-10大排序算法.gif图。  [1]  晚上回去写一遍;
4. ATOMIC文章看看别人怎么用的 [ppt大概已经整理,进一步看看]  
5. KB使用的综述[下午]---1 [没啥用，总结的其实并不够好]
6. 赶紧把文章正整理完，然后一点点的看代码，一点一点的看代码对照文章进行对比，进行实现。[1-文章看完]   
7. CosMos怎样使用外部资源,怎样合理的把外部资源融入到模型中来?   [长久话题]   


### 20191106 
1. CosMos基础代码跑通: 
    1) 看看别人怎么实现的,multiway-bert的实现过程; 
    2) case_study学习 
    3) 代码学习
2. atomic文章项目跑通
    1) 学习数据集的处理和使用
    2) 看看文章提出的几种使用atomic的方法怎么迁移? 
3. 其他代码学习
    1) 典型的使用KG作为 外部知识的代码



### 20191110
1. KagNet  
2. K-Bert

### 1. 常见知识推理数据集 & 外部资源的使用整理
1. 开源代码整理
2. 看主要思想



### 安排  
1. 接下来一个月时间就是 代码提高的时间,安安心心看一个月代码再说其他的. 

### 1 
{
1. 机器翻译项目    英语-->法语/德语 Anki数据集     开发/设计  两个项目的地址+解说-github博客有[1. https://www.jianshu.com/p/0652e77b49c5  2. https://www.cnblogs.com/HolyShine/p/9850822.html ]
* NLP机器翻译学习实战、NLP课程节课大作业  
* 利用LSTM结构实现Encoder-Decoder架构,掌握seq2seq模型的基础知识点
* 收获: 1) 数据清洗以及数据特征统计能力;  2) seq2seq模型的知识以及代码实现;  3)pytorch中计算图的可视化; 4)self-attention 机制在seq2seq模型中的使用; 
        5) Attention 机制可视化的技术;  6)结果可视化的技术; 7)不同损失函数的对比; 
2. 中文命名实体识别项目   CCKS2017医疗实体/conll2000/weiboNER  开发/设计    [我给你BERT-CRF 以及 BiLSTM-CRF代码]
* 分别利用Bert-CRF、Bert-LSTM-CRF、BiLSTM-CRF做NER,以及EMNLP2019-GCN_NER，快速读懂Bert源代码以及NLP-ner知识,GCN相关知识点的学习; 
* 收获: 1)Bert文章以及Transformer编码器的更深入的理解;  2) GCN的简单入门,GCN速度、性能远好于张越老师Lattice-LSTM; 3)CRF知识点的应用与掌握,CRF自己实现与Pytorch封装的对比. 

3. 句子/文本/情感分类项目      
* 分别使用Text-CNN、Text-RNN、BERT、HAN( Hierarchial Attention Network)实现文本分类任务
* 1)学习Text-CNN、Text-RNN、Bert等常见的模型; 2)学习HAN的实现,进一步掌握Attention，层次Ateention的相关知识。 3)Bert在小数据量上性能远好于其他。 



1. 医疗知识图谱的构建以及使用： 
   1. 语义知识图谱: 将医学文本进行组织和集成; 
   2. 一些电子病历中提取知识的
   3. 中医的知识图谱 
   4. 谷歌ios/安卓上推出的症状诊断: https://www.cnet.com/news/google-plays-doctor-by-identifying-your-medical-symptoms/ 
   5. 医学教材中: 症状分布比较分散。  根据编写教材的书写方式,一步步扩展症状实体的规模. 然后作为种子进一步筛选更多的症状实体; 
   6. 耶鲁--脑结构的知识图谱
   7. 刘焕勇: 5998关系数量

}



### 20191110  20191111 
1. KagNet代码跑起来，模型对应; | 
2. CosMos_baseline,源代码  | 
3. 常识推理综述--- 整理  | [11]
4. 常识推理常见数据集整理--以及相应文章整理 
5. 周五四篇文章整理;     
6. PPT做完--下午[]  

### 20191113 
1. KagNet代码  
2. CosMos源代码自己试着写
3. 常识推理综述---


### 20191114  
1. 常识推理综述   
2. 常识推理常见数据集整理--以及相应文章整理  [复现工作]  
3. pytorch中的  tensor操作总结 
4. 理解  Memory Networks   


还是没有一个很好的习惯，比如:  每天更新github记录，每天学习新的知识，每天的文章进行整理等等; 
1. arxiv-nlp 每日分享  
2. python-pytorch-每日学习  
3. 







































### 毕设相关 
* 开题: 
	   

### 20191105 
中午饭: WH   45   
晚饭：  ZT 160  
前天晚上: 100      


### 20191124   
1. CosMos代码自己照着写一遍  
2. pytorch中tensor的常见操作 
3. 初始化操作:    https://blog.csdn.net/tfcy694/article/details/80330616     
4. tensor常见的数学运算: https://blog.csdn.net/weicao1990/article/details/93738722   
5. https://blog.csdn.net/weicao1990/article/details/93738722
6. https://blog.csdn.net/TH_NUM/article/details/83088915  

### 20191125  
1. CosMos代码  
2. 项目这周计划，进度  
3. join()： 连接字符串数组。将字符串、元组、列表中的元素以指定的字符(分隔符)连接生成一个新的字符串。

split():split方法中不带参数时，表示分割所有换行符、制表符、空格。    



### 20191126  
1. 实习-两个网站简历筛选 
2. 云之声-搜狗  
3. 三篇文章-昨天的一篇文章  
4. pytorch的常见操作整理完备  
5. 症状原子症状结束  


### 20191127  
1. 文章--  
2. pytorch常见操作    



### 20191202周
1. CosMosQA源码---[代码]  
2. pytorch基础
3. 两篇常见的文章-BiDAF实现
4. 几篇文章  
5. 项目--周二一天  
6. Bert相关总结-几篇github | 师兄ppt总结
7. 

#### 20191202 
1. CosMos源码
2. pyotrch的总结 
3. 几篇文章  

#### 20191203  
1. 三篇文章 
2. cosmos源代码
3. 之前整理的笔记 -- 自己展开   

#### 20191204 死任务  
1. 项目的字典的构建: 上午2小时，词典基本构建完成，然后是词典的融合、评价等等  
2. 这几天的几篇文章  一定要先做完： 中午吃完饭 2篇再睡觉  
3. CosMos的基础代码一定要过去，以及其中整理的知识点，自己再写一遍  下午吃完饭  
4. 周报的ppt: 中午吃饭时候就着手开始做  
5. Bert相关的代码开始整理: 1) lmla 2)lpaqa 3) why lm need commonse 那一偏文章的代码
6. 种一棵树最好的时间是 10年前，其次是现在；  是不是对于自己走着合适的路线，多思考思考



#### 20191206   
1. 人体8大系统-部位器官的对应 的对应    
2. 图数据库的基本使用  
3. 四肢下面进一步进行分析整理，最好直接可以做到可以使用的地步；   
4. 再做两个到三个  大的部位 
5. 想想其中的难点    




#### 20191209 --20191210 --20191211
1. CosMos代码 --- 晚上  
2. pytorch --tensor常见操作赶紧总结--中午吃完饭  
3. 几篇文章代码 --- 晚上  
4. lama  --- 晚上  
5. 总结现在几个常看的研究方向，尽快确定自己想做的东西[ppt,github整理]  
   5.1 目前现有文章的整理: 1)Bert-LM 相关研究   2)常识推理   3)知识图谱 构建-补全-使用-embedding等等  4)机器阅读理解  
6. tensor的常见操作---整理打印
7. 一方面做 之前文章阅读的笔记，一方面 代码所有都试着复现一遍  
8. 剩一个月时间:  1) pytorch基础代码联系，常见操作  
9. 项目: 8大系统对应人体器官进行对应： 256骨头等详细对应
10. 尽快结束现在的事情，用到自己真正想做的方向上面来。   
11. 对于现在的数据集做一个分类
12. 之前在 Multi-hop 数据集上做的 基于实体的GCN entity-gcn 有没有考虑到 文本交互过程中  实体的状态变化？ 实体可能随着 文章的变化而变化，但是直接使用 extity建模，捕获不到这种信息。    
    12.1 构建图的时候  仅仅使用 entity-centirc就足够了吗？  1) 实体识别 2)实体对齐-消除歧义 3)实体信息足够表达文章中的信息了吗？
    12.2 能不能构图 以及 使用图同时进行，也就是 KG-Graph_GCN 和 文本预测 span-entity_选择 同时进行，之间互相交互帮助。  





13. pos编码重要吗？  对于机器学习重要吗？  怎样评测Bert中的  pos编码的效果？  有没有更好的解决方法？ 
14. 以前隐式的编码，比如: 之前的 各种多加出来的编码器真的能起到特殊的作用吗？   有没有更加显式的指导？ 
15. 机器学习 +  逻辑推理的方法怎样？  DROP数据集？ 
16. Pre-trained学习方法是不是已经证明  学习到了  逻辑推理？      Bert中各种能力目前已经有很多人做了研究，对于 离散推理-其他推理的能力好像目前并没有一个定性、定量、解释性的研究文章。 
17. DROP数据集使用了 逻辑推理+深度学习 但是效果并不是很好，有没有 逻辑推理使用的太简单的原因？  

#### 20191211 
1. 头颈部--四肢--全身---背部   词典、分类体系做完  
2. neo4j进行存储  

#### 20191213  
1. 全身图谱做完  
2. 剩余部位图谱进一步划分 |   
3. pytorch最基础一定要结束  
4. CosMos基础代码

学位课: 80.5  
所有课程: 79.88
#### 20191215  <!-- TOC -->

1. pytorch基础--tensor常见操作--中午吃完饭  
2. 医疗-法律 问答baseline整理  
3. CosMos - 代码  
4. 明天新文章  
5. 部位体系搭建整理
6. 初步整理 现有数据集 到 excel  --仅仅只是是数据集   
7. 现有数据集整理:  1)数据集背景; 2)类型;  3)数据集格式;  4)数据集统计; 数据集分析    


### 20191218   [未做完 ]
1. 医疗-法律-物理   三个数据集的整理  PPT-对应到 markdown 
* markdown基础语法大全：   
  * https://blog.csdn.net/weixin_35073408/article/details/100586583  
  * https://blog.csdn.net/witnessai1/article/details/52551362  

### 20191219  
1. 典标师兄-庆斌师兄PPT整合 再看一遍;  



### 20191220  [未做完]
1. 现有数据的三元组形式进行整理;  下午--  
2. 机器阅读理解-背景和研究热点，研究方向的总结   
3. Bert相关大家庭-- 典伯-庆斌PPT进行整理  
4. github进一步更新  
5. pytorch的常见操作--整理完整：  


### 20191224 
1. 图谱工作结束: 
   2.1 三元组整理完成,并形成相应的部位体系: PPT展现  +   三元组形式[CSV文件]
   2.2 图谱查询流程算是设计
2. 用  d3.js把目前的展示 原理，几种体系都可以展示一遍。   
   2.1 直接三个体系各自是  图谱，然后点开可以进一步查询。  
   2.2 部位全局统一 : 子宫、消化道等等
   2.3 原子症状要不要依附于 部位  
        * 依附  优点: 范围很好界定;   缺点: 看不见  ’无力‘ 相关的其他的大的部位  
        * 不依附  优点: 全局可以看见   缺点:太乱了，原子症状会有很多很多，后期如果  症状 4000+  原子症状: 500+左右就可以直接使用全局的原子症状; 
### 20191227 下周周二之前计划  
1. ACL2018、ACL2019  文章中有多少是刷榜的。   
2. 推理拿PPT讲清楚。     
3. RNN的网络结构、参数计算公式，自己画图、自己解释清楚。   找找比较清楚的LSTM、GRU的图  
4. 各种Attention的变体画图、公式理解      

### 20191230周: 
1. attention问题总结清楚: 
2. ACL2019 EMNLP2019文章整理完; 
3. DrQA代码













































































#### 20191125 深度学习未来探讨
1. 李航  
  1.1 符号推理、神经网络 
  1.2 让机器学会思考: 贝叶斯推理、因果推断  
      让机器合理行动: DL、ML 

2. 王立威 北大 
   2.1 tasks   -->  understanding 
   2.2 learning paradigms: self-supervised  
   2.3 model 