# Apache Calcite

This article summarizes my findings when I first begin to read Calcite's codebase. The codebase is written in Java and is relatively well written in terms of software engineering practices.

## SQL Query & Operators

Under `org.apache.calcite.sql` package, we can find the defitions related to SQL query and the oprators used in it. In Calcite, each SQL query is represented as a tree of operators (after being parsed by the `org.apache.calcite.sql.parser` module).

Each tree node _(including the root node)_ is an `SqlNode`. `SqlNode` is an abstract class, of which using `unparse` method could help to convert an operator tree back to an SQL query in its string format. There are 3 types of `SqlNode`s:

- `SqlOperator`: a type of node in an operator tree (but not a node in the tree because `SqlOperator` is also an abstract class).
	- In practice, we would use `SqlCall` rather than `SqlOperator` though;
	- `SqlKind` is an `enum` that further classifies it into: `QUERY`, `DML` and `DDL`.
- `SqlLiteral`: a constant value (which means _immutable_), like `NULL`, boolean values, numbers and date & time values.
- `SqlIdentifier`: the identifier _(or the name)_ of a database, table, column, etc. It could be compound though.

## Query Optimizer

All related code should be udner `org.apache.calcite.plan` package. The two built-in optimizers are defined at `org.apache.calcite.plan.hep` and `org.apache.calcite.plan.volcano` respectively. They will use the various transformation rules defined in `org.apache.calcite.rel.rules` package.
