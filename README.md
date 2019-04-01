nlu作用：基本的NLU工具，包括实体识别和意图识别两个任务。
比如我说一句话：帮我订一张从北京到上海的高铁。这句话意图是订票，实体有出发地（北京），目的地（上海），交通工具（高铁）三个。
国外的google的API.ai, Microsoft的Luis.ai, Facebook的Wit.ai都是实现这样的功能。我们今天介绍一款开源nlu，并且支持中文.

系统：centos 

硬件条件：32G内存（之前内存太小了导致训练不了）

rasa-nlu文档：https://rasa.com/docs/nlu/quickstart/

前置条件：https://www.cnblogs.com/anxminise/p/9650206.html  
GCC高于等于4.9（因为pip3 install MITIE 需要高版本的GCC）

第一步：创建目录 mkdir rasa

第二步：安装rasa_nlu  ：pip3 install rasa_nlu

第三步：创建配置文件  vim config.yml  在项目可以加入这个文件

配置如下：

language: "zh"

pipeline:
- name: "nlp_mitie"
  model: "total_word_feature_extractor_zh.dat"
- name: "tokenizer_jieba"
- name: "ner_mitie"
- name: "ner_synonyms"
- name: "intent_entity_featurizer_regex"
- name: "intent_featurizer_mitie"
- name: "intent_classifier_sklearn"
第四步：创建rasa_dataset_training.json（文本标记）


第五步：下载total_word_feature_extractor_zh.dat

https://pan.baidu.com/s/1-ma0ndXBWL0rnbUqCAcL-w  密码lhi4 （由GaoQ1提供）

第六步：训练语料

python3 -m rasa_nlu.train --config config.yml --data demo-rasa_zh_zuoxi.json --path models1

经过一段时间会得到models1文件，如果提示报错，pip3 install xxxx（缺少的组件）

第七步：开启http接口

python3 -m rasa_nlu.server --path models1

第八步：在另一个窗口发起http请求

curl -XPOST localhost:5000/parse -d '{"q":"我的流量多少"}'

就会返回意图和实体了



