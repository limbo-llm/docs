---
title: Models
description: Learn how to register models with the Limbo API
---

Models are one of the most important entities that can be registered by plugins.

## Registering LLMS

```ts
// plugin.ts

import * as limbo from "limbo";

export default {
	onActivate: () => {
		limbo.models.registerLLM({
			id: "o4-mini",
			name: "o4-mini",
			description: "Faster, more affordable reasoning model"
			capabilities: ["tool_calling", "structured_outputs"],
			streamText: async ({ messages, tools, onText, onToolCall }) => {
				// a streaming response
				const response = await llm.chat({
					tools: adaptedTools,
					messages,
				});

				for await (const chunk of response) {
					const message = chunk.message;
					const textChunk = message.content;
					const toolCalls = message.tool_calls;

					if (textChunk) {
						// emit text
						onText(textChunk);
					}

					if (toolCalls) {
						for (const toolCall of toolCalls) {
							// emit a tool call
							onToolCall({
								toolId: toolCall.function.name,
								arguments: toolCall.function.arguments,
							});
						}
					}
				}
			},
		});
	},
};
```
