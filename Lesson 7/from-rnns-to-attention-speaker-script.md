# From RNNs to Attention — Speaker Script

## Title slide — From RNNs to Attention

Today we are starting a journey that explains one of the big ideas behind modern language models. We will begin with models that carry a memory forward one word at a time, then see why that becomes difficult, and finally discover attention: a way for a word to look directly at useful earlier context. We are not building the full Transformer today. Our goal is to understand the problem it solves and to build a strong intuition for self-attention.

## Slide 01 — The journey of this lesson

Here is the map for today. First, an RNN carries one running memory through a sequence. Then LSTMs and GRUs improve that memory with gates, which decide what to keep or forget. Finally, attention changes the strategy: instead of hoping one memory carried everything safely, the model can look back and retrieve the information that matters right now.

The important thing is not to memorize architecture names. Keep asking one question: how does the model keep the right context available when it needs it?

## Slide 02 — Why sequences are different

In a sequence, order is part of the meaning. The same words in a different order can describe a completely different event. For example, “dog bites man” and “man bites dog” use the same words, but they do not mean the same thing.

That is why language, speech, music, and time-series data need special treatment. A model cannot treat every input as an unrelated column in a spreadsheet. It needs some way to know what came before and what came after.

## Slide 03 — A clue can be far away

Let’s use this sentence as our running example: “The robot picked up the battery because it was ___.” Before we fill in the blank, notice that the useful clue is not beside the blank. To decide what “it” refers to, we probably need to remember “battery,” which appeared earlier.

Ask the room what belongs in the blank. The point is not just that “low” is a plausible answer. The point is that the model needs to connect a later word to an earlier clue.

## Slide 04 — RNN: carry a memory forward

An RNN processes one step at a time. At each word, it receives the current input and the memory made at the previous step. Inside the RNN cell, those two pieces of information are combined to make a new memory. That new memory is passed forward to the next word.

So the loop is important: the previous output becomes an input to the next step. You can think of the hidden state as a tiny running notebook. It is the model’s attempt to summarize everything useful it has seen so far.

## Slide 05 — Reading one step at a time

This is what the RNN experience looks like over time. It reads a word, rewrites its memory, moves to the next word, rewrites the memory again, and continues. It does not see the whole sentence at once in the way we do.

That can work well for short, local patterns. But notice the pressure on the memory capsule: every new word may add something useful, while older information may be overwritten.

## Slide 06 — Where RNNs help

RNNs were an important step forward. They can be useful when sequences are short and nearby context is the main thing that matters. Examples include short text patterns, small sound segments, and simple time-series signals.

The key strength is that the same cell can be reused at every position. But that strength also comes with a limitation: information still has to travel through the chain one step at a time.

## Slide 07 — One memory has to carry everything

Here is the core bottleneck. A long sequence contains many details, but an ordinary RNN tries to squeeze what matters into one running summary. Imagine trying to summarize an entire movie using one sticky note that you must rewrite every minute.

Sometimes the summary is enough. But when a detail from the beginning suddenly matters near the end, that one memory may no longer contain the detail clearly.

## Slide 08 — The long-distance problem

Suppose the battery clue has to survive many handoffs before it helps us predict a later word. Each RNN step must pass the clue along successfully. After many steps, the signal can become faint or get mixed with other information.

This is why distant relationships are hard for basic RNNs. The clue is not gone because the model is careless; it is gone because the architecture makes it travel through many small updates.

## Slide 09 — Learning through a long chain

There is also a training problem. While training, the model sends feedback backward through the same long chain to decide how to adjust its weights. That feedback can become too weak, which makes early steps learn very slowly, or too unstable, which makes learning jump around.

You do not need calculus today. Just remember the intuition: a very long chain is difficult both when information travels forward and when learning signals travel backward.

## Slide 10 — LSTM: memory with controls

LSTM stands for Long Short-Term Memory. It keeps the RNN idea of moving through a sequence, but gives the memory more control. Instead of rewriting everything in one simple move, it has gates that decide what to forget, what to write, and what to expose as the current output.

The lane across the top is the important picture. It is a more protected route for long-term information. This makes it easier for an important clue, such as the battery, to survive many steps.

## Slide 11 — Gates protect useful information

Let’s make the gates concrete. If the model sees a useful clue about the battery, it can keep that information. If it sees a detail that is not useful for the current task, it can reduce or discard it.

These are not human-written rules. During training, the model learns gate settings that help it reduce errors. The gates are a better memory-management system, not a human-like decision maker.

## Slide 12 — GRU: a leaner gated memory

A GRU, or Gated Recurrent Unit, is another gated recurrent model. It has the same general goal as an LSTM: control what information should continue through time. It does this with a simpler design and fewer gates.

For us, the main lesson is that both LSTMs and GRUs were attempts to make recurrent memory more reliable. They improve the chain, but the model is still moving through the sequence step by step.

## Slide 13 — Better memory is still not direct access

Gated memory is much better than plain RNN memory, but it still asks one memory stream to carry useful facts forward. If I suddenly need one detail from far earlier, I am still depending on that detail having been preserved all the way along.

This leads to a new idea: rather than carrying every possible clue forward, what if the model could directly look back through the earlier context when it needs something?

## Slide 14 — A better question

Attention starts from a question. At the word “it,” the model does not need all earlier words equally. It needs to ask something like: “Which earlier thing could be low?”

That is a much more focused job. Attention does not replace the whole sequence with one summary. It compares the current need with available context, then brings the most relevant information forward.

## Slide 15 — Attention is selective lookup

This slide is the complete attention story in one picture. First, we have context: earlier words that are available. Next, we estimate relevance: how useful is each earlier item for the current word? Finally, we combine information from the useful items into one context-aware result.

The word “selective” matters. Attention is not simply copying the whole sentence. It gives more influence to the parts that are more relevant right now.

## Slide 16 — A running example

We will use one short example for the rest of the lesson: “The robot picked up the battery because it was low.” The word “it” is our active focus. We need to decide which earlier word gives “it” its meaning in this situation.

As humans, we quickly connect “it” to “battery.” The attention mechanism is one way for a model to learn a similar kind of context-dependent connection from data.

## Slide 17 — What should it look at?

At this point the model should not immediately assume the answer. It should consider several earlier possibilities. The robot, the battery, and even the corridor-like context are available candidates, but they are not equally useful.

Attention begins by looking across the context and asking which pieces match the current need. We have not scored anything yet; we are only setting up the search.

## Slide 18 — Words become meaning profiles

Computers do not begin with words as meanings. Each token becomes a learned collection of numbers, often called an embedding or a meaning profile. We cannot draw thousands of dimensions, so the 3D view is only an intuition tool.

The useful idea is that training can place related patterns in useful arrangements. The representation of “battery” can contain learned information connected to charge, objects, and nearby language patterns. It is not a dictionary entry, but it can become useful for many tasks.

## Slide 19 — Compare what fits

For the word “it,” the model creates a representation of what it is looking for. It compares that with representations of earlier candidates. A candidate that fits the question better receives a larger score.

You can imagine this as checking which arrow points most in the same direction. That is an intuition for a similarity calculation, not a literal picture of every model. In our example, “battery” should receive a stronger match than “robot” or “corridor.”

## Slide 20 — Query

The current word produces a query. A query is simply the question this position is asking of the earlier context. For “it,” a useful informal query is: “Which earlier thing could be low?”

The word itself does not contain that sentence. The model learns a numerical query representation. But thinking of it as a search question is the right intuition.

## Slide 21 — Keys

Each available word produces a key. A key is like a searchable label describing the kind of information that word can offer. The battery might offer “object with charge,” the robot might offer “agent,” and the corridor might offer “place.”

The model compares the current query with these keys. Queries ask; keys describe what is available to be found.

## Slide 22 — Values

Keys help the model decide where to look, but values contain the information the model actually takes from a selected word. For the battery, the value might carry useful information about charge level or battery state. For the robot, it may carry information about who acted.

Here is the simple distinction to remember: keys help choose; values supply information. A model can give the battery a high weight, then bring some of the battery’s value into the representation of “it.”

## Slide 23 — Attention score board

Now we can score the candidates. For the query made by “it,” the battery receives the strongest relevance score. The robot has some relationship to the sentence but is a weaker answer to the question. The corridor is weaker still.

These are raw scores, so they are not yet percentages. They only tell us the model’s relative preference before we convert them into a set of usable attention weights.

## Slide 24 — From scores to attention weights

Raw scores can have any size, so we convert them into attention weights that add up to one hundred percent. This normalization step turns “battery is much stronger” into a clear allocation of attention.

In this example, the battery gets most of the weight. That does not mean every other word becomes useless. It means the battery contributes most strongly to the next context-aware representation.

## Slide 25 — Build a context-aware meaning

Before attention, the word “it” has a general representation. After attention, the model has updated that representation using the information it retrieved from the battery and, to a smaller degree, other context.

The 3D picture is again only an intuition. It shows that the representation of a word can move depending on context. “It” in one sentence can mean a battery; in another sentence, it may mean a robot, a person, or something else.

## Slide 26 — Self-attention

This is self-attention. Every word in the same sequence can ask its own query, compare it with keys from the sequence, turn scores into weights, and blend values into an updated representation. The output is a new, context-aware version of every token.

Today we stopped at this core mechanism. Next session, we will see how multiple attention views work together and how these pieces become the Transformer architecture. The main idea to carry forward is simple: instead of relying only on a long running memory, a model can directly look up useful context.
