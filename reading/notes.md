These are my reading notes when I read papers, articles, or books. I will summarize the key points, my personal connections, and any outside connections I can make with the material. I will also include any questions or thoughts that arise while reading.

# Contents
- [Papers](#papers)
- [RAG](#RAG)
    - [Papers](#Papers)
    - [Online Articles](#Online-Articles)   
    - [Implementation](#Implementation)

# Papers
- Wei, Jason, Xuezhi Wang, Dale Schuurmans, Maarten Bosma, Brian Ichter, Fei Xia, Ed Chi, Quoc Le, and Denny Zhou. “Chain-of-Thought Prompting Elicits Reasoning in Large Language Models.” arXiv, January 10, 2023. http://arxiv.org/abs/2201.11903.

The field of large language models (LLMs) has seen significant advancements, bringing numerous benefits across various applications. Despite these gains, LLMs often struggle with tasks that demand arithmetic, commonsense, and symbolic reasoning. Earlier studies have investigated rationale-augmented training and fine-tuning methods, which rely on extensive sets of high-quality rationales. Moreover, traditional prompting methods typically underperform in scenarios requiring multi-step reasoning.

Addressing this challenge is critical, as robust reasoning capabilities are essential for LLMs to effectively tackle diverse applications from various fields. If LLMs could fully master these abilities, they would be well-equipped to handle more complex reasoning tasks.

In their paper, the authors highlight "chain of thought" prompting as a key innovation. This method introduces intermediate reasoning steps that culminate in a final answer, a departure from the conventional direct input-output pair format. They found that prompts structured as triples—comprising an input, a chain of thought, and an output—markedly improve performance on tasks involving arithmetic, commonsense, and symbolic reasoning.

Previously, state-of-the-art solutions for generating intermediate steps in arithmetic reasoning included training from scratch (Ling et al., 2017), fine-tuning pre-trained models (Cobbe et al., 2021), and employing neuro-symbolic methods (Roy and Roth, 2015; Chiang and Chen, 2019; Amini et al., 2019). The primary alternative technique before this study was in-context few-shot learning via prompting (Brown et al., 2020), which involves providing the model with a few input-output exemplars of the task without needing fine-tuning. However, this approach generally falls short in complex reasoning tasks, with minimal performance gains even as model sizes increase.

The paper presents compelling evidence that chain of thought prompting enhances reasoning capabilities in LLMs by comparing it against standard prompting across various benchmarks and scenarios. This comparative analysis consistently shows that chain of thought prompting outperforms the standard approach. An ablation study further confirmed these findings, demonstrating that alternative prompting techniques like equation-only, variable compute-only, and reasoning after answer did not match the effectiveness of chain of thought prompting. Additionally, robustness tests involving varied few-shot exemplar orders and different annotators' prompts still favored chain of thought in terms of solve rates.

While the paper is comprehensive, it would have been beneficial to explore how the chain of thought performs on content absent from the training data, particularly in commonsense reasoning scenarios. For instance, if the training corpus lacks references to cats, would the model correctly infer that "a cat flies in the sky" is incorrect using chain of thought prompting, even with a large model architecture? The provided examples are relatively simple, and assessing the technique against a broader spectrum of problem difficulties could provide deeper insights into its scalability and limitations.

The paper also discusses several potential limitations of chain of thought prompting. It remains uncertain whether the model truly engages in human-like reasoning or merely mimics the provided syntactic structure. Scaling up chain of thought annotations for fine-tuning is challenging, potentially limiting the approach's applicability to larger datasets or more diverse question sets. Furthermore, there is no assurance that the generated reasoning paths are accurate or logical, posing reliability concerns in critical applications. Finally, the observed improvements in solve rates are primarily noted in large-scale models, which may not be cost-effective for practical applications. Future research should investigate the efficacy of chain of thought prompting in smaller models to broaden its applicability.




# RAG 
## Papers
- Lewis, Patrick, Ethan Perez, Aleksandra Piktus, Fabio Petroni, Vladimir Karpukhin, Naman Goyal, Heinrich Küttler, et al. “Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks.” arXiv, April 12, 2021. http://arxiv.org/abs/2005.11401.


The first paper talks about RAG - models which combine pre-trained parametric and non-parametric memory for language generation. RAG models, the parametric memory is a pre-trained seq2seq transformer and the non-parametric memory is a dense vector index of Wikipedia, accessed with a pre-trained neural retriever.

other resource: Youtube video
- https://www.youtube.com/watch?v=JGpmQvlYRdU (by the Author of the paper)
- https://www.youtube.com/watch?v=dzChvuZI6D4 (explanation of the paper)
## Online Articles
### Introduction
- [Introduction to RAG — GenAI Systems for Knowledge](https://medium.com/curiosity-ai/introduction-to-rag-genai-systems-for-knowledge-918a34054228)
- [A Brief Introduction to Retrieval Augmented Generation(RAG)](https://medium.com/ai-in-plain-english/a-brief-introduction-to-retrieval-augmented-generation-rag-b7eb70982891)

RAG stands for Retrieval-Augumented Generation. RAG system works two steps: 1. Retrieve: It retrieves relevant information from a large corpus of text. 2. Generate: It generates a response based on the retrieved information. Common Use-case: question answering, document summarization, content generation.
![RAG](figs/rag.webp "How rag works")
Why do we need RAG? 1. avoid hallucination 2. timeliness 3. LLMs cannot access private data, feed more internal/user private data to get customized results. 4. Answer constraint. 

## Implementation
- https://scriv.ai/guides/retrieval-augmented-generation-overview/
- https://towardsdatascience.com/a-beginners-guide-to-building-a-retrieval-augmented-generation-rag-application-from-scratch-e52921953a5d

# Intro to LLM Fundamentals: What are LLMs and How do they come to be? 

- Serrano, S., Brumbaugh, Z., & Smith, N. A. (2023). Language Models: A Guide for the Perplexed. http://arxiv.org/abs/2311.17301

This paper provides a comprehensive, non-technical introduction to language models (LMs), including large language models (LLMs). It offers a high-level overview of how language models, including LLMs, are trained, evaluated, and utilized. I found the sections discussing data particularly insightful, and the discussion on practical applications of LLMs thought-provoking.

Data is crucial both for training and testing. Testing serves as an indicator of the system's output quality and should not be used for any other purpose prior to the final test. NLP tasks depend heavily on both the quality and quantity of training data. Understanding a model involves knowing its training data. However, nowadays, companies developing LLMs are reluctant to share their training datasets. This leads to concerns about hidden training data; for instance, if a model answers a complex question accurately and clearly, we should be impressed only if we are certain that the question and answer were not in the training data. Without access to the training data, we cannot verify whether the model is genuinely being tested fairly or simply recalling answers it has already seen.

The paper also discusses "hallucination," where the content generated by LLMs is inaccurate or nonfactual. From my own experience using ChatGPT, I have noticed this issue as well. The paper suggests that while LLMs rely on training data, they do not directly access this data; instead, they seem to encode patterns from the data but do not "remember" the data precisely at all times. Thus, for topics with ample supporting data and straightforward tasks, the likelihood of hallucination is lower. Conversely, with more complex tasks or less-discussed subjects, hallucination becomes less surprising. Moreover, since the training data may contain incorrect or biased information, the model might encode these inaccuracies.

The discussion on how to effectively use LLMs is also worth considering. The paper mentions that using LLMs to summarize newspapers may not be worthwhile since the first paragraph of a news article typically summarizes the entire piece. This perspective is particularly interesting in today's context, where there is significant buzz around employing LLMs. However, the challenge remains on how to fully leverage these technologies to solve everyday problems, which continues to be an area needing exploration.

- [Training Data for the Price of a Sandwich](https://foundation.mozilla.org/en/research/library/generative-ai-training-data/common-crawl/)

This article introduce Common Crawls and its usage in training LLMs. Common Crawl is a non-profit organization that crawls the web and freely provides its data to the public. This data can be used to train LLMs.

# Intro to HCI
- [Human-Computer Interaction (HCI)](https://www.interaction-design.org/literature/topics/human-computer-interaction)
- Schmager, Stefan; Pappas, Ilias; and Vassilakopoulou, Polyxeni, "Defining Human-Centered AI: A Comprehensive Review of HCAI Literature" (2023). MENACIS 2023. 13.

# Evaluation Methods of LLMs
- [EvalLM: Interactive Evaluation of Large Language Model Prompts on User-Defined Criteria](https://arxiv.org/pdf/2309.13633)

**Personal Connection**: The evaluation of large language models is both a current and challenging topic in the field. Historically, I believe that human evaluation was predominantly used due to the lack of objective ground truths and the subjective nature of evaluation criteria. I found the framework and the explanation of the UX/UI design to evaluate prompts, as illustrated in the paper, particularly enlightening. The concept of human and AI collaboration within this framework is very inspiring to me.
1. **Dual Components of Evaluation**:
    - The evaluation is driven by two LLM-based components: the automatic evaluation assistant and the criteria review.
        a). The automatic evaluation features user-defined criteria, allowing users to tailor their own evaluation metrics. The paper leverages two state-of-the-art methods—LLM-as-a-judge and FLASK—to empower the evaluation assistant to assess prompt outputs against these user-defined criteria.
        b). The criteria review component seeks to ensure that evaluation criteria are comprehensive and fair. It reviews users' criteria and offers revision suggestions, enhancing the overall reliability of the evaluations.

2. **UX/UI Interface Enhancements**:
    - The interface facilitates straightforward comparisons between previous and current prompts. This comparison is augmented by a scoring system that helps fine-tune prompts based on iterative evaluations.

**Outside Connection**:
In a recent project, I utilized GPT-4 to summarize customer support tickets to identify the most common questions and support needs. Despite using various prompts, the outputs varied significantly in format and content summary. Due to the vast number of tickets, it was impractical to manually verify the accuracy of these summaries. The methodologies described in the EvalLM paper, such as the evaluation assistant and criteria review components, could potentially refine the way outputs from GPT-4 are evaluated and improve the design of evaluation criteria for better accuracy in summaries.
