
## Weakness 1 instruction quality

We sincerely thank the reviewers for their valuable comments. Regarding the choice of instruction generation methods, we conducted extensive comparative experiments. Specifically, **we tested various large language models, including ChatGPT-3.5, GPT-4, and Qwen-72B, and also included manually written instructions as a baseline**. The results demonstrate that ChatGPT-3.5 offers the best overall performance in terms of **generation quality** (as shown in the appendix with typical smart home commands such as "Turn on the light in the living room"), **efficiency, and cost-effectiveness**. It is worth noting that **manually written instructions often suffer from semantic ambiguity in practical testing**, which is one of the key reasons we ultimately selected ChatGPT-3.5 as the primary generation tool.

## Weakness 2 experimental supplement

We sincerely appreciate the reviewer’s recognition of our research objectives. **It is important to clarify that during the time this study was conducted, models such as O1, O3, R1, and QWQ had not yet been officially released**. Following your suggestion, we have promptly conducted supplementary tests on these newly released models and updated the relevant sections of the paper with the detailed results. However, due to time constraints and the lack of batch API support for DeepSeek, as well as the relatively slow inference speed of QWQ-32B, we are currently unable to provide the full evaluation results for the DeepSeek and QWQ models. Nevertheless, our preliminary experiments indicate that the findings are consistent with the conclusions presented in our paper.

| Model         | VS SUCC | VS F1  | IS SUCC | IS F1  | VM SUCC | VM F1  | IM SUCC | IM F1  | MM SUCC | MM F1  | ALL SUCC | ALL F1 |
|---------------|---------|--------|---------|--------|---------|--------|---------|--------|---------|--------|----------|--------|
| o1-mini       | 38.65   | 39.46  | 20.34   | 30.34  | 1.19    | 2.09   | 0.0     | 27.75  | 0.73    | 15.13  | 21.85    | 23.20  |
| o3-mini       | 27.57   | 28.16  | 45.43   | 57.51  | 4.56    | 13.64  | 0.0     | 33.08  | 0.21    | 16.30  | 27.66    | 27.32  |
| Deepseek-V3-ICL | 80.00 | 80.06  | 58.58   | 58.48  | 54.69   | 77.29  | 11.34   | 32.22  | 5.35    | 52.74  | 53.41    | 58.74  |
| GPT-4o-ICL    | 74.25   | 74.30  | 87.10   | 87.09  | 45.71   | 71.48  | 61.86   | 81.18  | 23.35   | 78.98  | 66.85    | 79.58  |
| o1-mini-ICL   | 75.42   | 75.94  | 86.80   | 86.90  | 53.39   | 75.52  | 79.17   | 89.09  | 32.25   | 81.37  | 69.47    | 81.42  |
| o3-mini-ICL   | 83.77   | 84.25  | 88.36   | 88.38  | 57.51   | 80.49  | 64.04   | 85.19  | 38.49   | 85.57  | 74.44    | 85.75  |

## Weakness 3 inference latency

We appreciate the reviewer’s attention to the practical deployment of the system. Our tests on an RTX 3090 platform show that the inference latency of the locally deployed model can be controlled within the range of a few seconds, which generally meets practical requirements. However, to further enhance user experience—ideally reducing latency to under one second—optimizing inference speed is indeed one of the key directions for our future work.

| Models       | LLaMa3-8b | Qwen2.5-7B | Mistral-7B | Gemma2-9B | Deepseek-V3 | GPT-4o |
|--------------|-----------|------------|------------|-----------|--------------|--------|
| Latency      | 2.49s     | 1.99s      | 3.05s      | 6.92s     | 6.79s        | 1.16s  |

## Weakness 4 release code and dataset

As stated in the abstract, we are committed to releasing all research code and datasets. The GitHub repository is currently being organized and will be made publicly available soon.

## Suggestions 

Thank you for your suggestions.


## Weakness 1 distribution of different types of instructions

We sincerely thank the reviewer for the in-depth attention to this issue. **We believe the observed distribution is realistically reasonable.** From the perspective of load balancing theory, humans tend to execute a single type of task multiple times rather than perform multiple distinct tasks at once [1]. As a result, single-device operation commands are significantly more frequent than multi-device operation commands.
﻿
Regarding the relatively small number of IM instructions, from a probabilistic standpoint, **the likelihood of executing entirely valid or entirely invalid operations across multiple devices is much lower than that of mixed valid and invalid operations**. This trend is also reflected in the data distribution shown in Table 2, where the number of VM and IM instructions is roughly similar, yet both are considerably fewer than MM instructions.

> [1] John Sweller. 2011. Chapter two - cognitive load theory. volume 55 of Psychology of Learning and Motivation, pages 37–76. Academic Press.


## Weakness 2 quality verification

We sincerely apologize for the confusion caused by our incomplete description. As you correctly pointed out, our human quality control process does not focus solely on fluency. **We certainly also consider the consistency between the synthesized human instructions and the corresponding machine instructions.** If any inconsistency is found, the instruction is directly assigned a score of 0. We have revised the relevant statements in the paper accordingly.


## Suggestions 

Thank you very much for your valuable suggestion. We have adopted your feedback and made the corresponding revisions in the paper.


## Weakness 1 invalid instruction quality control

We sincerely appreciate the reviewer’s attention to this part of the work. We would like to clarify that the design of the invalid instructions does not stem from flaws in the synthesis process. In fact, **these invalid instructions are intentionally crafted to simulate two types of realistic error scenarios: operating on non-existent devices and operating on non-existent device attributes**. During the synthesis process, the quality control standards applied to  invalid  and valid instructions are equivalent; therefore, no additional quality control measures are required specifically for erroneous instructions. For better understanding, we kindly refer you to the detailed data examples provided in the appendix A.

## Weakness 2 RAG experiments

We have supplemented the statistical table of retrieval context error ratio in the paper.

|                          | None Useful State Context | None Useful Method Context | Lack Useful State Context | Lack Useful Method Context |
|--------------------------|---------------------------|-----------------------------|----------------------------|-----------------------------|
| Single Device instruction| 74.49%                    | 75.97%                      | -                          | -                           |
| Multi Devices instructions| 46.47%                   | 48.74%                      | 46.65%                     | 45.04%                      |

As discussed in Section 5.2, in most cases, the retrieved context does not match the required device states or methods specified by the input instruction. In the absence of correct context, the model exhibits the following behavior:


This context mismatch leads to a counterintuitive phenomenon—performance degradation on simple and effective tasks, but unexpected improvement on more difficult ones. Upon further analysis, **we find that this is caused by insufficient embedding similarity between device states/methods and user instructions, making it difficult for the similarity-based retrieval mechanism to distinguish between different context fragments effectively**. To address this issue, we suggest training a specialized retrieval optimization model to improve context alignment accuracy.









