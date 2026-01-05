# Embedding-Model-Comparison-for-Semantic-Similarity-

**Objective:** This notebook evaluates and compares the performance of different models for generating sentence embeddings, focusing on their ability to capture semantic similarity. A paraphrase dataset is used, where a good embedding model should assign high similarity scores between original sentences and their paraphrases.

**Dataset:** The [Humarin ChatGPT Paraphrases](https://huggingface.co/datasets/humarin/chatgpt-paraphrases) dataset is employed. It consists of pairs of sentences and their paraphrased versions, allowing quantitative assessment of how well embeddings preserve semantic meaning.

**Embedding Models:** 
- **`Qwen-3-0.6B`** – specialized embedding model from the Qwen series for semantic tasks.
- **`thenlper/gte-large`** – a large embedding model trained for general-purpose semantic similarity. 


**General Language Models for Embeddings:**  
- **`Qwen-3-0.6B`** 
 

**Evaluation Approach:**  
- **Cosine Similarity:** Cosine similarity is applied to compare embeddings. Higher cosine similarity (max 1) indicates that two sentences are semantically closer.  

**Goal:** The notebook aims to compare:  
1. Different models of varying sizes on semantic similarity tasks.  
2. Specialized embedding models versus general-purpose LLMs used to compute embeddings.  

### Evaluation of Embedding Models for Paraphrase Recognition

The table below reports the performance of three embedding models on a paraphrase recognition task. The evaluation includes ranking-based metrics (MRR) and cosine similarity statistics.

| Model                     | Dim  | MRR@1_para_to_orig | MRR@3_para_to_orig | MRR@5_para_to_orig | Recall@5_orig_to_para | Mean Cosine | Median Cosine | Std Dev Cosine | Min Cosine |
|----------------------------|------|-------------------|-------------------|-------------------|----------------------|-------------|---------------|----------------|------------|
| thenlper/gte-large         | 1024 | 0.95576           | 0.970813          | 0.972193          | 0.93804              | 0.958973    | 0.965250      | 0.026975       | 0.754064   |
| Qwen/Qwen3-Embedding-0.6B | 1024 | 0.95602           | 0.970983          | 0.972294          | 0.93806              | 0.903549    | 0.915874      | 0.059133       | 0.460187   |
| Qwen/Qwen3-0.6B            | 1024 | 0.65816           | 0.698337          | 0.705468          | 0.66728              | 0.952301    | 0.958187      | 0.031589       | 0.300971   |

---

#### Analysis

1. **Ranking metrics (MRR):**  
   - **thenlper/gte-large** and **Qwen/Qwen3-Embedding-0.6B** both excel in ranking the correct paraphrases, with MRR@1 ≈ 0.95 and MRR@5 > 0.97. This indicates that the first paraphrase retrieved is usually correct, and almost all correct paraphrases appear within the top 5.

   - **Qwen/Qwen3-0.6B** performs worse in ranking (MRR@1 ≈ 0.66, MRR@5 ≈ 0.71), meaning it often fails to rank the correct paraphrases at the very top.

2. **Cosine similarity statistics:**  
   - **thenlper/gte-large:**  
     Shows very high mean (0.959) and median (0.965) cosine similarities with low standard deviation (0.027). This indicates that embeddings for paraphrases are tightly clustered near their original sentence, leading to stable retrieval.

   - **Qwen/Qwen3-Embedding-0.6B:**  
     Also has good ranking metrics, but cosine similarity is lower (mean 0.904, median 0.916) with higher variance (0.059). This suggests embeddings are more spread out in vector space, but the ranking mechanism still manages to retrieve correct paraphrases.

   - **Qwen/Qwen3-0.6B:**  
     Has high cosine similarities (mean 0.952, median 0.958), but ranking performance is poor. This indicates that while the paraphrase embeddings are close to their originals, the model’s global embedding space may be less discriminative, causing unrelated sentences to sometimes appear close in cosine space.



---

### Embedding Distribution Analysis

The following table reports descriptive statistics for cosine similarity distributions across embeddings. Unlike the previous analysis (focused on original–paraphrase pairs), here the evaluation considers the cosine similarity between each original phrase and all of its paraphrases.

| Model Name                | Mean Cosine | Median Cosine | Std Dev Cosine | Min Cosine | Max Cosine | Q1 Cosine | Q3 Cosine |
|----------------------------|-------------|---------------|----------------|------------|------------|-----------|-----------|
| thenlper/gte-large         | 0.697441    | 0.695064      | 0.029403       | 0.557135   | 1.000000   | 0.677500  | 0.714620  |
| Qwen/Qwen3-Embedding-0.6B | 0.266335    | 0.261487      | 0.079906       | -0.095807  | 1.000000   | 0.211567  | 0.315185  |
| Qwen/Qwen3-0.6B            | 0.818316    | 0.828979      | 0.067720       | -0.064870  | 1.000000   | 0.786128  | 0.863603  |

---

#### Analysis

1. **thenlper/gte-large:**  
   Cosine similarities are relatively high and tightly clustered, with a mean of 0.697 and a low standard deviation of 0.029. The interquartile range (0.678–0.715) is narrow, indicating consistent embeddings. The minimum similarity (0.557) is moderately high, showing that even the least similar paraphrase is fairly close in the embedding space.

2. **Qwen/Qwen3-Embedding-0.6B:**  
   Shows generally low cosine similarities (mean 0.266) with higher variability (std 0.080). The minimum cosine similarity is negative (-0.096). The interquartile range (0.212–0.315) is relatively wide, suggesting embeddings are more dispersed.

3. **Qwen/Qwen3-0.6B:**  
   Shows very high mean similarity (0.818) and median (0.829), meaning embeddings tend to cluster closely. The standard deviation (0.068) indicates moderate spread. Since the statistics consider all pairwise comparisons in the space, the high mean can partly reflect embeddings being generally close to many points, which may reduce discrimination and make retrieval harder for individual paraphrase matches.


---

### Conclusion

- **thenlper/gte-large:**  
  Excels in both ranking (MRR) and cosine similarity. High mean (0.959) and median (0.965) cosine values with low variance (0.027) indicate that paraphrase embeddings are tightly clustered around their originals, ensuring reliable retrieval.

- **Qwen/Qwen3-Embedding-0.6B:**  
  Performs well in ranking (MRR@1 ≈ 0.956) despite lower cosine similarities (mean 0.904, median 0.916) and higher variance (0.059). This suggests the model can still identify the correct paraphrases, but embeddings are more spread out in vector space.

- **Qwen/Qwen3-0.6B:**  
  Shows high cosine similarity (mean 0.952, median 0.958) but poor ranking performance (MRR@1 ≈ 0.658). This indicates that while paraphrases are close to their originals in embedding space, the global embedding distribution is less discriminative, causing unrelated sentences to sometimes appear close and reducing retrieval accuracy.

**Overall:**  
High cosine similarity alone does not guarantee strong retrieval performance. Effective paraphrase recognition requires both tight local clustering (high similarity with correct paraphrases) and sufficient global separation from unrelated sentences to achieve high ranking metrics.


---
