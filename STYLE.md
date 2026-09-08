# Repository style

Idriç-facing source in this repository follows the canonical Idriç guide in
[`isomorphisms/Idric/STYLE.md`](https://github.com/isomorphisms/Idric/blob/Idri%C3%A7/STYLE.md)
and its canonical intent examples:

- [railway](https://github.com/isomorphisms/Idric/tree/Idri%C3%A7/examples/intent/railway)
- [HTTP server](https://github.com/isomorphisms/Idric/tree/Idri%C3%A7/examples/intent/http_server)

This file records only repository-specific constraints. Do not duplicate the
canonical guide here.

## Keep the form domain above adapters

The form builder's top-level vocabulary is the information a person or
institution means to collect: domain schema, cardinality, interaction,
validation, and the resulting typed record.

Networking, HTTP, sockets, file formats, databases, Android/framework plumbing,
and serialization belong below or outside that form-domain layer. Do not let a
transport or storage representation redefine the form semantics.

## Preserve semantic types and cardinality

Represent facts such as `Email_Address`, `Person_Name`, `one_or_more`, and
`zero_or_one` structurally. Do not encode plural values into delimiter strings
or replace domain restrictions with convenient machine types.

Validation should answer whether entered values belong to the declared semantic
type. Parsing a representation is not a substitute for validating the value.

## Keep intent visible

A top-level path should make the form job legible before exposing widget,
renderer, export, parser, or platform mechanics. Prefer names that say what the
person or institution is trying to collect and what the program is doing with
it.

Renderers and export adapters may vary independently. Keep them as explicit
deep-dive mechanisms rather than mixing them into the domain schema.
