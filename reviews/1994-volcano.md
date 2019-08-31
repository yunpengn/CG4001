# Graefe, G. (1994). Volcano — An Extensible and Parallel Query Evaluation System. IEEE Transactions on Knowledge and Data Engineering, 6(1), 120–135. https://doi.org/10.1109/69.273032

This paper introduces the idea of Volcano, a query evaluation system. With its design, it becomes the first query execution engine that combines extensibility and parallelism. Also, the authors emphasize that Volcano is more to provide a testbed for database research rather than a production-ready system (as it is not friendly to end-users).

There are 6 objectives that Volcano tries to follow:

- Modular and extensible to enable future research;
- Simple for student to use and research;
- Not too simple as well, realistic and similar to commercial database systems for students to really learn;
- No presumption of data model;
- To free new users from the intricacies of parallelism, but allow experimentation with parallel query processing;
- Make parallelism and data manipulation independent from the operator set and semantics.

To achieve the above goals, Volcano adopts a major design principle from operating system research: the concept of _**policies**_. The separation of mechanism and policies will help to achieve modularity and extensibility. Volcano has been influenced and based on these previous works:

- **WiSS (Wisconsin Storage System):** a record-oriented file system with heap files, B-tree & hash indexes, buffering and scans with predicates.
- **GAMMA:** a database system running on general-purpose CPUs as a backend to a UNIX host machine. GAMMA uses WiSS to access its local disk devices.
- **EXODUS:** an extensible database system with the tool-kit approach.

Volcano consists of two layers, the file system layer and the query processing layer.
