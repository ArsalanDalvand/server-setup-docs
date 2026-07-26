---
title: "JMESPath custom functions"
source: "https://redis.io/docs/latest/integrate/write-behind/reference/jmespath-custom-functions/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/write-behind/reference/jmespath-custom-functions"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "JMESPath custom functions"
---
# JMESPath custom functions

JMESPath custom function reference

Function
Description
Example
Comments

`base64_decode`
Decodes a base64(RFC 4648) encoded string
Input: `{"encoded": "SGVsbG8gV29ybGQh"}` 
 Expression: `base64_decode(encoded)` 
 Output: `Hello World!`

`capitalize`
Capitalizes all the words in the string
Input: `{"name": "john doe"}` 
 Expression: `capitalize(name)` 
 Output: `John Doe`

`concat`
Concatenates an array of variables or literals
Input: `{"fname": "john", "lname": "doe"}` 
 Expression: `concat([fname, ' ' ,lname])` 
 Output: `john doe`
This is equivalent to the more verbose built-in expression: `' '.join([fname,lname])`

`filter_entries`
Filters entries in a dictionary (object) using the given JMESPath predicate
Input: `{ "name": "John", "age": 30, "country": "US", "score": 15}` 
 Expression: `filter_entries(@, `key == 'name' || key == 'age'`)`
 Output:`{"name": "John", "age": 30 }`

`from_entries`
Converts an array of objects with `key` and `value` properties into a single object
Input: `[{"key": "name", "value": "John"}, {"key": "age", "value": 30}, {"key": "city", "value": null}]` 
 Expression: `from_entries(@)` 
 Output: `{"name": "John", "age": 30, "city": null}`

`hash`
Calculates a hash using the `hash_name` hash function and returns its hexadecimal representation
Input: `{"some_str": "some_value"}` 
 Expression: `hash(some_str, &#96;sha1&#96;)` 
 Output: `8c818171573b03feeae08b0b4ffeb6999e3afc05`
Supported algorithms: sha1 (default), sha256, md5, sha384, sha3_384, blake2b, sha512, sha3_224, sha224, sha3_256, sha3_512, blake2s

`in`
Checks if an element matches any value in a list of values
Input: `{"el": "b"}` 
 Expression: `in(el,`["a", "b", "c"]`)` 
 Output: `True`

`left`
Returns a specified number of characters from the start of a given text string
Input: `{"greeting": "hello world!"}` 
 Expression: `left(greeting,`5`)` 
 Output: `hello`

`lower`
Converts all uppercase characters in a string into lowercase characters
Input: `{"fname": "John"}` 
 Expression: `lower(fname)` 
 Output: `john`

`mid`
Returns a specified number of characters from the middle of a given text string
Input: `{"greeting": "hello world!"}` 
 Expression: `mid(greeting,`4`,`3`)` 
 Output: `o w`

`json_parse`
Returns parsed object from the given json string
Input: `{"data": '{"greeting": "hello world!"}'}` 
 Expression: `parse_json(data)` 
 Output: `{"greeting": "hello world!"}`

`regex_replace`
Replaces a string that matches a regular expression
Input: `{"text": "Banana Bannnana"}` 
 Expression: `regex_replace(text, 'Ban\w+', 'Apple Apple')` 
 Output: `Apple Apple`

`replace`
Replaces all the occurrences of a substring with a new one
Input: `{"sentence": "one four three four!"}` 
 Expression: `replace(sentence, 'four', 'two')` 
 Output: `one two three two!`

`right`
Returns a specified number of characters from the end of a given text string
Input: `{"greeting": "hello world!"}` 
 Expression: `right(greeting,`6`)` 
 Output: `world!`

`split`
Splits a string into a list of strings after breaking the given string by the specified delimiter (comma by default)
Input: `{"departments": "finance,hr,r&d"}` 
 Expression: `split(departments)` 
 Output: `['finance', 'hr', 'r&d']`
Default delimiter is comma - a different delimiter can be passed to the function as the second argument, for example: `split(departments, ';')`

`time_delta_days`
Returns the number of days between a given `dt` and now (positive) or the number of days that have passed from now (negative)
Input: `{"dt": '2021-10-06T18:56:16.701670+00:00'}` 
 Expression: `time_delta_days(dt)` 
 Output: `365`
If `dt` is a string, ISO datetime (2011-11-04T00:05:23+04:00, for example) is assumed. If `dt` is a number, Unix timestamp (1320365123, for example) is assumed.

`time_delta_seconds`
Returns the number of seconds between a given `dt` and now (positive) or the number of seconds that have passed from now (negative)
Input: `{"dt": '2021-10-06T18:56:16.701670+00:00'}` 
 Expression: `time_delta_days(dt)` 
 Output: `31557600`
If `dt` is a string, ISO datetime (2011-11-04T00:05:23+04:00, for example) is assumed. If `dt` is a number, Unix timestamp (1320365123, for example) is assumed.

`to_entries`
Converts a given object into an array of objects with `key` and `value` properties
Input: `{"name": "John", "age": 30, "city": null}` 
 Expression: `to_entries(@)` 
 Output: `[{"key": "name", "value": "John"}, {"key": "age", "value": 30}, {"key": "city", "value": null}]`

`upper`
Converts all lowercase characters in a string into uppercase characters
Input: `{"fname": "john"}` 
 Expression: `upper(fname)` 
 Output: `JOHN`

`uuid`
Generates a random UUID4 and returns it as a string in standard format
Input: None 
 Expression: `uuid()` 
 Output: `3264b35c-ff5d-44a8-8bc7-9be409dac2b7`

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)