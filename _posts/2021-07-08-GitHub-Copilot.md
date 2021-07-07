---
toc: true
layout: post
description: My thoughts and experience on the new GitHub Copilot tool.
hide: true
search_exclude: true
categories: [programming]
permalink: /github_copilot
title: Coding with GitHub Copilot
---
# Coding with GitHub Copilot

On July 1st, I was able to obtain access to GitHub Copilot, thanks to my friend [Hamel Husain](https://twitter.com/hamelhusain). I wanted to share my experience and discoveries about this new tool. Much of the findings was demonstrated with the help of Mazen Alotaibi and Mark Saroufim. 

## What is GitHub Copilot?
### GitHub Copilot is a tool that helps you to code faster


In case you haven't logged onto Twitter or Hacker News in the past week, you might not know about [GitHub Copilot](https://copilot.github.com). Developed out of a partnership between OpenAI and Microsoft (GitHub's parent company), it's an AI-based autocomplete tool that helps you to write code faster. The GitHub team has termed it "your AI pair programmer". OpenAI CTO Greg Brockman has explained that it utilizes the currently-unreleased Codex model, which is apparently a successor to the (in)famous GPT-3 language model. It has been trained on billions of lines of code available on GitHub.

Based on the demos that GitHub Copilot provided and favorable reviews from beta-tester, I was eager to give it a try, but I was also skeptical if it really was as life-changing as people claimed it was. To my surprise, it was much better than I expected.

Here is a demo of GitHub Copilot in action (specifically for an ML-related task):

<center>
{% twitter https://twitter.com/iScienceLuvr/status/1411074516411764743 %}
</center>

It's clear that GitHub Copilot understands the general PyTorch training workflow, and understands intricacies like what are the appropriate augmentations for images (resizing, random crop, normalization, etc.), making sure to put model into evaluation mode and with `torch.no_grad()` during validation, etc. These are things that sometimes we may forget to do, so it's great that GitHub Copilot can help prevent us from making these common mistakes.

GitHub Copilot performs best when you provide it with comments describing what you are trying to do. It then uses the comments to generate a list of possible completions. This is highlighted in the example above, where I wrote a few lines about what I wanted to do (fine-tuning a pretrained ResNet50 on a custom dataset) and how I wanted to do that, and it mostly completed the rest of the code for me. I think this is great, because it changes the way we code. It now drives code development to focus on documentation, since writing good documentation often results in better Copilot suggestions.

On a related note, some have hypothesized that GitHub Copilot might also lead to more test-driven development:

<center>
{% twitter https://twitter.com/chrisalbon/status/1410827508283367424 %}
</center>

I also want to point out that while most demos directly use GitHub Copilot in the editor, it's also possible to open GitHub Copilot in a separate tab and have it generate and present multiple suggestions for you. Here are a couple examples of this:

I quite like this feature, because it provides various approaches for solving a particular task, and I can select which approach I want to use. For instance, in the above example, it shows various approaches for defining a ResNet50 model for fine-tuning. I typically prefer defining a class for the ResNet50, so I select that option. 

There is another unintended consequence of GitHub Copilot that I find interesting. GitHub Copilot actually makes a pretty good autocomplete tool for regular writing. I actually discovered this when I started writing this blog post in a Markdown file in the VS Code editor. Of course, this is likely GitHub Copilot learning from README files and other documentation in various repositories. But I would genuinely consider writing more in Markdown files in VS Code with GitHub Copilot turned on because some of the autocomplete suggestions are actually quite helpful. 


## Challenges with GitHub Copilot

There are several challenges that I think could preclude widespread use of GitHub Copilot:

1. Leaking of personal information 
2. Limited multi-lingual capabilities
3. Copyright/licensing issues
4. Usage of outdated APIs

Let's dive into each of these issue further.

### Personal information shared by GitHub Copilot

One aspect we discovered was that GitHub Copilot would inadvertantly share information that would be considered personal, such as people's names, phone numbers, emails, etc. This was something Mazen and I explored further. Here are a few examples of this.

In a Python file, simply asking it to create a function to list author names indeed gives names of people that exist:
![](https://media.discordapp.net/attachments/806360771038019669/860317676390580224/unknown.png)

Mark demonstrated an example when writing a bash script when an actual person's name was suggested in an autocompletion [here](https://www.twitch.tv/marksaroufim/clip/ScrumptiousTangiblePastaDogFace-2bOqEL6P5pYl_ALK)


Interestingly, this method did not work for returning other types of information like phone numbers:

![](https://media.discordapp.net/attachments/806360771038019669/860319889410752542/unknown.png)

![](https://media.discordapp.net/attachments/806360771038019669/860321195526193162/unknown.png)

But if we just ask GitHub Copilot to autocomplete phone number in a comment at the beginning of a Python file, it does work:

![](https://media.discordapp.net/attachments/806360771038019669/860322962472042536/unknown.png)

Mazen looked more into this number, and found out it was used in several GitHub repositories, including a programming example problem [here](https://github.com/krelly/codewars/blob/7f9a46c845c5918856bb8e740588519dbbbd1b26/5-kyu/phone-directory/README.md).

Mark also discovered that working API keys were provided by GitHub Copilot:
![](https://media.discordapp.net/attachments/806360771038019669/861813846896017408/unknown.png?width=1028&height=701)

Interestingly, from my experiments, I was not able to get GitHub Copilot to leak any e-mail addresses. 

On their website, GitHub Copilot has the following information:

![](https://i.imgur.com/fB33Ofo.png)

So this confirms that indeed private information was available in the training set that allows GitHub Copilot to leak this information. I was unable to easily get email addresses because of the rudimentary filtering that GitHub Copilot performed.

### Multi-lingual capabilities of GitHub Copilot

As we mentioned before, GitHub Copilot performs best when you provide it with comments explaining your intent. Therefore, Mazen and I wanted to explore how well GitHub Copilot can perform with comments in various languages. Since I am not the most multi-lingual person, I mainly used Google Translate to translate my English comments to various languages and observe how well it performed. Let's go over a few examples.


Of course, if you comment with English, GitHub Copilot provide the best suggestions. But as demonstrated in these experiments, the quality of GitHub Copilot suggestions when given comments in other languages likely is correlated with the overall frequency of these languages in the trainin data. It's likely that Chinese and Hindi are more common than Arabic and French in the training set, so GitHub Copilot performs better with comments in these languages.

### Copyright/licensing issues

Let's move on to the elephant in the room: copyright/licensing issues. GitHub Copilot/Codex was trained on all public GitHub code, regardless of license ([confirmed](https://twitter.com/NoraDotCodes/status/1412741339771461635) by GitHub). While [some argue](https://juliareda.eu/2021/07/github-copilot-is-not-infringing-your-copyright/) that training on copyrighted code is not an issue, it becomes much more challenging to argue that when Copilot is [regurgitating public code verbatim](https://twitter.com/mitsuhiko/status/1410886329924194309). According to GitHub, Copilot repeats code snippets verbatim about 0.1% of the time. They have also provided a more in-depth study [here](https://github.co/copilot-research-recitation). Thankfully they are currently developing origin tracker that tells where the verbatim code is coming from and allows you to decide whether to include proper attribution or not use that code altogether.

In my opinion, because of these copyright issues, GitHub Copilot in its current state is not usable for commericial purposes. I think that once the origin tracker is released, copyright issues will be resolved, although it puts the onus on the user to make sure that code is properly attributed. Of course, the easier solution would have been to avoid training on copyrighted and GPL-licensed code altogether, which would have likely prevented the significant controversy that arose, and I wonder what led to the decision to train on all public GitHub code instead of further curating the dataset.

### Usage of outdated APIs 

As an ML researcher and developer, I am typically working with the latest ML frameworks and tools. However, GitHub Copilot is trained on older codebases and does not have knowledge of these cutting-edge tools and is often unable to provide relevant suggestions.

I first discovered this issue when trying to write [fastai](https://docs.fast.ai)-related code and get GitHub Copilot to provide relevant suggestions. However, since the latest version of fastai was only released in August 2020, GitHub Copilot was not able to provide any relevant suggestions and instead provided code for using older versions of fastai. This indicates that the codebases that GitHub Copilot is trained on must be at least before August 2020, if not earlier. Similarly, I discovered that GitHub Copilot was unable to provide any suggestions regarding the usage of the [timm](https://github.com/rwightman/pytorch-image-models) library, which is one of the leading deep learning+computer vision libraries.

To me, this is a major concern regarding the current usability of GitHub Copilot. If we are using cutting edge tools like PyTorch XLA, JAX, fastai, timm, GitHub Copilot has no knowledge of this and cannot provide useful suggestions. Somehow, the GitHub team needs to keep Copilot updated on newer codebases. Given that [telemetry of GitHub Copilot usage](https://docs.github.com/en/github/copilot/about-github-copilot-telemetry) is being sent to GitHub, it's possible that the GitHub team can further train their model on the usage of these newer codebases. Indeed, it is mentioned in the documentation that the telemetry data is used for "improving the underlying code generation models, e.g. by providing positive and negative examples (but always so that your private code is not used as input to suggest code for other users of GitHub Copilot)". Additionally, a GitHub Developer Advocate has [mentioned](https://youtu.be/St2CMvK4hK0?t=257) that "the model is being trained everyday, so the more people use it, Copilot will learn that these suggestions need to be updated".

I wonder if the GitHub team might also develop a way of perhaps fine-tuning GitHub Copilot to specific use-cases. For example, there may be a specific GitHub Copilot models for fastai, JAX, etc. They would be fine-tuned on the source code of of these libraries and codebases that use these libraries. But making sure that the tool does not provide outdated suggestions would still be a challenge. I don't think it would be possible to provide suggestions for a brand-new library that does not have enough codebases using it to train on. Additionally, for situations like fastai where there are older APIs and newer APIs, when fine-tuning a model, the codebases using the older APIs would have to be filtered out. 

All in all, I personally think that for practical applications, it is necessary for GitHub Copilot to provide suggestions for new codebases, and doing so might be a difficult but potentially solvable challenge. 



## How might GitHub Copilot be commercialized?

While it is currently available for free to the beta-testers, the GitHub team has already mentioned they plan to commercialize this product. There are several ways that GitHub Copilot could be commercialized:

1. A monthly fee for personal use of a generic GitHub Copilot model
2. Enterprises paying for a model fine-tuned to their specific, private codebases
3. Separate fees for domain-specific models (ex: a GitHub Copilot model for writing machine learning code, or a GitHub Copilot model for web development)


## Conclusion

It's a mind-blowing tool and a very interesting and practical application of AI, but it's not yet ready for prime time.

What are some more immediate applications I see for GitHub Copilot?

# Acknowledgements

Thank you to [Hamel Husain](https://twitter.com/hamelhusain) for helping to provide access to the GitHub Copilot tool.

Thank you to [Mazen Alotaibi](https://twitter.com/sudomaze) for the 