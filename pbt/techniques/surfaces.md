# Choosing what to test

Before writing a property, list the library's risky public surfaces: parsers and decoders, arithmetic and boundary logic, construction and configuration parameters, optimized or unsafe paths, stateful APIs, assertions or documented preconditions, and operations with documented complexity bounds.

Build the list from the project's public API index, including top-level documentation, exports, re-exports, and package indexes. Enumerate mechanically first, then rank by risk. Ranking sets the work order. It never removes an entry.

Entry points come in families: sibling methods of the same interface, the same operation on each supported type, operations or accessors that take the same input shape, and each advertised or default type configuration. List every member on its own line. A property on one member says nothing about its siblings.

When a generator cannot draw a structural shape, such as duplicate keys or repeated types, enumerate that property variant or record why it is out of scope.

Keep the list as a coverage checklist. Before stopping, give every entry a property or a stated reason.
