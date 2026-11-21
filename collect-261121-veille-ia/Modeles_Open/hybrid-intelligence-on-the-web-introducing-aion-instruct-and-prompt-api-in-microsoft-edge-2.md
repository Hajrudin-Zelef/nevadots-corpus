---
id: collect-261121-veille-ia/modeles-open/hybrid-intelligence-on-the-web-introducing-aion-instruct-and-prompt-api-in-microsoft-edge-2
title: "Hybrid intelligence on the Web: Introducing Aion Instruct and Prompt API in Microsoft Edge"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["prompt", "edge", "aion", "on-device", "microsoft", "model", "available", "instruct"]
source: https://thewindowsforum.com/threads/hybrid-intelligence-on-the-web-introducing-aion-instruct-and-prompt-api-in-microsoft-edge.114954/
source_anchor: ""
source_lines: [1, 155]
section: "Modeles_Open"
---
Rapidly evolving AI capabilities are redefining what sites can do for users on the web. With on-device AI in Microsoft Edge, sites can use task-specific and small language models for capabilities such as writing assistance, summarization, language detection, translation, and speech recognition. Through the Prompt API, they can also use a small language model for more general-purpose intelligence. Together, these capabilities provide a foundation for hybrid intelligence on the web: developers can leverage on-device AI models when they are available in the browser and well-suited, and switch to a configured cloud provider to extend device coverage or access more powerful inference when needed. To make on-device inference more broadly available, we're introducing three updates today:


Aion Instruct is available today as an open-weights model on Hugging Face and is coming to Windows later this year, laying the foundation for richer on-device AI experiences across Windows and Copilot+ PCs. In Microsoft Edge, Aion now replaces Phi-4-mini as the browser's built-in small language model. At 2.3B parameters, Aion is smaller, faster, and available on many more devices than Phi, while delivering comparable quality for common web tasks such as text synthesis and generation, summarization, instruction following, and structured outputs. Edge downloads Aion Instruct only when a site or feature first needs it, and only on devices that can run it well. Users stay in control and can turn off on-device AI in Edge settings, which removes the model and stops future downloads.

Until now, the Prompt API has been available as a developer preview only in Edge Canary and Dev channels. Now, with the Prompt API origin trial, you can call the API from your live site in Microsoft Edge and test it with a much broader audience. The origin trial lets you bring on-device AI features to your site, evaluate them under real-world conditions, and refine your implementation based on actual usage. Along the way, please share feedback on the API's design, the model's performance, and any interoperability challenges you encounter across different browsers and models. This will help inform discussions with other browser vendors and the web community as the Prompt API is considered for standardization. To get started, register for the Prompt API Origin Trial, read the developer documentation, try out the playground sample, and engage with us on the GitHub discussion about the API.

We're also exploring new uses for on-device AI with an early implementation of the experimental Decisions API, powered by Aion Instruct. It is available as a developer preview in Edge Canary and Dev channels for prototyping. Expect both the API and its implementation to change as we learn from developer feedback. The Decisions API enables sites to ask yes/no, multiple-choice, or scoring questions about text input and get back probabilities and a confidence value, computed in a single pass without any text generation. For instance, you can use the Decisions API to understand a user's intent and act on it, sort and route user-generated content, or score and rank items in real-time, all with a few lines of client-side JavaScript code:

// Ask three questions about a product review in a single pass

const model = await DecisionModel.create({

"context": "Customer reviews for an online electronics store",

"questions": [

{

"id": "topic",

"type": "choice",

"prompt": "What is the review mainly about?",

"options": [

{ "label": "quality", "description": "How the product works or feels" },

{ "label": "shipping", "description": "Delivery, packaging, or delays" },

{ "label": "price", "description": "Cost or value for money" }

]

},

{

"id": "rating",

"type": "score",

"prompt": "How satisfied is the customer, from 1 to 5?"

},

{

"id": "needs_reply",

"type": "boolean",

"prompt": "Is the customer asking for help?"

}

]

});


// Get a label, probabilities, and a confidence value for each question

const result = await model.decide(

"Love the headphones, but the box arrived crushed. Can I get a replacement?"

);

	
		
	
 To experiment with the Decisions API, read the explainer and developer documentation, try the playground sample, and share your thoughts on GitHub.

These updates are another step toward bringing hybrid intelligence to the web, giving developers more options to use on-device AI when it's well-suited and combine it with cloud inference when needed. With Aion Instruct now smaller, faster, and more broadly available, we're excited to see the new web experiences you'll build. We're actively looking for feedback on model performance and interoperability for the Prompt and Summarizer APIs. If you try the experimental Decisions API, let us know what you're building, if the API design works for you, and whether your latency requirements warrant a smaller model. We look forward to you testing the Prompt API using the Origin Trial, checking out the developer documentation and playground samples, and sharing your feedback with us on GitHub.


Continue reading...

				
			
- **General availability of Aion Instruct** as an open-weights model on Hugging Face, and as Microsoft Edge's built-in small language model.
- **An origin trial for the Prompt API** , so you can use the Prompt API and Aion Instruct on your live site today.
- **A developer preview of an experimental Decisions API** , which gets you fast, structured answers about text.

Aion Instruct is available today as an open-weights model on Hugging Face and is coming to Windows later this year, laying the foundation for richer on-device AI experiences across Windows and Copilot+ PCs. In Microsoft Edge, Aion now replaces Phi-4-mini as the browser's built-in small language model. At 2.3B parameters, Aion is smaller, faster, and available on many more devices than Phi, while delivering comparable quality for common web tasks such as text synthesis and generation, summarization, instruction following, and structured outputs. Edge downloads Aion Instruct only when a site or feature first needs it, and only on devices that can run it well. Users stay in control and can turn off on-device AI in Edge settings, which removes the model and stops future downloads.

Until now, the Prompt API has been available as a developer preview only in Edge Canary and Dev channels. Now, with the Prompt API origin trial, you can call the API from your live site in Microsoft Edge and test it with a much broader audience. The origin trial lets you bring on-device AI features to your site, evaluate them under real-world conditions, and refine your implementation based on actual usage. Along the way, please share feedback on the API's design, the model's performance, and any interoperability challenges you encounter across different browsers and models. This will help inform discussions with other browser vendors and the web community as the Prompt API is considered for standardization. To get started, register for the Prompt API Origin Trial, read the developer documentation, try out the playground sample, and engage with us on the GitHub discussion about the API.

We're also exploring new uses for on-device AI with an early implementation of the experimental Decisions API, powered by Aion Instruct. It is available as a developer preview in Edge Canary and Dev channels for prototyping. Expect both the API and its implementation to change as we learn from developer feedback. The Decisions API enables sites to ask yes/no, multiple-choice, or scoring questions about text input and get back probabilities and a confidence value, computed in a single pass without any text generation. For instance, you can use the Decisions API to understand a user's intent and act on it, sort and route user-generated content, or score and rank items in real-time, all with a few lines of client-side JavaScript code:

// Ask three questions about a product review in a single pass

const model = await DecisionModel.create({

"context": "Customer reviews for an online electronics store",

"questions": [

{

"id": "topic",

"type": "choice",

"prompt": "What is the review mainly about?",

"options": [

{ "label": "quality", "description": "How the product works or feels" },

{ "label": "shipping", "description": "Delivery, packaging, or delays" },

{ "label": "price", "description": "Cost or value for money" }

]

},

{

"id": "rating",

"type": "score",

"prompt": "How satisfied is the customer, from 1 to 5?"

},

{

"id": "needs_reply",

"type": "boolean",

"prompt": "Is the customer asking for help?"

}

]

});

// Get a label, probabilities, and a confidence value for each question

const result = await model.decide(

"Love the headphones, but the box arrived crushed. Can I get a replacement?"

);

These updates are another step toward bringing hybrid intelligence to the web, giving developers more options to use on-device AI when it's well-suited and combine it with cloud inference when needed. With Aion Instruct now smaller, faster, and more broadly available, we're excited to see the new web experiences you'll build. We're actively looking for feedback on model performance and interoperability for the Prompt and Summarizer APIs. If you try the experimental Decisions API, let us know what you're building, if the API design works for you, and whether your latency requirements warrant a smaller model. We look forward to you testing the Prompt API using the Origin Trial, checking out the developer documentation and playground samples, and sharing your feedback with us on GitHub.

Continue reading...
