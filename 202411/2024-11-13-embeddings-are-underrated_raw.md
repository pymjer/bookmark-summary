Title: Embeddings are underrated

URL Source: https://technicalwriting.dev/data/embeddings.html

Markdown Content:
Machine learning (ML) has the potential to advance the state of the art in technical writing. No, I’m not talking about text generation models like Claude, Gemini, LLaMa, GPT, etc. The ML technology that might end up having the biggest impact on technical writing is **embeddings**.

Embeddings aren't exactly new, but they have become much more widely accessible in the last couple years. What embeddings offer to technical writers is **_the ability to discover connections between texts at previously impossible scales_**.

Building intuition about embeddings[#](https://technicalwriting.dev/data/embeddings.html#building-intuition-about-embeddings "Link to this heading")
----------------------------------------------------------------------------------------------------------------------------------------------------

Here’s an overview of how you use embeddings and how they work. It’s geared towards technical writers who are learning about embeddings for the first time.

### Input and output[#](https://technicalwriting.dev/data/embeddings.html#input-and-output "Link to this heading")

Someone asks you to “make some embeddings”. What do you input? You input text.1 You don’t need to provide the same amount of text every time. E.g. sometimes your input is a single paragraph while at other times it’s a few sections, an entire document, or even multiple documents.

What do you get back? If you provide a single word as the input, the output will be an array of numbers like this:

\[-0.02387, -0.0353, 0.0456\]

Now suppose your input is an entire set of documents. The output turns into this:

\[0.0451, -0.0154, 0.0020\]

One input was drastically smaller than the other, yet they both produced an array of 3 numbers. Curiouser and curiouser. (When you work with real embeddings, the arrays will have hundreds or thousands of numbers, not 3. More on that later.)

Here’s the first key insight. Because we always get back the same amount of numbers no matter how big or small the input text, **we now have a way to mathematically compare any two pieces of arbitrary text to each other**.

But what do those numbers _MEAN_?

1 Some embedding models are “multimodal”, meaning you can also provide images, videos, and audio as input. This post focuses on text since that’s the medium that we work with the most as technical writers. Haven’t seen a multimodal model support taste, touch, or smell yet!

### First, how to literally make the embeddings[#](https://technicalwriting.dev/data/embeddings.html#first-how-to-literally-make-the-embeddings "Link to this heading")

The big service providers have made it easy. Here’s how it’s done with Gemini:

import google.generativeai as gemini

gemini.configure(api\_key\='…')

text \= 'Hello, world!'
response \= gemini.embed\_content(
    model\='models/text-embedding-004',
    content\=text,
    task\_type\='SEMANTIC\_SIMILARITY'
)
embedding \= response\['embedding'\]

The size of the array depends on what model you’re using. Gemini’s [text-embedding-004](https://ai.google.dev/gemini-api/docs/models/gemini#text-embedding) model returns an array of 768 numbers whereas Voyage AI’s [voyage-3](https://docs.voyageai.com/docs/embeddings) model returns an array of 1024 numbers. This is one of the reasons why you can’t use embeddings from different providers interchangeably. (The main reason is that the numbers from one model mean something completely different than the numbers from another model.)

#### Does it cost a lot of money?[#](https://technicalwriting.dev/data/embeddings.html#does-it-cost-a-lot-of-money "Link to this heading")

No.

#### Is it terrible for the environment?[#](https://technicalwriting.dev/data/embeddings.html#is-it-terrible-for-the-environment "Link to this heading")

I don’t know. After the model has been created (trained), I’m pretty sure that generating embeddings is much less computationally intensive than generating text. But it also seems to be the case that embedding models are trained in similar ways as text generation models2, with all the energy usage that implies. I’ll update this section when I find out more.

2 From [You Should Probably Pay Attention to Tokenizers](https://cybernetist.com/2024/10/21/you-should-probably-pay-attention-to-tokenizers/): “Embeddings are byproduct of transformer training and are actually trained on the heaps of tokenized texts. It gets better: embeddings are what is actually fed as the input to LLMs when we ask it to generate text.”

#### What model is best?[#](https://technicalwriting.dev/data/embeddings.html#what-model-is-best "Link to this heading")

Ideally, your embedding model can accept a huge amount of input text, so that you can generate embeddings for complete pages. If you try to provide more input than a model can handle, you usually get an error. As of October 2024 `voyage-3` seems to the clear winner in terms of input size3:

| Organization
 | Model name

 | Input limit ([tokens](https://seantrott.substack.com/p/tokenization-in-large-language-models))

 |
| --- | --- | --- |
| Voyage AI

 | [voyage-3](https://docs.voyageai.com/docs/embeddings)

 | 32000

 |
| Nomic

 | [Embed](https://www.nomic.ai/blog/posts/nomic-embed-text-v1)

 | 8192

 |
| OpenAI

 | [text-embedding-3-large](https://platform.openai.com/docs/guides/embeddings/#embedding-models)

 | 81914

 |
| Mistral

 | [Embed](https://docs.mistral.ai/getting-started/models/models_overview/#premier-models)

 | 8000

 |
| Google

 | [text-embedding-004](https://ai.google.dev/gemini-api/docs/models/gemini#text-embedding)

 | 2048

 |
| Cohere

 | [embed-english-v3.0](https://docs.cohere.com/v2/docs/models#embed)

 | 512

 |

For my particular use cases as a technical writer, large input size is an important factor. However, your use cases may not need large input size, or there may be other factors that are more important. See the [Massive Text Embedding Benchmark](https://arxiv.org/abs/2210.07316) (MTEB) [leaderboard](https://huggingface.co/spaces/mteb/leaderboard).

3 These input limits are based on [tokens](https://seantrott.substack.com/p/tokenization-in-large-language-models), and each service calculates tokens differently, so don’t put too much weight into these exact numbers. E.g. a token for one model may be approximately 3 characters, whereas for another one it may be approximately 4 characters.

4 Previously, I incorrectly listed this model’s input limit as 3072. Sorry for the mistake.

### Very weird multi-dimensional space[#](https://technicalwriting.dev/data/embeddings.html#very-weird-multi-dimensional-space "Link to this heading")

Back to the big mystery. What the hell do these numbers **MEAN**?!?!?!

Let’s begin by thinking about **coordinates on a map**. Suppose I give you three points and their coordinates:

| Point
 | X-Coordinate

 | Y-Coordinate

 |
| --- | --- | --- |
| A

 | 3

 | 2

 |
| B

 | 1

 | 1

 |
| C

 | \-2

 | \-2

 |

There are 2 dimensions to this map: the X-Coordinate and the Y-Coordinate. Each point lives at the intersection of an X-Coordinate and a Y-Coordinate.

Is A closer to B or C?

![Image 1: ../_images/embeddings-1.png](https://technicalwriting.dev/_images/embeddings-1.png)

A is much closer to B.

Here’s the mental leap. _Embeddings are similar to points on a map_. Each number in the embedding array is a _dimension_, similar to the X-Coordinates and Y-Coordinates from earlier. When an embedding model sends you back an array of 1000 numbers, it’s telling you the point where that text _semantically_ lives in its 1000-dimension space, relative to all other texts. When we compare the distance between two embeddings in this 1000-dimension space, what we’re really doing is **figuring out how semantically close or far apart those two texts are from each other**.

![Image 2: ../_images/mindblown.gif](https://technicalwriting.dev/_images/mindblown.gif)

The concept of positioning items in a multi-dimensional space like this, where related items are clustered near each other, goes by the wonderful name of [latent space](https://en.wikipedia.org/wiki/Latent_space).

The most famous example of the weird utility of this technology comes from the [Word2vec paper](https://arxiv.org/pdf/1301.3781), the foundational research that kickstarted interest in embeddings 11 years ago. In the paper they shared this anecdote:

embedding("king") - embedding("man") + embedding("woman") ≈ embedding("queen")

Starting with the embedding for `king`, subtract the embedding for `man`, then add the embedding for `woman`. When you look around this vicinity of the latent space, you find the embedding for `queen` nearby. In other words, embeddings can represent semantic relationships in ways that feel intuitive to us humans. If you asked a human “what’s the female equivalent of a king?” that human would probably answer “queen”, the same answer we get from embeddings. For more explanation of the underlying theories, see [Distributional semantics](https://en.m.wikipedia.org/wiki/Distributional_semantics).

The 2D map analogy was a nice stepping stone for building intuition but now we need to cast it aside, because embeddings operate in hundreds or thousands of dimensions. It’s impossible for us lowly 3-dimensional creatures to visualize what “distance” looks like in 1000 dimensions. Also, we don’t know what each dimension represents, hence the section heading “Very weird multi-dimensional space”.5 One dimension might represent something close to color. The `king - man + woman ≈ queen` anecdote suggests that these models contain a dimension with some notion of gender. And so on. [Well Dude, we just don’t know](https://youtu.be/7ZYqjaLaK08).

The mechanics of converting text into very weird multi-dimensional space are complex, as you might imagine. They are teaching _machines_ to _LEARN_, after all. [The Illustrated Word2vec](https://jalammar.github.io/illustrated-word2vec/) is a good way to start your journey down that rabbithole.

5 I borrowed this phrase from [Embeddings: What they are and why they matter](https://simonwillison.net/2023/Oct/23/embeddings/).

### Comparing embeddings[#](https://technicalwriting.dev/data/embeddings.html#comparing-embeddings "Link to this heading")

After you’ve generated your embeddings, you’ll need some kind of “database” to keep track of what text each embedding is associated to. In the experiment discussed later, I got by with just a local JSON file:

{
    "authors": {
        "embedding": \[…\]
    },
    "changes/0.1": {
        "embedding": \[…\]
    },
    …
}

`authors` is the name of a page. `embedding` is the embedding for that page.

Comparing embeddings involves a lot of linear algebra. I learned the basics from [Linear Algebra for Machine Learning and Data Science](https://www.coursera.org/learn/machine-learning-linear-algebra). The big math and ML libraries like [NumPy](https://numpy.org/doc/stable/) and [scikit-learn](https://scikit-learn.org/stable/) can do the heavy lifting for you (i.e. very little math code on your end).

Applications[#](https://technicalwriting.dev/data/embeddings.html#applications "Link to this heading")
------------------------------------------------------------------------------------------------------

I could tell you exactly how I think we might advance the state of the art in technical writing with embeddings, but where’s the fun in that? You now know why they’re such an interesting and useful new tool in the technical writer toolbox… go connect the rest of the dots yourself!

Let’s cover a basic example to put the intuition-building ideas into practice and then wrap up this post.

### Let a thousand embeddings bloom?[#](https://technicalwriting.dev/data/embeddings.html#let-a-thousand-embeddings-bloom "Link to this heading")

As docs site owners, I wonder if we should start freely providing embeddings for our content to anyone who wants them, via REST APIs or [well-known URIs](https://en.wikipedia.org/wiki/Well-known_URI). Who knows what kinds of cool stuff our communities can build with this extra type of data about our docs?

Parting words[#](https://technicalwriting.dev/data/embeddings.html#parting-words "Link to this heading")
--------------------------------------------------------------------------------------------------------

Three years ago, if you had asked me what 768-dimensional space is, I would have told you that it’s just some abstract concept that physicists and mathematicians need for unfathomable reasons, probably something related to string theory. Embeddings gave me a reason to think about this idea more deeply, and actually apply it to my own work. I think that’s pretty cool.

Order-of-magnitude improvements in our ability to maintain our docs may very well still be possible after all… perhaps we just need an order-of-magnitude-more dimensions!!

Appendix[#](https://technicalwriting.dev/data/embeddings.html#appendix "Link to this heading")
----------------------------------------------------------------------------------------------

### Implementation[#](https://technicalwriting.dev/data/embeddings.html#implementation "Link to this heading")

I created a [Sphinx extension](https://www.sphinx-doc.org/en/master/development/tutorials/extending_build.html) to generate an embedding for each doc. Sphinx automatically invokes this extension as it builds the docs.

import json
import os

import voyageai

VOYAGE\_API\_KEY \= os.getenv('VOYAGE\_API\_KEY')
voyage \= voyageai.Client(api\_key\=VOYAGE\_API\_KEY)

def on\_build\_finished(app, exception):
    with open(srcpath, 'w') as f:
        json.dump(data, f, indent\=4)

def embed\_with\_voyage(text):
    try:
        embedding \= voyage.embed(\[text\], model\='voyage-3', input\_type\='document').embeddings\[0\]
        return embedding
    except Exception as e:
        return None

def on\_doctree\_resolved(app, doctree, docname):
    text \= doctree.astext()
    embedding \= embed\_with\_voyage(text)  \# Generate an embedding for each document!
    data\[docname\] \= {
        'embedding': embedding
    }

\# Use some globals because this is just an experiment and you can't stop me
def init\_globals(srcdir):
    global filename
    global srcpath
    global data
    filename \= 'embeddings.json'
    srcpath \= f'{srcdir}/{filename}'
    data \= {}

def setup(app):
    init\_globals(app.srcdir)
    \# https://www.sphinx-doc.org/en/master/extdev/appapi.html#sphinx-core-events
    app.connect('doctree-resolved', on\_doctree\_resolved)  \# This event fires on every doc that's processed
    app.connect('build-finished', on\_build\_finished)
    return {
        'version': '0.0.1',
        'parallel\_read\_safe': True,
        'parallel\_write\_safe': True,
    }

When the build finishes, the embeddings data is stored in `embeddings.json` like this:

{
    "authors": {
        "embedding": \[…\]
    },
    "changes/0.1": {
        "embedding": \[…\]
    },
    …
}

`authors` and `changes/0.1` are docs. `embedding` contains the embedding for that doc.

The last step is to find the closest neighbor for each doc. I.e. to find the other page that is considered relevant to the page you’re currently on. As mentioned earlier, [Linear Algebra for Machine Learning and Data Science](https://www.coursera.org/learn/machine-learning-linear-algebra) was the class that taught me the basics.

import json

import numpy as np
from sklearn.metrics.pairwise import cosine\_similarity

def find\_docname(data, target):
    for docname in data:
        if data\[docname\]\['embedding'\] \== target:
            return docname
    return None

\# Adapted from the Voyage AI docs
\# https://web.archive.org/web/20240923001107/https://docs.voyageai.com/docs/quickstart-tutorial
def k\_nearest\_neighbors(target, embeddings, k\=5):
    \# Convert to numpy array
    target \= np.array(target)
    embeddings \= np.array(embeddings)
    \# Reshape the query vector embedding to a matrix of shape (1, n) to make it
    \# compatible with cosine\_similarity
    target \= target.reshape(1, \-1)
    \# Calculate the similarity for each item in data
    cosine\_sim \= cosine\_similarity(target, embeddings)
    \# Sort the data by similarity in descending order and take the top k items
    sorted\_indices \= np.argsort(cosine\_sim\[0\])\[::\-1\]
    \# Take the top k related embeddings
    top\_k\_related\_embeddings \= embeddings\[sorted\_indices\[:k\]\]
    top\_k\_related\_embeddings \= \[
        list(row\[:\]) for row in top\_k\_related\_embeddings
    \]  \# convert to list
    return top\_k\_related\_embeddings

with open('doc/embeddings.json', 'r') as f:
    data \= json.load(f)
embeddings \= \[data\[docname\]\['embedding'\] for docname in data\]
print('.. csv-table::')
print('   :header: "Target", "Neighbor"')
print()
for target in embeddings:
    dot\_products \= np.dot(embeddings, target)
    neighbors \= k\_nearest\_neighbors(target, embeddings, k\=3)
    \# ignore neighbors\[0\] because that is always the target itself
    nearest\_neighbor \= neighbors\[1\]
    target\_docname \= find\_docname(data, target)
    target\_cell \= f'\`{target\_docname} <https://www.sphinx-doc.org/en/master/{target\_docname}.html\>\`\_'
    neighbor\_docname \= find\_docname(data, nearest\_neighbor)
    neighbor\_cell \= f'\`{neighbor\_docname} <https://www.sphinx-doc.org/en/master/{neighbor\_docname}.html\>\`\_'
    print(f'   "{target\_cell}", "{neighbor\_cell}"')

As you may have noticed, I did not actually implement the recommendation UI in this experiment. My main goal was to get basic data on whether the embeddings approach generates decent recommendations or not.

### Results[#](https://technicalwriting.dev/data/embeddings.html#results "Link to this heading")

How to interpret the data: `Target` would be the page that you’re currently on. `Neighbor` would be the recommended page.

| Target
 | Neighbor

 |
| --- | --- |
| [authors](https://www.sphinx-doc.org/en/master/authors.html)

 | [changes/0.6](https://www.sphinx-doc.org/en/master/changes/0.6.html)

 |
| [changes/0.1](https://www.sphinx-doc.org/en/master/changes/0.1.html)

 | [changes/0.5](https://www.sphinx-doc.org/en/master/changes/0.5.html)

 |
| [changes/0.2](https://www.sphinx-doc.org/en/master/changes/0.2.html)

 | [changes/1.2](https://www.sphinx-doc.org/en/master/changes/1.2.html)

 |
| [changes/0.3](https://www.sphinx-doc.org/en/master/changes/0.3.html)

 | [changes/0.4](https://www.sphinx-doc.org/en/master/changes/0.4.html)

 |
| [changes/0.4](https://www.sphinx-doc.org/en/master/changes/0.4.html)

 | [changes/1.2](https://www.sphinx-doc.org/en/master/changes/1.2.html)

 |
| [changes/0.5](https://www.sphinx-doc.org/en/master/changes/0.5.html)

 | [changes/0.6](https://www.sphinx-doc.org/en/master/changes/0.6.html)

 |
| [changes/0.6](https://www.sphinx-doc.org/en/master/changes/0.6.html)

 | [changes/1.6](https://www.sphinx-doc.org/en/master/changes/1.6.html)

 |
| [changes/1.0](https://www.sphinx-doc.org/en/master/changes/1.0.html)

 | [changes/1.3](https://www.sphinx-doc.org/en/master/changes/1.3.html)

 |
| [changes/1.1](https://www.sphinx-doc.org/en/master/changes/1.1.html)

 | [changes/1.2](https://www.sphinx-doc.org/en/master/changes/1.2.html)

 |
| [changes/1.2](https://www.sphinx-doc.org/en/master/changes/1.2.html)

 | [changes/1.1](https://www.sphinx-doc.org/en/master/changes/1.1.html)

 |
| [changes/1.3](https://www.sphinx-doc.org/en/master/changes/1.3.html)

 | [changes/1.4](https://www.sphinx-doc.org/en/master/changes/1.4.html)

 |
| [changes/1.4](https://www.sphinx-doc.org/en/master/changes/1.4.html)

 | [changes/1.3](https://www.sphinx-doc.org/en/master/changes/1.3.html)

 |
| [changes/1.5](https://www.sphinx-doc.org/en/master/changes/1.5.html)

 | [changes/1.6](https://www.sphinx-doc.org/en/master/changes/1.6.html)

 |
| [changes/1.6](https://www.sphinx-doc.org/en/master/changes/1.6.html)

 | [changes/1.5](https://www.sphinx-doc.org/en/master/changes/1.5.html)

 |
| [changes/1.7](https://www.sphinx-doc.org/en/master/changes/1.7.html)

 | [changes/1.8](https://www.sphinx-doc.org/en/master/changes/1.8.html)

 |
| [changes/1.8](https://www.sphinx-doc.org/en/master/changes/1.8.html)

 | [changes/1.6](https://www.sphinx-doc.org/en/master/changes/1.6.html)

 |
| [changes/2.0](https://www.sphinx-doc.org/en/master/changes/2.0.html)

 | [changes/1.8](https://www.sphinx-doc.org/en/master/changes/1.8.html)

 |
| [changes/2.1](https://www.sphinx-doc.org/en/master/changes/2.1.html)

 | [changes/1.2](https://www.sphinx-doc.org/en/master/changes/1.2.html)

 |
| [changes/2.2](https://www.sphinx-doc.org/en/master/changes/2.2.html)

 | [changes/1.2](https://www.sphinx-doc.org/en/master/changes/1.2.html)

 |
| [changes/2.3](https://www.sphinx-doc.org/en/master/changes/2.3.html)

 | [changes/2.1](https://www.sphinx-doc.org/en/master/changes/2.1.html)

 |
| [changes/2.4](https://www.sphinx-doc.org/en/master/changes/2.4.html)

 | [changes/3.5](https://www.sphinx-doc.org/en/master/changes/3.5.html)

 |
| [changes/3.0](https://www.sphinx-doc.org/en/master/changes/3.0.html)

 | [changes/4.3](https://www.sphinx-doc.org/en/master/changes/4.3.html)

 |
| [changes/3.1](https://www.sphinx-doc.org/en/master/changes/3.1.html)

 | [changes/3.3](https://www.sphinx-doc.org/en/master/changes/3.3.html)

 |
| [changes/3.2](https://www.sphinx-doc.org/en/master/changes/3.2.html)

 | [changes/3.0](https://www.sphinx-doc.org/en/master/changes/3.0.html)

 |
| [changes/3.3](https://www.sphinx-doc.org/en/master/changes/3.3.html)

 | [changes/3.1](https://www.sphinx-doc.org/en/master/changes/3.1.html)

 |
| [changes/3.4](https://www.sphinx-doc.org/en/master/changes/3.4.html)

 | [changes/4.3](https://www.sphinx-doc.org/en/master/changes/4.3.html)

 |
| [changes/3.5](https://www.sphinx-doc.org/en/master/changes/3.5.html)

 | [changes/1.3](https://www.sphinx-doc.org/en/master/changes/1.3.html)

 |
| [changes/4.0](https://www.sphinx-doc.org/en/master/changes/4.0.html)

 | [changes/3.0](https://www.sphinx-doc.org/en/master/changes/3.0.html)

 |
| [changes/4.1](https://www.sphinx-doc.org/en/master/changes/4.1.html)

 | [changes/4.4](https://www.sphinx-doc.org/en/master/changes/4.4.html)

 |
| [changes/4.2](https://www.sphinx-doc.org/en/master/changes/4.2.html)

 | [changes/4.4](https://www.sphinx-doc.org/en/master/changes/4.4.html)

 |
| [changes/4.3](https://www.sphinx-doc.org/en/master/changes/4.3.html)

 | [changes/3.0](https://www.sphinx-doc.org/en/master/changes/3.0.html)

 |
| [changes/4.4](https://www.sphinx-doc.org/en/master/changes/4.4.html)

 | [changes/7.4](https://www.sphinx-doc.org/en/master/changes/7.4.html)

 |
| [changes/4.5](https://www.sphinx-doc.org/en/master/changes/4.5.html)

 | [changes/4.4](https://www.sphinx-doc.org/en/master/changes/4.4.html)

 |
| [changes/5.0](https://www.sphinx-doc.org/en/master/changes/5.0.html)

 | [changes/3.5](https://www.sphinx-doc.org/en/master/changes/3.5.html)

 |
| [changes/5.1](https://www.sphinx-doc.org/en/master/changes/5.1.html)

 | [changes/5.0](https://www.sphinx-doc.org/en/master/changes/5.0.html)

 |
| [changes/5.2](https://www.sphinx-doc.org/en/master/changes/5.2.html)

 | [changes/3.5](https://www.sphinx-doc.org/en/master/changes/3.5.html)

 |
| [changes/5.3](https://www.sphinx-doc.org/en/master/changes/5.3.html)

 | [changes/5.2](https://www.sphinx-doc.org/en/master/changes/5.2.html)

 |
| [changes/6.0](https://www.sphinx-doc.org/en/master/changes/6.0.html)

 | [changes/6.2](https://www.sphinx-doc.org/en/master/changes/6.2.html)

 |
| [changes/6.1](https://www.sphinx-doc.org/en/master/changes/6.1.html)

 | [changes/6.2](https://www.sphinx-doc.org/en/master/changes/6.2.html)

 |
| [changes/6.2](https://www.sphinx-doc.org/en/master/changes/6.2.html)

 | [changes/6.1](https://www.sphinx-doc.org/en/master/changes/6.1.html)

 |
| [changes/7.0](https://www.sphinx-doc.org/en/master/changes/7.0.html)

 | [extdev/deprecated](https://www.sphinx-doc.org/en/master/extdev/deprecated.html)

 |
| [changes/7.1](https://www.sphinx-doc.org/en/master/changes/7.1.html)

 | [changes/7.2](https://www.sphinx-doc.org/en/master/changes/7.2.html)

 |
| [changes/7.2](https://www.sphinx-doc.org/en/master/changes/7.2.html)

 | [changes/7.4](https://www.sphinx-doc.org/en/master/changes/7.4.html)

 |
| [changes/7.3](https://www.sphinx-doc.org/en/master/changes/7.3.html)

 | [changes/7.4](https://www.sphinx-doc.org/en/master/changes/7.4.html)

 |
| [changes/7.4](https://www.sphinx-doc.org/en/master/changes/7.4.html)

 | [changes/7.3](https://www.sphinx-doc.org/en/master/changes/7.3.html)

 |
| [changes/8.0](https://www.sphinx-doc.org/en/master/changes/8.0.html)

 | [changes/8.1](https://www.sphinx-doc.org/en/master/changes/8.1.html)

 |
| [changes/8.1](https://www.sphinx-doc.org/en/master/changes/8.1.html)

 | [changes/1.8](https://www.sphinx-doc.org/en/master/changes/1.8.html)

 |
| [changes/index](https://www.sphinx-doc.org/en/master/changes/index.html)

 | [changes/8.0](https://www.sphinx-doc.org/en/master/changes/8.0.html)

 |
| [development/howtos/builders](https://www.sphinx-doc.org/en/master/development/howtos/builders.html)

 | [usage/extensions/index](https://www.sphinx-doc.org/en/master/usage/extensions/index.html)

 |
| [development/howtos/index](https://www.sphinx-doc.org/en/master/development/howtos/index.html)

 | [development/tutorials/index](https://www.sphinx-doc.org/en/master/development/tutorials/index.html)

 |
| [development/howtos/setup\_extension](https://www.sphinx-doc.org/en/master/development/howtos/setup_extension.html)

 | [usage/extensions/index](https://www.sphinx-doc.org/en/master/usage/extensions/index.html)

 |
| [development/html\_themes/index](https://www.sphinx-doc.org/en/master/development/html_themes/index.html)

 | [usage/theming](https://www.sphinx-doc.org/en/master/usage/theming.html)

 |
| [development/html\_themes/templating](https://www.sphinx-doc.org/en/master/development/html_themes/templating.html)

 | [development/html\_themes/index](https://www.sphinx-doc.org/en/master/development/html_themes/index.html)

 |
| [development/index](https://www.sphinx-doc.org/en/master/development/index.html)

 | [usage/index](https://www.sphinx-doc.org/en/master/usage/index.html)

 |
| [development/tutorials/adding\_domain](https://www.sphinx-doc.org/en/master/development/tutorials/adding_domain.html)

 | [extdev/domainapi](https://www.sphinx-doc.org/en/master/extdev/domainapi.html)

 |
| [development/tutorials/autodoc\_ext](https://www.sphinx-doc.org/en/master/development/tutorials/autodoc_ext.html)

 | [usage/extensions/autodoc](https://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html)

 |
| [development/tutorials/examples/README](https://www.sphinx-doc.org/en/master/development/tutorials/examples/README.html)

 | [tutorial/end](https://www.sphinx-doc.org/en/master/tutorial/end.html)

 |
| [development/tutorials/extending\_build](https://www.sphinx-doc.org/en/master/development/tutorials/extending_build.html)

 | [usage/extensions/todo](https://www.sphinx-doc.org/en/master/usage/extensions/todo.html)

 |
| [development/tutorials/extending\_syntax](https://www.sphinx-doc.org/en/master/development/tutorials/extending_syntax.html)

 | [extdev/markupapi](https://www.sphinx-doc.org/en/master/extdev/markupapi.html)

 |
| [development/tutorials/index](https://www.sphinx-doc.org/en/master/development/tutorials/index.html)

 | [development/howtos/index](https://www.sphinx-doc.org/en/master/development/howtos/index.html)

 |
| [examples](https://www.sphinx-doc.org/en/master/examples.html)

 | [index](https://www.sphinx-doc.org/en/master/index.html)

 |
| [extdev/appapi](https://www.sphinx-doc.org/en/master/extdev/appapi.html)

 | [extdev/index](https://www.sphinx-doc.org/en/master/extdev/index.html)

 |
| [extdev/builderapi](https://www.sphinx-doc.org/en/master/extdev/builderapi.html)

 | [usage/builders/index](https://www.sphinx-doc.org/en/master/usage/builders/index.html)

 |
| [extdev/collectorapi](https://www.sphinx-doc.org/en/master/extdev/collectorapi.html)

 | [extdev/envapi](https://www.sphinx-doc.org/en/master/extdev/envapi.html)

 |
| [extdev/deprecated](https://www.sphinx-doc.org/en/master/extdev/deprecated.html)

 | [changes/1.8](https://www.sphinx-doc.org/en/master/changes/1.8.html)

 |
| [extdev/domainapi](https://www.sphinx-doc.org/en/master/extdev/domainapi.html)

 | [usage/domains/index](https://www.sphinx-doc.org/en/master/usage/domains/index.html)

 |
| [extdev/envapi](https://www.sphinx-doc.org/en/master/extdev/envapi.html)

 | [extdev/collectorapi](https://www.sphinx-doc.org/en/master/extdev/collectorapi.html)

 |
| [extdev/event\_callbacks](https://www.sphinx-doc.org/en/master/extdev/event_callbacks.html)

 | [extdev/appapi](https://www.sphinx-doc.org/en/master/extdev/appapi.html)

 |
| [extdev/i18n](https://www.sphinx-doc.org/en/master/extdev/i18n.html)

 | [usage/advanced/intl](https://www.sphinx-doc.org/en/master/usage/advanced/intl.html)

 |
| [extdev/index](https://www.sphinx-doc.org/en/master/extdev/index.html)

 | [extdev/appapi](https://www.sphinx-doc.org/en/master/extdev/appapi.html)

 |
| [extdev/logging](https://www.sphinx-doc.org/en/master/extdev/logging.html)

 | [extdev/appapi](https://www.sphinx-doc.org/en/master/extdev/appapi.html)

 |
| [extdev/markupapi](https://www.sphinx-doc.org/en/master/extdev/markupapi.html)

 | [development/tutorials/extending\_syntax](https://www.sphinx-doc.org/en/master/development/tutorials/extending_syntax.html)

 |
| [extdev/nodes](https://www.sphinx-doc.org/en/master/extdev/nodes.html)

 | [extdev/domainapi](https://www.sphinx-doc.org/en/master/extdev/domainapi.html)

 |
| [extdev/parserapi](https://www.sphinx-doc.org/en/master/extdev/parserapi.html)

 | [extdev/appapi](https://www.sphinx-doc.org/en/master/extdev/appapi.html)

 |
| [extdev/projectapi](https://www.sphinx-doc.org/en/master/extdev/projectapi.html)

 | [extdev/envapi](https://www.sphinx-doc.org/en/master/extdev/envapi.html)

 |
| [extdev/testing](https://www.sphinx-doc.org/en/master/extdev/testing.html)

 | [internals/contributing](https://www.sphinx-doc.org/en/master/internals/contributing.html)

 |
| [extdev/utils](https://www.sphinx-doc.org/en/master/extdev/utils.html)

 | [extdev/appapi](https://www.sphinx-doc.org/en/master/extdev/appapi.html)

 |
| [faq](https://www.sphinx-doc.org/en/master/faq.html)

 | [usage/configuration](https://www.sphinx-doc.org/en/master/usage/configuration.html)

 |
| [glossary](https://www.sphinx-doc.org/en/master/glossary.html)

 | [usage/quickstart](https://www.sphinx-doc.org/en/master/usage/quickstart.html)

 |
| [index](https://www.sphinx-doc.org/en/master/index.html)

 | [usage/quickstart](https://www.sphinx-doc.org/en/master/usage/quickstart.html)

 |
| [internals/code-of-conduct](https://www.sphinx-doc.org/en/master/internals/code-of-conduct.html)

 | [internals/index](https://www.sphinx-doc.org/en/master/internals/index.html)

 |
| [internals/contributing](https://www.sphinx-doc.org/en/master/internals/contributing.html)

 | [usage/advanced/intl](https://www.sphinx-doc.org/en/master/usage/advanced/intl.html)

 |
| [internals/index](https://www.sphinx-doc.org/en/master/internals/index.html)

 | [usage/index](https://www.sphinx-doc.org/en/master/usage/index.html)

 |
| [internals/organization](https://www.sphinx-doc.org/en/master/internals/organization.html)

 | [internals/contributing](https://www.sphinx-doc.org/en/master/internals/contributing.html)

 |
| [internals/release-process](https://www.sphinx-doc.org/en/master/internals/release-process.html)

 | [extdev/deprecated](https://www.sphinx-doc.org/en/master/extdev/deprecated.html)

 |
| [latex](https://www.sphinx-doc.org/en/master/latex.html)

 | [usage/configuration](https://www.sphinx-doc.org/en/master/usage/configuration.html)

 |
| [man/index](https://www.sphinx-doc.org/en/master/man/index.html)

 | [usage/index](https://www.sphinx-doc.org/en/master/usage/index.html)

 |
| [man/sphinx-apidoc](https://www.sphinx-doc.org/en/master/man/sphinx-apidoc.html)

 | [man/sphinx-autogen](https://www.sphinx-doc.org/en/master/man/sphinx-autogen.html)

 |
| [man/sphinx-autogen](https://www.sphinx-doc.org/en/master/man/sphinx-autogen.html)

 | [usage/extensions/autosummary](https://www.sphinx-doc.org/en/master/usage/extensions/autosummary.html)

 |
| [man/sphinx-build](https://www.sphinx-doc.org/en/master/man/sphinx-build.html)

 | [usage/configuration](https://www.sphinx-doc.org/en/master/usage/configuration.html)

 |
| [man/sphinx-quickstart](https://www.sphinx-doc.org/en/master/man/sphinx-quickstart.html)

 | [tutorial/getting-started](https://www.sphinx-doc.org/en/master/tutorial/getting-started.html)

 |
| [support](https://www.sphinx-doc.org/en/master/support.html)

 | [tutorial/end](https://www.sphinx-doc.org/en/master/tutorial/end.html)

 |
| [tutorial/automatic-doc-generation](https://www.sphinx-doc.org/en/master/tutorial/automatic-doc-generation.html)

 | [usage/extensions/autosummary](https://www.sphinx-doc.org/en/master/usage/extensions/autosummary.html)

 |
| [tutorial/deploying](https://www.sphinx-doc.org/en/master/tutorial/deploying.html)

 | [tutorial/first-steps](https://www.sphinx-doc.org/en/master/tutorial/first-steps.html)

 |
| [tutorial/describing-code](https://www.sphinx-doc.org/en/master/tutorial/describing-code.html)

 | [usage/domains/index](https://www.sphinx-doc.org/en/master/usage/domains/index.html)

 |
| [tutorial/end](https://www.sphinx-doc.org/en/master/tutorial/end.html)

 | [usage/index](https://www.sphinx-doc.org/en/master/usage/index.html)

 |
| [tutorial/first-steps](https://www.sphinx-doc.org/en/master/tutorial/first-steps.html)

 | [tutorial/getting-started](https://www.sphinx-doc.org/en/master/tutorial/getting-started.html)

 |
| [tutorial/getting-started](https://www.sphinx-doc.org/en/master/tutorial/getting-started.html)

 | [tutorial/index](https://www.sphinx-doc.org/en/master/tutorial/index.html)

 |
| [tutorial/index](https://www.sphinx-doc.org/en/master/tutorial/index.html)

 | [tutorial/getting-started](https://www.sphinx-doc.org/en/master/tutorial/getting-started.html)

 |
| [tutorial/more-sphinx-customization](https://www.sphinx-doc.org/en/master/tutorial/more-sphinx-customization.html)

 | [usage/theming](https://www.sphinx-doc.org/en/master/usage/theming.html)

 |
| [tutorial/narrative-documentation](https://www.sphinx-doc.org/en/master/tutorial/narrative-documentation.html)

 | [usage/quickstart](https://www.sphinx-doc.org/en/master/usage/quickstart.html)

 |
| [usage/advanced/intl](https://www.sphinx-doc.org/en/master/usage/advanced/intl.html)

 | [internals/contributing](https://www.sphinx-doc.org/en/master/internals/contributing.html)

 |
| [usage/advanced/websupport/api](https://www.sphinx-doc.org/en/master/usage/advanced/websupport/api.html)

 | [usage/advanced/websupport/quickstart](https://www.sphinx-doc.org/en/master/usage/advanced/websupport/quickstart.html)

 |
| [usage/advanced/websupport/index](https://www.sphinx-doc.org/en/master/usage/advanced/websupport/index.html)

 | [usage/advanced/websupport/quickstart](https://www.sphinx-doc.org/en/master/usage/advanced/websupport/quickstart.html)

 |
| [usage/advanced/websupport/quickstart](https://www.sphinx-doc.org/en/master/usage/advanced/websupport/quickstart.html)

 | [usage/advanced/websupport/api](https://www.sphinx-doc.org/en/master/usage/advanced/websupport/api.html)

 |
| [usage/advanced/websupport/searchadapters](https://www.sphinx-doc.org/en/master/usage/advanced/websupport/searchadapters.html)

 | [usage/advanced/websupport/api](https://www.sphinx-doc.org/en/master/usage/advanced/websupport/api.html)

 |
| [usage/advanced/websupport/storagebackends](https://www.sphinx-doc.org/en/master/usage/advanced/websupport/storagebackends.html)

 | [usage/advanced/websupport/api](https://www.sphinx-doc.org/en/master/usage/advanced/websupport/api.html)

 |
| [usage/builders/index](https://www.sphinx-doc.org/en/master/usage/builders/index.html)

 | [usage/configuration](https://www.sphinx-doc.org/en/master/usage/configuration.html)

 |
| [usage/configuration](https://www.sphinx-doc.org/en/master/usage/configuration.html)

 | [changes/1.2](https://www.sphinx-doc.org/en/master/changes/1.2.html)

 |
| [usage/domains/c](https://www.sphinx-doc.org/en/master/usage/domains/c.html)

 | [usage/domains/cpp](https://www.sphinx-doc.org/en/master/usage/domains/cpp.html)

 |
| [usage/domains/cpp](https://www.sphinx-doc.org/en/master/usage/domains/cpp.html)

 | [usage/domains/c](https://www.sphinx-doc.org/en/master/usage/domains/c.html)

 |
| [usage/domains/index](https://www.sphinx-doc.org/en/master/usage/domains/index.html)

 | [extdev/domainapi](https://www.sphinx-doc.org/en/master/extdev/domainapi.html)

 |
| [usage/domains/javascript](https://www.sphinx-doc.org/en/master/usage/domains/javascript.html)

 | [usage/domains/python](https://www.sphinx-doc.org/en/master/usage/domains/python.html)

 |
| [usage/domains/mathematics](https://www.sphinx-doc.org/en/master/usage/domains/mathematics.html)

 | [usage/referencing](https://www.sphinx-doc.org/en/master/usage/referencing.html)

 |
| [usage/domains/python](https://www.sphinx-doc.org/en/master/usage/domains/python.html)

 | [extdev/domainapi](https://www.sphinx-doc.org/en/master/extdev/domainapi.html)

 |
| [usage/domains/restructuredtext](https://www.sphinx-doc.org/en/master/usage/domains/restructuredtext.html)

 | [extdev/markupapi](https://www.sphinx-doc.org/en/master/extdev/markupapi.html)

 |
| [usage/domains/standard](https://www.sphinx-doc.org/en/master/usage/domains/standard.html)

 | [usage/domains/index](https://www.sphinx-doc.org/en/master/usage/domains/index.html)

 |
| [usage/extensions/autodoc](https://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html)

 | [tutorial/automatic-doc-generation](https://www.sphinx-doc.org/en/master/tutorial/automatic-doc-generation.html)

 |
| [usage/extensions/autosectionlabel](https://www.sphinx-doc.org/en/master/usage/extensions/autosectionlabel.html)

 | [usage/quickstart](https://www.sphinx-doc.org/en/master/usage/quickstart.html)

 |
| [usage/extensions/autosummary](https://www.sphinx-doc.org/en/master/usage/extensions/autosummary.html)

 | [tutorial/automatic-doc-generation](https://www.sphinx-doc.org/en/master/tutorial/automatic-doc-generation.html)

 |
| [usage/extensions/coverage](https://www.sphinx-doc.org/en/master/usage/extensions/coverage.html)

 | [usage/extensions/autodoc](https://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html)

 |
| [usage/extensions/doctest](https://www.sphinx-doc.org/en/master/usage/extensions/doctest.html)

 | [tutorial/describing-code](https://www.sphinx-doc.org/en/master/tutorial/describing-code.html)

 |
| [usage/extensions/duration](https://www.sphinx-doc.org/en/master/usage/extensions/duration.html)

 | [tutorial/more-sphinx-customization](https://www.sphinx-doc.org/en/master/tutorial/more-sphinx-customization.html)

 |
| [usage/extensions/example\_google](https://www.sphinx-doc.org/en/master/usage/extensions/example_google.html)

 | [usage/extensions/example\_numpy](https://www.sphinx-doc.org/en/master/usage/extensions/example_numpy.html)

 |
| [usage/extensions/example\_numpy](https://www.sphinx-doc.org/en/master/usage/extensions/example_numpy.html)

 | [usage/extensions/example\_google](https://www.sphinx-doc.org/en/master/usage/extensions/example_google.html)

 |
| [usage/extensions/extlinks](https://www.sphinx-doc.org/en/master/usage/extensions/extlinks.html)

 | [usage/extensions/intersphinx](https://www.sphinx-doc.org/en/master/usage/extensions/intersphinx.html)

 |
| [usage/extensions/githubpages](https://www.sphinx-doc.org/en/master/usage/extensions/githubpages.html)

 | [tutorial/deploying](https://www.sphinx-doc.org/en/master/tutorial/deploying.html)

 |
| [usage/extensions/graphviz](https://www.sphinx-doc.org/en/master/usage/extensions/graphviz.html)

 | [usage/extensions/math](https://www.sphinx-doc.org/en/master/usage/extensions/math.html)

 |
| [usage/extensions/ifconfig](https://www.sphinx-doc.org/en/master/usage/extensions/ifconfig.html)

 | [usage/extensions/doctest](https://www.sphinx-doc.org/en/master/usage/extensions/doctest.html)

 |
| [usage/extensions/imgconverter](https://www.sphinx-doc.org/en/master/usage/extensions/imgconverter.html)

 | [usage/extensions/math](https://www.sphinx-doc.org/en/master/usage/extensions/math.html)

 |
| [usage/extensions/index](https://www.sphinx-doc.org/en/master/usage/extensions/index.html)

 | [development/index](https://www.sphinx-doc.org/en/master/development/index.html)

 |
| [usage/extensions/inheritance](https://www.sphinx-doc.org/en/master/usage/extensions/inheritance.html)

 | [usage/extensions/graphviz](https://www.sphinx-doc.org/en/master/usage/extensions/graphviz.html)

 |
| [usage/extensions/intersphinx](https://www.sphinx-doc.org/en/master/usage/extensions/intersphinx.html)

 | [usage/quickstart](https://www.sphinx-doc.org/en/master/usage/quickstart.html)

 |
| [usage/extensions/linkcode](https://www.sphinx-doc.org/en/master/usage/extensions/linkcode.html)

 | [usage/extensions/viewcode](https://www.sphinx-doc.org/en/master/usage/extensions/viewcode.html)

 |
| [usage/extensions/math](https://www.sphinx-doc.org/en/master/usage/extensions/math.html)

 | [usage/configuration](https://www.sphinx-doc.org/en/master/usage/configuration.html)

 |
| [usage/extensions/napoleon](https://www.sphinx-doc.org/en/master/usage/extensions/napoleon.html)

 | [usage/extensions/example\_google](https://www.sphinx-doc.org/en/master/usage/extensions/example_google.html)

 |
| [usage/extensions/todo](https://www.sphinx-doc.org/en/master/usage/extensions/todo.html)

 | [development/tutorials/extending\_build](https://www.sphinx-doc.org/en/master/development/tutorials/extending_build.html)

 |
| [usage/extensions/viewcode](https://www.sphinx-doc.org/en/master/usage/extensions/viewcode.html)

 | [usage/extensions/linkcode](https://www.sphinx-doc.org/en/master/usage/extensions/linkcode.html)

 |
| [usage/index](https://www.sphinx-doc.org/en/master/usage/index.html)

 | [tutorial/end](https://www.sphinx-doc.org/en/master/tutorial/end.html)

 |
| [usage/installation](https://www.sphinx-doc.org/en/master/usage/installation.html)

 | [tutorial/getting-started](https://www.sphinx-doc.org/en/master/tutorial/getting-started.html)

 |
| [usage/markdown](https://www.sphinx-doc.org/en/master/usage/markdown.html)

 | [extdev/parserapi](https://www.sphinx-doc.org/en/master/extdev/parserapi.html)

 |
| [usage/quickstart](https://www.sphinx-doc.org/en/master/usage/quickstart.html)

 | [index](https://www.sphinx-doc.org/en/master/index.html)

 |
| [usage/referencing](https://www.sphinx-doc.org/en/master/usage/referencing.html)

 | [usage/restructuredtext/roles](https://www.sphinx-doc.org/en/master/usage/restructuredtext/roles.html)

 |
| [usage/restructuredtext/basics](https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html)

 | [usage/restructuredtext/directives](https://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html)

 |
| [usage/restructuredtext/directives](https://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html)

 | [usage/restructuredtext/basics](https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html)

 |
| [usage/restructuredtext/domains](https://www.sphinx-doc.org/en/master/usage/restructuredtext/domains.html)

 | [usage/domains/index](https://www.sphinx-doc.org/en/master/usage/domains/index.html)

 |
| [usage/restructuredtext/field-lists](https://www.sphinx-doc.org/en/master/usage/restructuredtext/field-lists.html)

 | [usage/restructuredtext/directives](https://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html)

 |
| [usage/restructuredtext/index](https://www.sphinx-doc.org/en/master/usage/restructuredtext/index.html)

 | [usage/restructuredtext/basics](https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html)

 |
| [usage/restructuredtext/roles](https://www.sphinx-doc.org/en/master/usage/restructuredtext/roles.html)

 | [usage/referencing](https://www.sphinx-doc.org/en/master/usage/referencing.html)

 |
| [usage/theming](https://www.sphinx-doc.org/en/master/usage/theming.html)

 | [development/html\_themes/index](https://www.sphinx-doc.org/en/master/development/html_themes/index.html)

 |
