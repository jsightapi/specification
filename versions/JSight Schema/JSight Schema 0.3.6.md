# JSight Schema 0.3 Specification

May 09, 2024

**JSight Schema version: *0.3.6* **  
Document version: *0.9.21-en*  

Authors: *Ivanov A., Malyshev K.*  
Checked by: *Reznitsky M.*

## Introduction

This document covers all aspects of JSight Schema language of the specified version. The language is
designed to describe structured data schema of any format. 

The document is intended for:

- Users of JSight Schema language;
- Authors of JSight Schema language manuals and tutorials;
- Software developers who use JSight Schema language.  

The JSight Schema language is currently in its “peak” stage of development. Not all of the planned
language constructs have made it into the current language version, some are still waiting in the
wings.

The JSight Schema language is widely used as part of the [JSight API language](jsight-api-0-3),
which is intended to describe the HTTP REST JSON API. For this reason, the JSight Schema language
contains constructs that, apart from the HTTP REST JSON API, may appear strange. For example, see
the section [“References to USER TYPES in the EXAMPLE”](#references-to-user-types-in-the-example).

## Terms and Definitions

- **ANNOTATION** — is the syntactic structure of the language that allows other syntactic structures
  to be specified.
- **COMMENT** — is the syntactic structure of the language that allows adding random textual remarks
  to any SCHEMA section. It is important to distinguish a COMMENT from a textual note to the group
  of RULES in the ANNOTATION. A note to the group of RULES refers to the corresponding element of
  the EXAMPLE and means the textual documentation of this element. A user COMMENT is a random
  comment made by the SCHEMA developer that has no specific relevance in the context of SCHEMA (see
  the sections [“User COMMENTS”](#user-comments), [“Text notes to RULES”](#text-notes-to-rules)). 
- Array **ELEMENT** — is some VALUE from a set of which the array's internal structure is composed.
- **DOCUMENT** — is some structured data recorded in a сertain FORMAT, which must be VALIDATED for
  compliance with the SCHEMA.
- **DOCUMENT FORMAT** — is the notation used to serialize structured data into a string and form a
  DOCUMENT.
- **ENUMERATION** — is a particular data type that limits the possible VALUE by a certain set of
  valid VALUES.
- **EXAMPLE** —  is a JSON document that is an example of correct data structure and is used as the
  basis of the SCHEMA. 
- Object **KEY** — is the name of the object PROPERTY.
- Object **PROPERTY** — is a pair of KEY-VALUE, its set composes the internal structure of the
  object.
- **RULES** — are additions to the EXAMPLE written in a certain manner, specifying  additional
  requirements for the data structure within the SCHEMA.
- **STANDARD TYPE** — is the TYPE that is built into the language out of the box. In contrast to
  USER TYPE names, STANDARD TYPE names never start with the symbol `@`.
- **SCHEMA** — is a document in the JSight Schema language that describes the requirements for a
  certain data structure. It can be used for validation, documentation, and generation of specific
  data.
- **SCHEMA ATTRIBUTES** — is a tool for adding some additional information to the SCHEMA, e.g.,
  SCHEMA title, SCHEMA description, SCHEMA version, USER TYPES description, links to external
  SCHEMAS, etc. The ATTRIBUTES mechanism is not implemented in the current language version.
- **TYPE** — is a named set of requirements for the VALUE in the DOCUMENT. The TYPE name is intended
  to reflect the essence of the corresponding VALUE. TYPES might be STANDARD (built into the
  language) and USER (user-defined).
- **USER TYPE** — is the TYPE that the user specifies using the named SCHEMA. The current language
  version does not describe the so-called "attribute mechanism" which allows the creation of a named
  SCHEMA. The reason for this is that this version of the language will be used not on its own, but
  as a part of the JSight API language which already has its own mechanism for declaring USER TYPES,
  i.e. naming SCHEMAS (for more information, see the [JSight API language
  specification](jsight-api-0-3)). USER TYPE names always start with the symbol `@`.
- **VALIDATION** — is the procedure for ensuring that the DOCUMENT data structure complies with the
  SCHEMA.
- **VALUE** — is some data element in the DOCUMENT (object, array, line, number, etc.). In objects,
  the PROPERTY VALUE should be distinguished from the PROPERTY KEY. The PROPERTY KEY essentially is
  not the VALUE of an object PROPERTY; rather, it names this PROPERTY within the object. 

## Concept

The main goal of the JSight Schema language is to reduce the time for developing data schema
descriptions and increase the speed of perceiving and understanding of these schemas by developers,
technical writers, etc.

The JSight Schema language is optimized primarily to work with DOCUMENTS in the JSON FORMAT, but it
also allows you to work with DOCUMENTS in other FORMATS.

The main idea of JSight Schema is that for a minimal description of the data structure, it is enough
to provide an example of data with the correct structure. 

## SCHEMA

The SCHEMA is a data EXAMPLE that can be supplemented by RULES. In general, EXAMPLE is an example of
real data with a correct structure according to the SCHEMA (exceptions to this rule will be
described [below](#references-to-user-types-in-the-example)). The EXAMPLE itself contains some
requirements for the described data structure (descriptions of these requirements will be detailed
[below](#example)). Additional RULES specify data structure requirements that are not not apparent
from the EXAMPLE itself.

In addition to the EXAMPLE and RULES, the schema may include textual explanations within ANNOTATIONS
and COMMENTS (will be described in detail [below](#annotations)).

The ATTRIBUTE mechanism will be added to the SCHEMA in the next versions of the language, allowing
to specify additional information to the SCHEMA, e.g., SCHEMA title, SCHEMA description, SCHEMA
version, USER TYPES description, links to external SCHEMAS, etc.

## EXAMPLE

> Requirement: req.jschema.example 0.1  
> Status: APPROVED 09.12.2019.

In its most basic form, the SCHEMA looks like an EXAMPLE of data. The EXAMPLE already carries a
certain set of requirements for the DOCUMENT. All possible requirements imposed by the EXAMPLE are
described in the sections below.

For example, the following SCHEMA describes such a data structure: an object containing only one
PROPERTY with the KEY `name` and the VALUE of TYPE `string`.

```jsight
{
  "name": "Tom"
}
```

### Correspondence between EXAMPLE and SCHEMA data TYPES 

> Requirements:  
>
> - req.jschema.example.number 0.2  
>   Status: APPROVED 14.02.2022.
> - req.jschema.example.string 0.1  
>   Status: APPROVED 09.12.2019.
> - req.jschema.example.boolean 0.1  
>   Status: APPROVED 09.12.2019.
> - req.jschema.example.null 0.1  
>   Status: APPROVED 09.12.2019.

The EXAMPLE of correct data expresses some of the data structure requirements in an obvious way. All
data structure requirements can be classified into four types:

1. Requirement for the type of the root data element.
2. Requirements for the set of keys of object properties.
3. Requirements for the types of values of object properties.
4. Requirements for the types of values of array elements.

If the root data element is of the scalar type, we have the simplest data structure with only one
element. In this case, requirements 2, 3, and 4 are not applicable to the data structure. However,
if the structure's root element is an object or an array, then requirements 2, 3, and 4 can be
applied to it.

The EXAMPLE of correct data allows us to partially see all of the four types of requirements listed.
In most cases, looking at the types of VALUES and the KEYS of objects in the EXAMPLE will be
sufficient to do this.

The EXAMPLE looks like a JSON document. The JSON specification only defines the following data
types: `object`, `array`, `string`, `number`, `boolean` (`true`, `false`) or `null`. JSight Schema
uses a broader range of data TYPES. In order to unambiguously define the requirements for the
DOCUMENT using the EXAMPLE, a strong correspondence between the JSON data types used in the EXAMPLE
and the JSight Schema data TYPES is required to be established.

The following table defines this correspondence.

<table>
<thead>
<tr>
  <th>JSight Schema Data Type</th>
  <th>JSON Data Type</th>
  <th>SCHEMA Data Type Definition Conditions</th>
</tr>
</thead>
<tbody>
<tr valign="top">
  <td><b>string</b></td>
  <td><b>string</b></td>
  <td>
    <p><code>string</code> — a string</p>
    <p>To determine the data TYPE <code>string</code>, you must specify any string in a VALUE in 
    the EXAMPLE.</p>
    <p>For example, the following SCHEMA specifies that the PROPERTY with the KEY <code>data</code> must
    have a VALUE of type <code>string</code>:</p>
    <pre className="language-jsight">{`{\n  "data": "Any string"\n}`}</pre>
  </td>
</tr>
<tr valign="top">
  <td><b>integer</b></td>
  <td><b>number</b></td>
  <td>
    <p><code>integer</code> — an integer value</p>
    <p>To determine the data TYPE <code>integer</code>, you must specify any integer as a VALUE in 
    the EXAMPLE.</p>
    <p>Example of a SCHEMA:</p>
    <pre className="language-jsight">{`{\n  "data": 1\n}`}</pre>
    <p>The following DOCUMENT is VALID:</p>
    <pre className="language-jsight">{`{"data": -123}`}</pre>
    <p>Also this DOCUMENT is VALID:</p>
    <pre className="language-jsight">{`{"data": 2e+3}`}</pre>
    <p>However, the following DOCUMENT is NOT VALID:</p>
    <pre className="language-jsight">{`{"data": 1.2}`}</pre>
  </td>
</tr>
<tr valign="top">
  <td><b>float</b></td>
  <td><b>number</b></td>
  <td>
    <p><code>float</code> — a floating point number</p>
    <p>Both fractional and integer numbers are accepted by this data type.</p>
    <p>To determine the data TYPE <code>float</code>, you must specify any number with a fractional 
    part as a VALUE in the EXAMPLE.</p>
    <p>Example of a SCHEMA:</p>
    <pre className="language-jsight">{`{\n  "data": 1.2\n}`}</pre>
    <p>In regard to the given SCHEMA, the following DOCUMENT is VALID:</p>
    <pre className="language-jsight">{`{"data": 2}`}</pre>
    <p>Also this DOCUMENT is VALID:</p>
    <pre className="language-jsight">{`{"data": 2.5}`}</pre>
    <p>Also this DOCUMENT is VALID:</p>
    <pre className="language-jsight">{`{"data": 2.987654321}`}</pre>
    <p>Also this DOCUMENT is VALID:</p>
    <pre className="language-jsight">{`{"data": 2e+3}`}</pre>
  </td>
</tr>
<tr valign="top">
  <td><b>decimal</b></td>
  <td><b>number</b></td>
  <td>
    <p><code>decimal</code> — a fixed-point number</p>
    <p>This data TYPE accepts both fixed-point fractional numbers and integers.</p>
    <p>To determine the data TYPE <code>decimal</code>, you must specify any number with a 
    fractional part as one of the values ​​in the EXAMPLE (like for <code>float</code>), and 
    additionally specify the RULE <code>precision</code> (for more information see 
    section <a href="#rule-precision">“RULE "precision"”</a>.</p>
    <p>Example of a SCHEMA:</p>
    <pre className="language-jsight">{`{\n  "data": 0.12 // {precision: 2}\n}`}</pre>
    <p>The given SCHEMA states that the key <code>data</code> must have the value of type <code>decimal</code> containing 
    an integer or fractional number with two or less decimal places after the decimal point.</p>
    <p>The following DOCUMENT is VALID:</p>
    <pre className="language-jsight">{`{"data": 9.12}`}</pre>
    <p>Also this DOCUMENT is VALID:</p>
    <pre className="language-jsight">{`{"data": 9.1}`}</pre>
    <p>Also this DOCUMENT is VALID:</p>
    <pre className="language-jsight">{`{"data": -9}`}</pre>
    <p>Also this DOCUMENT is VALID:</p>
    <pre className="language-jsight">{`{"data": 2e+3}`}</pre>
    <p>Also this DOCUMENT is VALID:</p>
    <pre className="language-jsight">{`{"data": 0.12}`}</pre>
    <p>Also this DOCUMENT is VALID:</p>
    <pre className="language-jsight">{`{"data": 0.1200}`}</pre>
    <p>Also this DOCUMENT is VALID:</p>
    <pre className="language-jsight">{`{"data": 12e-2}`}</pre>
    <p>However, the following DOCUMENT is NOT VALID:</p>
    <pre className="language-jsight">{`{"data": 9.123}`}</pre>
  </td>
</tr>
<tr valign="top">
  <td><b>boolean</b></td>
  <td><b>boolean</b></td>
  <td>
    <p><code>boolean</code> — a logical data TYPE (can take the 
    VALUE <code>true</code> or <code>false</code>)</p>
    <p>To determine the TYPE <code>boolean</code>, you must specify the 
    VALUE <code>true</code> or <code>false</code> in the EXAMPLE.</p>
    <p>Example of a SCHEMA:</p>
    <pre className="language-jsight">{`{\n  "data": true\n}`}</pre>
  </td>
</tr>
<tr valign="top">
  <td><b>null</b></td>
  <td><b>null</b></td>
  <td>
    <p>To determine the TYPE <code>null</code>, you must specify the VALUE <code>null</code> in the 
    EXAMPLE.</p>
    <p>Example of a SCHEMA:</p>
    <pre className="language-jsight">{`{\n  "data": null\n}`}</pre>
  </td>
</tr>
<tr valign="top">
  <td><b>email</b></td>
  <td><b>string</b></td>
  <td>
    <p><code>email</code> — an email address</p>
    <p>To determine the TYPE <code>email</code>, you must specify any string in the email format in 
    the EXAMPLE, and additionally specify the RULE <code>type</code> with the 
    value <code>email</code> (for more information, see 
    section <a href="#type-email">“TYPE "email"”</a>.</p>
    <p>Example of a SCHEMA:</p>
    <pre className="language-jsight">{`{\n  "data": "name@domain.com" // {type: "email"}\n}`}</pre>
  </td>
</tr>
<tr valign="top">
  <td><b>uri</b></td>
  <td><b>string</b></td>
  <td>
    <p><code>uri</code> — the universal resource identifier</p>
    <p>To determine the TYPE uri, in the EXAMPLE, you must specify any string in the uri format in 
    the EXAMPLE, and additionally specify the RULE <code>type</code> with the 
    value <code>uri</code> (for more information, see 
    section <a href="#type-uri">“TYPE "uri"”</a>).</p>
    <p>Example of a SCHEMA:</p>
    <pre className="language-jsight">{`{\n  "data": "http://domain.com/" // {type: "uri"}\n}`}</pre>
  </td>
</tr>
<tr valign="top">
  <td><b>date</b></td>
  <td><b>string</b></td>
  <td>
    <p><code>date</code> — a date</p>
    <p>To determine the TYPE <code>date</code>, you must specify a string containing date in the 
    EXAMPLE as defined by "full-date" 
    in <a href="https://xml2rfc.ietf.org/public/rfc/html/rfc3339.html#anchor14">RFC3339</a>, and 
    additionally specify the RULE <code>type</code> with the `date` value (for 
    more information, see section <a href="#type-date">“TYPE "date"”</a>).</p>
    <p>Example of a SCHEMA:</p>
    <pre className="language-jsight">{`{\n  "data": "2021-12-16" // {type: "date"}\n}`}</pre>
  </td>
</tr>
<tr valign="top">
  <td><b>datetime</b></td>
  <td><b>string</b></td>
  <td>
    <p><code>datetime</code> — date and time</p>
    <p>To determine the TYPE <code>datetime</code>, you must specify a string containing a 
    date-time in the EXAMPLE as defined by "date-time" 
    in <a href="https://xml2rfc.ietf.org/public/rfc/html/rfc3339.html#anchor14">RFC3339</a>, and 
    additionally specify the RULE <code>type</code> with the `datetime` value (for more 
    information, see section <a href="#type-datetime">“TYPE "datetime"”</a>).</p>
    <p>Example of a SCHEMA:</p>
    <pre className="language-jsight">{`{\n  "data": "2006-01-02T15:04:05+07:00" // {type: "datetime"}\n}`}</pre>
  </td>
</tr>
<tr valign="top">
  <td><b>uuid</b></td>
  <td><b>string</b></td>
  <td>
    <p><code>uuid</code> — the universal resource identifier</p>
    <p>To determine the TYPE <code>uuid</code>, you must specify any string in the uuid format in 
    the EXAMPLE, and additionally specify the RULE <code>type</code> with the 
    value <code>uuid</code> (for more information, see 
    section <a href="#type-uuid">“TYPE "uuid"”</a>).</p>
    <p>Example of a SCHEMA:</p>
    <pre className="language-jsight">{`{\n  "data": "550e8400-e29b-41d4-a716-446655440000" // {type: "uuid"}\n}`}</pre>
  </td>
</tr>
<tr valign="top">
  <td><b>object</b></td>
  <td><b>object</b></td>
  <td>
    <p><code>object</code> — an object</p>
    <p>To determine the TYPE <code>object</code>, you must specify any object in the EXAMPLE.</p>
    <p>For example:</p>
    <pre className="language-jsight">{`{\n  "object": {\n    "key": "value"\n  }\n}`}</pre>
  </td>
</tr>
<tr valign="top">
  <td><b>array</b></td>
  <td><b>array</b></td>
  <td>
    <p><code>array</code> — an array</p>
    <p>To determine the TYPE <code>array</code>, you must specify any array in the EXAMPLE.</p>
    <p>For example:</p>
    <pre className="language-jsight">{`{\n  "array": [1,2,3]\n}`}</pre>
  </td>
</tr>
<tr valign="top">
  <td><b>enum</b></td>
  <td><b>string, number, boolean, null</b></td>
  <td>
    <p><code>enum</code> — an enumerated TYPE</p>
    <p>It can take one of the specified VALUES.</p>
    <p>To determine the TYPE <code>enum</code>, you must specify the 
    RULE <code>enum</code> (described in more detail below), and in the EXAMPLE — any of the scalar 
    values listed in this RULE.</p>
    <p>For example:</p>
    <pre className="language-jsight">{`{\n  "data": 2 // {enum: [1, 2, "a", "b", true, false, null]}\n}`}</pre>
  </td>
</tr>
<tr valign="top">
  <td><b>mixed</b></td>
  <td><b>any of types</b></td>
  <td>
    <p><code>mixed</code> — a polymorphic TYPE</p>
    <p>The VALUE must correspond to one of the specified groups of RULES.</p>
    <p>To determine the type <code>mixed</code>, you must specify 
    the RULE <code>or</code> (described in more detail <a href="#rule-or">below</a>), 
    and in the EXAMPLE — the VALUE that matches any of the groups of RULES specified 
    in <code>or</code>.</p>
    <p>For example:</p>
    <pre className="language-jsight">{`{\n  "data": "abc" // {or: [{type: "string"}, {type: "integer", min: 0}]}\n}`}</pre>
  </td>
</tr>
<tr valign="top">
  <td><b>any</b></td>
  <td><b>any of types</b></td>
  <td>
    <p>To determine the data TYPE <code>any</code> you can specify any of the scalar JSON values 
    (string, number, boolean, or null), an empty object, or an empty array as VALUES in the EXAMPLE.
    </p>
    <p>Example of a SCHEMA:</p>
    <pre className="language-jsight">{`{\n  "data": 1 // {type: "any"}\n}`}</pre>
    <p>or</p>
    <pre className="language-jsight">{`{\n  "data": null // {type: "any"}\n}`}</pre>
    <p>or</p>
    <pre className="language-jsight">{`{\n  "data": [] // {type: "any"}\n}`}</pre>
  </td>
</tr>
</tbody>
</table>

:::note

It is not allowed to use numbers in exponent notation (for example: `2e2`, `3e+3` or `4E-4`) in 
the EXAMPLE of the SCHEMA, as it makes it difficult for a person to understand the EXAMPLE. Thus, 
such numbers can be used in the DOCUMENT. 

:::

### EXAMPLE requirements for object PROPERTIES

> Requirement: req.jschema.example.object 0.1  
> Status: APPROVED 09.12.2019.

If in the VALUE of the EXAMPLE an object is specified, then 

- The corresponding DOCUMENT object can contain PROPERTIES only with the KEYS indicated in the
  EXAMPLE.
- All listed PROPERTIES are mandatory.
- The TYPE of PROPERTY VALUES of the corresponding object of the DOCUMENT must match the TYPES
  defined by the SCHEMA.

For example:

```jsight
{
  "person": {
    "name": "John",
    "age": 25,
    "customer": true
  }
}
```

Such a SCHEMA states that there should be exactly three PROPERTIES in the object `person`:

1. `name` with a value of TYPE `string`;
2. `age` with a value of TYPE `integer`;
3. `customer` with a value of TYPE `boolean`.

All of the above restrictions on the object PROPERTIES can be changed using RULES (see section
[“RULES”](#rules)).

### EXAMPLE requirements for array ELEMENTS

> Requirement: req.jschema.example.array 0.1  
> Status: APPROVED 09.12.2019.

If an array is required to be described in the SCHEMA, then the EXAMPLE usually includes an array
containing at least one ELEMENT.

The TYPE of each array ELEMENT specified in the EXAMPLE determines data TYPE valid in the DOCUMENT
for an array ELEMENT with a *similar index*. 

Regardless of how many array ELEMENTS are specified in the EXAMPLE, by default, it is assumed that
the array can contain zero or more ELEMENTS (for more information, see the RULES
[`minItems`](#rule-minitems) and [`maxItems`](#rule-maxitems)).

The TYPE of last array ELEMENT specified in the EXAMPLE determines data TYPE valid in the DOCUMENT
for all array ELEMENTS with the *same or higher index*.

If the EXAMPLE of the SCHEMA contains an empty array, then nothing but an empty array can be used in
the DOCUMENT (since the TYPE of array elements is not defined). 

For example, the following SCHEMA states that there can be zero or more ELEMENTS of TYPE `string` in
the array `names`:

```jsight
{
  "names": [
    "Alex",
    "Jill",
    "Sophie"
  ]
}
```

The following SCHEMA states that there can be zero or more ELEMENTS in the array `data` of the
DOCUMENT, while an ELEMENT with index 0 must be of type `string` (if such an ELEMENT exists in the
array), and an ELEMENT with index 1 or more must be of type `boolean` (if such ELEMENTS exist in the
array).

```jsight
{
  "data": [
    "Alex",
    true
  ]
}
```

If an array of objects has to be described in the SCHEMA, at least one object must be specified as
an ELEMENT of the array in the EXAMPLE.

For example:

```jsight
[
  {"aaa": 111}
]
```

Another example of the SCHEMA:

```jsight
[
  {"aaa": 111},
  {"bbb": 222}
]
```

The following DOCUMENTS will be valid for such a SCHEMA:

```jsight
[]
```

and DOCUMENT:

```jsight
[
  {"aaa": 111}
]
```

and DOCUMENT:

```jsight
[
  {"aaa": 111},
  {"bbb": 222}
]
```

and DOCUMENT:

```jsight
[
  {"aaa": 111},
  {"bbb": 222},
  {"bbb": 333}
]
```

## ANNOTATIONS

> Requirement: req.jschema.annotation 0.2  
> Status: APPROVED 16.09.2021.

The SCHEMA can include ANNOTATIONS. By appearance, ANNOTATIONS look like comments in C-like
programming languages. However, ANNOTATIONS should not be confused with user COMMENTS (see section
[“User COMMENTS”](#user-comments)).

There could be a variety of constructs inside the ANNOTATION. For example, ANNOTATIONS may contain
RULES or descriptive text blocks. Requirements for the content of the ANNOTATION are defined by the
context, thus they are covered in other relevant sections.

By appearance, ANNOTATIONS can be of two types: *single-line* and *multi-line*.

By location, ANNOTATIONS can be of two types: *inline* and *interline*.

Accordingly, all ANNOTATIONS can be classified into four categories:

1. Inline single-line.
2. Inline multi-line.
3. Interline single-line.
4. Interline multi-line.

ANNOTATIONS cannot be placed anywhere in the text of the SCHEMA. The ability to input an ANNOTATION
in a particular place is determined by the context of that place. In other cases, only interline
ANNOTATIONS are allowed, in others only inline ANNOTATIONS, and in still others — annotations are
prohibited, and so on. For this reason, the ability to place ANNOTATION in one context or another is
covered in other relevant sections.

### Single-line ANNOTATIONS

> Requirement: req.jschema.annotation.oneline 0.2  
> Status: APPROVED 16.09.2021.

Single-line ANNOTATIONS begin with symbols `//` and continue till the end of the given line.

Example of single-line ANNOTATION:

```jsight
// Single-line annotation.
```

### Multi-line ANNOTATIONS

> Requirement: req.jschema.annotation.multiline 0.2  
> Status: APPROVED 16.09.2021.

Multi-line ANNOTATIONS begin with symbols `/*` and continue until closed with symbols `*/`.
Multi-line ANNOTATIONS can span from a single to several lines.

Examples of multi-line ANNOTATIONS:

```jsight
/* Multi-line annotation in one line. */

/* Multiline
annotation
in several lines. */
```

### Inline ANNOTATIONS

> Requirement: req.jschema.annotation.inline 0.2  
> Status: APPROVED 16.09.2021.

An ANNOTATION is called *inline* when some non-whitespace characters are present in the same line
before the symbol indicating the beginning of ANNOTATION. 

Examples of inline ANNOTATION:

```jsight
123 // {type: "integer"} - Inline single-line annotation.

123 /* {
        type: "integer"
        optional: true
      } - Inline multiline annotation. */
```

### Interline ANNOTATIONS

> Requirement: req.jschema.annotation.between_lines 0.2  
> Status: APPROVED 16.09.2021.

An ANNOTATION is called *interline* when there are no characters other than whitespace before the
symbol indicating the beginning of ANNOTATION.

Examples of interline ANNOTATION:

```jsight
[
    "BIG",
    // Interline single-line annotation.
    "SMALL"
]

[
    "BIG",
    /* Interline
    multi-line
    annotation. */
    "SMALL"
]
```

## User COMMENTS

> Requirement: req.jschema.comments 0.2  
> Status: APPROVED 06.01.2022.

You can leave random user COMMENTS anywhere in the SCHEMA. These COMMENTS are intended solely for
the developers of the SCHEMA. Unlike ANNOTATIONS, COMMENTS from the SCHEMA point of view have no
semantic meaning and are ignored when parsing SCHEMA.

There are two types of COMMENTS: *single-line* and *block*.

### Single-line COMMENT

> Requirement: req.jschema.comments.oneline 0.2  
> Status: APPROVED 06.01.2022.

A single-line COMMENT starts with the symbol `#`. After this symbol, the full subsequent line is a
COMMENT. The symbol `#` can appear either at the beginning or anywhere else in the line.

Examples of single-line COMMENTS:

```bash
# Single-line COMMENT

{
  "id": 5,
  "name": "John" # single-line COMMENT
}
```

The symbol `#` should not be interpreted as an opening symbol of a single-line COMMENT in the 
following cases: 

1. Inside a string literal in the EXAMPLE or in the RULE.
2. Inside a block COMMENT (see the section [“Block COMMENT”](#block-comment)).
3. Inside multi-line ANNOTATIONS (see the section
   [“Multi-line ANNOTATIONS”](#multi-line-annotations)).

### Block COMMENT

> Requirement: req.jschema.comments.multiline 0.2  
> Status: APPROVED 06.01.2022.

Block COMMENT opens with the symbol `###`. Thereafter, all subsequent lines are
considered a COMMENT until it is closed by the following symbol `###`.

COMMENTS cannot be nested inside each other.

Examples of block COMMENTS:

```jsight
###
Block
(multiline)
COMMENT
###

{
  "id": 5,
  "name": "John"
  ###
  Another
  block
  COMMENT
  ###
}
```

The symbols `###` should not be interpreted as the opening symbols of a block COMMENT in the
following cases:

1. Inside a string literal in the EXAMPLE or in the RULE.
2. Inside a block COMMENT (see the section
   [“Block COMMENT”](#block-comment)).
3. Inside multi-line ANNOTATIONS (see the section
   [“Multi-line ANNOTATIONS”](#multi-line-annotations)).
4. Inside single-line ANNOTATIONS (see the section  
   [“Single-line ANNOTATIONS”](#single-line-annotations)). Any symbol `#` inside single-line 
   ANNOTATIONS will be interpreted as the beginning of a single-line COMMENT.

## RULES

> Requirement: req.jschema.rules 0.1  
> Status: APPROVED 09.12.2019.

RULES allow to supplement the EXAMPLE and thereby specify additional requirements for the DOCUMENT.

One or several RULES are combined into groups of RULES. A group of RULES is drawn up in the form of
a JSON-like object (in the format [ECMA-262:
ECMAScript](https://www.ecma-international.org/ecma-262/5.1/#sec-7.4)) and is posted in ANNOTATIONS
inside the EXAMPLE, immediately after the opening symbol of a single-line or multi-line ANNOTATION.
A single ANNOTATION can contain only one group of RULES.

When the curly brace `{` is opened immediately after the opening symbol of ANNOTATION (excluding
whitespace), it means that the group of RULES has started. If there are non-whitespace characters
between the opening symbol of ANNOTATION and the symbol `{`, the construct will not be recognized as
a group of RULES. 

It is acceptable and even recommended not to quote the keys in the object describing a group of
RULES to improve the readability and conciseness of the record.

Example of a SCHEMA with the RULE:

```jsight
{
  "name": "John" // {minLength: 2}
}
```

Example of a SCHEMA with a group containing two RULES:

```jsight
{
  "name": "John" /* {minLength: 2, maxLength: 255} */
}
```

The position of the RULE in the EXAMPLE is important. The placement determines which exactly element
of the EXAMPLE the RULE refers to:

1. **RULES for arrays** — the ANNOTATION opening symbol should be placed in the EXAMPLE line
   containing the opening square bracket of the array;
2. **RULES for objects** — the ANNOTATION opening symbol should be placed in the EXAMPLE line
   containing opening curly brace of the object;
3. **RULES for PROPERTIES, PROPERTY KEYS, and PROPERTY VALUES of objects** — the ANNOTATION opening
   symbol should be placed in the EXAMPLE line containing the PROPERTY KEY of the object;
4. **RULES for array ELEMENTS** — the ANNOTATION opening symbol should be placed in the EXAMPLE line
   containing VALUE of an array element.
5. **RULES for a SCHEMA consisting of a single scalar VALUE** — should be placed in the EXAMPLE line
   containing the VALUE. 

:::note

*These five rules can be combined and used together. For example, if the PROPERTY VALUE of an
object, in turn, is an object, then to specify RULES for this child object, you need to use rules 1
and 3 simultaneously.*

:::

Example of a SCHEMA with the RULE for an array:

```jsight
{
  "array": [ /* {minItems: 1} */
    99
  ]
}
```

Example of a SCHEMA with the RULE for a scalar ARRAY ELEMENT:

```jsight
{
  "array": [
    99 // {min: 0}
  ]
}
```

Examples of RULES with different placements within ANNOTATIONS:

```jsight
{
  "key1": 2 // {min: 0}

  "key2": 2 /* {min: 0} */

  "key3": 2 /* {
                 min: 0 
               } */

  "key4": 2 /*
              {
                min: 0
              }
            */
}
```

The RULE cannot be placed on lines containing more than one EXAMPLE element to which the RULES may
apply.

For example, you can write a SCHEMA containing the following EXAMPLE:

```jsight
[1,2,3] # Valid SCHEMA without RULES
```

However, adding any RULE to such an EXAMPLE is impossible since it will be unclear to which element
of the EXAMPLE the RULE applies to:

```jsight
[1,2,3] // {min: 1} # ERROR!
```

Also, the following SCHEMA will be invalid since an array opening curly braces and the value of the
the first ELEMENT of an array is placed on a single-line (RULES can be applied to both elements). 

```jsight
[1] // {minItems: 1, min: 1} # ERROR!
```

It would be correct to place such elements on different lines. For example:

```jsight
[ // {minItems: 1}
  1 // {min: 1}
]
```

The sole exception to this principle is when the object's KEY and its VALUE are both found on the
same line.

Example of a valid RULE placement:

```jsight
{
  "key": 1 // {min: 1}
}
```

Another example of a valid RULE placement:

```jsight
{
  "key": {} // {optional: true}
}
```

Another example of a valid RULE placement:

```jsight
{
  "key": [ // {optional: true, minItems: 1}
    123
  ]
}
```

However, the following example contains an error since the EXAMPLE string may contain a key and
array opening square bracket, but not the VALUE of an array ELEMENT:

```jsight
{
  "key": [123] // {optional: true, minItems: 1} # ERROR !!! There can be no RULES in this line.
}
```

RULES should not contradict the EXAMPLE.

According to the JSON format, a valid JSON document can be an object, an array, or a scalar value.
For example, the number `1` is also a valid JSON document.

Example of a SCHEMA with the RULE for a SCHEMA consisting of a single scalar VALUE:

```jsight
  99 // {min: 0}
```

[Below](#rules) we will consider all the possible RULES.

### Text notes to RULES

> Requirement: req.jschema.rules.notes 0.1  
> Status: APPROVED 09.12.2019.

You can add a text note after the group of RULES in ANNOTATIONS. Notes to the RULES are used as
explanations for people, but they are not involved in the VALIDATION process.

To add a note after the group of RULES, following the last closing curly brace `}` of this group of
RULES, you need to place one or more whitespaces and the symbol "Hyphen" `-` (code 0x2D (0045)), and
then you can place a text note (whitespaces between the hyphen and the beginning of the note are
ignored).

Example of a SCHEMA with a note after the group of RULES:

```jsight
    {
      "number": 1 // {min: -99, max: 99} - Some note.
    }
```

Developers often omit groups of RULES because they accept values of all rules by default. There,
however, can still be a need to add a text note to the corresponding element in EXAMPLE. If the
group of RULES is omitted, the note should be placed immediately after the ANNOTATIONS opening
symbol (whitespaces between the ANNOTATION opening symbol and the beginning of the note will be
ignored).

Example of a SCHEMA with a note (in the case when the group of RULES is omitted):

```jsight
{
  "number": 1 // Some note.
}
```

Another example of a SCHEMA with a note in a multi-line ANNOTATION:

```jsight
{
  "number": 1 /* 
              Some note.
              */
}
```

:::note

*It is important to understand the difference between a note to the group of RULES and a user
COMMENT, indicated by symbols `#` or `###`. A note to the group of RULES refers to the corresponding
EXAMPLE element and and means the text documentation of that element. A user COMMENT is a random
comment from the SCHEMA developer, which has no special meaning according to the SCHEMA.*

:::

### RULE "additionalProperties"

> Requirement: req.jschema.rules.additionalProperties 0.2  
> Status: APPROVED.  

The RULE `additionalProperties` is required in order to impose requirements for the PROPERTIES of
object which were not explicitly described in the SCHEMA. For instance, you can disable additional
PROPERTIES in the object using random KEYS; you can just enable them; or you can enable them and
specify the TYPE of the PROPERTY VALUE, etc. 

The RULE `additionalProperties` is only applicable to objects.

In the RULE `additionalProperties`, you can specify either a boolean value, or a string. (For
example, `additionalProperties: true` or `additionalProperties: "@cat"`, or `additionalProperties:
"integer"`). The RULE is interpreted differently depending on the type of the value:

1. **The value of the RULE is `boolean`.**  
   In this case, the RULE means whether additional PROPERTIES are allowed in the object in general
   or not (for more information, see [below](#rule-additionalproperties-with-the-value-boolean)).
2. **The value of the RULE is `string`.**  
   In this case, the RULE states that the object is only allowed to have additional PROPERTIES with
   values of TYPE indicated in `additionalProperties` (for more information, see
   [below](#rule-additionalproperties-with-the-value-string)).

The RULE `additionalProperties` is set to `false` by default.

##### RULE "additionalProperties" with the value "boolean" 

> Requirement: req.jschema.rules.additionalProperties.boolean 0.2  
> Status: APPROVED 17.09.2021.

If the RULE `additionalProperties: true` is specified, it indicates that additional PROPERTIES with
any undeclared KEYS in the SCHEMA and any VALUES can be added to the corresponding object in the
DOCUMENT. 

If the RULE `additionalProperties: false` is specified, it indicates that additional PROPERTIES
cannot be added to the corresponding object in the DOCUMENT whose KEYS were not stated in the SCHEMA
of this object. I.e., the RULE `additionalProperties: false` meets the requirements for objects by
default.

Example:

```jsight
# Schema

{ // {additionalProperties: true}
  "id": 4,
  "name": "Kitty"
}

# Valid document
{
    "id"  : 123,
    "name": "Tom"
    "bla-bla-bla-bla-bla-bla": "Hurray, add whatever you want!"
}
```

##### RULE "additionalProperties" with the value "string"

> Requirement: req.jschema.rules.additionalProperties.string 0.2  
> Status: APPROVED 09.12.2019.

If a string is specified in the RULE `additionalProperties`, then this string should contain the
name of USER TYPE or STANDARD TYPE (except `decimal`, `enum`, and `mixed`).

In this case, the RULE `additionalProperties` states that additional PROPERTIES with any random KEYS
are allowed in addition to the explicitly described PROPERTIES in the object. Also, the value of
additional PROPERTIES must match the TYPE specified in `additionalProperties`.

Examples of a SCHEMA using the STANDARD TYPE `string` in `additionalProperties`.

```jsight
{ // {additionalProperties: "string"}
    "id": 1
}
```

Example of a VALID DOCUMENT corresponding to the given SCHEMA:

```jsight
{
    "id": 1, 
    "any_key": "any string", 
    "some_key": "some string", 
    "extra_key": "extra string"
}
```

Examples of a SCHEMA using the TYPE `any` in `additionalProperties`:

```jsight
{ // {additionalProperties: "any"}
    "id": 1
}
```

:::note

*The RULE `additionalProperties: "any"` has the same effect as the RULE `additionalProperties:
true`.*

:::

Example of a document corresponding to the given SCHEMA:

```jsight
{
    "id": 1,
    "any_key": true,
    "some_key": "value", 
    "extra_key": null
}
```

Examples of a SCHEMA using the USER TYPE in `additionalProperties`:

```jsight
{} // {additionalProperties: "@cat"}
```

Suppose, the USER TYPE `@cat` has the following SCHEMA:

```jsight
{
    "name": "Bob"
}
```

Then the following DOCUMENT will satisfy the given SCHEMA:

```jsight
{
    "myFirstCat" : {"name": "Tom" },
    "mySecondCat": {"name": "Loli"}
}
```

### RULE "allOf"

> Requirement: req.jschema.rules.allOf 0.1  
> Status: APPROVED 09.12.2019.

The RULE applies only to objects.

The RULE `allOf` allows you to extend the set of PROPERTIES of an object with the PROPERTIES of
another object (objects). In this case, any duplication of PROPERTIES should be considered as a
SCHEMA error.

Assume that we have a USER TYPE `@pet` which is specified by the following SCHEMA (for more
information about USER TYPES, see the section [“USER TYPES”](#user-types)):

```jsight
# USER TYPE @pet

{
  "petId": 123,
  "name" : "Tom"
}
```

This USER TYPE `@pet` can now be "inherited" in another USER TYPE `@cat` in the following way:

```jsight
# USER TYPE @cat

{ // {allOf: "@pet"}
  "favoriteFood": "MOUSE" // {enum: ["MOUSE", "MILK"]}
}
```

Example of a DOCUMENT that matches the given SCHEMA of TYPE `@cat`:

```jsight
{
  "petId": 4,
  "name" : "Marmalade",
  "favoriteFood" : "MILK"
}
```

The so-called "multiple inheritance" is allowed.

For example, assume we also have a USER TYPE `@astronaut`:

```jsight
# USER TYPE @austronaut

{
  "astronautId" : 223,
  "spaceSuitSize": "XXL" // {enum: ["S", "M", "L", "XL", "XXL"]}
}
```

Then we can create the object and inherit the properties `@pet` and `@astronaut` in the following
way:

```jsight

{ // {allOf: ["@pet", "@astronaut"]}
}

```
Example of a DOCUMENT that matches the given SCHEMA:

```jsight
{
  "petId"         : 123,
  "name"          : "Marmalade",
  "astronautId"   : 222,
  "spaceSuitSize" : "XXL"
}

```

### RULE "const"

> Requirement: req.jschema.rules.const 0.4  
> Status: APPROVED 14.02.2022.  

The rule `const` can be applied to any VALUE except arrays and objects.

The rule `const` is incompatible with USER TYPES, i.e., when the `type` contains some USER TYPE, for
example `type: "@catId"`.

Also, the RULE `const` is incompatible with the TYPE `mixed` and the TYPE `any`.

The rule `const` can take a value of type `boolean`. Furthermore, `const: true` means that the VALUE
in the DOCUMENT must exactly match the value specified in the EXAMPLE.

The value of the RULE `const: false` does not impose any additional requirements for the VALUE in
DOCUMENT.

The value of the RULE `const` is `false` by default.

Examples:

```jsight
# SCHEMA
{
    "responseCode": "OK" // {const: true}
}

# VALID DOCUMENT
{"responseCode": "OK"}

# INVALID DOCUMENT
{"responseCode": "FAIL"}
```

### RULE "enum"

> Requirement: req.jschema.rules.enum 0.3  
> Status: APPROVED 13.01.2022.

The RULE is used for VALUES of TYPE `enum`.

The RULE specifies a set of scalar JSON values (of types `string`, `number`, `boolean` or `null`).
Only a single value from this list can be used in a VALID DOCUMENT.

Example of a SCHEMA:

```jsight
{
  "data": 3 /* { enum: [
                  1.2, 
                  3, 
                  "abc", 
                  true, 
                  false, 
                  null]} */
}
```

Only the RULES `type`, `optional` and `nullable` can be used together with `enum` in one group of
RULES. Other RULES cannot be used. In this case, the RULE `type` can only have the value `enum`,
since the RULE `enum` automatically implies `type: "enum"`.

An integer and a floating point number are considered different values for `enum`. For example, the
following SCHEMA contains an error:

```jsight
{
  "data": 2.0 // {enum: [2]} # ERROR!
}
```

### RULE "exclusiveMaximum"

> Requirement: req.jschema.rules.exclusiveMaximum 0.1  
> Status: APPROVED 09.12.2019.

The RULE applies to VALUES of TYPES `integer`, `float` and `decimal`.

It can be either `true` or `false`. By default, it is `false`.

If the value is `true`, then the value of the RULE `max` (specified in the same group of RULES) is
excluded from the valid range of numeric values.

Example of a SCHEMA: 

```jsight
{
  "data": 1 // {max: 2, exclusiveMaximum: true}
}
```

### RULE "exclusiveMinimum"

> Requirement: req.jschema.rules.exclusiveMinimum 0.1  
> Status: APPROVED 09.12.2019.

The RULE applies to VALUES of TYPES `integer`, `float` and `decimal`.

It can be either `true` or `false`. By default, it is `false`.

If the value is `true`, then the value of the RULE `min` (specified in the same group of RULES) is
excluded from the valid range of numeric values.

Example of a SCHEMA:

```jsight
{
  "data": 1 // {min: 0, exclusiveMinimum: true}
}
```

### RULE "max"

> Requirement: req.jschema.rules.max 0.1  
> Status: APPROVED 09.12.2019.

The RULE applies to VALUES of TYPES `integer`, `float` and `decimal`.

It defines the maximum numeric value.

Example of a SCHEMA:

```jsight
{
  "data": 1.2 // {max: 3.4}
}
```

### RULE "maxItems"

> Requirement: req.jschema.rules.maxItems 0.1  
> Status: APPROVED 09.12.2019.

The RULE applies only to arrays.

It defines the maximum number of elements in an array (inclusive). The property `maxItems` is not
set by default (the maximum number of elements in the array is not limited).

Example of a SCHEMA:

```jsight
{
  "data": [ // {maxItems: 10}
    1
  ]
}
```

The given SCHEMA states that an array can only have 10 or fewer elements of type `integer`. 

### RULE "maxLength"

> Requirement: req.jschema.rules.maxLength 0.1  
> Status: APPROVED 09.12.2019.

The RULE applies to VALUES of TYPE `string`.

It defines the maximum length of a string (inclusive). The RULE `maxLength` is not set by default
(maximum string length is not limited).

Example of a SCHEMA:

```jsight
{
  "data": "Any string" // {maxLength: 255}
}
```

### RULE "min"

> Requirement: req.jschema.rules.min 0.1  
> Status: APPROVED 09.12.2019.

The RULE applies to VALUES of TYPES `integer`, `float` and `decimal`.

It defines the minimum numeric value.

Example of a SCHEMA:

```jsight
{
  "data": 1 // {min: 0}
}
```

### RULE "minItems"

> Requirement: req.jschema.rules.minItems 0.1  
> Status: APPROVED 09.12.2019.

The RULE applies only to arrays.

It defines the minimum number of elements in an array (inclusive). The default value is 0.

Example of a SCHEMA:

```jsight
{
  "data": [ // {minItems: 1}
    1
  ] 
}
```

The given SCHEMA states that an array can have one or more ELEMENTS of TYPE `integer`. 

### RULE "minLength"

> Requirement: req.jschema.rules.minLength 0.1  
> Status: APPROVED 09.12.2019.

The RULE applies to VALUES of TYPE `string`.

It defines the minimum string length (inclusive). The default value is 0.

Example of a SCHEMA:

```jsight
{
  "data": "Any string" // {minLength: 3}
}
```

### RULE "nullable"

> Requirement: req.jschema.rules.nullable 0.1  
> Status: APPROVED 09.12.2019.

The RULE applies to any VALUES.

It determines whether the VALUE can be null.

The RULE `nullable` can be either `true` or `false`. By default, it is `false`.

Example of a SCHEMA:

```jsight
{
  "data": 1 // {nullable: true}
}
```

Example of a valid DOCUMENT:

```jsight
{
  "data": null
}
```

### RULE "optional"

> Requirement: req.jschema.rules.optional 0.1  
> Status: APPROVED 09.12.2019.

The RULE determines whether or not a property of the object can be missing.

The RULE applies only to the PROPERTIES of objects.

The RULE `optional` can be either `true` or `false`. By default, it is `false`.

Example of a SCHEMA:

```jsight
{
  "data": 1 // {optional: true}
}
```

### RULE "or"

> Requirements:
>
> - req.jschema.rules.or 0.4  
>   Status: APPROVED 09.05.2024.  
> - req.jschema.rules.or.type_shortcuts 0.1  
>   Status: APPROVED 09.12.2019.

The RULE for VALUES of TYPE `mixed`.

The RULE `or` sets a list (an array) of groups of RULES. In the list `or`, you can specify a string
with a name of the TYPE (STANDARD or USER) instead of a group of RULES. However, the VALUE in the
DOCUMENT must match at least one of these sets of RULES or one of the specified TYPES. The VALUE in
the EXAMPLE must also match one of the sets of RULES or one of the TYPES.

Example of a SCHEMA with the RULE `or` containing a list of groups of RULES:

```jsight
{
  "data": "abc" /* {or: [
                     {type: "string" , maxLength: 3}, 
                     {type: "integer", min: 0}
                   ]} */
}
```

Example of a SCHEMA with the RULE `or` containing a list of groups of RULES which specify USER
TYPES:

```jsight
{
  "data": 123 /* {or: [
                       {type: "@dogId"}, 
                       {type: "@catId"}
                     ]} */
}
```

Example of a SCHEMA, similar to the previous example, specifies TYPE names in the shorthand form
instead of the group of rules.

```jsight
{
  "data": 123 // {or: ["@dogId", "@catId"]}
}
```

Example of a SCHEMA with the RULE `or` containing both, the shorthand form of the TYPE name and the
group of RULES:

```jsight
{
  "data": "abc" /* {or: [
                     "@cat", 
                     {type: "string", maxLength: 3}
                   ]} */
}
```

The TYPE must always be specified in the group of RULES in the `or` RULE. This will cause an error:

```jsight
{
  "data": 123 /* {or: [
                   {min: 100},        # ERROR: the `type` rule must be specified!
                   {type: "string"}
                 ]} */
}
```

:::caution IMPORTANT! 

While using the RULE `or` and the TYPE `mixed`, the value of the EXAMPLE **cannot contain an object,
an array or a reference to one or more USER TYPES** (for more information, see the section
[“References to USER TYPES in the EXAMPLE”](#references-to-user-types-in-the-example)). 

:::

Examples of violations of this rule:

```jsight
{
  "myPet1" : { // {or: ["@cat", "@dog"]}            # --ERROR! It is wrong..
    "id"   : 1,
    "name" : "Tom"
  },
  "myPets": [ // {or: ["@catList", "@dogList"]}     # --ERROR! It is wrong. .
    @cat
  ],
  "myPet3" : @cat,       // {or: ["@cat", "@dog"]}  # --ERROR! It is wrong.
  "myPet4" : @cat | @dog // {or: ["@cat", "@dog"]}  # --ERROR! It is wrong.
}
```

To specify the option in the SCHEMA to select one of the USER TYPES in the VALUE, you should use
references to USER TYPES in the EXAMPLE instead of the RULE `or` (see the section [“References to
multiple USER TYPES in the EXAMPLE VALUE”](#reference-to-the-user-type-in-the-example-value)). For
example: 

```jsight
{
  "myPet" : @cat | @dog,
  "myPets": @catList | @dogList
}
```

Only RULES `type`, `optional` and `nullable` can be used in one group of RULES together with `or`
(other RULES cannot be used). Also, the RULE `type` can only have one value which is `mixed` since
the RULE `or` automatically implies `type: "mixed"`.

Example of a SCHEMA with `type`, `optional` and `nullable`:

```jsight
{
  "data": 123 /* {
                  type: "mixed",
                  or: [{type: "string"}, {type: "integer"}],
                  optional: true,
                  nullable: true
                } */
}
```

### RULE "precision"

> Requirement: req.jschema.rules.precision 0.1  
> Status: APPROVED 09.12.2019.

The RULE applies to VALUES of TYPE `decimal`.

It defines the maximum number of decimal places that a number can have after the decimal point
(cannot be negative).

Example of a SCHEMA:

```jsight
{
  "data": 0.12 // {precision: 2}
}
```

### RULE "regex"

> Requirement: req.jschema.rules.regex 0.3  
> Status: APPROVED 14.02.2022.

The RULE applies to VALUES of TYPES `string`, `email`, `uri`, `date` and `datetime`.

It defines the regex that the string should match.

Example of a SCHEMA:

```jsight
{
  "data": "Any string" // {regex: "[A-Za-z ]+"}
}
```

### RULE "type"

> Requirement: req.jschema.rules.type 0.1  
> Status: APPROVED 06.12.2021.

The RULE `type` explicitly specifies the TYPE of the VALUE.

The RULE applies to scalar VALUES, objects or arrays. 

Possible values:

- `object`,
- `array`,
- `integer`,
- `float`,
- `decimal`,
- `boolean`,
- `string`,
- `email`,
- `uri`,
- `date`,
- `datetime`,
- `uuid`,
- `enum`,
- `mixed`,
- `any`,
- `null`,
- name of any USER TYPE.

It is not recommended to explicitly specify the TYPES `object`, `array`, `integer`, `float`,
`decimal`, `boolean`, `string`, since it is enough to look at the corresponding value in the EXAMPLE
to specify these types (see the section [“Correspondence of the EXAMPLE data TYPES and the
SCHEMA”](#correspondence-between-example-and-schema-data-types)). It is also not recommended to
explicitly specify the types `enum` and `mixed`, since it is enough to look at other RULES to
specify these types (`enum` and `or`).

Example of a SCHEMA with explicit `type`:

```jsight
{
  "contact": "name@domain.com" // {type: "email"}
}
```

Example of a SCHEMA without defining `type` which has the value `type: mixed` by default, as there
is the RULE `or` in the group of RULES:

```jsight
{
  "data": "abc" // {or: [{type: "string"}, {type: "integer"}]}
}
```

#### Specifying the USER TYPE in the RULE "type"

> Requirement: req.jschema.rules.type.reference 0.3  
> Status: APPROVED 13.01.2022.

You can specify not only a STANDARD TYPE, but also a USER TYPE in the RULE `type` (for more
information, see the section [“USER TYPES”](#user-types)).

For example:

```jsight
{
  "passport": "AA 234555" // {type: "@passportNumber"}
}
```

Such a SCHEMA states that the VALUE of the PROPERTY `passport` must meet the requirements specified
in a USER TYPE named `@passportNumber`.

:::caution IMPORTANT!

If the USER TYPE is specified in the RULE `type`, then **an object or an array, or a
reference to the USER TYPE cannot be specified** in the EXAMPLE. 

:::

For example, you cannot write as follows:  

```jsight
{
  "myCat": { // {type: "@cat"}  # --ERROR! It is wrong! 
    "id"  : 123,
    "name": "Tom"
  }
}
```

It is also not correct to write as follows: 

```jsight
{
  "myCatList": [ // {type: "@catList"}  # -- ERROR! It is wrong! 
    @cat
  ]
}
```

It is also not correct to write as follows: 
```jsight
{
  "myCat": @cat // {type: "@cat"}  # -- ERROR! It is wrong! 
}
```

If the USER TYPE is specified as a value in the RULE `type`, then only RULES `optional` and
`nullable` can be used in conjunction with this `type` in one group of RULES. Other rules cannot be
used.

#### TYPE "any"

> Requirement: req.jschema.rules.type.any 0.1  
> Status: APPROVED 09.12.2019.

The TYPE `any` accepts any VALUE of any TYPE.

Example of a SCHEMA:

```jsight
{
  "data": 1 // {type: "any"}
}
```

#### TYPE "array"

> Requirement: req.jschema.rules.type.array 0.1  
> Status: APPROVED 09.12.2019.

The TYPE `array` requires an array to be in the VALUE.

Example of a SCHEMA:

```jsight
{
  "data": [ // {type: "array"}
    "element1",
    "element2"
  ]
}
```

You can always omit the RULE `type: "array"`, since the TYPE `array` is implicitly specified by the
array in the EXAMPLE:

```jsight
{
  "data": [
    "element1",
    "element2"
  ]
}
```

#### TYPE "boolean"

> Requirement: req.jschema.rules.type.boolean 0.1  
> Status: APPROVED 09.12.2019.

The TYPE `boolean` requires the VALUE to be either `true` or `false`.

Example of a SCHEMA:

```jsight
{
  "data": true // {type: "boolean"}
}
```

You can always omit the RULE `type: "boolean"`, since the TYPE `boolean` is implicitly specified by
the boolean VALUE in the EXAMPLE:

```jsight
{
  "data": true
}
```

#### TYPE "date"

> Requirement: req.jschema.rules.type.date 0.2  
> Status: APPROVED 04.01.2022.

When the RULE `type` has the value `date`, it is assumed that the VALUE in the DOCUMENT must be a
string containing the date as defined by "full-date" in
[RFC3339](https://xml2rfc.ietf.org/public/rfc/html/rfc3339.html#anchor14).

Example of a schema with the type `date`: 

```jsight
{
  "data": "2006-01-02" // {type: "date"}
}
```

#### TYPE "datetime"

> Requirement: req.jschema.rules.type.datetime 0.3  
> Status: APPROVED 17.09.2021.

When the RULE `type` has the value `datetime`, it is assumed that the VALUE in the DOCUMENT must be
a string containing the date and time as defined by "date-time" in
[RFC3339](https://xml2rfc.ietf.org/public/rfc/html/rfc3339.html#anchor14).

Example of a schema with the type `datetime`: 

```jsight
{
  "data": "2006-01-02T15:04:05+07:00" // {type: "datetime"}
}
```

#### TYPE "decimal"

> Requirement: req.jschema.rules.type.decimal 0.1  
> Status: APPROVED 09.12.2019.

The TYPE `decimal` requires a fixed-precise number to be in the VALUE (with a certain number of
digits after the decimal point).

If the TYPE `decimal` is specified in RULES, then the RULE `precision`, which sets the required
precision in the number of decimals (see the section [“RULE precision”](#rule-precision)), must be
specified in the same group of RULES.

Example of a SCHEMA:

```jsight
{
  "data": 0.12 // {type: "decimal", precision: 2}
}
```

You can omit the RULE `type: "decimal"` since the RULE `precision` implicitly specifies the TYPE
`decimal`:

```jsight
{
  "data": 0.12 // {precision: 2}
}
```

#### TYPE "email"

> Requirement: req.jschema.rules.type.email 0.1  
> Status: APPROVED 09.12.2019.

When the RULE `type` has the value `email`, it is assumed that the VALUE in the DOCUMENT must be a
string containing a valid Email address. (see [RFC 5322, section
3.4.1](https://tools.ietf.org/html/rfc5322#section-3.4.1)).

Example of a SCHEMA:

```jsight
{
  "data": "tom@cats.com" // {type: "email"}
}
```

#### TYPE "enum"

> Requirement: req.jschema.rules.type.enum 0.1  
> Status: APPROVED 09.12.2019.

The TYPE `enum` indicates that the VALUE in the DOCUMENT can only be one of a list of valid values,
which are specified in the RULE `enum` (see the section [“RULE "enum"”](#rule-enum)). 

If the TYPE `type: "enum"` is specified, then this group of RULES must also contain the RULE `enum`.

Example of a SCHEMA:

```jsight
{
  "color": "white" // {type: "enum", enum: ["white", "blue", "red"]}
}
```

You can always omit the RULE `type: "enum"` since the RULE `enum` implicitly specifies the TYPE
`enum`:

```jsight
{
  "color": "white" // {enum: ["white", "blue", "red"]}
}
```

#### TYPE "float"

> Requirement: req.jschema.rules.type.float 0.1  
> Status: APPROVED 09.12.2019.

The TYPE `float` requires an integer or fractional number to be in the VALUE.

Example of a SCHEMA:

```jsight
{
  "data": 36.6 // {type: "float"}
}
```

You can omit the RULE `type: "float"`, since the TYPE `float` is implicitly specified by the
fractional number in the EXAMPLE:

```jsight
{
  "data": 36.6
}
```

#### TYPE "integer"

> Requirement: req.jschema.rules.type.integer 0.1  
> Status: APPROVED 09.12.2019.

The TYPE `integer` requires an integer to be in the VALUE.

Example of a SCHEMA:

```jsight
{
  "data": 123 // {type: "integer"}
}
```

You can always omit the RULE `type: "integer"`, since the TYPE `integer` is implicitly specified by
the VALUE in the EXAMPLE:

```jsight
{
  "data": 123
}
```

#### TYPE "mixed"

> Requirement: req.jschema.rules.type.mixed 0.1  
> Status: APPROVED 09.12.2019.

The TYPE `mixed` indicates that the VALUE in the DOCUMENT must satisfy one of the sets of RULES
listed in the RULE `or` (see the section [“RULE "or"”](#rule-or)).

If the TYPE `type: "mixed"` is specified, then this group of RULES must also contain the RULE `or`.

Example of a SCHEMA:

```jsight
{
  "data": 123 // {type: "mixed", or: [{type: "integer"}, {type: "string"}]}
}
```

You can always omit the RULE `type: "mixed"` since the RULE `or` implicitly specifies the TYPE
`mixed`:

```jsight
{
  "data": 123 // {or: [{type: "integer"}, {type: "string"}]}
}
```

For more information about the TYPE `mixed`, see the section [“RULE "or"”](#rule-or).

#### TYPE "null"

> Requirement: req.jschema.rules.type.null 0.1  
> Status: APPROVED 09.12.2019.

The TYPE `null` requires null to always be in the VALUE.

Example of a SCHEMA:

```jsight
{
  "data": null // {type: "null"}
}
```

You can always omit the RULE `type: "null"`, since the TYPE `null` is implicitly specified by
the null VALUE in the EXAMPLE:

```jsight
{
  "data": null // {type: "null"}
}
```

#### TYPE "object"

> Requirement: req.jschema.rules.type.object 0.1  
> Status: APPROVED 09.12.2019.

The TYPE `object` requires an object to be in the VALUE.

Example of a SCHEMA:

```jsight
{
  "data": { // {type: "object"}
    "someKey": "someValue"
  }
}
```

You can always omit the RULE `type: "object"`, since the TYPE `object` is implicitly specified by
the object in the EXAMPLE:

```jsight
{
  "data": {
    "someKey": "someValue"
  }
}
```

#### TYPE "string"

> Requirement: req.jschema.rules.type.string 0.1  
> Status: APPROVED 09.12.2019.

The TYPE `string` requires a string to be in the VALUE.

Example of a SCHEMA:

```jsight
{
  "name": "Tom" // {type: "string"}
}
```

You can always omit the RULE `type: "string"`, since the TYPE `string` is implicitly specified by
the VALUE in the EXAMPLE:

```jsight
{
  "name": "Tom"
}
```

#### TYPE "uri"

> Requirement: req.jschema.rules.type.uri 0.1  
> Status: APPROVED 09.12.2019.

When the RULE `type` has the value `uri`, it is assumed that the VALUE in the DOCUMENT must be a
string containing a valid URI (see [RFC 3986](https://tools.ietf.org/html/rfc3986)).

Example of a SCHEMA:

```jsight
{
  "data": "http://cats.com" // {type: "uri"}
}
```

#### TYPE "uuid"

> Requirement: req.jschema.rules.type.uuid 0.1  
> Status: APPROVED 09.12.2019.

When the RULE `type` has the value `uuid`, it is assumed that the DOCUMENT contains a string
containing a UUID.

Example of a SCHEMA:

```jsight
{
  "data": "550e8400-e29b-41d4-a716-446655440000" // {type: "uuid"}
}
```

## Named SCHEMA elements

### USER TYPES

The JSight Schema language extensively uses the USER TYPE mechanism. 

The USER TYPE is the TYPE that the user describes in a distinct SCHEMA and explicitly names this
TYPE.

All USER TYPE names must start with the symbol `@`. For example, `@cat`.

Unlike USER TYPES, STANDARD TYPES which are built into the language itself, never start with the
symbol `@`. For example, `string`, `email`.

USER TYPE names may be referenced, for example, in the following RULES:

- `type`,
- `or`,
- `additionalProperties`,
- `allOf`.

There is currently no way to declare and describe USER TYPES in the JSight Schema language. (This is
due to the fact that there is no ATTRIBUTE mechanism for declaring USER TYPES in this language
version). However, in the current version of the JSight Schema language, this is not required since
this version is not intended for standalone use, but rather as an integral part of the [JSight API
language](jsight-api-0-3), which has its own way of declaring USER TYPES. In this regard, we will
use the syntax of the [JSight API language](jsight-api-0-3) for further clarification.

Let's consider the following example (in the [JSight API language](jsight-api-0-3) using SCHEMAS of
the JSight Schema):

```jsight
TYPE @catId
  "CAT-123" // {regex: "CAT-\\d+"}

TYPE @cat
{
  "id" : "CAT-123" // {type: "@catId"}
}
```

In the given example, the VALUE of the PROPERTY `id` refers to the USER TYPE `@catId`. This means
that in the DOCUMENT, the VALUE of the PROPERTY `id` must match all the requirements imposed by the
SCHEMA of USER TYPE `@catId` (for more information, see the section [“References to USER TYPES in
the example”](#references-to-user-types-in-the-example)).

## References to USER TYPES in the EXAMPLE 

> Requirement: req.jschema.shortcuts 0.3  
> Status: APPROVED 23.09.2021.

The JSight Schema language allows you to directly refer to USER TYPES from the EXAMPLE. This section
describes all possible options of such references.

### Reference to the USER TYPE in the EXAMPLE VALUE

> Requirement: req.jschema.shortcuts.value_type 0.4  
> Status: APPROVED 13.01.2022.

If some VALUE in the EXAMPLE has some USER TYPE, then the name of this USER TYPE can be specified
directly in the EXAMPLE instead of this VALUE.

**Example 1.** A reference from the VALUE of the object PROPERTY:

```jsight
{
  "myCat": @cat
}
```

The SCHEMA specified in the given example requires the VALUE of the PROPERTY `myCat` to match the
value of the USER TYPE `@cat`.

**Example 2.** A reference from the VALUE of the array ELEMENT:


```jsight
{
  "myCatList": [
    @cat
  ]
}
```

In the given example, the SCHEMA defines that the property `myCatList` should contain an array of
ELEMENTS, the VALUE of which should match the USER TYPE `@cat`.

**Example 3.** A reference from the VALUE of the SCHEMA root element:

```jsight
@cat
```

The given example shows a SCHEMA that consists entirely of just one reference to the USER TYPE
`@cat`. Such a schema requires that the DOCUMENT completely matches the USER TYPE `@cat`.

It should be noted that from the EXAMPLE, you can directly refer only to USER TYPES and not to
STANDARD TYPES (`integer`, `string`, etc.).

In such a case where the VALUE of the EXAMPLE refers to the USER TYPE, only RULES `optional` and
`nullable` can be applied to this VALUE. Other rules cannot be used. For example:


```jsight
{
  "myCat": @cat // {optional: true, nullable: true}
}
```

:::note

*From the internal point of view of the JSight language, the reference to USER TYPE implicitly
sets the RULE `type` for the corresponding VALUE. This results in the following effect: If the USER
TYPE referenced by EXAMPLE is scalar (not an object or an array), then the same schema can be
written in a different way by using the RULE `type`.

:::

For example, the following two SCHEMAS are essentially the same (have the same requirements):

```jsight
# Schema with a reference to the USER TYPE in the EXAMPLE

{
  "size": @catSizeEnum
}

# Schema with a reference to the USER TYPE in the RULE `type`

{
  "size": "XXL" // {type: "@catSizeEnum"}   # @catSizeEnum — scalar type.
}
```

### Reference to several USER TYPES in the VALUE of the EXAMPLE 

> Requirement: req.jschema.shortcuts.value_or 0.4  
> Status: APPROVED 13.01.2022.

If some VALUE in the EXAMPLE can match one of several USER TYPES, then the names of these USER TYPES
can be specified directly in the EXAMPLE instead of this VALUE. Names of USER TYPES should be placed
on the same string, start with the symbol `@`, and be separated by a vertical line. There must be at
least one whitespace between the names of TYPES and the vertical lines.

Example of a reference to several USER TYPES in the PROPERTY VALUE of an object:

```jsight
{
  "myPet": @cat | @dog | @pig
}
```

The SCHEMA specified in the given example requires the VALUE of the PROPERTY `myPet` to match the
USER TYPE `@cat` or the USER TYPE `@dog`, or the USER TYPE `@pig`.

Example of a reference to several USER TYPES in the VALUE of an array ELEMENT:

```jsight
[
  @cat | @dog | @frog
]
```

Example of a reference to several USER TYPES in the VALUE of the SCHEMA root:

```jsight
@cat | @dog | @frog
```

STANDARD TYPES (`integer`, `string`, etc.) cannot be specified in the USER TYPE list.

In the case where the VALUE of the EXAMPLE refers to multiple USER TYPES, only RULES `optional` and
`nullable` can be applied to this VALUE. Other rules cannot be used. For example:

```jsight
{
  "myPet": @cat | @dog // {optional: true, nullable: true}
}
```

:::note

*According to the internal logic of the JSight language Schema, a reference to several USER TYPES
implicitly sets the RULE `type: mixed` and the RULE `or` in which only names of USER TYPES are
listed (for more information, see the section [“RULE `or`”](#rule-or)). This results in the
following effect: If the VALUE contains a reference to several USER TYPES that are scalar (not
objects and not arrays), then the same SCHEMA can be written using the RULE `or`.*

:::

For example, the following SCHEMAS are equivalent:

```jsight
# SCHEMA with a reference to several USER TYPES in the VALUE:

{
  "myPetId": @catId | @dogId
}

# SCHEMA with a reference to several USER TYPES in the RULE `or`: 

{
  "myPetId": "CAT-123" // {or: ["@catId", "@dogId"]}
}
```

### Reference to the USER TYPE in the PROPERTY KEY: 

> Requirement: req.jschema.shortcuts.key_type 0.3  
> Status: APPROVED 06.12.2021.

If some PROPERTY KEY in the EXAMPLE matches some USER TYPE, then the name of this USER TYPE can be
specified directly in the EXAMPLE instead of the PROPERTY KEY. Name of the TYPE should be specified
without quotes and start with the symbol `@`.

A reference in the PROPERTY NAME can only be applied to USER TYPES, and cannot be applied to
STANDARD TYPES (`uuid`, `email`, etc.).

The USER TYPE should not contradict the STANDARD TYPE `string`, since the KEY is in any case
represented as a string.

Example of a reference to the USER TYPE in the PROPERTY KEY:

```jsight
{
    @catsEmail : @cat
}
```

:::note

*According to the internal logic of the JSight language Schema, a reference to the USER TYPE
from the PROPERTY KEY means an implicit specification of the RULE `key.type`. Right now, the RULE
`key` is not included in the current language version, but it will be added in future versions.*

:::

## Edit history

|    Date    |  JSight Schema Version | Document version | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |        Author       |
| :--------: | :--------------------: |:----------------:|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------| :-----------------: |
| 05.09.2024 |     0.3.6              |    0.9.21-en     | Minor fixes in `additionalProperties` and `or` rules.                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Konstantin Malyshev |
| 14.02.2022 |     0.3.5              |    0.9.20-en     | <ul><li>Prohibiting on using numbers in exponent notation in the EXAMPLE of the SCHEMA.</li><li>Minor improvements to COMMENTS.</li><li>The RULE `regex` now applies to VALUES of TYPES `string`, `email`, `uri`, `date` and `datetime`.</li><li>The RULE `const` is incompatible with the TYPE `any`.</li></ul>  The following requirements were changed: <ul><li>req.jschema.example.number 0.1 to 0.2,</li><li>req.jschema.rules.regex 0.2 to 0.3,</li><li>req.jschema.rules.const 0.3 to 0.4.</li></ul> | Andrei Ivanov       |
| 13.01.2022 |     0.3.4              |    0.9.19-en     | The RULE `nullable: true` is allowed in conjunction with user type, type `enum` and type `mixed`. The following requirements were changed:<ul><li>req.jschema.rules.type.reference 0.2 to 0.3,</li><li>req.jschema.rules.or 0.2 to 0.3,</li><li>req.jschema.rules.enum 0.2 to 0.3,</li>  <li>req.jschema.shortcuts.value_type 0.3 to 0.4,</li><li>req.jschema.shortcuts.value_or 0.3 to 0.4.</li></ul>                                                                                                      | Konstantin Malyshev |
| 07.01.2022 |     0.3.3              |    0.9.18-en?    | Some fixes in format.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Konstantin Malyshev |
| 04.01.2022 |     0.3.3              |    0.9.17-en?    | Requirement changed: req.jschema.rules.type.datetime (new version 0.3). Requirement added: req.jschema.rules.type.date 0.2.                                                                                                                                                                                                                                                                                                                                                                                 | Konstantin Malyshev |
| 04.01.2022 |     0.3.2              |    0.9.16-en     | Fix format for docusaurus.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Konstantin Malyshev |
| 28.12.2021 |     0.3.2              |    0.9.15-en     | Translation in English.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | Konstantin Malyshev |
| 14.12.2021 |     0.3.2              |      0.9.15      | Minor clarifications of wordings within the same content.                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Konstantin Malyshev |
| 09.12.2021 |     0.3.2              |      0.9.14      | Errors in the previous version have been fixed. The version number of the language has not been changed as the document has not yet entered development.                                                                                                                                                                                                                                                                                                                                                    | Konstantin Malyshev |
| 06.12.2021 |     0.3.2              |      0.9.13      | <p>The ability to use stubs [] and &#123;&#125; for USER TYPES is removed. Due to this, new versions of requirements are as follows: </p><ul>  <li>req.jschema.rules.type.reference 0.2,</li>  <li>req.jschema.rules.or 0.2,</li>  <li>req.jschema.shortcuts 0.3,</li>  <li>req.jschema.shortcuts.value_type 0.3,</li>  <li>req.jschema.shortcuts.value_or 0.3,</li>  <li>req.jschema.shortcuts.key_type 0.3.</li></ul>                                                                                     | Konstantin Malyshev |
| 19.11.2021 |     0.3.1              |      0.9.12      | Adding TODO comments.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Konstantin Malyshev |
| 02.11.2021 |     0.3.1              |      0.9.11      | The requirement req.jschema.rules.const (the RULE `const`) has been clarified, the new requirement version is 0.3. A typo in the rule `regex` has been fixed (before it was `regexp`) — new requirement is req.jschema.rules.regex 0.2. Minor edits beside the point.                                                                                                                                                                                                                                       | Konstantin Malyshev |
| 19.10.2021 |     0.3.0              |      0.8.10      | Correcting typos.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | Konstantin Malyshev |
| 13.10.2021 |     0.3.0              |      0.8.9       | Correcting typos.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | Konstantin Malyshev |
| 13.10.2021 |     0.3.0              |      0.8.8       | Renaming languages to JSight Schema and JSight API. Changing the name of the FORMAT `htmlFormEncoded`. The language version was left the same since the changes had not yet been developed at that time.                                                                                                                                                                                                                                                                                                    | Konstantin Malyshev |
| 23.09.2021 |     0.3.0              |      0.8.7       | Many changes have been made based on the testing results.                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Konstantin Malyshev |
| 09.12.2019 |     0.2.1              |      0.7.4       | The first language version that was approved for the development of a validator prototype.                                                                                                                                                                                                                                                                                                                                                                                                                  |   Andrey Ivanov    |

## APPENDIX 1. A table of all built-in types and rules

The table below shows all types as well as all of the rules that can be applied to them.

| Type                         | Rules for this type |
| ---------------------------- | ------------------- |
| [`any`](#type-any)           | [`optional`](#rule-optional), [`nullable`](#rule-nullable) |
| [`array`](#type-array)       | [`optional`](#rule-optional), [`nullable`](#rule-nullable), [`minItems`](#rule-minitems), [`maxItems`](#rule-maxitems) |
| [`boolean`](#type-boolean)   | [`optional`](#rule-optional), [`nullable`](#rule-nullable), [`const`](#rule-const) |
| [`date`](#type-date)         | [`optional`](#rule-optional), [`nullable`](#rule-nullable), [`const`](#rule-const), [`regex`](#rule-regex) |
| [`datetime`](#type-datetime) | [`optional`](#rule-optional), [`nullable`](#rule-nullable), [`const`](#rule-const), [`regex`](#rule-regex) |
| [`decimal`](#type-decimal)   | [`optional`](#rule-optional), [`nullable`](#rule-nullable), [`const`](#rule-const), [`min`](#rule-min), [`max`](#rule-max), [`exclusiveMinimum`](#rule-exclusiveminimum), [`exclusiveMaximum`](#rule-exclusivemaximum), [`precision`](#rule-precision)         |
| [`email`](#type-email)       | [`optional`](#rule-optional), [`nullable`](#rule-nullable), [`const`](#rule-const), [`regex`](#rule-regex) |
| [`enum`](#type-enum)         | [`optional`](#rule-optional), [`nullable`](#rule-nullable), [`const`](#rule-const), [`enum`](#rule-enum) |
| [`float`](#type-float)       | [`optional`](#rule-optional), [`nullable`](#rule-nullable), [`const`](#rule-const), [`min`](#rule-min), [`max`](#rule-max), [`exclusiveMinimum`](#rule-exclusiveminimum), [`exclusiveMaximum`](#rule-exclusivemaximum)                    |
| [`integer`](#type-integer)   | [`optional`](#rule-optional), [`nullable`](#rule-nullable), [`const`](#rule-const), [`min`](#rule-min), [`max`](#rule-max), [`exclusiveMinimum`](#rule-exclusiveminimum), [`exclusiveMaximum`](#rule-exclusivemaximum) |
| [`mixed`](#type-mixed)       | [`optional`](#rule-optional), [`nullable`](#rule-nullable), [`or`](#rule-or) |
| [`null`](#type-null)         | [`optional`](#rule-optional), [`nullable`](#rule-nullable), [`const`](#rule-const) |
| [`object`](#type-object)     | [`optional`](#rule-optional), [`nullable`](#rule-nullable), [`additionalProperties`](#rule-additionalproperties), [`allOf`](#rule-allof) |
| [`string`](#type-string)     | [`optional`](#rule-optional), [`nullable`](#rule-nullable), [`const`](#rule-const), [`minLength`](#rule-minlength), [`maxLength`](#rule-maxlength), [`regex`](#rule-regex)     |
| [`uri`](#type-uri)           | [`optional`](#rule-optional), [`nullable`](#rule-nullable), [`const`](#rule-const), [`regex`](#rule-regex) |
| [`uuid`](#type-uuid)         | [`optional`](#rule-optional), [`nullable`](#rule-nullable), [`const`](#rule-const) |
| [@UserType](#user-types)     | [`optional`](#rule-optional), [`nullable`](#rule-nullable) |
