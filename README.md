# [Adversarial Over-Sensitivity and Over-Stability Strategies for Dialogue Models (CoNLL 2018)](https://arxiv.org/abs/1809.02079)

Authors' implementation of "Adversarial Over-Sensitivity and Over-Stability Strategies for Dialogue Models" (CoNLL 2018) in TensorFlow (the code was built upon TF 1.4, but any version later than that should also work). Note that if you use any Python version <= 3.5, you will need to manually change all [formatted string literals](https://docs.python.org/3/reference/lexical_analysis.html#f-strings).

Includes code for adversarial testing and training of the VHRED model, adversarial testing datasets, as well as pretrained checkpoints for normal and adversarially-trained models.

Authors: Tong Niu, Mohit Bansal

## Adversarial Testing

(1) Download the preprocessed version of [Ubuntu Dialogue Corpus](http://www.iulianserban.com/Files/UbuntuDialogueCorpus.zip), put the three raw .txt files under data/raw/, and put the vocab file Dataset.dict.pkl under data/).

(2) To generate the raw adversarial data for each strategy, run
```
python3 src/basic/generate_raw_adv.py [strategy]
```
where "strategy" is one of "normal_inputs", "random_swap", "stopword_dropout", "paraphrase", "grammar_errors", "add_not" and "antonym". We have also released the test set purturbed by each strategy under data/raw/. Note that to generate raw data for the Data-level Paraphrasing strategy, you need to download the small version of [PPDB2.0 dataset](http://paraphrase.org/#/download) and put it under data/; to generate raw data for Grammar Errors, you need to download the [AESW dataset](http://textmining.lt/aesw/aesw2016down.html) and put it under data/.

Note that raw adversarial data for the Generative-level Paraphrasing strategy need to be generated separately by downloading the implementation of the [Pointer-Generator model](https://github.com/abisee/pointer-generator) by [See et al. (2017)](https://arxiv.org/abs/1704.04368), and training on the [ParaNMT-5M dataset](https://drive.google.com/file/d/19NQ87gEFYu3zOIp_VNYQZgmnwRuSIyJd/view). After training, feed the contexts of the Ubuntu dataset to the trained model, and put the outputs under data/raw/.

(3) To index any raw data, run
```
python3 src/basic/preprocess.py [strategy]
```
where "strategy" is one of "normal_inputs", "random_swap", "stopword_dropout", "paraphrase", "generative_paraphrase", "grammar_errors", "add_not" and "antonym".

(4) To train the VHRED model by [Serban et al. 2016](https://arxiv.org/abs/1605.06069) from scratch, run
```
python3 src/main.py --batch_size [batch_size]  
```

(5) To test the VHRED model on the normal input and each adversarial strategy, run
```
python3 src/main.py --test_strategy [strategy] --start_epoch [start_epoch] --batch_size [batch_size]
```
where "strategy" is one of "normal_inputs", "random_swap", "stopword_dropout", "paraphrase", "generative_paraphrase", "grammar_errors", "add_not" and "antonym", and "start_epoch" is the epoch to restore from.

(6) To obtain the F1 scores for outputs generated based on the normal input and each adversarial strategy,
use the [evaluation code](https://github.com/julianser/Ubuntu-Multiresolution-Tools) by [Serban et. al. (2018)](https://arxiv.org/abs/1606.00776) on the output generated by testing on each adversarial strategy.

(7) Expected Results:
The model should obtain the following results ("N" stands for "Normal" and "A" stands for "Adversarial"):
![Result](image.png)

## Adversarial Training

(1) To train on each adversarial strategy, run
```
python3 src/main.py --train_strategy [strategy] --start_epoch [start_epoch] --batch_size [batch_size]
```
where "strategy" is one of "normal_inputs", "random_swap", "stopword_dropout", "paraphrase", "generative_paraphrase", "grammar_errors", "all_should_not_change", "add_not" and "antonym", and "start_epoch" is the epoch to restore from.

(2) To test on the trained adversarial model, run
```
python3 src/main.py --train_strategy [strategy] --test_strategy [strategy] --start_epoch [start_epoch] --batch_size [batch_size]
```

## Adversarial Data and Pre-trained Checkpoints
(1) The adversarial data for each strategy is provided under data/raw/

(2) Alternatively, you can also download our [pre-trained checkpoints (for the vhred model, as well as for each adversarially trained model)](https://drive.google.com/open?id=1ALmWLqvXdXj4LZylh0phuLCHJjSOjuYD) and put them under ckpt/ to reproduce the F1 results.

## Citations

If you happen to use our work, please consider citing [our paper](https://arxiv.org/abs/1809.02079).

```
@inproceedings{niu2018adversarial,
	author = {Niu, Tong  and Bansal, Mohit},
	title = {Adversarial Over-Sensitivity and Over-Stability Strategies for Dialogue Models},
	booktitle = {The SIGNLL Conference on Computational Natural Language Learning (CoNLL)},
	year = {2018},
}
```
