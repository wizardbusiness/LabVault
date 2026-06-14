### Task

- **Task 2 — Conditional extraction**
- **Return a list of all project titles where status == "active".**
- **Expected: ["Nighthawk", "Blueshift"]**
- **Things to handle: Projects are buried two levels inside a list of dicts. You need to recognise when you've hit a project node, not just traverse blindly.**

> [!example] **Data - Nested Dictionary**
> ![[Nested JSON object - EXAMPLE]]

  
> [!py]- py
> ```python
> def collect_active(root):
> 	acc = []
> 	if isinstance(root, list):
> 		for el in root:
> 			acc.extend(collect_active(el))
> 	elif isinstance(root, dict):
> 		if "status" in root and "title" in root:
> 			if root["status"] == "active":
> 	acc.append(root["title"])
> 	for key in root:
> 		acc.extend(collect_active(root[key]))
> 	return acc
> ```
> 
> > [!example]- **Decomposition**
> > - `keyword` - Definition
>
> >[!tip]-  **Recomposition**
> >- `keyword` + `keyword` + `keyword` - Definition
> 
> **Additional Thoughts**
> 
> **Places I got hungup**
> - when iterating over key in root passed just key instead of root\[key] to recursive call. I forgot  that for in gives you the key, not the value by default
> 	  - Wait does `for key, val in somedict` give you the key *and* value?
> 		  - **Not it does not, gives a ValueError, too many values to unpack**
>   - Was trying to check that root had a "title" attribute with `hasattr(root, "title")` and was confused why type check was failing.
> 	  - It turns out that `hasattr` is only for checking `object` attributes. Keys on dicts are *not* attributes, they are simply keys.
> 	   - To check for a key in a dict use `if "<key>" in <somedict>`. 
> 		   - Does that get costly with big dicts? 
> 	   - Tried to dot access "title" key - `root.title`
> 		   - Dicts in python *do not* support dot access
> 		     
>  







