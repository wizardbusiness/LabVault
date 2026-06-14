
https://spacy.io/usage/spacy-101

What it is: a nlp processor library built in python
Minimum Specs: Modern CPU
Where it runs: Locally

What's it good for? Spacy is used for working with large quantities of text where its infeasible to analyze it by hand. 
- Process and 'understand' large volumes of text

Concepts:
- Tokenization
- Part-of-speech (POS) Tagging
- Lemmatization 
- Sentence Boundary Detection (SBD)
- Named Entity Recognition (NER)
- Entity Linking (EL)
- Similarity 
- Text Classification (`textcat)
- Rule-based Matching
- Training
- Serialization
- Pipeline

See https://spacy.io/usage/spacy-101#features

Spacy lets you use pipelines with methods that employ pre-trained, fine-tuned, or trained-from-scratch models to analyze ingested text. 

 **Use a pretrained pipeline as-is when:**

- You need standard NLP tasks — POS tagging, dependency parsing, NER for common entity types (ORG, PERSON, GPE, DATE) — on general English text.
- Your input is clean, standard text similar to what the model was trained on. [GitHub](https://github.com/explosion/spaCy/discussions/11952)
- You need `textcat` — pretrained pipelines don't include it at all, so this always requires training. More on that below.

**Fine-tune an existing pipeline when:**

- The pretrained model mostly works but misses domain-specific entities. For example, `en_core_web_sm` knows "Apple" is an ORG, but won't know that "BRCA1" is a gene unless you train it. You can source the existing `ner` component and update it with your examples — this lets you mix and match components and create custom pipeline packages with updated trained components alongside components trained on your data. [spaCy](https://spacy.io/usage/processing-pipelines)
- If you find that a pretrained model's accuracy is unsatisfactory on your text, the best solution is to train a model on your own data. [GitHub](https://github.com/explosion/spaCy/discussions/11952)

**Train from scratch when:**

- You're adding a component that doesn't exist in any pretrained pipeline — `textcat` (text categorizer)is the main example. There's no pretrained text categorizer because categories are always domain-specific (your "POSITIVE/NEGATIVE" or "COMPLAINT/NOT_COMPLAINT" labels are yours alone).
- Your text domain is radically different — medical records, legal contracts, OCR output, code, chat logs. A model trained on Wikipedia will likely perform badly on Twitter. Similarly, a model trained on romantic novels will likely perform badly on legal text. [spaCy](https://spacy.io/usage/training)

```
Is your task general NLP (POS, deps, common NER)?
  → Use en_core_web_sm as-is.

Are you adding textcat?
  → Always train. No pretrained option exists.
  → BUT: use a pretrained pipeline as the BASE for tokenization/tok2vec.
       spacy init config will set this up automatically.

Do you need NER for custom entity types?
  → Source the existing ner component, fine-tune with your data.
  → Don't throw away the pretrained weights; you'll lose general NER.
```

**How to filter text tokens**

```python

with open(SIMPLE) as simp:
text = simp.read()
doc = nlp(text)

tokens = [token for token in doc if not token.is_punct and not token.text == '`']
print(tokens)
```

is_punct attribute checks whether a token is punctiation or not. 
\>? Why does this matter?
It has to do with how token similarity is measured. Spacy's tok2vec vectorizer measures similarity based on co-occurence with other tokens in the training data. Punctuation will have a very high co-occurence incidence with literally every word token, because it commonly occurs next to pretty much every word. 

**How to train a model**
[Spacy Docs - Training Quickstart](https://spacy.io/usage/training)
\>? What is the gradient of the loss?
\>? What is the gradient of the weights?
\>? What is backpropogation?
\>? How do you create a config file? 
\>? HOw do you structure the training data
\>? How do you structure the evaluation data?
\>? How complicated is setting up the config file?
\>? What is spacy's binary format?
\>? What prep do i need to do to determine if I even need to train a model? 

**Spacy's Architecture**

? > Spacy is fundamentally a pipeline that starts with a tokenizer that turns raw text into tokens and stores them in a Spacy `Doc` object. The `Doc` object is then passed through a `pipeline` of `components`. These components do various types of `Neurolinguistic Processsing` (NLP) on the `Doc`. They adds various kinds of information about the `Doc` and its contents as it passes through the pipeline. The types of information depend on the components that you use to make your pipeline. Some of the different components you can use are:
- tagger - a component that emits part-of-speech tags.   
- parser - 
- ner

? > What's the official definition of a component

The central data structures in spaCy 
- The Doc object. What Spacy uses to store the sequences of tokens that represent a text, and all of the annotations and assertions that spacy makes. Most Spacy components use this  
- Language class - Processes a text into a `Doc` object
-  The Vocab - Stores strings, word vectors and lecial attributes 
	- \> What kind of structure is `Token`
	- \> what is the differencae between an attribute the `Token` stores, and one the `Doc` stores?




Linguistic Annotations

What they are: Annotations give insights into a text's grammatical structure. This includes word types (as in noun or verb, and how different words are related to each other.  
Example - 'google' might be used as a noun or as a verb, and as the subject or object of a sentence. 

Getting started with using them

Here's an example of a Linguistic Annotation of the sentence "The cat looked at the man and said MEOW".

The DET det
cat NOUN nsubj
looked VERB ROOT
at ADP prep
the DET det
man NOUN pobj
, PUNCT punct
and CCONJ cc
said VERB conj
MEOW PROPN dobj

Each line is an ouptut that represents the form <token.text> <token.pos_> <token.dep_>
- token.text - The actual text analyzed
- token.pos - `POS` stands for Part-Of-Speech: Assigns word type to tokens like verb or noun. 
- token.dep - `Dep` stands for Dependency parsing. Assigning syntactic dependency labels, describing the relations between individual tokens, like subject or object. 
  
Here's the full code that resulted in the above example. 

```python
import spacy

nlp = spacy.load("en_core_web_sm")
doc = nlp("The cat looked at the man, and said MEOW")
for token in doc:
    print(token.text, token.pos_, token.dep_)
```

\> Is this an example of a spacy pipeline? 
> YES. the pipeline is abstracted away here, but under the hood nlp first tokenizes the the input text into a `Doc`, and then runs that `Doc` through the tokenizer -> tagger -> parser -> ner. 

Here's how we get to the point where we can implement the above in our own project. 

Install spacy in a virtual env (recommended), [or however you want](https://spacy.io/usage)
In the terminal inside a new project folder > 

Create a new virtual environment and pip install the following to it:
- setuptools wheel
- spacy

> [!py] `python`
> ```python
> python -m venv .env
> source .env/bin/activate
> pip install -U pip setuptools wheel
> pip install -U spacy
> ```
> 

Download and install the small nlp model. 
Note: Other models are available, this one chooses speed over accuracy and runs on the CPU. 

\> Spacy Models

\> model naming structure: `language_type_genre_size`

From the terminal whilst inside your virtual environment, download and install a trained pipeline-package

> [!py] `python`
> ```
> python -m spacy download en_core_web_sm
> ```


Import spacy and load the model inside your project
```python
# inside <your-project>\<your-module>.py
import spacy

nlp = spacy.load("en_core_web_sm")
``` 

instantiate a doc (continued from above)

```python
doc = nlp("The cat looked at the man, and said MEOW")
```

\> What is a doc defined as here>
> A `Doc` is a sequence of [`Token`](https://spacy.io/api/token) objects. Access sentences and named entities, export annotations to numpy arrays, losslessly serialize to compressed binary strings. The `Doc` object holds an array of [`TokenC`](https://spacy.io/api/cython-structs#tokenc) structs. The Python-level `Token` and [`Span`](https://spacy.io/api/span) objects are views of this array, i.e. they don’t own the data themselves.

```python
for token in doc:
	print(token.text, token.pos_, token.dep_)
```

Run the code and look at the terminal, you should see the same results as with the initial example!

\> what do the underscores in for example token.pos_ mean? 
Gives you the human-readable text version of the token. This is necessary because Spacy stores everything as a hash under the hood. 

\> What do pretrained pipelines in general let you do? 
> Trained pipelines let you read and return text annotated with linguistic structure. 
> You put in an unstructured string. You get back the same words, but with attributes that describe what they are linguistically. 

\> Is a trained pipeline package and a model the same thing? 
> Not exactly — a trained pipeline package _contains_ one or more models.
> 
> A **model** specifically refers to the statistical weights that let a component make predictions. A **pipeline package** is the full bundle: those weights, plus the pipeline components that use them, plus vocab, config, and metadata.
> 
> When people say "load a model" in casual spaCy usage they usually mean the whole package. The docs use "trained pipeline" precisely to avoid that ambiguity.

\> What do spacy's pretrained pipelines give you?

Out of the box, one call to `nlp(text)` gives you five useful things:

> **1. Tokenization** Splits text into tokens intelligently — "don't" → `do`, `n't`. Not just splitting on spaces.
\> Why is splitting like this intelligent?
> 
> **2. POS Tagging** (`w.pos_`) Every token gets a part-of-speech label: noun, verb, adjective, etc. Useful for filtering — _"give me only the nouns in this document."_
> 
> **3. Dependency Parsing** (`w.dep_`, `w.head`) Labels the grammatical relationship between tokens — subject, object, modifier. Lets you answer _"what is the subject of this verb?"_
> 
> **4. Named Entity Recognition** (`doc.ents`) Finds spans of text that are real-world entities — people, organizations, locations, dates, money. Useful immediately for information extraction.

\> Does this include other things, like blackjack or hookers, or just those above-mentioned 5 categories?
> **5. Sentence Segmentation** (`doc.sents`) Splits a block of text into sentences, using the dependency parse rather than just punctuation.

\> When's that useful?

\> What should I use `en_core_web_sm` for specifically?
- parsing web text - trained on web text
- When you don't need high-accuracy analysis results - low accuracy compared to medium and large models. 
- Low requirements (modernish cpu only) and high speed
  
\> Examples of good tasks for the small web model
> **NER — extracting named entities from support tickets**
> 
> ```python
> nlp = spacy.load("en_core_web_sm")
> doc = nlp("My MacBook Pro order #4821 hasn't arrived in Seattle.")
> for ent in doc.ents:
>     print(ent.text, ent.label_)
> # MacBook Pro   PRODUCT
> # 4821          CARDINAL
> # Seattle       GPE
> ```
> 
> 
> **Dependency parsing — extracting subject-verb-object triples from product reviews**
> 
> ```python
> doc = nlp("The battery drains quickly.")
> for token in doc:
>     print(token.text, token.dep_, token.head.text)
> # battery  nsubj   drains
> # drains   ROOT    drains
> # quickly  advmod  drains
> ```
> 
> You're walking the dependency tree, not measuring distance between concepts.
> 
> **POS tagging — filtering nouns from a corpus for keyword extraction**
> 
> ```python
> doc = nlp("The engineers deployed the updated authentication service.")
> nouns = [t.text for t in doc if t.pos_ == "NOUN"]
> # ['engineers', 'service']
> ```
> 
> **Rule-based matching on top of POS/NER labels** — Find patterns like "VERB + NOUN" or flagging all `ORG` entities in contracts. The rules operate on tags, not vector space.
> 
> The common thread: you're reading off **discrete annotations** the pipeline assigned. The moment you ask "how similar is X to Y," you've left `sm` territory.

 **What `en_core_web_sm` (the model we're starting with) does poorly:** word vectors. The small model has no real vector representations, so semantic similarity (`doc.similarity()`) is nearly useless. For that you need `md` or `lg`.
 
  > \> Why
> > > To make them compact and fast, spaCy’s small [pipeline packages](https://spacy.io/models) (all packages that end in `sm`) **don’t ship with word vectors**, and only include context-sensitive **tensors**. This means you can still use the `similarity()` methods to compare documents, spans and tokens – but the result won’t be as good, and individual tokens won’t have any vectors assigned. So in order to use _real_ word vectors, you need to download a larger pipeline package:
> > > ```
> > > + python -m spacy download en_core_web_lg
> > > ```

---

Working with vectors

Reexamine the text from the initial example again: "The cat looked at the person and said... MEOW"

This time we're analyzing it with vectors and the result will look like 

```
The True 6.167199 False
cat True 7.443447 False
looked True 6.7332373 False
at True 7.3004603 False
the True 6.167199 False
person True 6.082656 False
and True 6.1312947 False
said True 6.7332373 False
... True 5.485588 False
MEOW True 6.59512 False
```

\> Okay so what does that mean. 
This time, each line is an ouptut that represents the form <token.text> <token.has_vector> <token.vector_norm> <token.is_oov
- token.text - The original string of the token that appears in the original text. 
- token.has_vector -  If token.text was in the training corpus for the model's vectors, this returns `True`. 
- Token.vector_norm - The magnitude (length) of the vector in n-dimensional space. Its a scalar (single value). Useful for cosine similarity calculations. 
  \> what are cosine similarity calculations useful for?
- token.oov - `oov` stands for out-of-vocabulary. If the model has no corresponding vector for this token this returns `True`

How to write this in code

Download the medium web model if you haven't yet
From the terminal 
```
python -m spacy download en_core_web_md
```

It's pretty much the same, we're just swapping out the small web model for the medium one
because the small one doesn't have vectors included. 


```python
import spacy

nlp = spacy.load(en_core_web_md)

doc = nlp("The cat looked at the person and said... MEOW")

for token in doc:
	print(token.text, token.has_vector, token.vector_norm, token.oov)

```

You should see the same output as from the example above. 

**Put together:** these four attributes let you quickly audit whether your pipeline has meaningful vector coverage for a given token before you do any similarity or clustering work.

\> What do vectors in general let you do?
> **1. Semantic similarity Score**
> 
> ```python
> doc1 = nlp("dog")
> doc2 = nlp("cat")
> doc3 = nlp("volcano")
> 
> doc1.similarity(doc2)   # ~0.80 — high
> doc1.similarity(doc3)   # ~0.15 — low
> ```
> 
> `.similarity()` is cosine similarity under the hood — angle between vectors, not distance. Range −1 to 1, but in practice 0–1 for word vectors. Called "cosine" because it computes cos(θ) via the dot product formula.
> 
> **2. Finding nearest neighbors** (via `vocab.vectors`)
> 
> ```python
> queries = [doc[0].vector]  # "king"
> most_similar = nlp.vocab.vectors.most_similar(
>     np.array(queries),
>     n=5          # top-n neighbors
> )
> # returns (keys, best_rows, scores)
> # keys are hash IDs → convert with nlp.vocab.strings[key]
> ```
> 
> **3. Vector arithmetic** — the famous analogy trick
> 
> ```python
> # king - man + woman ≈ queen
> result = (
>     nlp.vocab["king"].vector
>   - nlp.vocab["man"].vector
>   + nlp.vocab["woman"].vector
> )
> # then find nearest neighbor to `result`
> ```
> 
> This works because gender, royalty, plurality etc. encode as consistent _directions_ in the space — not magic, just geometry.
> 
> **4. Features for ML models**
> 
> Vectors are the original transfer learning — feed `.vector` as input features to a classifier instead of one-hot encoding. Before transformers, this was the standard way to give a sklearn/PyTorch model semantic awareness.
> 
---

## What a word vector actually is

A word vector is a fixed-length array of floats (e.g. 300 numbers) that encodes a word's meaning as a position in high-dimensional space. Trained by predicting context words (Word2Vec, GloVe, fastText), so words in similar contexts end up geometrically close.

```
"king"  → [0.21, -0.45, 0.87, ... ] # 300 floats
"queen" → [0.19, -0.41, 0.91, ... ] # nearby in space
"table" → [-0.88, 0.12, -0.33, ...]  # far away
```

---
## How spaCy exposes them

Vectors live on three objects — `Token`, `Span`, and `Doc` — all sharing the same `.vector` attribute. That consistency is intentional.

```python
import spacy
nlp = spacy.load("en_core_web_md")  # md or lg — sm has NO vectors

doc = nlp("king queen bank river")

token = doc[0]
token.vector          # np.ndarray, shape (300,)
token.has_vector      # bool — False for OOV words
token.is_oov          # out-of-vocabulary flag
token.vector_norm     # L2 magnitude — used in similarity calc
```
\> Why np.ndarray?
\> What does shape (300, ) mean? 

**Why `md`/`lg` and not `sm`?** The small models strip vectors out to save disk space. They're pipeline-only models — tagging, parsing, NER — no semantic geometry.

**Span and Doc vectors** are the component-wise mean of their token vectors:

```python
doc[0:2].vector   # mean of "king" + "queen" vectors
doc.vector        # mean of all token vectors in doc
```

Averaging is crude but works well enough for short texts.

---
## The key limitation to know upfront

These are **static vectors** — one vector per word type, regardless of context. "Bank" (financial) and "bank" (riverbank) get the **same vector**. This is the core problem that contextual embeddings (ELMo, BERT, spaCy's transformer models via `en_core_web_trf`) solve. With `trf` models, you use `token.tensor` instead, which encodes context — but similarity methods stop working directly because the geometry is no longer comparable across documents.

\> What is the official definition of a static vector? 

---

**Lemmas**

\> What is a Lemma? 
>? Lemma (also called a base form) is the shared root of a word group. Ie the lemma for running, runs, and ran is *run*

**The Spacy Pipeline**
![[pipeline.svg]] 
 **Pipeline Components**
1. Tokenizer - 
2. tagger
3. parser
4. ner
5. lemmatizer
6. textcat
7. custom

> [!quote] quote
> The capabilities of a processing pipeline always depend on the components, their models and how they were trained. For example, a pipeline for named entity recognition needs to include a trained named entity recognizer component with a statistical model and weights that enable it to **make predictions** of entity labels. This is why each pipeline specifies its components and their settings in the [config](https://spacy.io/usage/training#config):
> ```
> [nlp]
> pipeline = ["tok2vec", "tagger", "parser", "ner"]
> ```
> 


 \> Do these pipeline components need to be in this exact order?
 **The tokenizer always goes at the start**
 - There can only be one tokenizer
	 - \> can other components have multiple instances in the same pipeline?
- ALL pipeline components EXCEPT the tokenizer take and return a `Doc` object. There can only be one tokenizer, and *it* takes a string of text and turns it into a `Doc`. So it has to come at the start of the 
> It depends, some components need the data emitted from another particular component. Some are independent.
> Independent components can be in any order typically.
> - Statistical Components
> 	- NER (named entity recognizer) - Doesn't use any features from the tagger or parser. 
> 	- \> Other components?

\> What is a statistical component?
> I'm guessing anything that emits a 'guess' about how like other comparison objects a token or doc is. 

Dependent components may need data emitted from another specific components
\> What are some example of dependent components? 
- Entity Ruler
	- If added *before* a statistical entity recognizer, the entity recognizer will take the existing entities into acccount when making predictions
		- \> What does taking existing entities into account mean in terms of what the Entity Ruler emits?
- EntityLinker - Resolves named entities to knowledge base ID's
	- Should be preeceeded by a component that recognizes entities, like the EntityRecognizer.
		- \> Does the entity recognizer emit entities it recognize?
		  
Some components may share a 'token-to-vector' component like Tok2Vec or Transformer. See [Embedding Layers](https://spacy.io/usage/embeddings-transformers#embedding-layers) 

/> Are the docs saying components that share 
token-to-vector components are depenendent on each other, or are they saying that even indepedent components might share token-to-vector components?

Custom components
- May depdent on annotations set by other components. 
	- ex: Custom lemmaitzer needs part-of-speech tags assigned. So it needs to come after the tagger, which is a component that emits part-of-speech tags.   

\> Are there multiple options for each component
> Yes, each component can be swapped for another. Either built by spacy or the community, or custom. Source [Spacy Docs](https://spacy.io/usage/spacy-101#pipeline-components-order)
> \> Where to get alternative components?
 
\> What do each of these components do? 
\> Is there a place to compare different options for a particular component?

**Embeddings**

\> What are embeddings 
\> How do they differ from vectors?

