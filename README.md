# toml

`parse` and `parse_with_limits` return `Result[TomlDocument, ParseError]`.
Errors expose the UTF-8 byte offset, line and byte column. Invalid values,
duplicate keys/tables, malformed escapes and integer overflow are rejected.
`toml_parse_document` is the compatibility wrapper and panics on invalid input.
The compiler propagates parser errors through its manifest diagnostics.

The parser supports signed 64-bit integers (including base prefixes and digit
separators), floats, booleans, basic/literal and multiline strings, nested
arrays, dotted/quoted keys, tables, arrays of tables, inline tables and validated
local/offset date-time tokens. Tables use insertion-ordered hash indexes;
line, whitespace and key scans are iterative. Resource limits bound input,
allocation, nesting and collection sizes. This is not a claim of complete
TOML conformance; conformance cases live in the package tests.

`TomlSerializer[T]` implements `serialize::Serializer[str]` and
`TomlDeserializer[T]` implements `serialize::Deserializer[T]`. `encode`/`decode`
are convenience functions. Reflection supports records, tuples, enum variants,
`Vec`, string-keyed `Dict` and `Option`. `serialize.rename` is honored. Document
roots must be records or dictionaries. None table fields are omitted; None
array elements and unit values return UnsupportedType because TOML has no null.
Integer output must fit i64. Date/time tokens can be read into string fields.

```encore
import toml::{encode, decode}
struct Config { name: str retries: i64 }
let text = encode(Config { "worker", 3 })?
let config = decode[Config](text)?
```

The legacy section getters remain available; `toml_get_integer` checks the i32
range and `toml_get_i64` exposes the full TOML integer range.

## SDK development

Encore pins this package as a Git submodule. Clone the complete SDK with
`git clone --recurse-submodules --branch trunk https://github.com/encore-ecosystem/encore.git`
and edit this package under `packages/toml/`. Sibling `path@` dependencies
are resolved from that checkout. Encore CI tests the exact package commits
with the compiler on all supported native targets. Published package archives
rewrite path dependencies to exact index versions.
