# TextSumm
Download Stanford CoreNLP:
1. Download the package on the website, and unzip it.
2. cd src
3. export CLASSPATH=/home/jwen6/Downloads/PreSumm/stanford-corenlp-3.9.2.jar

Preprocess:
1. python preprocess.py -mode tokenize -raw_path ../raw_data -save_path ../raw_data_json
2. python preprocess.py -mode format_to_lines -raw_path ../raw_data_json -save_path ../raw_data -n_cpus 1 -use_bert_basic_tokenizer false -map_path MAP_PATH
3. python preprocess.py -mode format_to_bert -raw_path ../raw_data -save_path ../preprocessed_raw_data  -lower -n_cpus 1 -log_file ../logs/preprocess.log

To generate extractive summary:
	Run the command below in the ??src?? folder:
python train.py -task ext -mode test -batch_size 3000 -test_batch_size 500 -bert_data_path ../tokenized_raw_data/raw_data -log_file ../logs/demo_test_ext -model_path ../ext_model -sep_optim true -use_interval true -visible_gpus 1 -max_pos 512 -max_length 200 -alpha 0.95 -min_length 50 -result_path ../result_ext/ext_summ -test_from ../ext_model/model_step_50000.pt

To generate extractive summary:
	Run the command below in the ??src?? folder:
python train.py -task abs -mode test -batch_size 3000 -test_batch_size 500 -bert_data_path ../tokenized_raw_data/raw_data -log_file ../logs/demo_test_abs -model_path ../abs_model -sep_optim true -use_interval true -visible_gpus 1 -max_pos 512 -max_length 200 -alpha 0.95 -min_length 50 -result_path ../result_abs/abs_summ -test_from ../abs_model/model_step_200000.pt 
