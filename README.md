# Multi-DYLE

This is code for the ACL 2023 paper [ExplainMeetSum: A Dataset for Explainable Meeting Summarization Aligned with Human Intent](https://aclanthology.org/2023.acl-long.731.pdf)

Model named Multi-DYLE is extended-version of DYLE([https://github.com/Yale-LILY/DYLE](https://github.com/Yale-LILY/DYLE)) for "[DYLE: Dynamic Latent Extraction for Abstractive Long-Input Summarization](https://arxiv.org/pdf/2110.08168.pdf)," which distributed under MIT License Copyright (c) 2021 Yale-LILY.


### Build dataset
You can build dataset by executing python file.

```bash
# or you can specify your own path
python data/convert.py \
    --qmsum QMSUM_ROOT_DIRECTORY \
    --dialogue_act ACL2018_ABSSUMM_ROOT_DIRECTORY \
    --save_dir EXPLAIMEETSUM_DIRECTORY
```

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

### Dependency
Install dependencies via:
```
conda create -n multidyle python=3.9.6
conda activate multidyle
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
