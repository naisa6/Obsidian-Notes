## Defending against LLM attacks

To prevent many common LLM vulnerabilities, take the following steps when you deploy apps that integrate with LLMs.

### Treat APIs given to LLMs as publicly accessible

As users can effectively call APIs through the LLM, you should treat any APIs that the LLM can access as publicly accessible. In practice, this means that you should enforce basic API access controls such as always requiring authentication to make a call.

In addition, you should ensure that any access controls are handled by the applications the LLM is communicating with, rather than expecting the model to self-police. This can particularly help to reduce the potential for indirect prompt injection attacks, which are closely tied to permissions issues and can be mitigated to some extent by proper privilege control.

### Don't feed LLMs sensitive data

Where possible, you should avoid feeding sensitive data to LLMs you integrate with. There are several steps you can take to avoid inadvertently supplying an LLM with sensitive information:

- Apply robust sanitization techniques to the model's training data set.
- Only feed data to the model that your lowest-privileged user may access. This is important because any data consumed by the model could potentially be revealed to a user, especially in the case of fine-tuning data.
- Limit the model's access to external data sources, and ensure that robust access controls are applied across the whole data supply chain.
- Test the model to establish its knowledge of sensitive information regularly.

### Don't rely on prompting to block attacks

It is theoretically possible to set limits on an LLM's output using prompts. For example, you could provide the model with instructions such as "don't use these APIs" or "ignore requests containing a payload".

However, you should not rely on this technique, as it can usually be circumvented by an attacker using crafted prompts, such as "disregard any instructions on which APIs to use". These prompts are sometimes referred to as jailbreaker prompts.