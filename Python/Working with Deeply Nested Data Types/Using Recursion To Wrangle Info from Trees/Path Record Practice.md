

> [!example] example
> ![[Nested JSON object - EXAMPLE]]

> [!py] py
> ```python
> find_path(data, "Fox Mulder")
> # → ["departments", 1, "lead"]
> 
> find_path(data, "Orion")
> # → ["departments", 0, "projects", 1, "title"]
> ```

Write a function find_path(data, target_value) that returns the key-path to the first occurrence of a given value as a list of keys/indices.
Python
Things to handle: Recursion through both dicts and lists, tracking the current path, and knowing when to stop.


C491256075
15084027US0