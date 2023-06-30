# Musings on Generating Code with ChatGPT
### and a desired alternative
#### (all words by the editor)
## Challenges with ChatGPT
* the ChatGPT interface, with a long list of user queries and assistant responses _suggests that_, and makes it _appear like_ Chat has perfect recall even across a lengthy conversation, but it does not:
* while user inputs are kept verbatim there is a sliding window and you can easily fill the user-query cache, leading to eviction on a fifo basis
* its memory of its own answers is exceptionally short and may not even be 100% for even the last exchange
  * when asked about prior answers it gives something similar, but not the same; rather than drawing from a message history it appears the answer is regenerated from the corresponding user input
   * as a result if in one exchange you ask it to correct some code and then in a later exchange you ask for some different change, it is quite likely to have forgotten about the first change: it causes no end of regression bugs

## Workarounds
One way to combat this is to remember that when 'conversing' with Chat you are actually simply sending an abbreviated history of your prior conversation, mostly consisting of _your_ queries, and very little of the assistant's responses, and then getting an answer simply based on that historical context and your latest query. The "conversation" with the underlying model is like talking to a person with zero short term memory where every question has to repeat the context of said question -- it can get mighty tedious and frustrating.  Chat is merely an abstraction, an opinionated simplification of that tediousness. 

You can work around the issues with Chat's design by manipulating the context; flood the user query buffer but with a context useful to the assistant, such as
* the current state of your code (there is no reason NOT to submit as much of this as useful for each query)
* a summary of your asks to date, rather than the full verbatim history which may not be necessary
  * for this you can simply ask GPT to do generate the summary for you, then use it in the next query, along with any other context and whatever it is you wish to ask

## Desired Alternative 
It would be helpful to have an alternate Chat interface for iterative, collaborative work such as code generation/fixing. 
* While the UX has the complete conversation history (and code state in the form of event-like deltas) it would be useful for the code state to be managed separately, with a living snapshot of its state.  
* Additionally, one might want to be able to optionally submit just the code _signature_, or even a summary of the signature, with or without explanatory verbiage. 
  * By making use of a code inspection function and GPT function invocations one might submit just the signature and instruct GPT to request the complete code for whichever function it would want to explore further...
* Whenever a change is suggested by the Assistant it would be helpful to see the diff with the current code, and edit/accept the change proposal
  * this functionality would happen in the UX, and the LLM itself would not be involved
* For code like Python/JS/TS a REPL environment could be useful for testing the produced code in a sandbox (such as Webassembly)
* A "background" request could demand that the LLM generate a set of tests to accompany the code, and for Python/JS/TS which could then be executed in the sandbox as part of a function invocation, with the LLM correcting any errors and trying again, agent style

---
Commentary by GPT-4, along with further brainstorming: https://chat.openai.com/share/1171acc9-67dc-4445-bb89-da6b0c9063f8
---
Back to [Chapter 4](Chapter4.md)
