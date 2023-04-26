# MuTAP

`MuTAP` is a prompt-based learning technique to generate effective test cases with Large Language Models (LLMs). This is an implementation of a method described in 
'<strong>Effective Test Generation Using Pre-trained Large Language Models and Mutation Testing</strong>'. <br />

![](https://github.com/ExpertiseModel/MuTAP/blob/master/diagram_mutant.png)

`MuTAP` starts by calling an intial prompt on Codex to generate test cases for a Program Under Test (PUT). The initial prompt uses zero-shot or few-shot learning technique. Then, `MuTAP` attempts to repair the syntax and functional errors within the test cases generated by Codex. After generating the test cases, `MuTAP` attempts to identify and repair any syntax or functional errors. Once these issues are fixed, `MuTAP` generates different mutants for the PUT and calculates the Mutation Score (MS) of the test cases. If there are any surviving mutants, `MuTAP` uses them to augment the initial prompt and reprompt Codex to improve its own output by generating new test case that can kill the surviving mutants.<br />


## 1. Initial prompt on LLMC (Codex) and syntax Fixer
At the end of this step, a unit test, including several test cases , will be generated for a `PUT`. The test cases are syntactically fixed.
```
$ python generate_test_oracle.py [prompt type] [dataset] [task_num] [script name] [output prefix name] [csv report filename (.csv)]
```
- `[prompt type]` defines the type of intial prompt. It is `normal` to indicate `zero-shot learning` or `fewshot` to indicate `few-shot learning`.
- `[dataset]` indicate the database. We have two dataset in our experiment. The first one is `HumanEval` and the second one is `Refactory` that is a benchmark for bug repairing.
- `[task_num]` is the identifier or task number within the dataset. It is within `[1,163]` for `HumanEval` dataset, and within `[1,5]` for `Refoctory`.
- `[script name]` is the name of PUT.
- `[output prefix name]' is the prefix to name the output or the test cases.
- `[csv report filename (.csv)]` is a report on different number of attempts on LLMC and fixing its syntax erros. It is up to 10 attemps. 

## 2. Functional Repair
`semantic_err_correction.py` repairs the functional errors in assertions.
```
$ python semantic_err_correction.py [dataset] [task_num] [test name] [output prefix name] [csv report filename (.csv)]
```
- `[dataset]` indicate the database. We have two dataset in our experiment. The first one is `HumanEval` and the second one is `Refactory` that is a benchmark for bug repairing.
- `[task_num]` is the identifier or task number within the dataset. It is within `[1,163]` for `HumanEval` dataset, and within `[1,5]` for `Refoctory`.
- `[test name]` is the name of Initial Unit Test (IUT), generated in step 1.
- `[output prefix name]' is the prefix to name the output or the test cases.
- `[csv report filename (.csv)]` is a report on a summary of functional repair (number of assertions with functional issue, number of fixed, etc.) 

### Run generate mutants:

```
$ python Mutants_generation.py [dataset] [task_num] [input prefix name] [csv report filename (.csv)]
```

### calculte mutation score:

```
$ python Mutation_Score.py [dataset] [task_num] [testfile prefix name] [csv report filename (.csv)]
```

### prompt augmentation:

```
$ python augmented_prompt.py [prompt type] [dataset] [task_num] [input prefix name] [output prefix name] [csv report filename (.csv)]
```

The model that is used to generated embedding vectors for dev2vec:APIs is the pretrained model from the article <a href="https://ieeexplore.ieee.org/abstract/document/9401957?casa_token=G8DjJLSm2sQAAAAA:3h8AEP8d0XLzSgHaVkSal9k7AyQ1pfXt18uuCCeIyiCMEmEKqlkgR1xsaoJj-iJIbGVP-hbeRg"><strong>Representation of Developer Expertise in Open Source Software</strong></a> <br />

-------------------------------------------------------------------------------------------------------------------------------------------------
## Citation
<a href="https://arxiv.org/abs/2207.05132"><strong>Dev2vec: Representing Domain Expertise of Developers in an Embedding Space</strong></a>
```
@article{dakhel2022dev2vec,
  title={Dev2vec: Representing Domain Expertise of Developers in an Embedding Space},
  author={Dakhel, Arghavan Moradi and Desmarais, Michel C and Khomh, Foutse},
  journal={arXiv preprint arXiv:2207.05132},
  year={2022}
}
```

    
