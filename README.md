# Abstract_Goal
Variational modeling of abstract goal for next action anticipation

This code accompines the paper [Predicting the Next Action by Modeling the Abstract Goal](https://arxiv.org/abs/2209.05044)

# Features

  * Download RGB, Flow and OBJ features from [RULSTM](https://github.com/fpv-iplab/rulstm) project, specifically this script
    https://github.com/fpv-iplab/rulstm/blob/master/RULSTM/scripts/download_data_ek55.sh
    The data is now in ```<pwd>/data/ek55/<rgb><flow><obj>```
  * Download EK55 [annotations](https://github.com/fpv-iplab/rulstm/tree/master/RULSTM/data/ek55) and suppose you save in ```<annot>```
  * Change the path of the following variables in ```main.py```
    * ```train_ann_file = '<annot>/data/ek55/training.csv'```
    * ```val_ann_file = '<annot>/data/ek55/validation.csv'```
    * ```test_ann_files = ['<annot>/data/ek55/test_seen.csv', '<annot>/data/ek55/test_unseen.csv']```
    * ```paths = { 'rgb': '<pwd>/data/ek55/rgb', 'flow': '<pwd>/data/ek55/flow', 'obj': '<pwd>/data/ek55/obj'}```

# Training
  * We train two models - one for verb and another for noun using each feature RGB/FLOW/OBJ to get 6 models.
  * Example, train a verb model with RGB features
  
  ``` python main.py  --modality rgb --dataset ek55 --outputs verb --obs_sec 2 --ant_sec 1.0  --latent_dim 128 --num_act_cand 10 --num_goal_cand 3  --hidden_dim 256 --n_layers 1 --nepochs 15 --losses og na ng oa gc --scheduler none --batch_size 256 --sampling 10 ```
  * Explanation of options
    * ```--modality, choices=['rgb', 'flow', 'obj' ], 'Choose tsn (rgb or flow) or obj or a combination for fusion' ```
    * ```--dataset, choices=['ek55', 'ek100', 'egtea'], 'Choose between EK55, EK100 and EGTEA' ```
    * ```--outputs, choices=['verb', 'noun', 'act'], Choose between verb or noun (for all datasets) or act (only for EGTEA)```
    * ```--obs_sec, default=2, 'Choose observed duration in secs 1-6'```
    * ```--ant_sec, choices=[2, 1.75, 1.5, 1.25, 1, 0.75, 0.5, 0.25], 'Choose anticipation time' - (1 for EK55 and EK100, 0.5 for EGTEA)```
    * ```--latent_dim, default=128, 'Choose latent dimension for observed and next goal distribution'```
    * ```--num_act_cand, default=10, 'Choose number of candidates to sample for next verb/noun' ```
    * ```--num_goal_cand, default=3, 'Choose number of abstract goals to sample'```
    * ```--hidden_dim, type=int, default=256, 'Choose hidden dimension for RNN'```
    * ```--n_layers, default=1, help='Choose # layers in RNN for next visual feature```
    * ```--dropout, default=0.8, 'Dropout rate'```
    * ```--sampling, default=10, 'Choose sampling freq of input features' (10 means 30 fps/10 = 3fps)```
    * ```--nepochs, default=10, 'Choose number of training epochs'```
    * ```--scheduler, default='none', choices=['cosine', 'none'], 'Choose scheduler - cosineLR or none (for AdamW optimizer)'```
    * ```--batch_size, default=256, choices=[32, 64, 128, 256], 'Choose batch_size'```
    * ```--losses', choices=[og', 'na', 'ng', 'oa', 'gc'], 'Choose losses'``` 
      * ```og - KLD for observed goal```
      * ```na - CE for next action(required)```
      * ```ng - KLD for next goal(required)```
      * ```oa - CE for observed action```
      * ```gc - symmetric KLD between observed and next goal```

# Testing on test set
  
  * Train all the 6 models - 3 for verb using RGB, Flow and Obj and 3 for noun using RGB, Flow and Obj
    * ```hidden_dim=256``` for verb models and  ```hidden_dim=1024``` for noun models
  * Run main.py as follows



# Acknowledgment

This research/project is supported in part by the National Research Foundation, Singapore under its AI Singapore Program (AISG Award No: AISG2-RP-2020-016) and the National Research Foundation Singapore under its AI Singapore Program (Award Number: AISG-RP-2019-010).

  
In case of issues, please write to debadityaroy5555 at gmail dot com
