# TextSumm

## Step 1: Set python3.6 environment
Open terminal in `bert_summ_demo` folder, and run the command below:
```
pip install -r requirements.txt
```
## Step 2: Download Stanford CoreNLP
We will need Stanford CoreNLP to tokenize the data. Download it [here](https://stanfordnlp.github.io/CoreNLP/) and unzip it.
then:
```
cd src
export CLASSPATH=/home/jwen6/Downloads/PreSumm/stanford-corenlp-3.9.2.jar
```
You should replace `/home/jwen6/Downloads/PreSumm` with the path where you download it on your laptop.

**Note**: 1. If the version is not 3.9.2, you should change the version as well. 2. You can also add `export CLASSPATH=/home/jwen6/Downloads/PreSumm/stanford-corenlp-3.9.2.jar` in the bash_file, so that you dont have to run the command above every time you open a new terminal.

## Step 3: Preprocess
open terminal in `src` folder, and run these commands below in oreder:
```
1. python preprocess.py -mode tokenize -raw_path ../raw_data -save_path ../raw_data_json
2. python preprocess.py -mode format_to_lines -raw_path ../raw_data_json -save_path ../raw_data -n_cpus 1 -use_bert_basic_tokenizer false -map_path MAP_PATH
3. python preprocess.py -mode format_to_bert -raw_path ../raw_data -save_path ../preprocessed_raw_data  -lower -n_cpus 1 -log_file ../logs/preprocess.log
```
**Note**: you dont have to do any change

## Then we can generate summaries now

- **To generate `extractive` summary**:

open terminal in `src` folder, and run the command below:
```
python train.py -task ext -mode test -batch_size 3000 -test_batch_size 500 -bert_data_path ../tokenized_raw_data/raw_data -log_file ../logs/demo_test_ext -model_path ../ext_model -sep_optim true -use_interval true -visible_gpus 1 -max_pos 512 -max_length 200 -alpha 0.95 -min_length 50 -result_path ../result_ext/ext_summ -test_from ../ext_model/model_step_50000.pt
```
**Note**: the `extractive` summary will be saved in `result_ext` folder and named as `ext_summ_step50000.candidate`.

- **To generate `abstractive` summary**:

open terminal in `src` folder, and run the command below:
```
python train.py -task abs -mode test -batch_size 3000 -test_batch_size 500 -bert_data_path ../tokenized_raw_data/raw_data -log_file ../logs/demo_test_abs -model_path ../abs_model -sep_optim true -use_interval true -visible_gpus 1 -max_pos 512 -max_length 200 -alpha 0.95 -min_length 50 -result_path ../result_abs/abs_summ -test_from ../abs_model/model_step_200000.pt 
```
**Note**: the `abstractive` summary will be saved in `result_abs` folder and named as `abs_summ.200000.candidate`.

## Note
if you want to generate summaries for a new text, you should go to `raw_data` folder, and replace the content in `demo.story` file. Do not add a new file, there must be only one file in this folder, and do not change the file name (keep it as `demo.story`). Then you can generate summaries by starting from `Step 3`.

