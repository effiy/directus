---
title: Internal RunAst Documentation
tags: [directus, ast, sql, query-engine, database, knex]
category: database
created: 2020-05-15
updated: 2026-10-20
source: external
type: index
status: stable
lifecycle: active
---

# Internal RunAst Documentation

---

## 💡 概述

The `runAst()` function is a crucial and complex part of Directus. When a request is made through REST, GQL or WebSockets,
it is internally turned into an AST (Abstract Syntax Tree) before being translated into one or more SQL requests sent to
the database.

On the root level of an AST is the root node, which references a list of child nodes. Each child node can be either a
primitive field (FieldNode), a relational field (M2ONode, O2MNode, A2MNode), or a function field (FunctionFieldNode).
Relational fields can also have additional children and nested queries.

---

## ⚡ 功能

- Translates an AST into executable SQL queries via Knex
- Supports primitive field selection through FieldNode leaf nodes
- Handles relational data fetching through M2ONode, O2MNode, and A2MNode with recursive `runAst()` calls
- Supports nested relational queries with automatic data injection into parent levels
- Processes function field nodes for `count(o2m_relation)` aggregations
- Applies query parameters (filters, sorts, etc.) via `getDBQuery()` and `applyQuery()`
- Manages table joins with an alias map to reuse existing joins for nested field operations

---

## 📊 指标

「待补充」

---

## 🏛️ 设计

### AST Node Types

- **FieldNode**: Describes a primitive field and is a leaf node of the AST
- **M2ONode / O2MNode / A2MNode**: Relational fields that recursively call `runAst()` and inject resulting data into the parent level
- **FunctionFieldNode**: Inserted into the AST only for `count(o2m_relation)`; all other functions are kept as FieldNodes with the function name embedded

### RunAst Pipeline

`runAst()` translates the AST into an executable SQL request, requests the appropriate data from the database, and returns a single item or a list of items as the result.

### getDBQuery

The RootNode and relational fields each have their own query parameters. These are applied to the SQL query by `getDBQuery()`, which internally uses `applyQuery()`. Inner queries are used when necessary, and `applyQuery()` may be called multiple times in different places depending on the query structure.

### applyQuery

`applyQuery()` modifies the SQL request to apply filters, sorts, and all other query parameter variants. For nested field operations, `addJoin()` joins the required tables and registers them in the alias map so that subsequent references to the same collection reuse the existing join.

---

## 📁 目录

「待补充」

---

## 🔗 资源

「待补充」

---

## 🚀 快速开始

「待补充」

---

## ⚙️ 技术栈

- Directus API
- Knex (SQL query builder)
- TypeScript