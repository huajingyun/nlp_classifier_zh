
# nlp_classifier_zh
包含multi-class和multi-label两种分类问题，  
采用roberta-large作为加载的预训练模型进行微调finetuning  

## multi_label_classifier  
data文件夹中存放train.csv, dev.csv, test.csv    
文件格式：  

| index| text | a | b | c | d | e | f | 
|------|------|---|---|---|---|---|---|
|   0  |   A  | 1 | 1 | 1 | 0 | 0 | 0 | 
|   1  |  B   | 1 | 0 | 1 | 0 | 0 | 0 | 
|   2  |  C   | 1 | 1 | 0 | 0 | 0 | 1 | 

label部分类似one-hot格式  

在bert代码的基础之上，仅需修改run_classifier.py文件，  
其中添加MultiLabelClassifierProcessor类，将其中tf.nn.softmax()改为sigmoid  
更多细节详见代码

## Reference：  
1. bert
2. roberta

Eng. version


