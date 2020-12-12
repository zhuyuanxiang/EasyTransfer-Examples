# tian-chi-nlp

天池比赛《NLP中文预训练模型泛化能力挑战赛》的使用样例及部分说明。

## Goal

比赛目标：测试 EasyTransfer 对预训练模型的适应能力，找到更加优秀的预训练模型。

## Description

例子说明：这个例子演示了如何将原始数据转成 tensorflow 的记录，再经过多任务调优找出预测的结果。

## Datasets

比赛数据：原始例子中未提供数据，需要通过[训练主页下载数据](https://aliyuntianchiresult.cn-hangzhou.oss.aliyun-inc.com/file/race/documents/531841/NLP_A_Data1128.md?Expires=1607740822&OSSAccessKeyId=LTAILBoOl5drlflK&Signature=GRapc7lAnyslNS0a2VU0SFllxGU%3D&response-content-disposition=attachment%3B%20)。但是下载的数据不能直接使用，我已经将原始数据处理后放在例子中。

1.  数据处理：
    1.  训练数据文件名：`XXX_train.csv`，需要拆分成`dev.csv`和`train.csv`两个文件；
    2.  预测数据文件名：`XXX_a.csv`，需要处理后提交到网站由比赛方测评

## Environment

环境配置：基于本地执行对部分代码进行了修正

### Hardware

建议硬件：CPU(i7-9700)+GPU(2060)+内存(48G)

### Software

    1.  软件必须：Ubuntu+Python 3.6+tensorflow(1.12.0)+easytransfer(0.1.2)
    2.  软件建议：VSCode+PyCharm+jupyter notebook

## Run

## Shell+Python

运行测试环境

1.  数据准备：存放目录 `./tian-chi-nlp/tianchi_datasets/`
2.  配置文件准备：存放目录 `./tian-chi-nlp/config/`
    1.  配置文件「XXX_preprocess_train.json」预处理阶段「训练」数据说明
    2.  配置文件「XXX_preprocess_train.json」预处理阶段「验证」数据说明
    3.  配置文件「multitask_fintune.json」精细调优
    4.  预训练模型配置：预处理阶段的两个配置文件的模型必须相同；精细调优的配置文件可以与预训练阶段的配置文件的模型不同。
3.  预训练模型准备：存放目录 `~/.eztransfer_modelzoo/bert`，如果为空，执行时系统会自动下载

    ```json
    google-bert-tiny-zh     # Ali 网站没有这个模型
    google-bert-small-zh    # Ali 网站没有这个模型
    google-bert-base-zh
    google-bert-large-zh    # Ali 网站没有这个模型
    pai-bert-tiny-zh
    pai-bert-small-zh       # 不能正确 finetune
    pai-bert-base-zh        # 不能正确 finetune
    pai-bert-large-zh       # 不能正确 finetune
    ```

4.  执行脚本：
    1.  先完成 csv 转 tfrecord：`sh run_convert_csv_to_tfrecrods.sh`
    2.  再完成「模型的训练与评估」：`sh run_multitask_finetune.sh`
    3.  注1：原始例子中脚本格式为「CR+LF」回车，在Linux下无法正确执行，这里已经修改
    4.  注2：run_convert_csv_to_tfrecrods.sh 中数据解包有误，已经屏蔽
    5.  注3：run_multitask_finetune.sh 中 CUDA 设置有误，已经屏蔽

### Notebook

按顺序执行，已经解决在本地运行时报错「UnrecognizedFlagError」的问题。

## 文本目录结构

```text
├── config    #配置文件目录
│   ├── OCEMOTION_preprocess_dev.json
│   ├── OCEMOTION_preprocess_train.json
│   ├── OCNLI_preprocess_dev.json
│   ├── OCNLI_preprocess_train.json
│   ├── TNEWS_preprocess_dev.json
│   └── TNEWS_preprocess_train.json
├── convert_csv_to_tfrecords.py    #csv 转 tfrecord 代码
├── multitask_finetune.py    #多任务训练代码
├── run_convert_csv_to_tfrecords.sh     #数据转换脚本
├── run_multitask_finetune.sh    #训练脚本
└── tianchi_datasets    #数据文件目录
    ├── OCEMOTION
    │   ├── dev.csv
    │   ├── dev.tfrecord
    │   ├── train.csv
    │   └── train.tfrecord
    ├── OCNLI
    │   ├── dev.csv
    │   ├── dev.tfrecord
    │   ├── train.csv
    │   └── train.tfrecord
    └── TNEWS
        ├── dev.csv
        ├── dev.tfrecord
        ├── train.csv
        └── train.tfrecord
``
