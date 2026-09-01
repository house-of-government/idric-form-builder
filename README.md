# Idriç Form Builder

A small form system for ordinary institutional data collection: schools, local government, clinics, employers, clubs, and other places where people mostly need to get correct information into a file without learning somebody else's accidental interface conventions.

The important target is not programmers building forms. It is someone filling out an ordinary school or government form and expecting it to work.

## Design boundary

Form building is its own problem.

- **Domain schema:** what information is being requested and its type.
- **Cardinality:** whether a field has one value, zero or one, one or more, etc.
- **Interaction:** how a person enters those values.
- **Validation:** whether each entered value belongs to the declared type.
- **Export:** how the resulting typed record is serialized for the institution's file or database.

Networking, sockets, HTTP, and transport belong below or outside this layer. A form should produce a typed record before any transport or storage adapter gets involved.

Likewise, a file format should not leak backward into the human interface. If a downstream CSV happens to require commas, the person filling out the form should not have to know that.

## Do not model facts the institution does not need

A form should not invent a family ontology just because several people may need the same information.

If the real requirement is "notify everyone who needs this information," then `mother`, `father`, `guardian`, `grandmother`, `cousin`, `older_sister`, and similar role classes are usually the wrong abstraction. The institution may not need to know those relationships at all.

Collect the operational fact instead: where should the information go?

```text
notification_destinations : one_or_more Notification_Destination
```

A destination may eventually be an email address, text-message number, push destination, or another supported channel. The schema should not force a relationship label unless there is an independent reason the institution genuinely needs that relationship.

This also keeps notification presentation outside the school's problem. A person may give the school an email address and then use their own software to turn selected messages into phone notifications, summaries, reminders, or alerts. The school does not need to know how that happens.

Likewise, a preference such as "only interrupt me when I have to do something" is conceptually a notification/filtering policy, not a property of the person's family role. If a form ever needs to collect such a preference, model that preference directly.

## First real-life target: school lunch contact addresses

Observed failure mode: a school lunch form said that a response would be sent to the email addresses provided, but exposed only one scalar email-address field. Entering multiple addresses separated by commas or semicolons was rejected.

That is a schema/UI mismatch. The requested datum is plural, but the widget is singular.

The first executable slice can remain deliberately simple:

```text
reply_addresses : one_or_more Email_Address
```

The renderer can then provide a repeatable email control (for example, one box plus “add another”). It may also accept pasted address lists with a documented normalization rule, but canonical form data is a sequence of `Email_Address` values, never a delimiter-encoded string.

The export layer decides later whether that sequence becomes multiple CSV columns, repeated rows, JSON array values, database rows, or something else.

The broader direction can later generalize from `Email_Address` to notification destinations without complicating the first case prematurely.

## First acceptance contract

For a `one_or_more Email_Address` field:

1. One valid address is accepted.
2. Two or more valid addresses can be entered without guessing a separator convention.
3. If list pasting is supported, comma, semicolon, and newline input normalize to the same canonical sequence.
4. Each member is validated as an `Email_Address`; list parsing is not a substitute for address validation.
5. Empty input is rejected because the cardinality is `one_or_more`.
6. The typed result contains a sequence of addresses, not the original delimiter string.
7. CSV/JSON/etc. serialization is handled by an export adapter and does not change the field semantics.

## Direction

The general goal is consistency. A school or government office should be able to declare the information it needs in terms of ordinary semantic types and cardinalities, render a usable form, validate the result, and export it into the file it already uses.

The form builder should make the easy, common case boring:

```text
School_Lunch_Response
  student_name     : Person_Name
  reply_addresses  : one_or_more Email_Address
```

Do not add entities merely because an object-oriented design makes them available. Ask for the smallest fact that the institution actually needs.

The same schema should be usable by different renderers and different export adapters. That separation is the point: form logic should not become networking logic, storage conventions should not become burdens placed on the person filling out the form, and notification delivery should not become family-role modeling.
