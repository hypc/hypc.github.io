---
title: 字符串书写规范
date: 2020-10-04 18:21:36
tags:
---

Case                | Description                                                                                       | Example       | Resule
--------------------|---------------------------------------------------------------------------------------------------|---------------|---------------
camel case          | Transform into a string with the separator denoted by the next word capitalized                   | `test string` | `testString`
capital case        | Transform into a space separated string with each word capitalized                                | `test string` | `Test String`
constant case       | Transform into upper case string with an underscore between words                                 | `test string` | `TEST_STRING`
dot case            | Transform into a lower case string with a period between words                                    | `test string` | `test.string`
header case         | Transform into a dash separated string of capitalized words                                       | `test string` | `Test-String`
no case             | Transform into a lower cased string with spaces between words                                     | `test string` | `test string`
param case          | Transform into a lower cased string with dashes between words                                     | `test string` | `test-string`
kebab case          | Transform into a lower cased string with dashes between words                                     | `test string` | `test-string`
pascal case         | Transform into a string of capitalized words without separators                                   | `test string` | `TestString`
path case           | Transform into a lower case string with slashes between words                                     | `test string` | `test/string`
sentence case       | Transform into a lower case with spaces between words, then capitalize the string                 | `testString`  | `Test string`
snake case          | Transform into a lower case string with underscores between words                                 | `test string` | `test_string`
title case          | Transform a string into title case following English rules                                        | `test string` | `Test String`
swap case           | Transform a string by swapping every character from upper to lower case, or lower to upper case   | `test string` | `tEST sTRING`
lower case          | Transforms the string to lower case                                                               | `TEST STRING` | `test string`
lower case first    | Transforms the string with the first character in lower cased                                     | `TEST STRING` | `tEST STRING`
upper case          | Transforms the string to upper case                                                               | `test string` | `TEST STRING`
upper case first    | Transforms the string with the first character in upper cased                                     | `test string` | `Test string`

## Reference

* https://github.com/blakeembrey/change-case
