![[Nested JSON object - EXAMPLE]]

Sum every numeric value (both int and float) found anywhere in the structure.
Expected: one number — the total of 2011, 4_200_000.00, 2_000_000, 5_000_000, 12_000_000, 42, 300_000, 150_000, 11, 80_000, 45_000, 9, founded year... all of it.
Things to handle: bool is a subclass of int in Python — True and False will match isinstance(x, int) unless you explicitly exclude them. This one will bite you if you're not watching.