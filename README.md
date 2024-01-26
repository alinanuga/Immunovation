# Immunovation
Оптимизация терапевтических антител против SARS-COV-2 с помощью ML


## Установка

```bash
conda create -n <env_name> python==3.9
conda activate <env_name>
pip install -r requirements.txt
```

### 1. Fine-tune модели

```bash
python bert_finetuning_er_main.py --config ./config/common/bert_finetuning_er_common_Cov_abdab.json
```
Результат сохраниться в папке `../Result_cov_adbab/checkpoints/BERT-Finetunning-Antibody-Binding-common-abdab/XXXX_XXXXXX` folder.

### 2. Тренировка модели для генерации

```bash
python bert_finetuning_seq2seq_main.py --config ./config/common/bert_finetuning_er_seq2seq_common.json
```
Результат сохранится в папке `../Result_seq2seq/checkpoints/ABAG-Finetuning-Seq2seq-Common/XXXX_XXXXXX/`.

### 4. Генерауия последовательностей CDR3H антитела

```bash
python generate_antibody.py --config ./config/common/seq2seq_generate.json
```
Результат `../Result_seq2seq_gen/datasplit/CoV_AbDab-Seq2seq-Evaluate-Common/XXXX_XXXXXX/result.csv`.

Сгенерированный файл содержит пять столбцов: Antigen, Generated_CDR_H3, Heavy_Chain, Light_Chain. Среди них Antigen и Light_Chain не отличаются от входных данных, Generated_CDR_H3 представляет собой последовательность области cdrh3, сгенерированную моделью, а Heavy_Chain заменяет естественную тяжелую область cdrh3 сгенерированной последовательностью Generated_CDR_H3.

### 5. Предсказание аффиннности сгенерированных антител

```bash
python eval_generate_seq.py --config ./config/common/bert_eval_generation.json
```
Результат `../Result_eval/datasplit/Eval-genetation/XXXX_XXXXXX/test_result.csv` 

Сгенерированный файл содержит четыре столбца: heavy, light, antigen, y_pred. В данном случае, чем выше значение y_pred, тем выше вероятность хорошей аффинности. 

