
### Task

- **Collect every string value that appears anywhere in the structure into a flat list. Don't hardcode any keys — the function should work on any similar object.**
- **Expected output includes "Axiom Labs", "Portland", "Dana Scully", "Nighthawk", "2024-11-01", etc.**
- **Things to handle: None values, strings nested inside lists, strings nested inside dicts.**


> [!example] **Data - Nested Dictionary**
> ![[Nested JSON object - EXAMPLE]]
### Method 1
> [!py] Python `snippet`
> ~~~python
> def get_strs(root):
> 	acc = []
> 	if isinstance(root, str):
> 		acc.append(root)
> 	elif isinstance(root, list):
> 		for el in root:
> 			acc.extend(get_strs(el))
> 	elif isinstance(root, dict):
> 		for key in root:
> 		acc.extend(get_strs(root[key]))
> 	return acc
> ~~~
> > [!example]- **Decomposition**
> > - `keyword` - Definition
>
> >[!tip]-  **Recomposition**
> >- `keyword` + `keyword` + `keyword` - Definition
> 
> **Additional Thoughts**
> - 


> [!py] Python`snippet`
> ~~~python
> def collect_strings(node):
> 	acc = []
> 	if isinstance(node, str):
> 		acc.append(node)	return acc
> 	elif isinstance(node, list):
> 		for item in node:
> 			acc.extend(collect_strings(item))
> 		return acc
> 	elif not isinstance(node, dict):
> 		return acc
> 	for child in node:
> 		acc.extend(collect_strings(node[child]))
> 	return acc
> ~~~
> 
> > [!example]- **Decomposition**
> > - `keyword` - Definition
>
> >[!tip]-  **Recomposition**
> >- `keyword` + `keyword` + `keyword` - Definition
> 
> **Additional Thoughts**
> - 
> 

