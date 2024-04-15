---
tags: engineering/ai, llm, accuracy, self-refinement, prompt-engineering, chatgpt
author: Pham Duc Thanh
github_id: zlatanpham
date: 2023-06-29
icy: 10
---

Self-refinement is a technique where the model evaluates and refines its own output. Normally, when using an LLM, you provide a prompt and the model generates a completion. With self-refinement, you can instruct the model to review the content it has generated, score it, and refine the output. This process can be repeated multiple times, allowing the model to iteratively improve its own output.

For instance, if the model is asked to write a tweet, it can then be prompted to make the tweet more engaging, rate its quality, and refine it accordingly.

![](assets/llm's-accuracy---self-refinement_llm-self-refinement-step-1.png)

![](assets/llm's-accuracy---self-refinement_llm-self-refinement-step-2.png)

![](assets/llm's-accuracy---self-refinement_llm-self-refinement-step-3.png)

Notably, this technique does not require supervised data or [[Reinforcement Learning | reinforcement learning]]. The model's ability to self-evaluate and refine its output is inherent, making this a powerful and efficient method for improving LLM's accuracy.

Key Points:

- Self-refinement involves the model reviewing, scoring, and refining its own output.
- The technique has been effective, especially for models like GPT-4.
- It outperforms baselines in many use cases without the need for supervised data or reinforcement learning.

---
<!-- cta -->

### Contributing
At Dwarves, we encourage our people to read, write, share what we learn with others, and [[CONTRIBUTING|contributing to the Brainery]] is an important part of our learning culture. For visitors, you are welcome to read them, contribute to them, and suggest additions. We maintain a monthly pool of $1500 to reward contributors who support our journey of lifelong growth in knowledge and network.

### Love what we are doing?
- Check out our [products](https://superbits.co)
- Hire us to [build your software](https://d.foundation)
- Join us, [we are also hiring](https://github.com/dwarvesf/WeAreHiring)
- Visit our [Discord Learning Site](https://discord.gg/dzNBpNTVEZ)
- Visit our [GitHub](https://github.com/dwarvesf)