# Day 20 — Excessive Data Exposure: The "Return All" Logic Flaw

## 1. The Core Vulnerability: Over-Trusting the Frontend
- **Actions:** Analyzed how backends rely on frontends to filter data, exposing sensitive fields like passwordHash, isAdmin, or internalNotes in raw API responses.
- **Outcome:** Unlike BOLA (access breaking), Excessive Data Exposure breaks privacy. Successful authentication doesn't justify transmitting sensitive metadata.
- **Strategic Insight:** Proved APIs must enforce field-level filtering independent of UI presentation.

---

## 2. The Developer Trap: Model Dumping
- **Actions:** Identified developers using SELECT * or returning entire database objects, assuming object access equals field access.
- **Outcome:** Key Principle: APIs should emit only minimum data required for specific client-side functions.

---

## 3. BOLA vs. API3 (The Distinction)
- **Actions:** BOLA (API1): Accessing resources you don't own (Horizontal movement). Excessive Data Exposure (API3): Leaking secrets from resources you do own (Vertical leakage).
- **Outcome:** Clarified distinction between unauthorized resource access and authorized resource over-exposure.

---

## 4. The GraphQL Multiplier
- **Actions:** Examined GraphQL's flexibility enabling single queries to map entire object trees without field-level authorization.
- **Outcome:** GraphQL amplifies Excessive Data Exposure by exposing internal attributes never intended for public consumption.

---

## Summary
- **Actions:** Shifted focus from "hacking" to "inspecting," identifying "hidden" fields that provide roadmaps for secondary attacks like Privilege Escalation or BFLA.
