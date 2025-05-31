# PoliticalK.O🥊

⚠️ Due to the large size of some datasets over 25MB, we will upload all the reamining datasets soon!

We introduce the **PoliticalK.O**🥊 (Political comments for Korean Offensive language) dataset which contains summarized 114,000 news articles and 9.28 million user comments covering six topics: _Presidential Office_, _National Assembly / Political Parties_, _North Korea_, _Administration_, _National Defense / Foreign Affairs_, and _General Politics_. We conducted three distinct judgment strategies to enable label inference for newly collected comments: **supervised ensemble judgment** (SEJ) on the left, **prompt-variants ensemble judgment** (PEJ) in the center, and **multi-debate reasoning judgment** (MRJ) on the right. Each constitutes a distinct approach characterized by its methodology for decision guidance and aggregation.

<img width="1342" alt="Image" src="https://github.com/user-attachments/assets/ca2e5dd4-a633-4a0c-bc80-f4cd8042053e" />


### Dataset Construction
We collected news articles and user comments from [Naver](https://news.naver.com/section/100), South Korea's largest news platform, covering all politic news articles published in 2024. Please check out Table 5 in our paper for the detailed statistics for each topic.


### Supervised Ensemble Judgment (SEJ)
We fine-tuned [multilingual RoBERTa](https://huggingface.co/FacebookAI/xlm-roberta-base) and [KcBERT](https://huggingface.co/beomi/kcbert-base) on five Korean offensive language datasets and made a decision by majority voting to aggregate the results. We re-split the original datasets to ensure a balanced label distribution and trained the models with diverse hyperparamter settings.

|offensive language dataset|collection period|volume|
|:---|:---|:---|
|[K-Haters](https://github.com/ssu-humane/K-HATERS?tab=readme-ov-file)|July - August 2021|192.1k|
|[KODOLI](https://github.com/cardy20/KODOLI/tree/main)|October - December 2020|38.5k|
|[KoLD](https://github.com/boychaboy/KOLD)|March 2020 - March 2022|40.4k|
|[K-MHaS](https://github.com/adlnlp/K-MHaS)|January 2018 - June 2020|109.6k|
|[UnSmile](https://github.com/smilegate-ai/korean_unsmile_dataset)|January 2019 - June 2020|18.7k|
|**PoliticalK.O**🥊|**January - December 2024**|**9.28m**|


### Prompt-variants Ensemble Judgment (PEJ)
We instructed three recent LLMs known for strong performance in Korean: [Exaone](https://huggingface.co/LGAI-EXAONE/EXAONE-3.5-7.8B-Instruct), [Trillion](https://huggingface.co/trillionlabs/Trillion-7B-preview), and [HyperclovaX](https://huggingface.co/naver-hyperclovax/HyperCLOVAX-SEED-Text-Instruct-1.5B). We employed five prompt variants and made a decision by majority voting to aggregate the results.

|prompt variants|details|
|:---|:---|
|Vanilla prompt|provide the target comment to be classified with standard formulation|
|Defn prompt (D)|provide an explicit definition of offensive language targeted to politics|
|Summ prompt (S)|provide the summarized source article's title and content into three sentences|
|FewShots prompt (F)|provide few-shot samples which were drawn from other articles on that topic|
|D+S+F prompt|combines all above elements into a single formulation|


### Multi-debate reasoning Judgment (MRJ)
We conducted multi-agent framework for offensive language detection by assigning distinct personas to each agent. We instructed agents with opposing viewpoints to generate a stance for each rationale. For example, an agent with an offensive perspective was asked to debate a rationale from a non-offensive standpoint, and vice versa. We finally made a decision by a judge agent based on all the rationales and stances.

In this case, we employed the model [Trillion](https://huggingface.co/trillionlabs/Trillion-7B-preview) because it exhibited the best test performance on five prior offensive language datasets across the three recent LLMs employed in conducting PEJ. Please check out Appendix B.3 in our paper for more details.
