# Lesson 7: From RNNs to Attention

## Session Goal

In this session, we move from **recurrent neural networks (RNNs)** to the core intuition behind **self-attention**.

The goal is not to build or fully explain the Transformer yet. Instead, we ask a simpler question:

> How can a model use an important clue that appeared far earlier in a sequence?

We start with RNNs, see why their single running memory can become difficult to use over long distances, introduce LSTMs and GRUs as better memory systems, and then arrive at attention as a way to look up relevant context directly.

By the end of this lesson, students should understand this journey:

```text
Sequence
→ RNN carries one running memory
→ gated models control that memory better
→ attention looks up relevant context directly
→ self-attention creates context-aware word representations
```

This lesson builds on the previous lessons about neural networks. We still use the same core ideas: inputs move through a network, the network makes a prediction, loss measures mistakes, and backpropagation adjusts learned parameters. The new challenge is that the input now has an order.

---

## 1. Why Sequences Need Special Treatment

A sequence is not just a group of inputs. The **order** changes the meaning.

```text
dog bites man
man bites dog
```

The same words appear in both sentences, but they describe different events. This is also true for speech, music, sensor readings, stock prices, and many other kinds of data that unfold over time.

For a sequence model, the important question is not only “what is the input?” It is also:

> What came before this input, and which earlier details still matter now?

### A long-distance clue

Consider this sentence:

```text
The robot picked up the battery because it was low.
```

To understand what `it` means, the useful clue is the word `battery`, even though other words appear between them. A good sequence model needs a way to connect the current word to useful earlier context.

---

## 2. Recurrent Neural Networks: Carrying Memory Forward

An **RNN** reads a sequence one step at a time.

At each step, the RNN receives two inputs:

1. the **current input** — for example, the current word;
2. the **previous memory** — a summary created at the earlier step.

It combines them to create a new memory, then passes that new memory to the next step.

```text
current word + previous memory
→ RNN cell
→ new memory
→ next word
```

The repeating memory is often called the **hidden state**. You can think of it as a small running notebook. The notebook is rewritten after every word so that it hopefully carries the useful story forward.

### Where RNNs can help

RNNs can work well for short sequences with local patterns. Examples include:

- short text completion;
- small speech or sound segments;
- simple time-series signals;
- short sensor sequences.

The same RNN cell is reused at every step, which makes it a natural architecture for variable-length sequences.

---

## 3. The Limits of One Running Memory

The main RNN limitation is a **memory bottleneck**.

A long sequence can contain many useful details, but the RNN tries to fit what matters into one running hidden state. As more words arrive, old information can be weakened, overwritten, or mixed together with new information.

```text
many earlier details
→ one running summary
→ later prediction
```

### The long-distance problem

If an early clue must help much later, it has to survive every step in between. In the battery example, the useful information about `battery` must travel through several updates before it can help interpret `it`.

This is difficult because the signal may fade over many handoffs. It can also become confused with other information added along the way.

### A training problem too

RNNs also learn through long chains. During training, feedback travels backward through the sequence to adjust earlier weights. Over many steps, that feedback can become:

- **too weak** — early parts of the sequence learn very slowly;
- **too unstable** — updates become too large or jumpy.

These problems are often called **vanishing gradients** and **exploding gradients**. You do not need the calculus behind them yet. The useful intuition is that a very long chain is hard both for carrying information forward and for sending learning feedback backward.

---

## 4. LSTMs and GRUs: Better Memory Management

LSTMs and GRUs are improved versions of recurrent models. They still read a sequence step by step, but they give the memory more control.

### LSTM

An **LSTM** stands for **Long Short-Term Memory**. It uses gates that learn when to:

- **forget** information that is no longer useful;
- **write** important new information into memory;
- **output** the information needed at the current step.

The important idea is that LSTMs create a more protected route for long-term information. A useful clue can be kept instead of being rewritten at every step.

### GRU

A **GRU**, or **Gated Recurrent Unit**, has the same overall goal: better control over what the model keeps and updates. It uses a leaner design with fewer gates than an LSTM.

For this course, the key takeaway is not which gated model is always best. Both LSTMs and GRUs improve sequence memory by letting the network learn what to retain and what to discard.

### The remaining limitation

Gated recurrent models are much better than a plain RNN, but they still depend on information traveling through a sequence of steps. If a word far back in the sentence suddenly becomes important, the model still relies on that detail having survived the whole path.

This motivates a different strategy:

> Instead of carrying every clue forward, let the model look back and retrieve the clue it needs.

---

## 5. Attention: Selective Lookup

**Attention** lets the model inspect the available context and decide which parts deserve the most influence for the current task.

The basic story is:

```text
Look back
→ estimate relevance
→ bring useful information forward
```

Attention is **selective**. It does not copy every earlier word with the same importance. It gives stronger influence to context that better matches the current need.

### Running example

We use this sentence throughout the lesson:

```text
The robot picked up the battery because it was low.
```

When the model processes `it`, it should look at several earlier words. `robot`, `battery`, and the surrounding context are all available. But `battery` is the strongest match for the idea “the thing that could be low.”

Attention gives the model a learned way to make this kind of comparison.

---

## 6. From Words to Meaning Profiles

Neural networks work with numbers, not raw words. Each token is turned into a learned list of numbers called an **embedding**.

```text
word
→ learned numeric representation
→ useful patterns of meaning and context
```

An embedding is not a dictionary definition. It is a learned representation that can capture patterns useful for the training task. For example, a representation of `battery` can become useful for recognizing ideas related to charge, objects, tools, and the language that often appears near it.

Embeddings can have hundreds or thousands of dimensions. We sometimes draw them in 2D or 3D only to build intuition. The drawing is not the real size of the representation.

---

## 7. Query, Key, and Value

Attention uses three learned roles for each token: **query**, **key**, and **value**.

### Query: what am I looking for?

The current token creates a **query**. It represents the kind of information that the token needs right now.

For `it`, an informal version of the query might be:

```text
Which earlier thing could be low?
```

The network does not store this English sentence. It learns a numeric representation that plays the same role.

### Key: what kind of information do I offer?

Every available token creates a **key**. A key is a searchable clue about the kind of information that token can offer.

In our example:

| Token | Useful key intuition |
|---|---|
| `battery` | object with charge |
| `robot` | agent / actor |
| `corridor` | place |
| `low` | state |

The query from `it` is compared with the keys from the context. A better match receives a larger relevance score.

### Value: what information can I contribute?

Each token also creates a **value**. The value contains information that can actually be brought forward after the model chooses where to attend.

For example, the value for `battery` may contain useful learned information about battery state or charge level. The value for `robot` may contain information about who performed the action.

> **Keys help choose. Values supply information.**

---

## 8. From Scores to Attention Weights

The model compares the current query with every key and produces **relevance scores**.

For the token `it`, a simplified score board may look like this:

```text
battery  → high score
robot    → medium score
corridor → low score
```

The raw scores are then converted into **attention weights**. These weights are non-negative and add up to 1, or 100% if we display them as percentages.

```text
raw relevance scores
→ normalize
→ attention weights
```

If `battery` receives most of the weight, its value has the strongest influence on the updated representation of `it`. Other words can still contribute a little; attention is usually a weighted blend, not a single hard choice.

---

## 9. Context-Aware Meaning

Before attention, the token `it` has a more general representation. After attention, it has a **context-aware representation** that has been updated using useful information from the sentence.

```text
original representation of “it”
→ retrieve weighted context
→ updated representation of “it”
```

This matters because the same word can mean different things in different sentences. In one sentence, `it` may refer to a battery; in another, it may refer to a robot, a person, or an abstract idea. Attention helps the representation adapt to the current context.

---

## 10. Self-Attention

When the queries, keys, and values all come from the **same sequence**, the mechanism is called **self-attention**.

Every token can:

1. make a query about what it needs;
2. compare that query with keys from the sequence;
3. turn the scores into attention weights;
4. blend the corresponding values;
5. produce an updated, context-aware representation.

```text
Input tokens
→ queries, keys, values
→ relevance scores
→ attention weights
→ weighted value blend
→ contextualized token representations
```

Self-attention does not mean every word treats every other word equally. Each token can learn its own pattern of attention. A pronoun may look strongly at a noun, while another word may focus on a nearby adjective, verb, or punctuation pattern.

---

## 11. What We Have Not Covered Yet

This lesson deliberately stops at **single-head self-attention**. We have built the intuition needed for the next session, but we have not yet covered:

- multi-head attention;
- positional information;
- the complete Transformer block;
- encoder and decoder structures;
- how large language models train on huge datasets;
- agents and tool use.

Those topics become much easier once the selective-lookup story is clear.

---

## 12. Summary of the Journey

1. **Sequences have order.** Earlier context can change the meaning of a later input.
2. **RNNs** process one step at a time and carry a running hidden-state memory forward.
3. A single running memory can struggle with long-distance clues and long training chains.
4. **LSTMs and GRUs** use gates to keep, forget, and update information more carefully.
5. **Attention** lets a model look back at available context instead of relying only on one carried summary.
6. A **query** expresses what the current token needs, a **key** helps identify useful context, and a **value** supplies information.
7. Relevance scores become **attention weights**, which decide how strongly each value contributes.
8. **Self-attention** produces context-aware representations for every token in the same sequence.

The main idea to remember is:

> RNNs try to carry useful context forward. Attention lets the model look useful context up when it needs it.

*Next session: multiple views of attention, the Transformer architecture, and the road toward large language models.*
