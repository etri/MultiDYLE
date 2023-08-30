# Multi-DYLE

This is code for the ACL 2023 paper [ExplainMeetSum: A Dataset for Explainable Meeting Summarization Aligned with Human Intent](https://aclanthology.org/2023.acl-long.731.pdf)

  * [Training](#training)

## Prerequsite
```bash
git clone https://github.com/etri/MultiDYLE.git
conda create -n multidyle
conda activate multidyle
```

### Build dataset
You can build dataset by executing python file.

```bash
git submodule update --remote
cd ExplainMeetSum
pip install nltk==3.6.2
python data/convert.py

# or you can specify your own path
python data/convert.py \
    --qmsum QMSUM_ROOT_DIRECTORY \
    --dialogue_act ACL2018_ABSSUMM_ROOT_DIRECTORY \
    --save_dir EXPLAIMEETSUM_DIRECTORY
```

### Dataset Format
You can find format of each dataset in [here](data/README.md)

#### TLDR;
##### ExplainMeetSum
ExplainMeetSum data is extended-version of QMSum dataset. To annotate evidence sentence by sentence, we had to split sentences as correctly as we can. Below are our methods how to split sentences

1. meeting transcripts
    * Committee: use `nltk.sent_tokenize()`
    * Academic(ICSI), Product(Ami): use dialogue act files in [ACL2018_AbsSumm](https://bitbucket.org/dascim/acl2018_abssumm/src/master/)
2. answers in query_list (i.e. summary)
    * use `nltk.sent_tokenize()` and merge sentences that splited wrongly (if you want to know, refer to `sentence_split.py`)
    * splited answers are already stored in `data/SummaryEvidence`

##### QMSum
ExplainMeetSum data should also contain `meeting_transcripts` which doesn't exist in `data/SummaryEvidence`.\
So, you need original QMSum/ dataset.

##### ACL2018_AbsSumm
We splited `meeting_transcripts` of ICSI and Ami dataset in QMSum by using dialogue act files.\
So, you need acl2018_abssumm/ for dialogue act files.

## Model
> _To be written_

### Dependency
Install dependencies via:
```
conda create -n explainmeetsum python=3.9.6
conda activate explainmeetsum
conda install pytorch==1.8.0 torchvision==0.9.0 torchaudio==0.8.0 cudatoolkit=11.1 -c pytorch -c conda-forge
pip install nltk==3.6.2 pyrouge==0.1.3 transformers==4.8.1 rouge==1.0.0 datasets==1.11.0
```

### Training

```
sh train_multi_dyle.sh
```
You can see other results by editing config.py

### Evaluation

```
sh test_multi_dyle.sh
```
You can see other results by editing config.py

### Results

|Model||R-1|R-2|R-L|
|-|-|-|-|-|
|$\text{Multi-DYLE}(\mathsf{X^{ROG}_o})$||35.93|11.24|31.26|
|$\text{Multi-DYLE}(\mathsf{X^{CES}_o})$||36.63|11.81|31.82|
|$\text{Multi-DYLE}(\mathsf{X^{ROG}_o}, \mathsf{X^{CES}_o})$||**37.55**|**12.43**|**32.76**|

* $\mathsf{X^{ROG}_o}$ : Train with turn-level oracle
* $\mathsf{X^{CES}_o}$ : Train with turn-level CESs
* $\mathsf{X^{ROG}_o}, \mathsf{X^{CES}_o}$ : Train with both

Without change in `config.py`, you can see result same with $(\mathsf{X^{ROG}_o}, \mathsf{X^{CES}_o})$

## Acknowledgements
Dataset named ExplainMeetSum is extended-version of QMSum([https://github.com/Yale-LILY/QMSum](https://github.com/Yale-LILY/QMSum)) for "[QMSum: A New Benchmark for Query-based Multi-domain Meeting Summarization](https://arxiv.org/pdf/2104.05938v1.pdf)", which distributed under MIT License Copyright (c) 2021 Yale-LILY.

Model named Multi-DYLE is extended-version of DYLE([https://github.com/Yale-LILY/DYLE](https://github.com/Yale-LILY/DYLE)) for "[DYLE: Dynamic Latent Extraction for Abstractive Long-Input Summarization](https://arxiv.org/pdf/2110.08168.pdf)", which distributed under MIT License Copyright (c) 2021 Yale-LILY.

## Citation
If you extend or use our work, pleas cite the [paper](https://aclanthology.org/2023.acl-long.731.pdf)

```bibtex
@inproceedings{kim2023explainmeetsum,
  title={ExplainMeetSum: A Dataset for Explainable Meeting Summarization Aligned with Human Intent},
  author={Kim, Hyun and Cho, Minsoo and Na, Seung-Hoon},
  booktitle={Proceedings of the 61st Annual Meeting of the Association for Computational Linguistics (Volume 1: Long Papers)},
  pages={13079--13098},
  year={2023}
}
```
