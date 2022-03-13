---
description: Intrinsics are low-level operations that are natively supported in REDscript.
---

# Intrinsics

| Intrinsic        | Type                                                                                | Description                                                     |
| ---------------- | ----------------------------------------------------------------------------------- | --------------------------------------------------------------- |
| `Equals`         | `(A, A) -> Bool`                                                                    | Equality check (for references, enums, strings and booleans)    |
| `NotEquals`      | `(A, A) -> Bool`                                                                    | Inequality check (same as above)                                |
| `IsDefined`      | <p><code>(ref&#x3C;A>) -> Bool</code></p><p><code>(wref&#x3C;A>) -> Bool</code></p> | Null check                                                      |
| `ToString`       | `(A) -> String`                                                                     | String conversion                                               |
| `EnumInt`        | `(A) -> Int32`                                                                      | Enum-to-Int32 conversion                                        |
| `IntEnum`        | `(Int32) -> A`                                                                      | Int32-to-enum conversion                                        |
| `ToVariant`      | `(A) -> Variant`                                                                    | Variant constructor                                             |
| `FromVariant`    | `(Variant) -> A`                                                                    | Variant extractor (fails at runtime if the type does not match) |
| `ArraySize`      | `(array<A>) -> Int32`                                                               |                                                                 |
| `ArrayPush`      | `(array<A>, A) -> Void`                                                             |                                                                 |
| `ArrayPop`       | `(array<A>) -> A`                                                                   |                                                                 |
| `ArrayClear`     | `(array<A>) -> Void`                                                                |                                                                 |
| `ArrayResize`    | `(array<A>, Int32) -> Void`                                                         |                                                                 |
| `ArrayFindFirst` | `(array<A>, A) -> A`                                                                |                                                                 |
| `ArrayFindLast`  | `(array<A>, A) -> A`                                                                |                                                                 |
| `ArrayContains`  | `(array<A>, A) -> Bool`                                                             |                                                                 |
| `ArrayCount`     | `(array<A>, A) -> Int32`                                                            |                                                                 |
| `ArrayInsert`    | `(array<A>, Int32, A) -> Void`                                                      |                                                                 |
| `ArrayRemove`    | `(array<A>, A) -> Bool`                                                             |                                                                 |
| `ArrayGrow`      | `(array<A>, Int32) -> Void`                                                         |                                                                 |
| `ArrayErase`     | `(array<A>, Int32) -> Void`                                                         |                                                                 |
| `ArrayLast`      | `(array<A>) -> A`                                                                   |                                                                 |
