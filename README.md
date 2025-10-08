# Personal Transaction Tracker

A full-stack, containerized personal finance management app built with React, Node.js/Express, TypeScript, PostgreSQL, and Docker Compose.

---

## Table of Contents
- [Overview](#overview)
- [Features](#features)
- [System Architecture](#system-architecture)
- [Data Model](#data-model)

---

## Overview
This project is a robust, user-friendly platform for tracking personal finances. It replaces complex spreadsheets with a modern dashboard, secure authentication, and insightful analytics. All components are containerized for easy deployment.

**Motivation:**
- Consolidate financial tracking into a single, efficient platform.
- Learn and apply best practices in full-stack development, security, and DevOps.

---

## Features
- **User Authentication:** Secure signup/login with JWT and password hashing.
- **User-specific Data:** Each user's data is isolated and protected.
- **Expense & Income Tracking:** Add, view, and categorize transactions.
- **Responsive Dashboard:** Modern UI with charts (Recharts, MUI) and mobile-friendly design.
- **REST API:** Clean separation between frontend and backend.
- **Dockerized:** One-command setup for local or production.

---

## System Architecture

```mermaid
graph TD
    subgraph "User's Browser"
        A["React Frontend"]
    end

    subgraph "Docker Environment"
        B["Frontend Container\nNginx"]
        C["Backend Container\nNode.js/Express"]
        D["Database Container\nPostgreSQL"]
        E["Migration Container"]
    end

    subgraph "Backend Logic"
        F["Auth Routes"]
        G["Feed Routes (Protected)"]
        H["Auth Controller"]
        I["Feed Controller"]
        J["Auth Service"]
        K["Feed Service"]
        L["is-auth Middleware"]
    end
    
    subgraph "Database Schema"
        M["users Table"]
        N["transactions Table"]
        O["SQL Functions"]
        P["expense_categories Table"]
        Q["expense_types Table"]
    end

    A -- "HTTP Request" --> B
    B -- "API Calls" --> C

    C --> F & G

    F --> H
    G -- "Verifies JWT via" --> L
    L --> I
    
    H --> J
    I --> K
    
    J --> M
    K --> N & O
    
    E -- "Applies Schema to" --> D
    D --> M & N & O & P & Q

    style A fill:#e3f2fd,stroke:#333,stroke-width:2px
    style B fill:#e8f5e9,stroke:#333,stroke-width:2px
    style C fill:#fff3e0,stroke:#333,stroke-width:2px
    style D fill:#f3e5f5,stroke:#333,stroke-width:2px
    style E fill:#efebe9,stroke:#333,stroke-width:2px
    
    style F fill:#ffebee,stroke:#333,stroke-width:2px
    style G fill:#ffebee,stroke:#333,stroke-width:2px
    style H fill:#fffde7,stroke:#333,stroke-width:2px
    style I fill:#fffde7,stroke:#333,stroke-width:2px
    style J fill:#e0f7fa,stroke:#333,stroke-width:2px
    style K fill:#e0f7fa,stroke:#333,stroke-width:2px
    style L fill:#fce4ec,stroke:#333,stroke-width:2px
    
    style M fill:#f1f8e9,stroke:#333,stroke-width:2px
    style N fill:#f1f8e9,stroke:#333,stroke-width:2px
    style O fill:#f1f8e9,stroke:#333,stroke-width:2px
    style P fill:#f1f8e9,stroke:#333,stroke-width:2px
    style Q fill:#f1f8e9,stroke:#333,stroke-width:2px
```

---

## Data Model

### Entity-Relationship Diagram

```mermaid
erDiagram
    USERS {
        int id PK
        varchar email
        varchar password_hash
        timestamp created_at
    }
    TRANSACTIONS {
        int id PK
        date date
        decimal amount
        int type_id FK
        int category_id FK
        int user_id FK
    }
    EXPENSE_CATEGORIES {
        int id PK
        varchar category_name
    }
    EXPENSE_TYPES {
        int id PK
        varchar type_name
        int category_id FK
    }
    
    USERS ||--o{ TRANSACTIONS : "has"
    EXPENSE_CATEGORIES ||--o{ EXPENSE_TYPES : "contains"
    EXPENSE_CATEGORIES ||--o{ TRANSACTIONS : "categorizes"
    EXPENSE_TYPES ||--o{ TRANSACTIONS : "typed by"
    EXPENSE_TYPES }o--|| EXPENSE_CATEGORIES : "belongs to"
    TRANSACTIONS }o--|| USERS : "belongs to"
    TRANSACTIONS }o--|| EXPENSE_CATEGORIES : "belongs to"
    TRANSACTIONS }o--|| EXPENSE_TYPES : "belongs to"
```

**Table Descriptions:**
- **users:** Stores user credentials and metadata. Each user has a unique email and securely hashed password.
- **transactions:** Records all income/expenses. Each transaction is linked to a user, a category, and a type.
- **expense_categories:** High-level groupings (e.g., Housing, Personal Running Costs).
- **expense_types:** Specific types within a category (e.g., Restaurants, Groceries).

---


