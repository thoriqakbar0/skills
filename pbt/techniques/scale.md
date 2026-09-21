# The scale probe

When size, recursion, lifecycle, or complexity can hide a defect, run one dedicated scale probe outside the generators. Build a very large instance, such as hundreds of thousands of elements or deeply nested input, within an explicit resource budget.

Exercise every supported operation and derived behavior on that instance, including formatting or debugging, copying or cloning, cleanup, iteration, comparison, hashing, and serialization when the API provides them.

Look for per-element recursion, accidental quadratic work, and retained memory. The scale probe complements generated cases. It does not replace them.
