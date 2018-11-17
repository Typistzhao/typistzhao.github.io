---
layout: post
comments: false
categories: NLP
--- 

# Tie Input and Output Embedding

## 相关论文
> [Using the Output Embedding to Improve Language Models](https://arxiv.org/pdf/1608.05859.pdf)      
> 代码参见 [ptb_word_lm](https://github.com/ofirpress/UsingTheOutputEmbedding/blob/master/README.md)


> [Tying Word Vectors and Word Classifiers: A Loss Framework for Language Modeling](https://arxiv.org/pdf/1611.01462.pdf)   
> 代码参见 [论文图解](https://github.com/icoxfog417/tying-wv-and-wc)

## 笔记
1. 作者指出,使用RNN结构实现语言模型,loss方程的设计仅使用了one hot,即仅使用正确的word对应的损失.作者提出使用方程分布的形式添加额外的loss, 考虑如下情况: "高兴"和"开心"是相似词, 如果output layer的target word是"高兴", 那"开心"也很可能是正确的target,而one hot设计没有考虑这部分相似信息. paper使用了预训练得到的word embedding来描述词与词这种相似性, 设为groundtruth(其包含了每个词与其它词之间的相似度, 假定该相似关系构成了分布Q), 在projection layer部分,为了使target word下,其余词的分布P与Q相等, 可推得input embedding和output projection相等. 判断分布P与分布Q的差异使用了KL散度. [#摘录#](https://www.paperweekly.site/papers/notes/336)

## 个人想法
1. 实现是比较简单的，设置 decoder.weight=encoder.weight 即可。
2. 问题就是 decoder 的权重是不训练的？

## 代码
语言模型中也有涉及 [LM](https://github.com/pytorch/examples/blob/master/word_language_model/model.py)