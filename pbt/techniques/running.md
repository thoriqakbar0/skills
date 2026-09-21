# Case counts

Run each property with a high, explicit case count when the execution budget permits. Thousands of cases often reach paths that the library default misses, especially for cheap generators.

Keep every run bounded and reproducible with a case limit, time limit, seed, or framework replay token. If the suite is slow, reserve higher counts for properties with rich input spaces instead of lowering every property. Report exhaustion, timeout, and unreproducible execution as gaps.
