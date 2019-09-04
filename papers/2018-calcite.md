# Begoli, E., Camacho-Rodríguez, J., Hyde, J., Mior, M. J., & Lemire, D. (2018). Apache calcite: A foundational framework for optimized query processing over heterogeneous data sources. Proceedings of the ACM SIGMOD International Conference on Management of Data, 221–230. https://doi.org/10.1145/3183713.3190662

This paper presents **Calcite** under Apache Foundation, an extensible framework for query processing, optimization and query language support to different data processing systems. It consists of a query optimizer with rule-based optimization, a query processor supporting multiple query languages, an adapter architecture to support various data models and stores.

The authors first introduces the motivation behind Calcite. Nowadays, people have been developing specialized data processing systems. The "one size fits all" paradigm (for SQL) has gone away. Although these systems have different logics, most still need to support some common features influenced by SQL. To save efforts, it is important to have a unifying framework to solve this problem. Specifically, Calcite helps to address the implementation of 3 components, query execution engine, query optimizer and query languages. However, Calcite leaves other components (such as the storage layer) to the logics of specific systems. Notice that Calcite has a very similar vision to Volcano, a framework proposed in the 90s.

The paper then compares Calcite with other similar frameworks. Compared to them, Calcite can be used as a standalone query execution engine & optimizer that can be used with different storage and processing backends. Its optimizer uses a bottom-up DP-based planning algorithm based on Volcano (and also extended to support multi-stage optimizations as in Orca).

Then, the architecture of Calcite is presented. It contains the following components:

- A query parser and validator: translate a given SQL query into a tree of relational operators.
- An SQL query optimizer: Calcite can convert the optimized result back to an SQL query so that it works as a pluggable, stand-alone system (because the original system can further process based on Calcite's optimization result). Calcite can even help to optimize queries parsed by other systems.

Calcite supports all relational algebra operators, as well as advanced operators such as the window function. It uses _**traits**_ to describe the physical properties associated with an operator. Some common traits include _ordering_, _grouping_ and _partitioning_. There is an important trait called _calling convention_, which represents the data processing system in which the expression will be executed. This helps Calcite to transparently treat queries whose execution would span over different engines. This approach makes it possible to put different systems together and implement a huge federated database system.

To enable the flexibility to support multiple storage backends, Calcite introduces the concept of _**adapters**_. It consists of 3 components: _model_ (specifying the physical properties of the data source), schema (the format and layouts of the data found in the model) and schema factory (to acquire meta information and instantiate the schemas). In addition, the adapter can define a set of rules, which will be added to the query optimizer. An adapter has to minimally implement the table scan operator on the physical data source. The adapters can implement an enumerable interface to allow Calcite to implement more operators. The adapters are free to implement more data source-specific rules. For example, if the underlying data source supports an efficient filtering, the adapter could feed this rule to Calcite's optimizer and this could be used potentially if the associated cost is lower.

The query optimizer in Calcite works by repeatedly applying _**planner rules**_ to a relational expression. The driver algorithm performs cost-based pruning to guide this process. Each rule first checks whether the operator tree matches a certain pattern, and applies the transformation if so. The optimizer always tries to push down the operators (such as _filter_) if possible because it could be more efficient to execute them in the storage backends. The driver algorithm will fetch information from _**metadata providers**_ to calcute the cost of a given plan. The adapters can of course provide extra metadata to the optimizer. The driver algorithm is called a _**planner engine**_. There are two built-in planner engines: a cost-based planner engine (uses DP to do cost-based pruning) and an exhaustive planner engine (apply transformation rules until the plan cannot be further modified). The query optimizer can also be used with _**materialized views**_.