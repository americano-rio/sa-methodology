# Object Diagram

Shows instances (objects) and their attribute values at a specific point in time.

## Approach in Mermaid

Use **`classDiagram`** with `<<object>>` annotation to represent instances.  
Attributes are listed inside the class body as `fieldName = "value"` style entries.

## Key Elements

| Element | Mermaid Syntax | Description |
|---|---|---|
| Object | `class "name : ClassName"` with `<<object>>` | Instance with class type |
| Attribute value | `fieldName : value` inside class body | Attribute assignment |
| Link | `obj1 --> obj2 : label` | Association between instances |
| Composition | `obj1 *-- obj2` | Ownership link |

## Recommended Colors (classDef or style)

| Element | Fill | Stroke | Usage |
|---|---|---|---|
| Entity object | `#dae8fc` | `#6c8ebf` | Domain objects |
| Value object | `#d5e8d4` | `#82b366` | Value types |
| Reference object | `#fff2cc` | `#d6b656` | Referenced entities |
| Collection | `#ffe6cc` | `#d79b00` | Lists/sets |
| Config object | `#e1d5e7` | `#9673a6` | Configuration |

## Example 1

University system snapshot showing students, courses, and their relationships:

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'primaryColor': '#dae8fc', 'primaryBorderColor': '#6c8ebf', 'fontFamily': 'Arial'}}}%%
classDiagram
  class alice["alice : Student"] {
    <<object>>
    studentId : S2024001
    name : Alice Johnson
    gpa : 3.8
    major : Computer Science
  }

  class bob["bob : Student"] {
    <<object>>
    studentId : S2024002
    name : Bob Smith
    gpa : 3.5
    major : Mathematics
  }

  class cs101["cs101 : Course"] {
    <<object>>
    courseId : CS-101
    title : Intro to Programming
    credits : 3
    semester : Fall 2024
  }

  class math201["math201 : Course"] {
    <<object>>
    courseId : MATH-201
    title : Linear Algebra
    credits : 4
    semester : Fall 2024
  }

  class prof1["prof1 : Professor"] {
    <<object>>
    staffId : P1001
    name : Dr. Carol Lee
    department : Computer Science
  }

  class addr1["addr1 : Address"] {
    <<object>>
    street : 123 Campus Dr
    city : Springfield
    zip : 62701
  }

  alice --> cs101   : enrolled
  alice --> math201 : enrolled
  bob   --> cs101   : enrolled
  bob   --> math201 : enrolled
  prof1 --> cs101   : teaches
  alice --> addr1   : lives at
  bob   --> addr1   : lives at
```

## Example 2

E-commerce order snapshot with composed objects:

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'primaryColor': '#dae8fc', 'primaryBorderColor': '#6c8ebf', 'fontFamily': 'Arial'}}}%%
classDiagram
  class order42["order42 : Order"] {
    <<object>>
    orderId : ORD-042
    status : Shipped
    total : $149.97
  }

  class item1["item1 : LineItem"] {
    <<object>>
    product : Wireless Mouse
    qty : 2
    price : $24.99
  }

  class item2["item2 : LineItem"] {
    <<object>>
    product : USB-C Hub
    qty : 1
    price : $99.99
  }

  class ship["ship : ShippingInfo"] {
    <<object>>
    carrier : FedEx
    tracking : FX123456
    eta : 2024-12-20
  }

  class cust7["cust7 : Customer"] {
    <<object>>
    name : Jane Doe
    email : jane@example.com
  }

  order42 *-- item1
  order42 *-- item2
  order42 --> ship   : shipped via
  order42 --> cust7  : placed by
```
