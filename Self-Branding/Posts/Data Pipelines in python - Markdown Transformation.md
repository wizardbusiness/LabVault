Tools: 
- Mistletoe
- sys
- spacy

Stage 1: *ingest*
- load directory md files
- Convert MD to AST with mistletoe
  
Stage 2: *transform*
A. Extract target content from ast
- traverse md ast
	- What to find: 
		- Keywords + defs in code decomposition
			- This should target a fuzzy pattern, when dealing with documentation leave room for human inconsistency.
	- Abstraction
		- todo: Load in a keyword sample, and infer pattern from that. 
- extract target content into record objects
	- enclosing ast token
	- keyword
	- keyword def
	
B. Pass to nlp for analysis
- Tokenize keyword and def
- 

