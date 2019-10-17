[**中文说明**](./README.md) | [**English**](./README_EN.md)

# nlp_classifier_zh
this project includes two classifier: multi-class case and multi-label case,  
and both fine-tune based on [RoBERTa-zh-Large](https://github.com/brightmart/roberta_zh).  

# multi_label_classifier  
### 1. dataset prepare
data fold includes train.csv, dev.csv, test.csv      
example：(like one-hot encode)  

| index | text | a | b | c | d | e | f |  
|-------|------|---|---|---|---|---|---|  
|  0    |   A  | 1 | 1 | 1 | 0 | 0 | 0 | 
|  1    |   B  | 1 | 0 | 1 | 0 | 0 | 0 | 
|  2    |   C  | 1 | 1 | 0 | 0 | 0 | 1 | 


### 2.code
just modify `run_classifier.py` based on raw bert code,  
add `MultiLabelClassifierProcessor` Class, use `sigmoid` replace `tf.nn.softmax()`   
for more detail, [run_classifier.py](https://github.com/AarynBaelish/nlp_classifier_zh/blob/master/multi_label_classifier/run_classifier.py)  

### 3. model train  
This example code fine-tunes RoBERTa-zh-Large on your own train.csv and evalutes on dev.csv. `BERT_BASE_DIR` is RoBERTa-zh-Large path, `DATA_DIR` is data fold path，`TRAINED_CLASSIFIER` is RoBERTa-zh-Large path here.

```bash
export BERT_BASE_DIR=./pretrain_models/roeberta_zh_L-24_H-1024_A-16
export DATA_DIR=./multi_label_classifier/data
export TRAINED_CLASSIFIER=./pretrain_models/roeberta_zh_L-24_H-1024_A-16

python run_classifier.py \
  --task_name=mlc \
  --do_train=true \
  --do_eval=true \
  --data_dir=$DATA_DIR \
  --vocab_file=$BERT_BASE_DIR/vocab.txt \
  --bert_config_file=$BERT_BASE_DIR/bert_config.json \
  --init_checkpoint=$TRAINED_CLASSIFIER \
  --max_seq_length=512 \
  --output_dir=./multi_label_classifier/mlc_output/
```

### 4. model predict
This example code predicts your own test.csv data.`BERT_BASE_DIR` is RoBERTa-zh-Large path, `DATA_DIR` is data fold path，`TRAINED_CLASSIFIER` is your own fine-tuned model path here.

```bash
export BERT_BASE_DIR=./pretrain_models/roeberta_zh_L-24_H-1024_A-16
export DATA_DIR=./multi_label_classifier/data
export TRAINED_CLASSIFIER=./pretrain_models/multi_label_classifier/mlc_output/model.ckpt-10000

python run_classifier.py \
  --task_name=mlc \
  --do_train=true \
  --do_eval=true \
  --do_predict=true \
  --data_dir=$DATA_DIR \
  --vocab_file=$BERT_BASE_DIR/vocab.txt \
  --bert_config_file=$BERT_BASE_DIR/bert_config.json \
  --init_checkpoint=$TRAINED_CLASSIFIER \
  --max_seq_length=512 \
  --output_dir=./multi_label_classifier/mlc_output/
```

## Reference：  
1. [RoBERTa: A Robustly Optimized BERT Pretraining Approach](https://github.com/google-research/bert)
2. [BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding](https://github.com/brightmart/roberta_zh)
