# toml

Parses the manifest subset used by Encore: sections, strings, integers, booleans
and arrays of strings. Basic strings decode quote, backslash, control-character
and Unicode escapes (`\uXXXX` and `\UXXXXXXXX`). Single-quoted strings preserve
backslashes. Quoted delimiters and trailing comments are handled without
including the comment in the string value. String scanning uses UTF-8 byte
coordinates consistently.

## SDK development

Encore pins this package as a Git submodule. Clone the complete SDK with
`git clone --recurse-submodules --branch trunk https://github.com/encore-ecosystem/encore.git`
and edit this package under `packages/toml/`. Sibling `path@` dependencies
are resolved from that checkout. Encore CI tests the exact package commits
with the compiler on all supported native targets. Published package archives
rewrite path dependencies to exact index versions.
