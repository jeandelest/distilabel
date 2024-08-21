---
hide:
  - navigation
---
# GenerateShortTextMatchingData

Generate short text matching data with an `LLM` to later on train an embedding model.



`GenerateShortTextMatchingData` is a `Task` that generates short text matching data with an
    `LLM` to later on train an embedding model. The task is based on the paper "Improving
    Text Embeddings with Large Language Models" and the data is generated based on the
    provided attributes, or randomly sampled if not provided.



### Note
Ideally this task should be used with `EmbeddingTaskGenerator` with `flatten_tasks=True`
with the `category="text-matching-short"`; so that the `LLM` generates a list of tasks that
are flattened so that each row contains a single task for the text-matching-short category.



### Attributes

- **language**: The language of the data to be generated, which can be any of the languages  retrieved from the list of XLM-R in the Appendix A of https://aclanthology.org/2020.acl-main.747.pdf.

- **seed**: The random seed to be set in case there's any sampling within the `format_input` method.  Note that in this task the `seed` has no effect since there are no sampling params.





### Input & Output Columns

``` mermaid
graph TD
	subgraph Dataset
		subgraph Columns
			ICOL0[task]
		end
		subgraph New columns
			OCOL0[input]
			OCOL1[positive_document]
			OCOL2[model_name]
		end
	end

	subgraph GenerateShortTextMatchingData
		StepInput[Input Columns: task]
		StepOutput[Output Columns: input, positive_document, model_name]
	end

	ICOL0 --> StepInput
	StepOutput --> OCOL0
	StepOutput --> OCOL1
	StepOutput --> OCOL2
	StepInput --> StepOutput

```


#### Inputs


- **task** (`str`): The task description to be used in the generation.




#### Outputs


- **input** (`str`): the input generated by the `LLM`.

- **positive_document** (`str`): the positive document generated by the `LLM`.

- **model_name** (`str`): the name of the model used to generate the short text matching  data.





### Examples


#### Generate synthetic short text matching data for training embedding models
```python
from distilabel.pipeline import Pipeline
from distilabel.steps.tasks import EmbeddingTaskGenerator, GenerateShortTextMatchingData

with Pipeline("my-pipeline") as pipeline:
    task = EmbeddingTaskGenerator(
        category="text-matching-short",
        flatten_tasks=True,
        llm=...,  # LLM instance
    )

    generate = GenerateShortTextMatchingData(
        language="English",
        llm=...,  # LLM instance
    )

    task >> generate
```




### References

- [Improving Text Embeddings with Large Language Models](https://arxiv.org/abs/2401.00368)

