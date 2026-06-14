Source: [Claude - Visualizing token similarity for technical term classification](https://claude.ai/share/8f4a3dc5-68fb-442a-957a-b7ab1480ca5b)

Context: I'm using an NLP pipeline in spacy to classify terms as Technical Terms (in IT) vs non-technical. Ex> Technical -> Disk Space, Non-Technical -> Daffodil

When to use this: Do this to see how accurate your model to help decide whether to do additional model training. 

\>? When a category is this broad, and so many words from plain english have been adapted or re-purposed as technical terms in different domains, Is it better to do training on sub categories within the broader classification, or group all training together? For example, cat: 
- cat -> Small furry cute pet that meows
- vs cat -> bash command to output text to terminal.  



The below snippet will give a `pandas` DataFrame with tokens on both axes. Symmetrical, diagonal = 1.0. This shows not just pairwise distance but cluster membership
\>? What is pairwise distance?
> *How far terms are from each other?* 
\>? What is cluster membership? 
> - *Pretty sure it's seeing terms clustered together, which means that they are closely related to each other. The more closely grouped, the more closely related.*
> - *You don't necessarily care how close individual related terms are to each other, jsut that they are closer together to each other than non-related terms, so you can see how whether the model is making an accurate distinction.*

\>? What's a dataframe? 
\> What does it mean for tokens to be on both axes? 
\>? What does the diagonal being on 1.0 mean? 
> *I'm guessing that the tokens that fall on the 1.0 diagonal are ones that are perfectly related to each other*

\>? How is relationship determined in Spacy? 
	\>? Does that differ from how relationships are determined with other nlp or llm models?
> > > *I'm Guessing that this has to do with static vectors vs whatever the non-static ones are. Static vectors are vectors that encode only one relationship, how often words occur next to each other (colocation)*

**Example: *shows pairwise distance between tokens***
```python
import numpy as np
from sklearn.decomposition import PCA
import matplotlib.pyplot as plt

# Get vectors from spaCy tokens
vectors = np.array([nlp(item["text"])[0].vector for item in labeled])
texts   = [item["text"]  for item in labeled]
colors  = ['blue' if item["label"] == "TECHNICAL" else 'red' for item in labeled]

# PCA: reduce from 300 dimensions (md model vector size) down to 2
pca = PCA(n_components=2)
reduced = pca.fit_transform(vectors)  # shape: (n_tokens, 2)

# Plot
fig, ax = plt.subplots()
for i, text in enumerate(texts):
    ax.scatter(reduced[i, 0], reduced[i, 1], color=colors[i])
    ax.annotate(text, (reduced[i, 0], reduced[i, 1]))
plt.show()
```

##### Imports
> [!py] Python `snippet`
> ~~~python
> import numpy as np
> from sklearn.decomposition import PCA
> import matplotlib.pylot as plt
> ~~~
> > [!example]- **Decomposition**
> > - `import` - A Python statement that loads code from another file or module into your current program so you can use its functions, classes, and variables.
> > - `numpy` - A Python library that provides fast, efficient tools for working with arrays and mathematical operations on large datasets.
> > - `as` - Renames modules or functions to shorter and/or meaningful names in your code. 
> > - `np` - Standard alias for numpy, but you can call it something else.
> > - `from` - Python keyword used in import statements to specify which objects (functions, classes, variables) specifically you want to import from a particular module or package. 
> > - `keyword` - Definition
> > - `keyword` - Definition
> > - `keyword` - Definition
> > - 
>
> >[!tip]-  **Recomposition**
> >- `keyword` + `keyword` + `keyword` - Definition
> 
> **Additional Thoughts**
> - 
> 



