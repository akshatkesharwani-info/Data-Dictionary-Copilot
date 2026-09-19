# Data Dictionary Copilot

A natural-language assistant for exploring a database schema -- so analysts stop pinging data
engineering to ask "which table has customer revenue" for the hundredth time.

## Problem
Analysts constantly interrupt the data engineering team with schema questions. A self-serve
copilot that already knows the schema eliminates most of these interruptions.

## What It Does
- Pulls table/column/foreign-key metadata straight from `INFORMATION_SCHEMA` and `sys.foreign_keys`
- Turns each table into one embedded document (not one per column) for better retrieval
- Answers plain-English schema questions and gives the exact join path when asked
- Gracefully falls back to a small sample schema if it can't reach a SQL Server instance

## Real Results (from an actual run, AdventureWorksDW2019-style schema)
- Q: *"How do I join FactInternetSales to DimCustomer?"* -> correctly answered the exact FK path:
  `FactInternetSales.CustomerKey -> DimCustomer.CustomerKey`, with working SQL
- Q: *"What column stores the order date in sales facts?"* -> correctly identified `OrderDate`
- In this run, Colab couldn't reach a local SQL Server, so it automatically used the fallback
  sample schema -- and still answered both questions correctly

## Tech Stack
Python, pyodbc, MS SQL Server 2022, ChromaDB, Sentence-Transformers, Cerebras/Groq free tier

## How to Run
Point it at a real SQL Server instance (e.g. AdventureWorksDW2019 via SSMS) for full results, or
just run it as-is in Colab -- it'll fall back to a small sample schema automatically.
