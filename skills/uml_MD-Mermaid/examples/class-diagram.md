# Class Diagram

Shows class structure with attributes, methods, and relationships between classes.

## Key Elements

- **Class**: `class ClassName { }` — define attributes and methods inside braces
- **Abstract class**: add `<<abstract>>` annotation inside class
- **Interface**: add `<<interface>>` annotation inside class
- **Enumeration**: add `<<enumeration>>` annotation inside class
- **Visibility**: `+` public, `#` protected, `-` private, `~` package
- **Static member**: `$` suffix on member
- **Abstract method**: `*` suffix on method

## Relationships

| Relationship | Syntax | Description |
|---|---|---|
| Inheritance | `<\|--` | Hollow triangle (extends) |
| Realization | `..\|>` | Dashed + hollow triangle (implements) |
| Association | `-->` | Open arrow |
| Aggregation | `o--` | Hollow diamond (has-a) |
| Composition | `*--` | Filled diamond (owns) |
| Dependency | `..>` | Dashed open arrow (uses) |

## Recommended Colors (via classDef)

| Element | Fill | Stroke | Usage |
|---|---|---|---|
| Interface | `#d5e8d4` | `#82b366` | Contract definitions |
| Abstract class | `#f8cecc` | `#b85450` | Base classes |
| Concrete class | `#dae8fc` | `#6c8ebf` | Regular classes |
| Enum | `#fff2cc` | `#d6b656` | Enumerations |
| Subclass | `#ffe6cc` | `#d79b00` | Derived classes |
| Utility | `#e1d5e7` | `#9673a6` | Utility/helper classes |

## Example 1

Zoo management system with interfaces, abstract class, enums, and various relationships:

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'primaryColor': '#dae8fc', 'primaryBorderColor': '#6c8ebf', 'lineColor': '#333333', 'fontFamily': 'Arial'}}}%%
classDiagram
  class IAnimal {
    <<interface>>
    +makeSound() void
    +move() void
  }

  class Animal {
    <<abstract>>
    #name String
    #age int
    -animalCount$ int
    +makeSound()* void
  }

  class DietType {
    <<enumeration>>
    HERBIVORE
    CARNIVORE
    OMNIVORE
  }

  class Zoo {
    -name String
    -location String
    +addAnimal(a Animal) void
    +getAnimalCount() int
  }

  class Cage {
    -cageId int
    -capacity int
    +clean() void
  }

  class Dog {
    -breed String
    -isVaccinated boolean
    +makeSound() void
  }

  class Cat {
    -indoor boolean
    -livesRemaining int
    +makeSound() void
  }

  Animal ..|> IAnimal
  Animal ..> DietType : uses
  Dog --|> Animal
  Cat --|> Animal
  Zoo "1" *-- "1..*" Cage
  Zoo "1" o-- "0..*" Animal
  Cage "1" --> "0..*" Dog : houses

  note for Animal "Abstract class implements IAnimal interface"
```

## Example 2

Observer pattern with generic types and dependency relationships:

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'primaryColor': '#dae8fc', 'primaryBorderColor': '#6c8ebf', 'lineColor': '#333333', 'fontFamily': 'Arial'}}}%%
classDiagram
  class Observer~T~ {
    <<interface>>
    +update(event T) void
  }

  class Subject~T~ {
    <<interface>>
    +subscribe(o Observer~T~) void
    +unsubscribe(o Observer~T~) void
    +notify(event T) void
  }

  class EventBus~T~ {
    -observers List~Observer~T~~
    +subscribe(o Observer~T~) void
    +unsubscribe(o Observer~T~) void
    +notify(event T) void
  }

  class PriceAlert {
    -threshold double
    +update(event PriceChange) void
  }

  class Dashboard {
    -charts List~Chart~
    +update(event PriceChange) void
  }

  class StockService {
    -ticker String
    -currentPrice double
    +fetchPrice() double
  }

  EventBus ..|> Subject
  PriceAlert ..|> Observer
  Dashboard ..|> Observer
  EventBus o-- "*" Observer
  StockService --> EventBus : publishes to
```
