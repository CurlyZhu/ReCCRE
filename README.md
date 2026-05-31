# Understanding user-LLM conversations with the ReCCRE (Request Content, Context, Roles, and Expressions) dataset
Code and data of EACL 2026 findings ["Show or Tell? Modeling the evolution of request-making in Human-LLM conversations"](https://aclanthology.org/2026.findings-eacl.265/).

# Data

<i> Disclaimer: Our data work performs annotations on top of published data resources, including [WildChat](https://wildchat.allen.ai/) and the [Stanford Politeness corpus](https://aclanthology.org/P13-1025/). While the ReCCRE annotation scheme and analysis scripts are open-source, note that **this does not circumvent or override the licensing and copyrights of source datasets**, and this repo does not license the original WildChat database. Any further application or re-distribution of the annotated data should still abide fully with the source dataset, such as the requirements of attribution and risk management. </i>

## Core data source
<b>ReCCRE core users</b>: `core_user_encoded.npz` [[Download link]](https://drive.google.com/file/d/18ulWN_Ecvh8Ik4KYoKP2b2c7W_uAyLsF/view?usp=sharing) (182MB)

<b> When you have obtained the data, please put it under `datasets/wildchat/`. </b>

This includes the 59,175 chat input from 2,092 core users, derived from [the WildChat dataset](https://wildchat.allen.ai/) created and [published](https://openreview.net/forum?id=Bl8u7ZRlbM) by Wenting Zhao et al. under the [Open Data Commons Attribution (ODC-BY) License](https://opendatacommons.org/licenses/by/). The annotations and other derivations are under [the MIT license](https://opensource.org/license/mit) of this work.

The dataset follows the format of WildChat, and each data point inherits the major attributes from there (under keys `first_turn` and `conversation_metadata`), such as the model name and hashed IP.

The data used in our work is collected under a separate new key, `processed_first_turn`. The embeddings generated from gte-large-en-v1.5 have been incorporated.


## Stanford Politeness datasets
The ReCCRE annotation scheme can naturally be applied to other request-making conversations, including a key resource in related fields, the [Stanford Politeness corpus](https://aclanthology.org/P13-1025/). We provide processed versions of Stanford Politeness in the same format as the main dataset under `datasets/stanford_politeness_dataset/`.

## Anchor points
A few anchor points are used to construct and visualize the expression types, including the embeddings for the 41 main anchors in the experiments (`datasets/baseline_anchors_full.npy`) and some complementary anchors (`datasets/complementary_baselines.json`). In most cases these are util files that won't need to be inspected or modified.

# Prompt
The instructions for LLM annotators as prompts can be found in `prompts/annotate_components.json`.

# Code
We provide an all-in-one notebook that allows one-click replication of the analysis! 

Simply (1) fetch the ReCCRE core data `core_user_encoded.npz` following the above instructions, and (2) run `data_analysis.ipynb`.

# Q&A

- <i>The core users are defined by >=10 sessions created; why does some core users still have very small session numbers? </i>

This is defined by the key difference of any inputs and filtered, task-performing ones. We identify core users by one's total number of chats, but not all their inputs are eligible for the analysis: 
1. We study the <i>request-making</i> inputs and filter out ones that doesn't involve making a request (e.g. simple retrieval of fact); This is described in the paper and visible from an attribute `query_type` under `processed_first_turn` (need to be "performing tasks").
2. We also filtered out data during processing, including detection of possible spamming and filtering by language (only the sessions marked as English by WildChat are used for now).

<br>

- <i>I want to look at the raw data before processing. / I want to see other possible data collections besides core users. </i>

While we don't have plans to publish these raw data at this point, you may reach out to the first author with detailed descriptions of your request, e.g. why it's important to your work and how it will be used. We might be able to share some raw data files individually for relevant studies.

In the future, we will consider publishing the collection of non-core user data.

<br>

- <i>I want to conduct the analyses on my own data. / Can I have the full local annotation model or training script?</i>

The analysis script will depend on the preprocessing and annotations in the ReCCRE style. 

If you would like to transfer/perform the full data production process, we strongly recommend following the detailed descriptions in our paper and the prompt given to <i>fine-tune your LLM annotator</i>. The model we employed (Qwen2.5-14B-Instruct), while commonly used around the time of the work, may not reflect the best capability you can get at this point! We don't have plans to further introduce the model part.


# Future Aspects
We are excited to announce our latest follow-up work!
Please check out the preprint if you are interested in user-LLM interaction, mining large-scale chat logs, or behavioral patterns over time:

[Priming, Path-dependence, and Plasticity: Understanding the molding of user-LLM Interaction and its implications from (many) chat logs in the wild](https://arxiv.org/abs/2605.05767)


# Citations
```bibtex
@inproceedings{zhu-etal-2026-show,
    title = "Show or Tell? Modeling the evolution of request-making in Human-{LLM} conversations",
    author = "Zhu, Shengqi  and
      Rzeszotarski, Jeffrey  and
      Mimno, David",
    editor = "Demberg, Vera  and
      Inui, Kentaro  and
      Marquez, Llu{\'i}s",
    booktitle = "Findings of the {A}ssociation for {C}omputational {L}inguistics: {EACL} 2026",
    month = mar,
    year = "2026",
    address = "Rabat, Morocco",
    publisher = "Association for Computational Linguistics",
    url = "https://aclanthology.org/2026.findings-eacl.265/",
    doi = "10.18653/v1/2026.findings-eacl.265",
    pages = "5023--5034",
    ISBN = "979-8-89176-386-9"
}
```
