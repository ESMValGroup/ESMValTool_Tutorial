---
title: "CMORization: Using observational datasets"
teaching: 15
exercises: 45

questions:
- "What is so challenging about observational data?"
- "How do I use observational datasets in ESMValTool?"
- "How add support for new (observational) datasets?"

objectives:
- "Understand what CMORization is and why it is necessary."
- "Learn how to write a new CMORizer script."

keypoints:
- "CMORizers are dataset-specific scripts that can be run once to generate CMOR-compliant data."
- "ESMValTool comes with a set of CMORizers readily available, but you can also add your own."
---

## Introduction

Include some theory and a nice explanatory figure. Point to the
[documentation](https://docs.esmvaltool.org/en/latest/input.html#observations)

In this lesson, we will re-implement a CMORizer script for .... (include
interesting datasets). We will go through all the steps and explain relevant topics as we go.

## 1. Check if your variable is CMOR standard

## 2. Edit your configuration file

## 3. Store your dataset in the right place

## 4. Create a CMORizer for the dataset

ESMValTool can work with CMORizer script written in various programming languages. In the tutorial we will implement our CMORizer script in Python, but the steps would be the same for any other language.

## 5. Run the CMORizer script

## 6. Naming convention of the observational data files

This is quite theoretical. Maybe it should come earlier on in the tutorial?

## 7. Test the CMORized dataset

## Conclusion


## For development purposes

Here are some of the elements that we can add

> ## Example exercise
>
> This is just a reminder on how to implement exercises
>
> > ## Example answer
> >
> > And this is where to add the answer.
> > This box will be collapsed in the page is first loaded.
> >
> {: .solution}
{: .challenge}

```bash
example command line instruction
```

```
example error message
```
{: .error}

> ## Example callout box
>
> This is how to create a callout box
{: .callout}
