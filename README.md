# Asset Portfolio – Class Diagram

**Workbook 6a – Advanced OOP · Exercise 1 (Fixed Assets)**

This project is a UML class diagram for an asset portfolio application. It shows how we extended a portfolio that only held fixed assets (jewelry, gold, houses) so it can also include liquid accounts (bank accounts) and debt (credit cards), giving a better estimate of real net worth.

![Asset Portfolio class diagram](assets-diagrams/asset-portfolio-class-diagram.png)

A PDF version and the editable draw.io source are in [`assets-diagrams/`](assets-diagrams/).

## Project specs

**Starting point:** A `Portfolio` is made up of `FixedAsset` objects (`Jewelry`, `Gold`, `House`) and can calculate the total value of everything in it.

**Requirements for this change:**

- Add a `BankAccount` class with `deposit` and `withdraw` methods.
- Add a `CreditCard` class with `charge` and `pay` methods (debt).
- Fixed assets must **not** have `withdraw`, `deposit`, `charge` or `pay`.
- `BankAccount` and `CreditCard` must **not** extend the `Asset` class.
- Create a new interface named `Valuable` that defines `getValue()`.
- Update `Portfolio` so its list can hold the new classes.

## How the diagram is structured

| Class / Interface | Type | Role |
|---|---|---|
| `Valuable` | Interface | The contract: anything with a value must provide `getValue()` |
| `Portfolio` | Class | Holds an `ArrayList<Valuable>` and totals their values |
| `FixedAsset` | Abstract class | Shared `name` and `marketValue` for physical assets; implements `Valuable` |
| `Jewelry`, `Gold`, `House` | Classes | Extend `FixedAsset`; each overrides `getValue()` |
| `BankAccount` | Class | Implements `Valuable`; has `deposit` and `withdraw` |
| `CreditCard` | Class | Implements `Valuable`; has `charge` and `pay`; `getValue()` returns `-balance` |

Relationships used:

- **Implements** (dashed line, hollow triangle): `FixedAsset`, `BankAccount` and `CreditCard` → `Valuable`
- **Extends** (solid line, hollow triangle): `Jewelry`, `Gold` and `House` → `FixedAsset`
- **Aggregation / has-a** (hollow diamond): `Portfolio` holds 0 or more `Valuable` objects

## Why we designed it this way

**1. `Valuable` is an interface, not another parent class.**
`BankAccount` and `CreditCard` cannot extend `Asset`, and Java allows a class to extend only one parent. An interface lets unrelated classes share a contract without sharing an inheritance chain. A class can implement as many interfaces as it needs.

**2. `Portfolio` depends on `Valuable`, not on specific classes.**
`Portfolio` only needs to ask "what are you worth?" It does not need to know about `karat`, `withdraw` or `charge`. This is abstraction: expose only what is necessary. Because the list is `ArrayList<Valuable>`, we can add new kinds of valuables later without changing `Portfolio`.

**3. `FixedAsset` stays abstract and implements `Valuable`.**
`FixedAsset` holds what all physical assets share (`name`, `marketValue`) but cannot be created on its own. Since a parent that implements an interface passes it down to its children, `Jewelry`, `Gold` and `House` are all `Valuable` through `FixedAsset`. They do not need their own arrow to the interface. Each one still has to override `getValue()`.

**4. Liquid accounts are separate from fixed assets.**
Bank balances and credit card balances change often. Fixed asset values do not. Keeping `deposit`, `withdraw`, `charge` and `pay` out of `FixedAsset` stops physical assets from getting methods that make no sense for them.

**5. Debt counts as negative value.**
`CreditCard.getValue()` returns `-balance`. When the `Portfolio` adds up all the `getValue()` results, debt lowers the total automatically, with no special case in `Portfolio`.

**6. Colors and legend.**
Purple is the interface, blue is the abstract class and fixed assets, green is the liquid accounts, and yellow is `Portfolio`. This makes the "fixed vs. liquid" split visible at a glance.

## Corrections made to the original diagram

- Moved the aggregation diamond to the `Portfolio` end. In UML the diamond goes on the container, and the original had it on the `FixedAsset` end.
- Renamed the `Asset(...)` constructor to `FixedAsset(...)` so it matches the class name.
- `BankAccount` and `CreditCard` attributes (`name`, `balance`) are our own choice, since the workbook does not specify them.

## Concepts demonstrated

- Interfaces as contracts (`implements`)
- Abstract classes vs. interfaces
- Why Java has single inheritance but multiple interface implementation
- Abstraction and programming to an interface
- UML relationships: implementation, inheritance, aggregation

## Tools

Diagram made with [draw.io](https://www.drawio.com) (free).

## Files

```
assets-diagrams/
├── asset-portfolio-class-diagram.drawio   (editable source)
├── asset-portfolio-class-diagram.pdf
└── asset-portfolio-class-diagram.png
README.md
```
