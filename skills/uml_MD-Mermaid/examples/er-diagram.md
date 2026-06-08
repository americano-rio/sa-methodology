# ER Diagram (Entity-Relationship)

Shows database entities, attributes, and their relationships.  
This is a Mermaid-native diagram type with no PlantUML equivalent.

## Key Elements

- **Entity**: `ENTITY_NAME { ... }` — table/entity rectangle
- **Attribute**: `type name PK/FK/UK` — column inside entity
- **Relationship**: `A ||--o{ B : "label"` — connecting line
- **Relationship direction**: always left to right

## Relationship Cardinality

| Symbol | Meaning |
|---|---|
| `\|o` | Zero or one |
| `\|\|` | Exactly one |
| `o{` | Zero or many |
| `\|{` | One or many |

Combine left and right sides:
| Syntax | Meaning |
|---|---|
| `\|\|--\|\|` | exactly one — exactly one |
| `\|\|--o{` | exactly one — zero or many |
| `\|\|--\|{` | exactly one — one or many |
| `o\|--o{` | zero or one — zero or many |

## Attribute Types

Common type names: `int`, `string`, `varchar`, `boolean`, `decimal`, `datetime`, `uuid`, `json`

Modifiers: `PK` (primary key), `FK` (foreign key), `UK` (unique key)

## Example 1

E-commerce database schema:

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'fontFamily': 'Arial', 'primaryColor': '#dae8fc', 'primaryBorderColor': '#6c8ebf'}}}%%
erDiagram
  CUSTOMER {
    uuid   id        PK
    string email     UK
    string firstName
    string lastName
    string phone
  }

  ADDRESS {
    uuid   id         PK
    uuid   customerId FK
    string street
    string city
    string zip
    string country
    boolean isDefault
  }

  ORDER {
    uuid     id         PK
    uuid     customerId FK
    string   status
    decimal  total
    datetime createdAt
  }

  ORDER_ITEM {
    uuid    id        PK
    uuid    orderId   FK
    uuid    productId FK
    int     quantity
    decimal unitPrice
  }

  PRODUCT {
    uuid    id       PK
    string  sku      UK
    string  name
    decimal price
    int     stock
    uuid    categoryId FK
  }

  CATEGORY {
    uuid   id       PK
    string name     UK
    uuid   parentId FK
  }

  PAYMENT {
    uuid     id      PK
    uuid     orderId FK
    string   method
    string   status
    decimal  amount
    datetime paidAt
  }

  CUSTOMER   ||--o{ ADDRESS    : "has"
  CUSTOMER   ||--o{ ORDER      : "places"
  ORDER      ||--|{ ORDER_ITEM : "contains"
  ORDER      ||--|| PAYMENT    : "paid via"
  PRODUCT    ||--o{ ORDER_ITEM : "appears in"
  CATEGORY   ||--o{ PRODUCT    : "groups"
  CATEGORY   o|--o{ CATEGORY   : "parent of"
```

## Example 2

Blog platform schema:

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'fontFamily': 'Arial'}}}%%
erDiagram
  USER {
    uuid   id       PK
    string username UK
    string email    UK
    string role
  }

  POST {
    uuid     id        PK
    uuid     authorId  FK
    string   title
    string   slug      UK
    string   status
    datetime publishedAt
  }

  TAG {
    uuid   id   PK
    string name UK
  }

  POST_TAG {
    uuid postId FK
    uuid tagId  FK
  }

  COMMENT {
    uuid     id       PK
    uuid     postId   FK
    uuid     authorId FK
    string   body
    datetime createdAt
  }

  MEDIA {
    uuid   id     PK
    uuid   postId FK
    string url
    string type
  }

  USER     ||--o{ POST    : "writes"
  USER     ||--o{ COMMENT : "writes"
  POST     ||--o{ COMMENT : "has"
  POST     ||--o{ MEDIA   : "contains"
  POST     }o--o{ TAG     : "tagged with"
  POST_TAG }o--|| POST    : ""
  POST_TAG }o--|| TAG     : ""
```
