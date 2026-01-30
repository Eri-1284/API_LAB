# Day 8 — Practical GraphQL Discovery & Introspection

## 1. Endpoint Identification
- **Actions:** Scanned for standard GraphQL endpoints such as `/api/graphql`, `/v1/graphql`.
- **Actions:** Reviewed HTTP history and JavaScript files to pinpoint the API entry point.
- **Outcome:** Successfully located the GraphQL interface, ready for deeper technical analysis.

---

## 2. Experimental Query Execution
- **Actions:** Crafted targeted queries to fetch specific data fields and nested objects from a public playground.
- **Actions:** Deliberately requested "extra" fields to test data exposure boundaries.
- **Outcome:** Confirmed the API returns all requested fields unrestricted, revealing potential Excessive Data Exposure.

---

## 3. Introspection Attack Simulation
- **Actions:** Executed a `__schema` query to extract the complete API structure and object types.
- **Actions:** Analyzed the returned schema for sensitive or administrative mutations.
- **Outcome:** Obtained a complete, accurate map of API capabilities without needing official documentation or guesswork.

---

## Summary
- **Actions:** Conducted manual GraphQL querying and full introspection testing.
- **Outcome:** Demonstrated how GraphQL's flexibility can be weaponized through introspection to expose the entire database-to-API mapping.
-
