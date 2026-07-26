---
title: "Operators"
source: "https://stalw.art/docs/configuration/expressions/operators"
source_format: official-html-to-markdown
language: "en"
doc_slug: "configuration/expressions/operators"
category: "configuration"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Operators"
---
# Operators

      

Expressions may combine values using arithmetic, logical, and comparison operators.

## Arithmetic

[Section titled “Arithmetic”](#arithmetic)
OperatorMeaning`+`Addition; also string and array concatenation when either operand is non-numeric.`-`Subtraction, or unary negation at the start of a subexpression.`*`Multiplication.`/`Division.

The `+` operator composes strings as well as numbers. Expressions such as `'sql_' + listener` or `sender + '@' + system('domain')` are the common way to build identifiers and addresses from context variables.

## Logical

[Section titled “Logical”](#logical)
OperatorMeaning`&&`Logical AND; `&` is accepted as a synonym.`||`Logical OR; `|` is accepted as a synonym.`^`Logical XOR.`!`Logical NOT; prefix unary.

Non-boolean values are coerced when used in a logical context: the empty string and the integer `0` are treated as false, every other value as true.

## Comparison

[Section titled “Comparison”](#comparison)
OperatorMeaning`==`Equal; `=` is accepted as a synonym.`!=`Not equal.`>`Greater than.`<`Less than.`>=`Greater than or equal; `=>` is accepted as a synonym.`<=`Less than or equal; `=<` is accepted as a synonym.

Operands must be of compatible types. Comparing a string against a number with `>` or `<` does not produce a runtime error, but the ordering reflects the string representation of the number rather than its numeric value. Keeping the operands of a comparison consistent (both numeric, or both strings) avoids surprising behaviour.

## Precedence

[Section titled “Precedence”](#precedence)

From highest to lowest:

Unary `!` and unary `-`.
`*`, `/`.
`+`, `-`.
`>`, `<`, `>=`, `<=`.
`==`, `!=`.
`^`.
`&&`.
`||`.

Parentheses override precedence:

```
(retry_num > 3 || notify_num > 1) && is_local_domain(rcpt_domain)
```

Without the parentheses, `&&` would bind tighter than `||` and change the meaning.