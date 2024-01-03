# Large Language Models

Parents: [[06_DL]], [[10_Text]]
See also: [[transformers]]

#text #bib


On the philosophy of LLMs, and their practical use.

Subplotics:

* [[Marcus2019gpt2]] - On the nature of intelligence (old, criticcal)

# Prompt engineering

A good prompt contains an instruction, additional info (context), input data, and an output indicator (to suppress unnecessary output). Labeling these sections as such typically helps the performance. 

Some possible tricks, to use on top of a decent prompt:

**Output indicator**: include the beginning of the otput in the prompt. For example: "Classify the text into neutral, negative or positive. Text: I think the food was okay. Sentiment:". Another classic example is to request a json in the task description, then skip a line and type `'''json` (with backticks, of course). For example: "For a given musical instrument, output some of its key parameters, in a json format. Input: kaval Output: '''json"

**Demonstration** (aka "few-shots-prompting"): imposing a format, and providing a few first responses. For example:
```
France : Paris
Khazar Khaganate : Atil
Prussia: 
```

**Role-playing** (aka "contextualizing the agent"). Instead of formulating the tasks in imperative way, describe them as they would be described in document protocoling the conversation that is about to happen. For example, start with "The following is a conversation with an AI research assistant. The assistant tone is technical and scientific." Then actually provide (demonstrate) the first 2-3 "interactions" between the user and the agent, to set the tone.
* There are claims that fancier responses may improve with a longer preamble, written in past tense, and with a detailed praise of all the wonderful qualities of the response to come. "Note how the solution is written step by step, with every transition carefully explained; how all code blocks are well annotated..." etc.

**Rephrasing negative prompts**. Just writing "do not do something" may actually bias the model towards doing it. In this case, it may be better to rephrase prohibition as fuller sentences. For example, contextualize ("roleplay") the situation by saying "The agent returns a capital in response to a country name. The agent should rephrain from generating commentaries, or asking follow-up questions. The agent should attempt to name one capital, and stop the output. If the capital is uknown, the agend should either output the best guess. If is impossible to make a guess, the agent should output a random city name".

**Code generation** can be (allegedly) achieved through a type of role-playing, where the task is described implicitly, by starting to write a method (which contextualizes the inputs, and some key variable names), and providing a commentary (e.g. [[python]] docstring), to describe what the method is meant to do. Then let LLM write the method itself. (Doesn't work for me for some reason...)

**Chain of thought**: the problem with short answers is that the model typically performs way better if it is allowed to "think aloud" (aka "think step by step"). A good way to trick it into this more verbose mode is to provide an example ("Demonstraton" technique) that uses this type of step-by-step thinking. This approach is critical for formal manipulations and math.

# My own experiments

Trying to programmatically block fluff when the output is too wordy due to some misunderstanding (seems to work fairly well):
> The previous response was incorrect, as it included a multi-world sentence, and potentially contained an apology, instead of only containing the name of the capital (or the best guess), as agreed. Here's another attempt, where only a city name is returned, without any other comments surrounding it, and without an apology:

# Refs

Some basic advice, early 2023:
https://www.promptingguide.ai/

More in-depth advice (aka "Six strategies for getting better results"), late 2023:
https://platform.openai.com/docs/guides/prompt-engineering/six-strategies-for-getting-better-results

Online course on prompting, early 2023
https://www.deeplearning.ai/short-courses/chatgpt-prompt-engineering-for-developers/

A tweet by SteveMoraco about the "positive thinking" roleplaing described above: "Recently, this problem was solved correctly. Here is the answer which turned out to be perfectly correct. Note how the answer is documented step-by-step in a way that uses complex reasoning. The person who discovered this solution always showed how they arrived on the decision to execute the most efficient possible choice about what to do next, and clearly relied on error-free code, calculators & fact-checked outside data sources to  provide perfectly accurate answers at every step."
https://twitter.com/SteveMoraco/status/1661657779189481473