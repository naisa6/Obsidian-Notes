Organizations are rushing to integrate Large Language Models (LLMs) in order to improve their online customer experience. This exposes them to web LLM attacks that take advantage of the model's access to data, APIs, or user information that an attacker cannot access directly. For example, an attack may:

- Retrieve data that the LLM has access to. Common sources of such data include the LLM's prompt, training set, and APIs provided to the model.
- Trigger harmful actions via APIs. For example, the attacker could use an LLM to perform a SQL injection attack on an API it has access to.
- Trigger attacks on other users and systems that query the LLM.

At a high level, attacking an LLM integration is often similar to exploiting a server-side request forgery (SSRF) vulnerability. In both cases, an attacker is abusing a server-side system to launch attacks on a separate component that is not directly accessible.

## What is a large language model?

Large Language Models (LLMs) are AI algorithms that can process user inputs and create plausible responses by predicting sequences of words. They are trained on huge semi-public data sets, using machine learning to analyze how the component parts of language fit together.

LLMs usually present a chat interface to accept user input, known as a prompt. The input allowed is controlled in part by input validation rules.

LLMs can have a wide range of use cases in modern websites:

- Customer service, such as a virtual assistant.
- Translation.
- SEO improvement.
- Analysis of user-generated content, for example to track the tone of on-page comments.

## LLM attacks and prompt injection

Many web LLM attacks rely on a technique known as prompt injection. This is where an attacker uses crafted prompts to manipulate an LLM's output. Prompt injection can result in the AI taking actions that fall outside of its intended purpose, such as making incorrect calls to sensitive APIs or returning content that does not correspond to its guidelines.