<!-- TOC -->
* [ChatGPT](#chatgpt)
  * [Text Generation](#text-generation)
    * [Chat Completions API](#chat-completions-api)
      * [Chat Completions response format](#chat-completions-response-format)
        * [The difference between this and Completion APIs](#the-difference-between-this-and-completion-apis)
      * [JSON mode New](#json-mode-new)
      * [Reproducible outputs](#reproducible-outputs)
      * [Managing tokens](#managing-tokens)
    * [Completions API](#completions-api)
      * [Completions format](#completions-format)
      * [Inserting text](#inserting-text)
      * [The difference between Chat Completion and Completion APIs](#the-difference-between-chat-completion-and-completion-apis)
      * [Chat Completions vs. Completions](#chat-completions-vs-completions)
    * [Which model should I use?](#which-model-should-i-use)
  * [Prompt engineering](#prompt-engineering)
* [FAQ](#faq)
      * [How should I set the temperature parameter?](#how-should-i-set-the-temperature-parameter)
  * [Function calling](#function-calling)
<!-- TOC -->

# ChatGPT

## Text Generation

### Chat Completions API

Typically, a _conversation_ is **formatted** with a **_system_** message **first**, **followed** by
**alternating** _user_ and _assistant_ messages.

The system message helps **set** the **behavior** of the _assistant_.
For example, you can modify the personality of the assistant or
provide specific instructions about how it should behave throughout the conversation.
However note that the _system_ message is **optional**
and the _model_’s behavior without a system message is likely to be
similar to using a **generic message** such as `You are a helpful assistant.`.

The _user_ messages **provide** **requests** or **comments** for the _assistant_ to respond to.
_Assistant_ messages **store previous** assistant **responses**,
but can also be **written by you** to give **examples** of **desired behavior**.

**Including** conversation **history** is important
when user instructions **refer to prior messages**.

#### Chat Completions response format

```json
{
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "message": {
        "content": "The 2020 World Series was played in Texas at Globe Life Field in Arlington.",
        "role": "assistant"
      }
    }
  ],
  "created": 1677664795,
  "id": "chatcmpl-7QyqpwdfhqwajicIEznoc6Q47XAyW",
  "model": "gpt-3.5-turbo-0613",
  "object": "chat.completion",
  "usage": {
    "completion_tokens": 17,
    "prompt_tokens": 57,
    "total_tokens": 74
  }
}
```

Every response will include a `finish_reason`. The possible values for finish_reason are:

`stop`: API returned complete message, or a message terminated by one of the stop sequences provided
via the stop parameter
`length`: Incomplete model output due to max_tokens parameter or token limit
`function_call`: The model decided to call a function
`content_filter`: Omitted content due to a flag from our content filters
`null`: API response still in progress or incomplete

##### The difference between this and Completion APIs

+ [The difference between Chat Completion and Completion APIs](#The-difference-between-Chat-Completion-and-Completion-APIs)

#### JSON mode New

A common way to use Chat Completions is
to **instruct** the _model_ to always **return** a _JSON_ object
that makes sense for your use case,
by specifying this in the system message. While this does work in some cases,
occasionally the models may generate output that does not parse to valid _JSON_ objects.

To prevent these errors and improve model performance,
when calling `gpt-4-1106-preview` or `gpt-3.5-turbo-1106`,
you can **set** `response_format` to `{ "type": "json_object" }` to **enable _JSON_ mode**.
When JSON mode is enabled,
the model is constrained to only generate strings that parse into valid JSON object.

> When using _JSON_ mode
> , **always** instruct the model to produce JSON via some message in the conversation
> , for example via your system message.
> If you don't include an explicit instruction to generate _JSON_
> , the model may generate an unending stream of whitespace and
> the request may **run** continually **until** it reaches the **token limit**.

To help ensure you don't forget, the _API_ will **throw an error**
if the string `JSON` does not appear somewhere in the context.

The JSON in the message the model **returns** may be **partial** (i.e. cut off)
if `finish_reason` is `length`. To guard against this, check `finish_reason` before parsing the
response.

Note that _JSON_ mode is **always enabled**
when the model is generating arguments as part of **function calling**.

#### Reproducible outputs

**Set** the **_seed_ parameter** to any _integer_ of your choice
and use the same value across requests you'd like deterministic outputs for.
**Ensure** all other parameters (like `prompt` or `temperature`)
are the exact **same across requests**.

#### Managing tokens

Language models read and write text in chunks called tokens.
In English, a token can be **as short as one character** or **as long as one word**,
and in some languages tokens can be even shorter than one character or even longer than one word.

+ [Counting tokens for chat API calls](https://platform.openai.com/docs/guides/text-generation/managing-tokens)

### Completions API

#### Completions format

**Instead** of the input being a **list** of messages,
the _input_ is a **freeform** text **string** called a `prompt`.

```python
from openai import OpenAI

client = OpenAI()

response = client.completions.create(
    model="gpt-3.5-turbo-instruct",
    prompt="Write a tagline for an ice cream shop."
)
```

#### Inserting text

+ [Inserting text](https://platform.openai.com/docs/guides/text-generation/inserting-text)

#### The difference between Chat Completion and Completion APIs

`/v1/completions` endpoint is for the old models such as _DeVinci_.
It is a very **powerful model** that **gets instruction** and **produces output**.

The `/v1/chat/completions` API is for the newer - _chat_ models.
_gpt-3.5-turbo_ is great because it can do everything _DeVinci_ can,
but its **cheaper** (1/10 the cost).
The down side is that - for it to perform the same as _DeVinci_ it might **require bigger input**
and the input might be **more complex**.

+ [stackoverflow](https://stackoverflow.com/questions/76192496/openai-v1-completions-vs-v1-chat-completions-end-points)

#### Chat Completions vs. Completions

The Chat Completions format can be made similar to the completions format
by constructing a request using a single user message.
For example, one can translate from English to French with the following completions prompt:

`Translate the following English text to French: "{text}"`

And an equivalent chat prompt would be:

`[{"role": "user", "content": 'Translate the following English text to French: "{text}"'}]`

Likewise, the completions API can be used to simulate a chat between a user and an assistant by
formatting the input accordingly.

### Which model should I use?

We generally recommend that you use either `gpt-4` or `gpt-3.5-turbo`.

By contrast `gpt-3.5-turbo` is more likely to **follow** **just one part** of a **complex**
multi-part **instruction**. gpt-4 is less likely than gpt-3.5-turbo to make up information, a
behavior known as "hallucination".

## Prompt engineering

+ [Prompt engineering](https://platform.openai.com/docs/guides/prompt-engineering)
+ [OpenAI Cookbook](https://cookbook.openai.com/)

# FAQ

+ [FAQ](https://platform.openai.com/docs/guides/text-generation/faq)

#### How should I set the temperature parameter?

Lower values for **temperature** result in more consistent outputs (e.g. 0.2), while higher values
generate more diverse and creative results (e.g. 1.0). Select a temperature value based on the
desired trade-off between coherence and creativity for your specific application. The temperature
can range is from 0 to 2.

## Function calling

Function calling allows you to more reliably get structured data back from the model. For example, you can: