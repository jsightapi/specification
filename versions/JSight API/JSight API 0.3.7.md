# JSight API 0.3 Specification

July 12, 2022

**JSight API version: *0.3.7* **

Authors: *Reznitsky M., Ivanov A., Malyshev K.*

## Introduction

This document describes the entire JSight API language specification.

JSight API 0.3 language is primarily used to describe/document HTTP REST JSON and JSON-RPC 2.0 based
APIs.

The JSight API language is intended to be used in a number of API development tools, including:

- validators (check the validity of incoming and outgoing messages),
- documentation generators (generate human-readable documentation on API),
- testers (help to test API),
- code generators (generate code stubs for implementing the API on the backend or for use the API on
  the frontend),
- converters (convert API description from one language to another),
- plugins for highlighting API description language syntax in various IDE,
- generators of descriptions inherited by API,
- and others.

## Dependences

- [JSight Schema 0.3.5](jsight-schema-0-3).

## Concept

These are the principles that we follow while developing this language:

1. __Clarity__ — the language should be easy for a person to read and navigate through. The meaning
   of a separate block should be clear without additional instructions to the intermediate
   developer familiar with REST API concepts.

2. __Convenience__ — the use of the language should be simple, and the user should not be stressed
   by inconvenient constructs. There should be no inflated restrictions.

3. __Examples first__ — it is much easier to understand information when an example is given first,
   followed by an explanation. This principle is built into the [JSight Schema
   language](jsight-schema-0-3), which is used extensively in the JSight API language.

4. __Conciseness__ — reducing the number of structural and syntactic constructs, keywords, etc.

5. __Principle “80 to 20”__ — if the syntax appears simple in 80% of real-world cases, but is
   difficult in 20% of the cases, such a syntax is more preferable to syntax which is moderately
   difficult in 100% of the cases.

6. __Alternative syntax__ — alternative simplified constructs are allowed to be used for some
   complex constructs if it increases clarity in 80% of cases.

7. __Defaults__ — in order to simplify the syntax, in 80% of cases we widely use the so-called
   “default values” that can be omitted. For example, a DIRECTIVE may have a default CHILD DIRECTIVE
   that can be omitted in some cases.

8. __Modularity__ — the description of a part of the API (for example, one HTTP-METHOD for one
   RESOURCE) should be able to exist separately, as an independent PROJECT.

9. __Follow whatever is familiar__ — it is preferable to use concepts and constructs that the
   developers are already familiar with in some form or another. For example, a DIRECTIVE with
   PARAMETERS is similar to a function with parameters or a console command with parameters.

10. __Reuse__ — we provide the ability to reuse some blocks of code in other blocks of code. For
    example, the DIRECTIVE `TYPE` allows you to specify a USER TYPE of data only once and refer to
    this TYPE by name.

11. __Unique interpretation__ — constructs that allow uncertain intuitive understanding are not
    welcomed. Intuition should guide developers' thoughts along a single path.

12. __Conformity of OpenAPI 3.0__ — Since the OpenAPI 3.0 specification is currently the de facto
    standard in the REST API world, the JSight specification should be written in such a way that
    there is no loss of information when converting from OpenAPI 3.0 to JSight. When converting from
    JSight to OpenAPI 3.0 minor loss of information is acceptable.

## Terms and Definitions

- **ANNOTATION** — is a syntactic structure of the language that allows other syntactic structures
  to be specified. It is actively used in the [JSight Schema language](jsight-schema-0-3) to specify
  RULES, add text notes to EXAMPLE objects, etc. In the JSight API, it is also used to add text
  notes to the DIRECTIVES.

- **API** — is REST API or/and JSON-RPC API, which is described in the PROJECT.

- **BODY** of the DIRECTIVE — is a code block that starts on the following line after the DIRECTIVE
  is declared and ends before the BODY of the DIRECTIVE is explicitly or implicitly terminated (see
  the section [“Boundaries of the BODY of the
  DIRECTIVE”](#boundaries-of-the-body-of-the-directive)).

- **CHILD DIRECTIVE** — is a DIRECTIVE that is nested in the BODY of another DIRECTIVE which is a
  PARENT DIRECTIVE for it. The CHILD DIRECTIVE concept only applies to one nesting level.

- **COMMENT** — is a text in the PROJECT separated from the code by special symbols, has no internal
  JSight API syntax restrictions and is ignored by language parsers.

- **CONTEXT** — is a certain semantic section of the code that affects syntax rules within that
  section. For example, there is a ROOT CONTEXT. Each DIRECTIVE also has its own CONTEXT (see the
  section [“CONTEXTS OF THE DIRECTIVE”](#context-of-directives)).

- **DIRECTIVE** — is a key syntactic construct in the language that is used to describe one or more
  API properties. For example, there are DIRECTIVES for describing REST API elements such as
  HTTP-RESOURCE, HTTP-METHOD, and HTTP-HEADER. Each DIRECTIVE has a KEYWORD that is used to declare
  that DIRECTIVE. The keyword also serves as the name of the DIRECTIVE.

- **DIRECTIVES-HTTP-METHODS** — are a name of a group of DIRECTIVES, that describe different
  HTTP-request methods: `GET`, `POST`, `PUT` and others.

- **DIRECTIVES-RESPONSES** — are a name of a group of DIRECTIVES, that describe HTTP-RESPONSES with
  different http-status codes. For example, DIRECTIVES `200`, `301`, `404`, etc.

- **ENUMERATION** — is a clearly defined named set of values that can be used in rule `enum` in the
  [JSight Schema language](jsight-schema-0-3).

- **FORMAT** — is a set of certain rules for serializing structured data to a string.

- **KEYWORD** of the DIRECTIVE — is the word that signals the start of the DIRECTIVE.

- **NAME** — is a name of a certain named block that will be used repeatedly in the PROJECT. For
  example, TYPES have a NAME.

- **NON-CHILD DIRECTIVE** — is a DIRECTIVE that is not CHILD to other DIRECTIVE we are dealing with.

- **PARAMETERS** of the DIRECTIVE — are text parameters that appear immediately after the KEYWORD of
  the DIRECTIVE in the same line and specify basic information about the DIRECTIVE operation. They
  may be omitted in some cases.

- **PARENT DIRECTIVE** — is a DIRECTIVE in which another DIRECTIVE is nested which is a CHILD
  DIRECTIVE for it. The PARENT DIRECTIVE concept only applies to one nesting level.

- **PROJECT** — is a text, located in one or several files, which describes some single API.

- **REFERENCE** — is specifying of a certain NAME, which means that instead of this NAME, some code
  block should be substituted, that is described and named in another part of the PROJECT.

- **ROOT CONTEXT** — is a CONTEXT containing DIRECTIVES that do not have PARENT DIRECTIVES (see the
  section [“ROOT CONTEXT”](#root-context)).

- **RULES** — is a term from the [JSight Schema language](jsight-schema-0-3). RULES are additions to
  an EXAMPLE written in a certain way that specify the requirements for the data structure within
  the SCHEMA (for more information, see the [JSight Schema language specification, section
  “RULES”](jsight-schema-0-3#rules)).

- **SCHEMA** — is a code that describes the structure of some data and is written using one or
  another SCHEMA NOTATION.

- **SCHEMA NOTATION** — is the language used to describe this or that SCHEMA, for example `jsight`,
  `regex`.

- **SPACE CHARACTERS** — are characters that are perceived as spaces by readers. In the current
  language version, SPACE CHARACTERS include the space and tab characters.

- **USER TYPE** — is a named description of the data schema specified using the DIRECTIVE `TYPE`.

### HTTP REST API terms

- **BODY (HTTP-BODY)**
- **HEADER (HTTP HEADER)**
- **METHOD (HTTP-METHOD)**
- **REQUEST (HTTP-REQUEST)**
- **RESOURCE (HTTP-RESOURCE)**
- **RESPONSE (HTTP-RESPONSE)**
- **PARAMETERS of the PATH**
- **PATH**
- **QUERY STRING**

## Basic concepts

This section describes the basic concepts and the most basic syntax constructs of the JSight API
language.

### PROJECT

PROJECT is a code in the JSight API language that describes one API. The PROJECT can be submitted as
a single or multiple files with the preferred extension `*.jst`, containing one or more DIRECTIVES.

The files in a multiple-files PROJECT connect with each other by the DIRECTIVE `INCLUDE` (see
the section [“DIRECTIVE "INCLUDE"”](#directive-include)).

A PROJECT must have at least one DIRECTIVE.

Example of the simplest PROJECT:

```jsight
JSIGHT 0.3

GET /users
```

This PROJECT describes the REST API as follows:

1. The API has only one resource `/users`.
2. The resource `/users` only accepts REQUESTS with the METHOD GET.
3. There are no any restrictions imposed by documentation on the composition of the REQUEST and
   RESPONSE when requesting `GET/users`.

### DIRECTIVES

> Requirement: req.japi.directive 0.3.  
> Status: APPROVED 10/13/2021.

A PROJECT entirely consists of DIRECTIVES. Any code in the PROJECT is certainly a part of that or
other DIRECTIVE (except for user COMMENTS, which are ignored by the parser).

DIRECTIVES are mostly used to describe specific API properties. The structure of all DIRECTIVES is
the same. In general, the DIRECTIVE is made up of the following components:

1. **KEYWORD** of the DIRECTIVE declares the beginning of the DIRECTIVE. The KEYWORD is also the
   name of the DIRECTIVE. For example, if we say “Directive GET”, it means that this DIRECTIVE is
   declared using the keyword `GET`. The KEYWORD is always written on a new line (may be preceded
   by any number of SPACE CHARACTERS).

2. **Values ​​of DIRECTIVE PARAMETERS** are placed immediately after the KEYWORD in the same line.
   The kit and the format of the PARAMETERS depend on the particular DIRECTIVE (see the section
   [“Reference of DIRECTIVES”](#reference-of-directives)). There are DIRECTIVES without PARAMETERS.
   PARAMETERS can have default values, which can be omitted in this case. PARAMETERS are separated
   by SPACE CHARACTERS (similar to UNIX console command options). At least one SPACE CHARACTER
   should separate the first PARAMETER from the KEYWORD. The rules for writing the values of
   DIRECTIVE PARAMETERS are described in the section [“Rules for writing values of DIRECTIVE
   PARAMETERS”](#rules-for-writing-the-values-of-parameters-of-the-directive) below.

3. **ANNOTATION** of the DIRECTIVE is a free-form explanation of the purpose and features of the API
   element described in this DIRECTIVE. It is written right after the PARAMETERS. At least one
   SPACE CHARACTER should separate ANNOTATION from the last PARAMETER value or from the KEYWORD.
   The format of writing ANNOTATIONS is detailed in the section [“ANNOTATIONS”](#annotations).

4. **BODY** of the DIRECTIVE is a multi-line code block that starts on the line following the
   declaration of the DIRECTIVE and ends when another NON-CHILD DIRECTIVE begins (see
   [further](#boundaries-of-the-body-of-the-directive)). The syntax rules for BODY content depend
   on the particular DIRECTIVE (see the section [“Reference of
   DIRECTIVES”](#reference-of-directives)). BODY can be empty, or it can include the following:

    1. Other CHILD DIRECTIVES (for example, the DIRECTIVE `GET`).
    2. The SCHEMA (for example, the DIRECTIVE `Body`).
    3. Random text (for example, the DIRECTIVE `Description`).

Not any  DIRECTIVE can be nested in another DIRECTIVE. The rules about which DIRECTIVES can be
nested in each other are placed in the description of specific DIRECTIVES in the section [“Reference
of DIRECTIVES”](#reference-of-directives).

Let's consider an example of the DIRECTIVE:

```jsight
TYPE @cat jsight // Type “Cat”.
  {
    "id"  : 123,
    "name": "Tom"
  }
```

In the given example, the DIRECTIVE `TYPE` is declared in the first line, which includes:

1. The KEYWORD `TYPE`.
2. Two PARAMETER values: `@cat` and `jsight`.
3. The ANNOTATION `Type “Cat”`.
4. The BODY of a DIRECTIVE in the form of four lines of the SCHEMA (lines 2–5), which describes the
   structure of TYPE `@cat` using the NOTATION `jsight` (in [JSight Schema
   language](jsight-schema-0-3)).

Another example:

```jsight
GET /pets // Get a list of animals

  200
    [
      {
        "id"  : 121,
        "name": "Tom"
      }
    ]
```

In the given example, the DIRECTIVE `GET` is declared in the first line, which includes:

1. The KEYWORD `GET`.
2. One PARAMETER `/pets`.
3. The ANNOTATION `Get a list of animals`.
4. The BODY of the DIRECTIVE, which includes another DIRECTIVE `200` and all its contents (lines
   3–9). For more details on the content of the BODY of the DIRECTIVE `200`, see the section
   [“DIRECTIVES-RESPONSES”](#directives-responses).

#### Register of KEYWORDS

The JSight API language has strict requirements for the register of KEYWORDS. The KEYWORDS register
must exactly match the register indicated in this document. For example, the DIRECTIVE `GET` can
only be written in capital letters and cannot be written as `Get` or `get`.

Some DIRECTIVES are written entirely in capital letters, for example, `GET`, `URL`, `INFO`. Some
DIRECTIVES are written in lowercase and the first letter of each internal word is capitalized (known
as  “Pascal Style”), for example, `Request`, `Headers`, `Path`, `BaseUrl`.

The following principle applies here: if the DIRECTIVE *can* be placed in the ROOT CONTEXT, then it
is written entirely in capital letters; if the DIRECTIVE *CANNOT* be placed in the ROOT CONTEXT,
then it is written in lowercase and with the first capital letter of each internal word (Pascal
Style).

#### Rules for writing the values of PARAMETERS of the DIRECTIVE

> Requirement: req.japi.directive.parameters 0.3.  
> Status: APPROVED 10/13/2021.

- The values of PARAMETERS of the DIRECTIVE are separated by at least one SPACE CHARACTER.
- If the value of the PARAMETER of the DIRECTIVE does not contain SPACE CHARACTERS, symbol `#`,
  quotation marks `"` and forward slashes `\`, then it can be written either in quotation marks `"`
  or without it.
- If the value of the PARAMETER of the DIRECTIVE contains SPACE CHARACTERS, symbol `#`, quotation
  marks `"` or forward slash `\`, then it can only be written in quotation marks `"`.
- If the value of the PARAMETER of the DIRECTIVE is written in quotation marks, the quotation marks
  `"` and the forward slashes `\` must be escaped within it. The backslash character `\` is used as
  an escape character.

Examples of notations of PARAMETER values:

```
DIRECTIVE parameter_value_without_spaces "Parameter value with spaces and \"escaping\""
```

#### Boundaries of the BODY of the DIRECTIVE

> Requirement: req.japi.directive.body_borders 0.1.  
> Status: APPROVED 10/12/2021.

The BODY of the DIRECTIVE begins immediately on the next line after the KEYWORD of the DIRECTIVE
(not counting empty lines, which are ignored by the parser).

The end of the BODY of the DIRECTIVE can be set up in two ways: *explicitly* and *implicitly*:

1. **Explicit boundaries of the BODY of the DIRECTIVE** are set using parentheses.

  - The opening parenthesis `(` must be placed on a new line immediately after the line containing
    the KEYWORD of the DIRECTIVE, or through any number of empty lines, since empty lines are
    ignored by the parser. This parenthesis declares the beginning of the BODY of the DIRECTIVE.
    Apart from the opening parenthesis, there should be nothing else on this line.

  - The closing parenthesis `)` must be placed on a separate line immediately after the contents of
    the BODY of the DIRECTIVE or through any number of empty lines, since empty lines are ignored by
    the parser. The closing parenthesis declares the end of the BODY of the DIRECTIVE. Except the
    closing parenthesis, there should be nothing else on this line.

2. **Implicit boundaries of the BODY of the DIRECTIVE** are defined if the boundaries of the BODY
   have not been set explicitly. In this case, the following rule applies: if there is another
   DIRECTIVE after the beginning of the BODY of the DIRECTIVE that cannot be a CHILD for this
   DIRECTIVE, this means that the BODY of the DIRECTIVE is completed, and a new NON-CHILD DIRECTIVE
   has started.

:::note

*Indents and tabs have no effect on the definition of the boundaries of the BODY of the DIRECTIVE
and are only used for readability in our examples (see the section [“Indents and
tabs”](#indents-and-tabs))*

:::

An example of a PROJECT, in which one of the DIRECTIVES has explicit boundaries of the BODY:

```jsight
JSIGHT 0.3

URL /cats/{id}
(
  GET
    200 @cat
)

URL /cats/{id}/friends
  GET
    200 [@cat]

TYPE @cat
{
  "id"  : 1,
  "name": "Tom"
}
```

In the given example, the boundaries of the BODY of the DIRECTIVE `URL /cats/{id}` are explicitly
set using parentheses.

Let's add parentheses to this PROJECT example to explicitly specify the BODIES of all DIRECTIVES:

```jsight
JSIGHT 0.3

URL /cats/{id}
(
    GET
    (
      200 @cat
    )
)

URL /cats/{id}/friends
(
  GET
  (
    200 [@cat]
  )
)

TYPE @cat
(
  {
    "id"  : 1,
    "name": "Tom"
  }
)
```

The boundaries of the BODIES of all DIRECTIVES are now clearly specified. As you can see, this
method of specifying the boundaries of the BODIES of DIRECTIVES can significantly reduce code
readability.

In practice, parentheses are almost never required, so they can usually be omitted. However,
parentheses may be needed when using the DIRECTIVE `MACRO` (see the section [“DIRECTIVE
"MACRO"”](#directive-macro)) and DIRECTIVE `Description` (see the section [“Directive
"Description"”](#directive-description)).

#### CONTEXT OF DIRECTIVES

> Requirement: req.japi.context 0.1.  
> Status: APPROVED 10/12/2021.

As previously stated, DIRECTIVES can be nested within each other. When one DIRECTIVE is inserted
into another, the PARENT DIRECTIVE serves as the CONTEXT for the CHILD DIRECTIVE.

If some DIRECTIVE “A” is in the BODY of some other DIRECTIVE “B”, we can say the following about
such situation:

1. The DIRECTIVE “B” *is placed in the CONTEXT of the DIRECTIVE “A”*.
2. The DIRECTIVE “A” *sets CONTEXT for the DIRECTIVE “B”*.
3. The DIRECTIVE “B” *is "embedded" in the DIRECTIVE “A”* or *is a "CHILD" in relation to DIRECTIVE
   “A”*.
4. The DIRECTIVE “A” *is "PARENT" for DIRECTIVE "B"*.

:::note

<i>The phrase “DIRECTIVE CONTEXT” can be interpreted in two ways. On the one hand, there is 
an <u>external</u> DIRECTIVE CONTEXT — that is, the CONTEXT within which this DIRECTIVE exists. On the
other hand, we can discuss the <u>internal</u> DIRECTIVE CONTEXT — that is, the CONTEXT that this
DIRECTIVE establishes for its CHILD DIRECTIVES. When using words “DIRECTIVE CONTEXT”, it is
important to make it clear to the reader which CONTEXT is being discussed: internal or external.</i>

:::

It is important to distinguish between the internal CONTEXT and the BODY of the DIRECTIVE. At first
glance, the scope of the internal CONTEXT and the BODY of the DIRECTIVE appear to be the same.
However, this is not always the case. The fact is that the DIRECTIVE can be simultaneously in
multiple BODIES of PARENT and grandparent DIRECTIVES, but never can be in multiple outer CONTEXTS.
External CONTEXT is specified only by the *immediate* parent of the DIRECTIVE. For example, the
grandfather of the DIRECTIVE no longer affects its external CONTEXT.

Let's consider what was said using the following example:

```jsight
URL /cats
  GET
    200 @cat
```

In this example:

- The DIRECTIVE `GET` is in the BODY of the DIRECTIVE `URL` and in the CONTEXT of the DIRECTIVE
  `URL`.
- The DIRECTIVE `200` is in the BODY of the DIRECTIVE `GET`, also in the BODY of the
  DIRECTIVE-grandfather `URL`, but only in one CONTEXT of the DIRECTIVE `GET`.

##### ROOT CONTEXT

DIRECTIVES are always in one or another external CONTEXT. Nested CHILD DIRECTIVES are in the CONTEXT
of their PARENT DIRECTIVES. DIRECTIVES that are not nested in other DIRECTIVES are in the so-called
ROOT CONTEXT.

Let's consider the following example:

```jsight
JSIGHT 0.3

GET /cats // Get a list of cats.
  200
    [
      {
        "id"  : 121,
        "name": "Tom"
      }
    ]
```

In this example:

- The DIRECTIVE `GET` is placed in the ROOT CONTEXT,
- The DIRECTIVE `200` is placed in the CONTEXT of the DIRECTIVE `GET`.

It should be noted that the DIRECTIVE `200` is NOT placed in the ROOT CONTEXT, since the external
CONTEXT for a DIRECTIVE is only the CONTEXT set by the immediate PARENT DIRECTIVE (see the section
[“CONTEXTS OF DIRECTIVES”](#context-of-directives)).

#### CHILD DIRECTIVES by default

> Requirement: req.japi.context.default_child 0.4.  
> Status: APPROVED 10/12/2021.

If a DIRECTIVE can have multiple CHILD DIRECTIVES, then one of these CHILD DIRECTIVES can be defined
as *DIRECTIVE by default* by the language specification. This means that if there is one and only
one CHILD DIRECTIVE in the BODY of the PARENT DIRECTIVE, which is defined for this PARENT DIRECTIVE
as the default DIRECTIVE, then the declaration of this CHILD DIRECTIVE can be omitted, and its BODY
placed directly in the PARENT DIRECTIVE.

Let us explain with examples.

Let's consider the following JSight API code:

```jsight
GET /cats // Get all cats.
  200
    Body
      [@cat]      
```

In the given example, the DIRECTIVE `200` contains the CHILD DIRECTIVE `Body`, which contains a
SCHEMA of a successful response (array of cats).

The directive `Body` is *the default DIRECTIVE* for the PARENT DIRECTIVE `200` (see the section
[“DIRECTIVES-RESPONSES”](#directives-responses)). This means that we can omit the DIRECTIVE `Body`
in this case. Then the BODY of the DIRECTIVE `Body` will stand right inside the BODY of the
DIRECTIVE `200`, giving us a more compact structure:

```jsight
GET /cats // Get all cats.
  200
    [@cat]      
```

If the BODY of the DIRECTIVE contains two or more CHILD DIRECTIVES, one of which is a CHILD
DIRECTIVE by default, then it is no longer possible to omit the declaration of this DIRECTIVE:

```jsight
GET /cats // Get a list of all cats. 
  200
    Headers
      @headers
    Body        # The DIRECTIVE `Body` cannot be omitted, since there is the DIRECTIVE `Headers`.
      "OK"
```

:::note

*Described rule, that allows the omission of declarations of CHILD DIRECTIVES by default,
theoretically can act on multiple child levels rather than one. The current language version does
not yet have examples of such a case.*

:::

#### Order of DIRECTIVES

In most cases, the order of the DIRECTIVES in the ROOT CONTEXT is irrelevant. For example, a
DIRECTIVE `TYPE`, which declares a USER TYPE, can be placed either before or after where this TYPE
is referenced.

The order of DIRECTIVE in the ROOT CONTEXT is only relevant for the DIRECTIVE `JSIGHT`. This
DIRECTIVE specifies the version of the JSight API language that was used to create the PROJECT. The
DIRECTIVE `JSIGHT` must be placed before any other DIRECTIVE in the file.

### ANNOTATIONS

> Requirement: req.japi.annotation 0.1.  
> Status: APPROVED 10/12/2021.

Constructs of the JSight API language can be accompanied by so-called ANNOTATIONS. By appearance,
ANNOTATIONS look like comments in C-like programming languages. However, ANNOTATIONS should not be
confused with user COMMENTS (see the section [“User COMMENTS”](#user-comments)).

ANNOTATIONS cannot be placed anywhere. The ability to input an ANNOTATION in a particular place is
determined by the specific situation. The ability to place ANNOTATION in one or another place of the
PROJECT is described in other relevant sections.

Various syntactic constructs may be within the ANNOTATION. The requirements for the content of the
ANNOTATION are determined by the situation, so they are described in other relevant sections. In the
current version of the JSight API language, the ANNOTATIONS are only used for DIRECTIVES (see the
section ["DIRECTIVES"](#directives)).

ANNOTATIONS are also widely used in the JSight Schema language (see the specification for the JSight
Schema language, section [“ANNOTATIONS”](jsight-schema-0-3#annotations)).

By appearance, ANNOTATIONS can be of two types: *single-line* and *multi-line*.

#### Single-line ANNOTATIONS

> Requirement: req.japi.annotation.oneline 0.3.  
> Status: APPROVED 10/12/2021.

Single-line ANNOTATIONS are opened with `//` characters and continue till the end of the given line.

Example of single-line ANNOTATION:

```jsight
GET /cats // Single-line annotation.
```

#### Multi-line ANNOTATIONS

> Requirement: req.japi.annotation.multiline 0.3.  
> Status: APPROVED 10/12/2021.

Multi-line ANNOTATIONS begin with `/*` and continue until closed with `*/`. Multi-line ANNOTATIONS
can span from a single to several lines.

Examples of multi-line ANNOTATIONS:

```jsight
GET /cats /* Multi-line annotation in one line. */

GET /cats/{id} /* Multi-line
                  annotation
                  in several lines. */
```

### User COMMENTS

> Requirement: req.japi.comments 0.1.  
> Status: APPROVED 10/12/2021.

You can leave random user COMMENTS almost anywhere in the PROJECT (see the sections [“Single-line
COMMENT”](#single-line-comment) and [“Block COMMENT“](#block-comment)). These
COMMENTS are intended solely for the developers of the PROJECT. Unlike ANNOTATIONS, COMMENTS
from the PROJECT point of view have no semantic meaning and are ignored when parsing PROJECT.

There are two types of COMMENTS: *single-line* and *block*.

#### Single-line COMMENT

> Requirement: req.japi.comments.oneline 0.1.  
> Status: APPROVED 10/12/2021.

A single-line COMMENT starts with the symbol `#`. After this symbol, the full subsequent line is a
COMMENT. The symbol `#` can appear either at the beginning or anywhere else in the line.

The symbol `#` should not be interpreted as an opening symbol of a single-line COMMENT in the
following cases:

1. When the symbol `#` is inside a string literal, for example in the [value of a PARAMETER of
   DIRECTIVE](#rules-for-writing-the-values-of-parameters-of-the-directive), in the EXAMPLE or in
   the RULE in the SCHEMA of the [NOTATION `jsight`](#notation-jsight).
2. When the symbol `#` is inside a block COMMENT (see [below](#block-comment)).
3. When the symbol `#` is escaped (for the escaping rules in different language constructs, see the
   relevant sections).
4. Inside regular expression within the SCHEMA of the [NOTATION `regex`](#notation-regex).
5. Inside Markdown text (see the section [“DIRECTIVE "Description"”](#directive-description)).
6. Inside multi-line ANNOTATIONS (see the section [“Multi-line ANNOTATIONS”](#multi-line-annotations)).

Examples of single-line COMMENTS:

```jsight
# Single-line COMMENT

GET /cats // Get a list of cats. # Single-line COMMENT
  200 [@cat]
```

#### Block COMMENT

> Requirement: req.japi.comments.block 0.1.  
> Status: APPROVED 10/12/2021.

Block COMMENT opens with symbols `###`. Thereafter, all subsequent lines are considered
a COMMENT until it is closed by the following symbols `###`.

COMMENTS cannot be nested inside each other.

The symbols `###` should not be interpreted as the opening symbols of a block COMMENT in the
following cases:

1. When the symbols `###` are inside a string literal, for example, in the [value of a PARAMETER of
   DIRECTIVE](#rules-for-writing-the-values-of-parameters-of-the-directive), in the EXAMPLE or in
   the RULE in the SCHEMA of the [NOTATION `jsight`](#notation-jsight).
2. Inside regular expression within the SCHEMA of the [NOTATION `regex`](#notation-regex).
3. Inside Markdown text (see the section [“DIRECTIVE "Description"”](#directive-description)).
4. Inside multi-line ANNOTATIONS (see the section [“Multi-line ANNOTATIONS”](#multi-line-annotations)).
5. Inside single-line ANNOTATIONS (see the section [“Single-line ANNOTATIONS”](#single-line-annotations)).
   Any symbol `#` inside single-line ANNOTATIONS will be interpreted as the beginning of a
   single-line COMMENT.
6. Inside a KEYWORD of the DIRECTIVE.

Examples of block COMMENTS:

```jsight
JSIGHT 0.3

###
Block
(multiline)
COMMENT
###

GET /cats/{id} ### Another block COMMENT ### 

  200
    {
      "id": 5,
      "name": "Tom"
    }
```

## Common syntax rules

> Requirement: req.japi.common_syntax 0.1.  
> Status: APPROVED 10/12/2021.

### SPACE CHARACTERS

SPACE CHARACTERS are space and tab characters. The objective of the SPACE CHARACTER is to separate
lexemes of the language. In semantic terms, SPACE CHARACTERS do not differ from each other in any
way.

Several SPACE CHARACTERS in a row count as one SPACE CHARACTER.

A line consisting of only SPACE CHARACTERS and “line feed” symbol is considered an empty string.

Empty lines do not have any semantic meaning in the language and are ignored by the parser.

### Line feed

Line feed can be done with the characters “line break” (code 10) or “carriage return” (code 13), as
well as using a combination of these two symbols.

Line feed has some semantic meaning in the language. For example, a new line following the line with
KEYWORD of the DIRECTIVE indicates that the BODY of the DIRECTIVE has started.

The specific semantic load of line feed is indicated in the corresponding sections dedicated to one
or another language constructs.

### Indents and tabs

The JSight API language does not require lines to be aligned to the left with indentation and tabs.
It is recommended that indents and tabs be used to improve code readability (see all examples in
this document).

### Case

The JSight API language is case sensitive. If two lexemes differ only in letter case, such tokens
are considered distinct.

### Names

The JSight API language widely uses a method for naming various entities. All names in the language
are classified as *built-in* and *user-defined*. Built-in names are built into the language itself.
User-defined names are created and are assigned to various entities by users. For example, a user
can give names to servers, TYPES, and macros.

Any user-defined names must start with the symbol `@`.

Built-in names never start with the symbol `@`. For example, names of NOTATIONS `jschema`, `any`,
and etc.

Names can contain only the following characters:

- the symbol "commercial at" `@` — only at the beginning of the user-defined name,
- uppercase and lowercase characters of the Latin alphabet,
- numbers,
- underscore `_`.

## Reference of DIRECTIVES

This section describes all the DIRECTIVES that can be used in the PROJECT.

:::note

*An additional set of DIRECTIVES is used to describe the JSON-RPC 2.0 protocol (see section
["Language extensions for describing the JSON-RPC 2.0
protocol"](#language-extensions-for-describing-the-json-rpc-20-protocol) ).*

:::


### DIRECTIVE **"BaseUrl"**

> Requirement: req.japi.server.base_url 0.4.  
> Status: APPROVED 01/10/2022.

The DIRECTIVE `BaseUrl` is intended to describe the base PATH of API that is linked to a specific
server where the API is physically located.

|    DIRECTIVE "BaseUrl"  |                      |
| ----------------------- | :------------------: |
| KEYWORD                 |       `BaseUrl`      |
| PARENT DIRECTIVE        |       `SERVER`       |
| PARAMETERS              |        `Path` *      |
| ANNOTATION              |      Not allowed     |
| BODY                    |          No          |
| CHILD DIRECTIVES        |          No          |
| Default CHILD DIRECTIVE |          No          |

##### PARENT DIRECTIVES

The DIRECTIVE `BaseUrl` can only be placed inside the DIRECTIVE `SERVER`.

Technically you can place DIRECTIVE `BaseUrl` in DIRECTIVE `MACRO`, but it does not make sense (see
the section [“DIRECTIVE "MACRO"”](#directive-macro)).

##### PARAMETERS

The DIRECTIVE `BaseUrl` has one mandatory parameter `Path`, in which the URL of this server must be
specified.

Example:

```jsight
SERVER @CATS_API_SERVER
  BaseUrl "https://catsbook.com/api"
```

##### ANNOTATION

The DIRECTIVE `BaseUrl` CANNOT have ANNOTATION.

##### BODY

The DIRECTIVE `BaseUrl` does not have a BODY.

### DIRECTIVE **"Body"**

> Requirement: req.japi.body 0.3.  
> Status: APPROVED 10/12/2021.  
> It is planned:
>
>  - to add a third PARAMETER `Format`.

The DIRECTIVE `Body` is intended to describe the required HTTP-BODIES in the DIRECTIVE `Request` and
DIRECTIVES-RESPONSES (`200`, `301`, etc.).

|        DIRECTIVE "Body"      |                    |
| ---------------------------- | :----------------: |
| KEYWORD                      |       `Body`       |
| PARENT DIRECTIVES (possible) | <ul align="left" style={{margin: 0}}><li>`Request`</li><li>DIRECTIVES-RESPONSES (e. g. `200`, `301`, etc.)</li></ul> |
| PARAMETERS                   | <ul align="left" style={{margin: 0}}><li><code>Type</code></li><li><code>SchemaNotation</code></li></ul><p style={{"text-align": "left", margin: 0}}>In future versions also: `Format`</p> |
| ANNOTATION                   |    Not allowed     |
| BODY                         |       SCHEMA       |
| CHILD DIRECTIVES             |        No          |
| Default CHILD DIRECTIVE      |        No          | 

##### PARENT DIRECTIVES

The DIRECTIVE `Body` can be placed in the following CONTEXTS:

- DIRECTIVE `Request`,
- DIRECTIVES-RESPONSES (`200`, `301`, `404`, etc.).

Also, the DIRECTIVE `Body` can be placed in the macro defined by the DIRECTIVE `MACRO` (see the
section [“DIRECTIVE "MACRO"”](#directive-macro)).

##### PARAMETERS

The DIRECTIVE `Body` can have two optional PARAMETERS:

  - `SchemaNotation` — sets the SCHEMA NOTATION, which is used to describe the data SCHEMA in the
    BODY of the DIRECTIVE `Body`. Examples of SCHEMA NOTATIONS: `jsight`, `regex`, `any`, `empty`
    (see the section [“PARAMETER "SchemaNotation"”](#parameter-schemanotation)).
  - `Type` — specifies the TYPE of data, which should be placed in the HTTP-BODY. The parameter
    `Type` can be written in two ways:
    1. <u>A USER TYPE name that starts with a symbol <code>@</code></u>, for example, <code>Body
       @cat</code>. In this case, the BODY of HTTP REQUEST (or HTTP RESPONSE) must contain a JSON
       document of the specified TYPE. In future language versions, it will be possible to set the
       FORMAT of the document using the third PARAMETER <code>Format</code>, for example, XML rather
       than JSON.

    2. <u>A USER TYPE name that starts with a symbol <code>@</code> and <i>is enclosed in square
       brackets</i></u>, for example <code>Body [@cat]</code>. In this case, the BODY of HTTP
       REQUEST (or HTTP RESPONSE) must contain a JSON document that corresponds to <i>an array of
       elements of the specified TYPE</i>. In future language versions, it will be possible to set
       the FORMAT of the document using the third PARAMETER <code>Format</code>, for example, XML
       rather than JSON.

PARAMETERS `Type` and `SchemaNotation` cannot be declared simultaneously. Thus, the DIRECTIVE `Body`
can have the following sets of PARAMETERS:

1. The PARAMETER `Type` is specified. In this case, the BODY of the DIRECTIVE `Body` must be empty.
2. The PARAMETER `SchemaNotation` is specified. In this case, the BODY of the DIRECTIVE `Body`
   cannot be empty and must contain a SCHEMA in the specified NOTATION.
3. There are no PARAMETERS. In this case, the PARAMETER `SchemaNotation` is considered equal to
   `jsight`, and the BODY of the DIRECTIVE `Body` must contain the SCHEMA in the NOTATION `jsight`.

Let's consider some examples:

**Example 1.**

```jsight
GET /cats/{id}
  200
    Body jsight
      {
        "id"  : 1,
        "name": "Tom"
      }
```

In the given example, the PARAMETER `SchemaNotation` with the value `jsight` is specified in the
DIRECTIVE `Body`. Accordingly, the BODY of the DIRECTIVE `Body` contains the SCHEMA in the NOTATION
`jsight`.

**Example 2.**

```jsight
GET /cats/{id}
  200
    Body
      {
        "id"  : 1,
        "name": "Tom"
      }
```

In the given example, there are no PARAMETERS in the DIRECTIVE `Body`, so it is assumed by default
that PARAMETER `SchemaNotation` has a value `jsight`. Accordingly, the BODY of the DIRECTIVE `Body`
contains the SCHEMA in the NOTATION `jsight`, as in the previous example.

**Example 3.**

```jsight
GET /cats/{id}
  200
    Body @cat
```

In this example, the PARAMETER `Type` with the value `@cat` is specified in the DIRECTIVE `Body`.
This means that a JSON-document corresponding to the USER TYPE `@cat` should come in the RESPONSE to
the REQUEST `GET /cats`.

**Example 4.**

```jsight
GET /cats
  200
    Body [@cat]
```
In the given example, the PARAMETER `Type` with the value `[@cat]` is specified in the DIRECTIVE
`Body`. This means that a JSON-document containing *an array of elements* and corresponding to the
USER TYPE `@cat`, should come in the RESPONSE to the REQUEST `GET /cats`.

##### ANNOTATION

The DIRECTIVE `Body` CANNOT have ANNOTATION.

##### BODY

The BODY of the DIRECTIVE `Body` may contain a SCHEMA of that NOTATION, which is set by the
PARAMETER `SchemaNotation` — either explicitly or by default. Some NOTATIONS compulsory require the
presence of a SCHEMA (for example, the NOTATION `jsight`). Other NOTATIONS may not require a SCHEMA
(for example, the NOTATION `any`).

If the PARAMETER `Type` is specified in the DIRECTIVE, then the BODY of the DIRECTIVE must be empty.

Let's consider some valid examples.

**Example 1.** The BODY is not required since the PARAMETER `Type` is specified:

```jsight
GET /cats/{id}
  200 // Returning JSON with a cat.
    Body @cat
```

**Example 2.** The BODY is required since the PARAMETER `SchemaNotation` explicitly specifies the
SCHEMA NOTATION `regex`:

```jsight
GET /cats/{id}/name
  200 // Returning a string with the name of the cat.
    Body regex
      /[A-Z][a-z]*/
```

**Example 3.** The BODY is not required since the specified NOTATION `any` always implies an empty
SCHEMA:

```jsight
POST /cats/counter
  200 // BODY doesn't matter.
    Body any
```

**Example 4.** The BODY in JSight Schema language is required, since the PARAMETER `SchemaNotation`
implies the NOTATION `jsight` by default:

```jsight
GET /cats/{id}
  200 // Returning a cat.
    Body
      {
        "id"  : 123,
        "name": "Tom"
      }
```

##### Default FORMATS

The same structured data can be serialized in different FORMATS. In the future language versions,
the option to explicitly control the serialization FORMAT using the PARAMETER `Format` will be added
in the DIRECTIVE `Body`. In the current language version, the FORMAT is set implicitly, depending on
the specified SCHEMA NOTATION.

The SCHEMA NOTATION, in turn, is specified either explicitly using the PARAMETER `SchemaNotation` or
implicitly through the specified USER TYPE `Type`, which has its own NOTATION.

The table below displays the default schema FORMATS for all currently supported SCHEMA NOTATIONS:

| SCHEMA NOTATION |   Default FORMAT    |
| :-------------: | :-----------------: |
|     jsight      |         json        |
|     regex       |     plainString     |
|     any         |        binary       |
|     empty       |        binary       |

Let's consider an example:

```jsight
GET /json-endpoint
  200
    Body jsight
      "Hello, World!" // {const: true}

GET /plain-string-endpoint
  200
    Body regex
      /Hello, World!/
```

In the given example, the same data structure is described in both RESOURCES in  DIRECTIVES `Body`.
However, this is done using different NOTATIONS: `jsight` and `regex`. The NOTATION `jsight` implies
the default FORMAT `json`, while the NOTATION `regex` implies the default FORMAT `plainString`. This
means that if you send a request for the first resource `/json-endpoint`, then the response will
contain the line `"Hello, World!"` (in quotes), which corresponds to the FORMAT `json`. And if you
send a request for the second resource `/plain-string-endpoint`, then the response will contain  the
line `Hello, World!` (without quotes), which corresponds to the FORMAT `plainString`. In other
words:

```
$ curl /json-endpoint
"Hello, World!"

$ curl /plain-string-endpoint
Hello, World!
```

### DIRECTIVE **"Description"**

> Requirements:  
> - req.japi.description 0.1 Status: APPROVED 10/12/2021.  
> - req.japi.description.markdown 0.3 Status: APPROVED 10/12/2021.

The DIRECTIVE `Description` is intended for detailed text descriptions of different DIRECTIVES.

|       DIRECTIVE "Description" |                      |
| ----------------------------- | :------------------: |
| KEYWORD                       |     `Description`    |
| PARENT DIRECTIVES (possible)  | <ul align="left"><li>`INFO`</li><li>DIRECTIVES-HTTP-METHODS (`GET`, `POST`, etc.)</li></ul> <p align="left">In future versions also: `Request`, `SERVER`, `TYPE`</p> |
| PARAMETERS                    |         No           |
| ANNOTATION                    |      Not allowed     |
| BODY                          |      Markdown Text   |
| CHILD DIRECTIVES              |          No          |
| Default CHILD DIRECTIVE       |          No          |

##### PARENT DIRECTIVES

The DIRECTIVE `Description` can be placed in the following DIRECTIVES:

- DIRECTIVE `INFO`,
- DIRECTIVES-HTTP-METHODS (`GET`, `POST`, etc.).

Also, the DIRECTIVE `Description` can be placed in the macro defined by the DIRECTIVE `MACRO` (see
the section [“DIRECTIVE "MACRO"”](#directive-macro)).

In future language versions, the DIRECTIVE `Description` will be added as a CHILD to some other
DIRECTIVES. For example, it will be possible to specify `Description` inside DIRECTIVES: `Request`,
`SERVER`, `TYPE`.

##### PARAMETERS

The DIRECTIVE `Description` has no PARAMETERS.

##### ANNOTATION

The DIRECTIVE `Description` CANNOT have ANNOTATION.

##### BODY

The BODY of the DIRECTIVE `Description` must contain text in Markdown format.

Example:

```jsight
GET /cats
  
  Description
    With this method you can get a list of all cats that are registered on the Catsbook.

  200 [@cat]
```

The same example with parentheses that explicitly specify the boundaries of the BODY of the
DIRECTIVE `Description`:

```jsight
GET /cats
  
  Description
  (
    With this method you can get 
    a list of all cats that are registered 
    on the Catsbook
  )

  200 [@cat]
```

In the current language version, the escaping of KEYWORDS and the closing parenthesis in BODY of the
DIRECTIVE `Description` is not supported. Escaping will be implemented in future versions. You need
to avoid using keywords or closing parenthesis at the beginning of a line in the current version.
Let's have a look at some examples.

Example with error:

```jsight
GET /cats
  
  Description
    With this method you can get 
    200 cats, that are registered             --ERROR!!! Will be perceived as DIRECTIVE 200
    on the Catsbook.

  200 [@cat]
```

Let's fix the error:

```jsight
GET /cats
  
  Description
  With this method you can get 200 
  cats, that are registered             
  on the Catsbook.


  200 [@cat]
```

Another example with error:

```jsight
GET /cats
  
  Description
  (
    With this method
    you can get a list of 
    cats (and female cats
  ), that are registered         --ERROR!!! The paranthesis will be perceived as the BODY completion
    on the Catsbook.
  )

  200 [@cat]
```

Let's fix the error:

```jsight
GET /cats
  
  Description
  (
    With this method 
    you can get a list of
    cats (and female cats),
    that are registered 
    on the Catsbook
  )

  200 [@cat]
```

### DIRECTIVE **"Headers"**

> Requirement: req.japi.headers 0.3.  
> Status: APPROVED 10/12/2021.

The DIRECTIVE `Headers` is intended to describe the required HTTP-HEADERS in the DIRECTIVE `Request`
and DIRECTIVES-RESPONSES.

|        DIRECTIVE "Headers"   |                    |
| ---------------------------- | :----------------: |
| KEYWORD                      |     `Headers`      |
| PARENT DIRECTIVES (possible) | <ul align="left" style={{margin: 0}}><li>`Request`</li><li>DIRECTIVES-RESPONSES (e. g. `200`, `301` etc.)</li></ul> |
| PARAMETERS                   |        No          |
| ANNOTATION                   |    Not allowed     |
| BODY                         |    SCHEMA `jsight` |
| CHILD DIRECTIVES             |        No          |
| Default CHILD DIRECTIVE      |        No          | 

##### PARENT DIRECTIVES

DIRECTIVE `Headers` can be placed in the following CONTEXTS:

- DIRECTIVE `Request`,
- DIRECTIVES-RESPONSES (`200`, `301`, `404`, etc.).

Also, the DIRECTIVE `Headers` can be placed in the macro defined by the DIRECTIVE `MACRO` (see the
section [“DIRECTIVE "MACRO"”](#directive-macro)).

##### PARAMETERS

The DIRECTIVE `Headers` has no PARAMETERS.

##### ANNOTATION

The DIRECTIVE `Headers` CANNOT have ANNOTATION.

##### BODY

The BODY of the DIRECTIVE `Headers` must contain the SCHEMA NOTATION `jsight` (in the JSight Schema
language). The SCHEMA describes the HEADERS that should be in the request or response. At the root
of the SCHEMA, there should be an object, each of whose properties describes a separate HEADER. The
property key describes the name of the HEADER, and the property value describes the value of the
HEADER.

The root SCHEMA object cannot have the RULE `nullable: true`.

As an example, let's consider the following:

```jsight
GET /cats
  200
    Headers
      {
        "Authorization": "Basic dG9tQGNhdC5jb206YWJjMTIz=", // {regex: "Basic [A-Za-z0-9+\/=]+"}
        "Content-Type": "application/json"                  // {const: true}
      }
    Body 
      [@cat]
```

This example describes two commonly used HEADERS: `Authorization` and `Content-Type` using the
syntax of the [JSight Schema language](jsight-schema-0-3).

**IMPORTANT!!!** The rule `additionalProperties` of the SCHEMA root object in the DIRECTIVE
`Headers` is `true` by default, not `false`, as [JSight Schema language](jsight-schema-0-3)
defines it by default. It was developed to ensure that client and server applications, that send and
respond to HTTP-REQUESTS, were allowed to add any extra headers of their choice, without violating
the described requirements. You can explicitly restrict additional headers if necessary by
specifying `additionalProperties: false` in the root object of the SCHEMA:


```jsight
GET /cats
  200
    Headers 
      { // {additionalProperties: false}
        "Content-Type": "application/json" // {const: true}
      }
    Body [@cat]
```

### DIRECTIVES-**HTTP-METHODS**

> Requirement: req.japi.http_method 0.4.  
> Status: APPROVED 10/12/2021.

DIRECTIVES-HTTP-METHODS describe specific HTTP-REQUESTS. This language version supports the
following DIRECTIVES-HTTP-METHODS: `GET`, `POST`, `PUT`, `PATCH`, `DELETE`.

| DIRECTIVES-HTTP-METHODS      |                                                                                                                                                      |
|------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------:|
| KEYWORDS                     |                                                               `GET`, `POST`, `PUT`, `PATCH`, `DELETE`                                                |
| PARENT DIRECTIVES (possible) |                                            <ul align="left" style={{margin: 0}}><li>ROOT CONTEXT</li><li>`URL`</li></ul>                             |
| PARAMETERS                   |                                                                               `Path`                                                                 |
| ANNOTATION                   |                                                                               Allowed                                                                |
| BODY                         |                                                                          CHILD DIRECTIVES                                                            |
| CHILD DIRECTIVES (possible)  | <ul align="left"><li>`Description`</li><li>`Request`</li><li>`Query`</li><li>`Path`</li><li>DIRECTIVES-RESPONSES (i. e. `200`, `404` etc.)</li></ul> |
| Default CHILD DIRECTIVE      |                                                                                 No                                                                   | 

Any number of DIRECTIVES-HTTP-METHODS is allowed in the ROOT CONTEXT.

##### PARENT DIRECTIVES

DIRECTIVES-HTTP-METHODS can be placed in the following CONTEXTS:

- ROOT CONTEXT (in this case, the PATH must be specified in the PARAMETER of the DIRECTIVE `Path`,
  see below),
- The DIRECTIVE `URL` (in this case, the PARAMETER of the DIRECTIVE `Path` must not be specified).

Also, DIRECTIVES-HTTP-METHODS can be placed in the macro defined by the DIRECTIVE `MACRO` (see the
section [“DIRECTIVE "MACRO"”](#directive-macro)).

##### PARAMETERS

All DIRECTIVES-HTTP-METHODS have only one PARAMETER `Path`, which must contain an HTTP-REQUEST PATH.
The PATH `Path` may contain PATH PARAMETERS in curly braces, for example: `/cats/{id}`. For more
information on the PARAMETER of the DIRECTIVE `Path`, see the section “Reference of common
PARAMETERS of DIRECTIVES”, subsection [“PARAMETER "Path"”](#parameter-path).

If the DIRECTIVE-HTTP-METHOD is in the ROOT CONTEXT, then the PARAMETER `Path` must contain an
absolute PATH which starts with a slash `/` (for example, `/cats`).

If the DIRECTIVE-HTTP-METHOD is in the DIRECTIVE `URL` CONTEXT, then the PARAMETER `Path` must not
be specified.

In other words, if the PARAMETER `Path` is specified, then this DIRECTIVE is nested within the ROOT
CONTEXT. If the PARAMETER `Path` is not specified, then this DIRECTIVE is nested in the previous
DIRECTIVE `URL`. Let us explain it with the following EXAMPLE:

```jsight
GET /cats       # is in the ROOT CONTEXT

URL /cats

  POST    # is in the CONTEXT `URL /cats`

GET /dogs       # is in the ROOT CONTEXT
```

##### ANNOTATION

ANNOTATIONS are allowed to the DIRECTIVES-HTTP-METHOD.

The ANNOTATION to the DIRECTIVE-HTTP-METHOD is intended to provide a brief description of the
HTTP-METHOD.

##### BODY

The BODY of the DIRECTIVE-HTTP-METHOD:

- can be empty,
- can consist of one or more CHILD DIRECTIVES.

DIRECTIVES-HTTP-METHODS allow the following optional CHILD DIRECTIVES:

- DIRECTIVE `Description` (not more than one) — sets a detailed text description of the given HTTP
  REQUEST.
- DIRECTIVE `Request` (not more than one) — describes the request part of the HTTP REQUEST.
- DIRECTIVE `Path` (not more than one) — describes the PARAMETERS of the PATH, declared in the
  PARAMETER `Path`.
- DIRECTIVE `Query` (not more than one) — describes the requirements for the QUERY STRING.
- DIRECTIVES-RESPONSES (`200`, `301`, `404`, etc.) — document possible HTTP-RESPONSES to an
  HTTP-REQUEST. Several DIRECTIVES-RESPONSES can be specified in the HTTP-METHOD CONTEXT. Also,
  DIRECTIVES-RESPONSES may be repeated. If the same DIRECTIVE-RESPONSE is repeated several times,
  this means that different responses with the same code may come to the given HTTP REQUEST.

:::note

*In future language versions, the CHILD DIRECTIVES `Authorization` and `Cookies` will be available to
be specified in DIRECTIVES-HTTP-METHODS.*

:::

If the DIRECTIVE-HTTP-METHOD does not contain any DIRECTIVE-RESPONSE, then it means that the
corresponding HTTP-REQUEST can receive **any** response.

For DIRECTIVE-HTTP-METHODS, the default CHILD DIRECTIVE is not defined (see the [“CHILD DIRECTIVES
by default”](#child-directives-by-default)).

Here are some examples.

**Example 1.**

```jsight
JSIGHT 0.3

GET /cats
```

In the given example, we can see a PROJECT with only one HTTP-METHOD `GET` declared in relation to
the RESOURCE `/cats`. The DIRECTIVE `GET` is in the ROOT CONTEXT. According to this, it indicates a
PATH of `/cats`. There is no DIRECTIVE-RESPONSE specified for DIRECTIVE `GET`, this means that any
response can come to this HTTP REQUEST.

**Example 2.**

```jsight
URL /cats

  GET  // Get a list of cats.
    200 [@cat]

  POST // Create a new cat.
    Request
      @cat

URL /cats/{id} 
  GET // Get a cat.    
    200 @cat

  PUT // Update the whole cat.
    Request
      @cat

  PATCH // Change the status of a cat.
    Request
      @catStatus

  DELETE // Delete a cat.
    200
      "OK" // {const: true}
```

In the given example, we can observe the following:

- The resource `/cats` with METHODS `GET` and `POST` is declared.
- The resource `/cats/{id}` with METHODS `GET`, `PUT`, `PATCH`, `DELETE` is declared.

**Example 3.**

```jsight
GET /pets/{id} // Get an animal by its id.
  200 @cat // If it's a cat.
  200 @dog // If it's a dog.
  200 @pig // If it's a pig.
  401 any // Authorization error.
```

The given example describes a number of HTTP-RESPONSES with the same code `200`. This means that the
request `GET /pets/{id}` may result in one of the specified successful responses.

### DIRECTIVE **"INCLUDE"**

> Requirement: req.japi.include 0.1.  
> Status: APPROVED 04/29/2022.

The DIRECTIVE `INCLUDE` is intended to include the contents of an external file in the project.

|        DIRECTIVE "INCLUDE"   |                       |
| ---------------------------- | :-------------------: |
| KEYWORD                      |       `INCLUDE`       |
| PARENT DIRECTIVES (possible) |          Any          |
| PARAMETERS                   |      `Filename` *     |
| ANNOTATION                   |     Not allowed       |
| BODY                         |          No           |
| CHILD DIRECTIVES (possible)  |          No           |
| Default CHILD DIRECTIVE      |          No           |

##### PARENT DIRECTIVES

The DIRECTIVE `INCLUDE` can be placed in the CONTEXT of any DIRECTIVE.

##### PARAMETERS

The DIRECTIVE `INCLUDE` has only one mandatory PARAMETER `Filename`, which specifies the path to the
file on disk. You can only specify the relative path to a file located in the same directory or in
child directories as of the main project file.

Recursive inclusion of files by the DIRECTIVE `INCLUDE` is prohibited.

The separator for directories and files should be the symbol `/`. The file path must not start
with the symbols `.` or `/`. The file path must not contain the symbols `/./` or `/../`.

The DIRECTIVE `INCLUDE` can be placed anywhere within the PROJECT, where the syntax allows a
new DIRECTIVE to be placed.

Let's consider some examples.

**Example 1.**

```jsight
JSIGHT 0.3

INCLUDE types.jst

GET /cats
    200 [@cat]
```

In this example, it is assumed that TYPE `@cat` is declared in a file `types.jst`. The file
`types.jst` is included in the *main* PROJECT file. It is not necessary to define TYPE `@cat` a
second time in the PROJECT.

**Example 2.**

```jsight
JSIGHT 0.3

INCLUDE types/cat.jst
INCLUDE types/dog.jst

GET /cats
    200 [@cat]

GET /dogs
    200 [@dog]
```

In the given example, the DIRECTIVE `INCLUDE` is used couple times to define TYPES (we assume 
that the `@cat` and `@dog` TYPES are described in the corresponding files).

**Example 3.**

```jsight
JSIGHT 0.3

GET /cats
  200 [@cat]
  INCLUDE common_errors.jst

GET /dogs
  200 [@dog]
  INCLUDE common_errors.jst
```

In the given example, the DIRECTIVE `INCLUDE` is used to include the same file twice.

##### ANNOTATION

The DIRECTIVE `INCLUDE` CANNOT have ANNOTATION.

##### BODY

The DIRECTIVE `INCLUDE` has no BODY.

### DIRECTIVE **"INFO"**

> Requirement: req.japi.info 0.3.  
> Status: APPROVED 10/12/2021.

The DIRECTIVE `INFO` is intended to describe the PROJECT in general.

|        DIRECTIVE "INFO"     |                   |
| --------------------------- | :---------------: |
| KEYWORD                     |      `INFO`       |
| PARENT DIRECTIVE            |    ROOT CONTEXT   |
| PARAMETERS                  |        No         |
| ANNOTATION                  |    Not allowed    |
| BODY                        | CHILD DIRECTIVES  |
| CHILD DIRECTIVES (possible) | <ul align="left" style={{margin: 0}}><li>`Title`</li><li>`Version`</li><li>`Description`</li></ul> |
| Default CHILD DIRECTIVE     |         No        |

In the ROOT CONTEXT only one DIRECTIVE `INFO` is allowed.

##### PARENT DIRECTIVES

The DIRECTIVE `INFO` can only be placed in the ROOT CONTEXT.

Technically you can place DIRECTIVE `INFO` in DIRECTIVE `MACRO`, but it does not make sense (see
the section [“DIRECTIVE "MACRO"”](#directive-macro)).

##### PARAMETERS

The DIRECTIVE `INFO` has no PARAMETERS.

##### ANNOTATION

The DIRECTIVE `INFO` CANNOT have ANNOTATION.

##### BODY

The BODY of the DIRECTIVE `INFO` may contain the following CHILD DIRECTIVES:

- `Title` (not more than one) — sets the title of the PROJECT.
- `Version` (not more than one) — indicates the version of the API PROJECT.
- `Description` (not more than one) — gives a detailed description of the PROJECT.

Example:

```jsight
INFO
  Title "Catsbook API"
  Version 1.0
  Description
    API of social network
    for cats Catsbook.
```

### DIRECTIVE **"JSIGHT"**

> Requirement: req.japi.jsight 0.4.  
> Status: APPROVED 01/10/2022.

The DIRECTIVE `JSIGHT` is intended to specify the version of the JSight API language.

|     DIRECTIVE "JSIGHT"    |                      |
| ------------------------- | :------------------: |
| KEYWORD                   |       `JSIGHT`       |
| PARENT DIRECTIVES         |     ROOT CONTEXT     |
| PARAMETERS                |       `Version` *    |
| ANNOTATION                |      Not allowed     |
| BODY                      |          No          |
| CHILD DIRECTIVES          |          No          |
| Default CHILD DIRECTIVE   |          No          |

An important feature of the DIRECTIVE `JSIGHT` is that it must be placed the first in the root of
the PROJECT, before any other DIRECTIVES.

In one PROJECT only one DIRECTIVE `JSIGHT` is allowed.

The directive `JSIGHT` cannot be omitted.

Example:

```jsight
JSIGHT 0.3

GET /cats
  200 [@cat]
```

##### PARENT DIRECTIVES

The DIRECTIVE `JSIGHT` can only be placed in the ROOT CONTEXT.

##### PARAMETERS

The DIRECTIVE `JSIGHT` has one PARAMETER `Version`. The PARAMETER `Version` must contain a string
with the version number of the JSight API language that was used to write this PROJECT.

Example:

```jsight
JSIGHT 0.3
```

##### ANNOTATION

The DIRECTIVE `JSIGHT` CANNOT have ANNOTATION.

##### BODY

The DIRECTIVE `JSIGHT` does NOT have a BODY.

### DIRECTIVE **"MACRO"**

> Requirement: req.japi.macro 0.3.  
> Status: APPROVED 02/10/2022.

The DIRECTIVE `MACRO` is intended to declare a named macro (group of DIRECTIVES) that can be
pasted by DIRECTIVE `PASTE` (see the section [“DIRECTIVE "PASTE"”](#directive-paste)) at
different places within the PROJECT.

|        DIRECTIVE "MACRO"     |                       |
| ---------------------------- | :-------------------: |
| KEYWORD                      |        `MACRO`        |
| PARENT DIRECTIVES (possible) |      ROOT CONTEXT     |
| PARAMETERS                   |        `Name` *       |
| ANNOTATION                   |     Not allowed       |
| BODY                         |    CHILD DIRECTIVES   |
| CHILD DIRECTIVES (possible)  |  Any, except `MACRO`  |
| Default CHILD DIRECTIVE      |          No           |

In the ROOT CONTEXT any number of DIRECTIVES `MACRO` is allowed.

##### PARENT DIRECTIVES

The DIRECTIVE `MACRO` can only be placed in the ROOT CONTEXT.

##### PARAMETERS

The DIRECTIVE `MACRO` has only one mandatory PARAMETER `Name`, in which the name of the macro is
specified.

The name of the macro must be distinct from all other names of macros within the PROJECT.

Other requirements for the name of the macro are outlined in the subsection [“Names”](#names) of the
section “Common Syntax Rules”.

Example:

```jsight
MACRO @commonHeaders
(
  Headers
    {
      "Authorization": "Basic dG9tQGNhdC5jb206YWJjMTIz=", // {regex: "Basic [A-Za-z0-9+\/=]+"}
      "Content-Type": "application/json"                  // {const: true}
    }
)
```

In the given example, a macro is declared with the name `@commonHeaders`.

##### ANNOTATION

It is not allowed to input an ANNOTATION to the DIRECTIVE `MACRO`.

##### BODY

The BODY of the DIRECTIVE `MACRO` is made up of one or more nested CHILD DIRECTIVES. It is required
that there be at least one nested DIRECTIVE in the BODY.

The DIRECTIVE `MACRO` allows any CHILD DIRECTIVE, except `MACRO`. CHILD DIRECTIVES, in turn, may
have other CHILD DIRECTIVES.

For DIRECTIVE `MACRO`, the default CHILD DIRECTIVE is not defined.

Example:

```jsight
MACRO @commonErrors
(
  400
    Body regex
      /Bad request/
  401 any
  405 any
  500 any
)
```

In the given example, the macro `@commonErrors` is declared, which contains four
CHILD DIRECTIVES `400`,` 401`, `405`, and `500`.

The directive `MACRO` is useful for specifying frequently repeated elements of the PROJECT. After a
macro using the `MACRO` DIRECTIVE is declared, you may insert this macro as many times as you like
throughout the PROJECT by referring to it by name using the `PASTE` DIRECTIVE (see the section
[“DIRECTIVE "PASTE"”](#directive-paste)).

Note, that the DIRECTIVE `MACRO` allows DIRECTIVE `PASTE` as a CHILD DIRECTIVE.

Example:

```jsight
MACRO @errors
(
    PASTE @errors_3xx
    PASTE @errors_4xx
)

MACRO @errors_3xx
(
    300 any
    301 any
)

MACRO @errors_4xx
(
    400 any
    404 any
    409 any
)
```

:::note

*The DIRECTIVE `MACRO` may have any CHILD DIRECTIVES, except DIRECTIVES `MACRO`. Considering this,
it is advised to always explicitly mark the boundaries of its BODY with parenthesis, while using the
DIRECTIVE `MACRO`, otherwise the DIRECTIVE `MACRO` would contain all the remaining directives until
the next DIRECTIVE `MACRO`.*

:::

### DIRECTIVE **"PASTE"**

> Requirement: req.japi.paste 0.3.  
> Status: APPROVED 02/10/2022.

The DIRECTIVE `PASTE` is intended to paste a named macro (group of DIRECTIVES) defined by
DIRECTIVE `MACRO` (see the section [“DIRECTIVE "MACRO"”](#directive-macro)) to different places
within the PROJECT.

|        DIRECTIVE "PASTE"     |                       |
| ---------------------------- | :-------------------: |
| KEYWORD                      |        `PASTE`        |
| PARENT DIRECTIVES (possible) |          Any          |
| PARAMETERS                   |        `Name` *       |
| ANNOTATION                   |     Not allowed       |
| BODY                         |          No           |
| CHILD DIRECTIVES (possible)  |          No           |
| Default CHILD DIRECTIVE      |          No           |

##### PARENT DIRECTIVES

The DIRECTIVE `PASTE` can be placed in the CONTEXT of any DIRECTIVE.

##### PARAMETERS

The DIRECTIVE `PASTE` has only one mandatory PARAMETER `Name`, in which the name of the macro is
specified. A macro with this name must be declared anywhere within the PROJECT using the DIRECTIVE
`MACRO`.

The DIRECTIVE `PASTE` can be placed anywhere within the PROJECT, where the syntax allows a
new DIRECTIVE to be placed. Also, the macro pasted by the DIRECTIVE `PASTE` should be
interpreted in the same way, as if the BODY of the DIRECTIVE `MACRO` on which the reference is
made, was used in the code instead of the DIRECTIVE `PASTE`.

The directives `MACRO` and `PASTE` are useful for specifying frequently repeated elements of the
PROJECT.

Let us explain with an example:

```jsight
GET /cats

  200 [@cat]
  PASTE @commonErrors

GET /dogs

  200 [@dog]
  PASTE @commonErrors

MACRO @commonErrors
(
  400 any
  401 any
  405 any
  500 any
)
```

In the given example, we can see the declared macro `@commonErrors` by the DIRECTIVE `MACRO`.
Also, we see this macro referenced in the CONTEXT of DIRECTIVES `GET`, using DIRECTIVES `PASTE`.
This example should be interpreted as if the BODY of the DIRECTIVE `MACRO @commonErrors` is used
instead of the DIRECTIVE `PASTE @commonErrors`. That is, the given example is similar to the
following code:

```jsight
GET /cats

  200 [@cat]
  400 any
  401 any
  405 any
  500 any

GET /dogs

  200 [@dog]
  400 any
  401 any
  405 any
  500 any

MACRO @commonErrors
(
  400 any
  401 any
  405 any
  500 any
)
```

##### ANNOTATION

The DIRECTIVE `PASTE` CANNOT have ANNOTATION.

##### BODY

The DIRECTIVE `PASTE` has no BODY.

### DIRECTIVE **"Path"**

> Requirement: req.japi.http_method.path 0.3.  
> Status: APPROVED 10/12/2021.

The DIRECTIVE `Path` is intended to describe the RESOURCE PATH PARAMETERS in the DIRECTIVE `URL` and
in the DIRECTIVES-HTTP-METHODS (`GET`, `POST`, etc.).

|        DIRECTIVE "Path"      |                      |
| ---------------------------- | :------------------: |
| KEYWORD                      |        `Path`        |
| PARENT DIRECTIVES (possible) | <ul align="left" style={{margin: 0}}><li>`URL`</li><li>DIRECTIVES-HTTP-METHODS (`GET`, `POST`, etc.)</li></ul> |
| PARAMETERS                   |         No           |
| ANNOTATION                   |      Not allowed     |
| BODY                         |     SCHEMA `jsight`  |
| CHILD DIRECTIVES             |          No          |
| Default CHILD DIRECTIVE      |          No          |

##### PARENT DIRECTIVES

The DIRECTIVE `Path` can be placed in the following CONTEXTS:

- DIRECTIVE `URL`,
- DIRECTIVES-HTTP-METHODS (`GET`, `POST` and etc.).

Also, the DIRECTIVE `Path` can be placed in the macro defined by the DIRECTIVE `MACRO` (see the
section [“DIRECTIVE "MACRO"”](#directive-macro)).

##### PARAMETERS

The DIRECTIVE `Path` has no PARAMETERS.

##### ANNOTATION

The DIRECTIVE `Path` CANNOT have ANNOTATION.

##### BODY

The BODY of the DIRECTIVE `Path` must contain a SCHEMA of the TYPE `jsight` (in [JSight Schema
language](jsight-schema-0-3)). The SCHEMA describes the requirements for the PATH PARAMETERS that
are declared in the PARAMETER `Path` of the PARENT DIRECTIVE. (The PARENT DIRECTIVE can be either
`URL` or DIRECTIVE-HTTP-METHOD). At the root of the SCHEMA an object should be placed, each of whose
properties describes a separate PATH PARAMETER `Path`. The KEYS properties must match the names of
PATH PARAMETERS that were declared in `Path`.

The root object of the SCHEMA cannot have RULES: `nullable: true`, `or`,
`additionalProperties: true`.

It is not required to describe all PATH PARAMETERS, only a part of the PATH PARAMETERS can be
described. There are no any requirements for undescribed PATH PARAMETERS.

Let's consider an example:

```jsight
GET /cats/{id}

  Path
    {
      "id": 12 // Cat identifier.
    }

  200 @cat
```

In the given example, a requirement is specified for the PATH PARAMETER `id` — it must be an
integer. The caption of this PARAMETER is also indicated: “Cat identifier”.

Another example with two PATH PARAMETERS:

```jsight
URL /cats/{id}/friends/{friendId}

  Path
    {
      "id"      : 12, // Cat identifier.
      "friendId": 13  // Another cat identifier.
    }

  GET
    200 @cat
```

For one PATH PARAMETER, you can set only once the requirements using the DIRECTIVE `Path` (for more
information, see the section [“PARAMETER "Path"”](#parameter-path)). Let's consider an example of a
rule violation:

```jsight
URL /cats/{id}
  Path
    {
      "id": 12
    }
  GET
    200 @cat

GET /cats/{id}/friends
  Path
    {
      "id": 12      # --ERROR!!! The requirements to the PATH PARAMETER `id` 
    }               #            have already been specified earlier.
  200 @cat
```

The way of processing RESOURCES PATHS is a key algorithm of the JSight language. More information on
the features of this algorithm, in which the DIRECTIVE `Path` also participates, can be seen in the
section [“PARAMETER "Path"”](#parameter-path).

### DIRECTIVE **"Query"**

> Requirement: req.japi.http_method.query 0.4.  
> Status: APPROVED 01/10/2022.

The DIRECTIVE `Query` is intended to describe the URL QUERY STRING in DIRECTIVES-HTTP-METHODS.

|    DIRECTIVE "Query"    |                       |
| ----------------------- | :-------------------: |
| KEYWORD                 |        `Query`        |
| PARENT DIRECTIVES       | DIRECTIVES-HTTP-METHODS (`GET`, `POST`, etc.) |
| PARAMETERS              | <ul align="left" style={{margin: 0}}><li>`QueryExample`</li><li>`Format`</li></ul> |
| ANNOTATION              |      Not allowed      |
| BODY                    |     SCHEMA `jsight`   |
| CHILD DIRECTIVES        |          No           |
| Default CHILD DIRECTIVE |          No           |

##### PARENT DIRECTIVES

The DIRECTIVE `Query` can only be placed in the CONTEXT of DIRECTIVES-HTTP-METHODS.

Also, the DIRECTIVE `Query` can be placed in the macro defined by the DIRECTIVE `MACRO` (see the
section [“DIRECTIVE "MACRO"”](#directive-macro)).

##### PARAMETERS

The DIRECTIVE `Query` has two optional PARAMETERS:

1. `QueryExample` — an example of a valid QUERY STRING that does not contain a leading question mark
   `?`. The value `QueryExample` must be URL-encoded (see
   https://datatracker.ietf.org/doc/html/rfc3986#section-1.3) and conform to the FORMAT, specified
   in the PARAMETER `Format`. Also, `QueryExample` can be omitted.

2. `Format` — the FORMAT, in which the data encoded in the QUERY STRING must be presented. In the
   current language version, only one FORMAT `htmlFormEncoded` is supported (see RFC 1866, section
   [“The form-urlencoded Media Type”](https://www.ietf.org/rfc/rfc1866.html#section-8.2.1)).
   Furthermore, the PARAMETER `Format` can take the value of `noFormat`, which means that there are
   no any requirements for the FORMAT of the QUERY STRING. If the PARAMETER `Format` is omitted, the
   default value is `htmlFormEncoded`.

Examples of the DIRECTIVE `Format` with different PARAMETERS are given below in the subsection
[“BODY”](#directive-query-body).

##### ANNOTATION

The DIRECTIVE `Query` CANNOT have ANNOTATION.

##### BODY {#directive-query-body}

The BODY of the DIRECTIVE `Query` must contain a SCHEMA in the NOTATION `jsight` (in [JSight Schema
language](jsight-schema-0-3)). The SCHEMA describes the data structure that must be serialized into
a QUERY STRING in the FORMAT specified.

If the PARAMETER `QueryExample` is specified, then the value of the PARAMETER `QueryExample` must
match the SCHEMA requirements taking into account the serialization FORMAT `Format`.

Let's consider some examples.

**Example 1.**

```jsight
GET /cats // Get a page with cats.

  Query "page=1&per_page=50"
    {
      "page": 1,
      "per_page": 50 // {optional: true}
    }

  200 [@cat]
```

The given example requires the mandatory parameter `page` and optional parameter `per_page` to be
specified in the QUERY STRING.

**Example 2.**

```jsight
GET /cats // Get a page with cats of the right size.

  Query "page=1&per_page=50&filter[size]=XXL"
    {
      "page": 1,
      "per_page": 50,  // {optional: true}
      "filter": {      // {optional: true}
        "size": "XXL", // {optional: true, enum: ["S", "L", "M"]} - Filter by cat's size.
        "age" : 12     // {optional: true                       } - Filter by cat's age.
      }
    }

  200 [@cat]
```

In the given example an optional object `filter` with the ability to specify parameters for
filtering cats by `size` and `age` is added in the QUERY STRING.

**Example 3.**

```jsight
GET /cats // Get a page with cats of the right size.

  Query
    {
      "page": 1,
      "per_page": 50,  // {optional: true}
      "filter": {      // {optional: true}
        "size": "XXL", // {optional: true, enum: ["S", "L", "M"]} - Filter by cat's size.
        "age" : 12     // {optional: true                       } - Filter by cat's age.
      }
    }

  200 [@cat]
```

The given example is similar to the previous one, but the PARAMETER `QueryExample` is omitted in the
the DIRECTIVE `Query`. It is preferable to always specify the PARAMETER `QueryExample` because it
helps to faster understand the structure of the QUERY STRING. However, if the example of a valid
QUERY LINE is too long to be inserted in the documentation, it can be omitted.

**Example 4.**

```jsight
GET /cats // Get a page with cats.

  Query "page=1&per_page=50" htmlFormEncoded
    {
      "page": 1,
      "per_page": 50 // {optional: true}
    }

  200 [@cat]
```

In the given example, the FORMAT of serialization of data into a QUERY STRING is explicitly
specified.

**Example 5.**

Unfortunately, there are no strict standards in the world for the FORMAT of data serialization in
the QUERY STRING. It might happen, that the JSight API language will not be able to describe the
requirements for the QUERY STRING, nevertheless, this QUERY STRING should be given at least as an
example.

In this case, you must specify `noFormat` in the DIRECTIVE `Query` in the PARAMETER `Format`.

```jsight
GET /cats // Get a page with cats.

  Query "myVeryStrange[way]of%20query=string&encoding" noFormat
    {
      "myVeryStrange": [
        "way"
      ],
      "of query": [
        "string",
        "encoding"
      ]
    }

  200 [@cat]
```

### DIRECTIVE **"Request"**

> Requirement: req.japi.http_method.request 0.3.  
> Status: APPROVED 10/12/2021.

The DIRECTIVE `Request` describes the request part of an HTTP REQUEST.

|        DIRECTIVE "Request"  |                         |
| --------------------------- | :---------------------: |
| KEYWORD                     |        `Request`        |
| PARENT DIRECTIVES           | DIRECTIVES-HTTP-METHODS |
| PARAMETERS                  | <ol align="left" style={{margin: 0}}><li>`Type`</li><li>`SchemaNotation`</li></ol> |
| ANNOTATION                  |        Not allowed      |
| BODY                        |     CHILD DIRECTIVES    |
| CHILD DIRECTIVES (possible) | <ul align="left" style={{margin: 0}}><li>`Headers`</li><li>`Body` *</li></ul> |
| Default CHILD DIRECTIVE     |          `Body`         | 

##### PARENT DIRECTIVES

The DIRECTIVE `Request` can only be placed in the CONTEXT of DIRECTIVE-HTTP-METHODS.

Also, the DIRECTIVE `Request` can be placed in the macro defined by the DIRECTIVE `MACRO` (see the
section [“DIRECTIVE "MACRO"”](#directive-macro)).

##### PARAMETERS

The DIRECTIVE `Request` has no PARAMETERS of its own.

However, if the declaration of the CHILD DIRECTIVE by default `Body` is omitted, then the PARAMETERS
of the omitted DIRECTIVE `Body` can be specified in the PARAMETERS of the DIRECTIVE `Request`,
namely the PARAMETERS `Type` or `SchemaNotation` (for more information, see the section [“DIRECTIVE
"Body"”](#directive-body), and also the section [“CHILD DIRECTIVES by
default”](#child-directives-by-default)).

As an example, consider the following PROJECT:

```jsight
JSIGHT 0.3

POST /cats // Add a cat.
  Request
    Body jsight
      {
        "id" : 1,
        "name" : "Tom"
      }
```

If the CHILD DIRECTIVE by default `Body` is omitted, the PARAMETER `SchemaNotation` with the value
`jsight` should be moved to the DIRECTIVE `Request`, as follows:

```jsight
POST /cats // Add a cat.
  Request jsight
    {
      "id" : 1,
      "name" : "Tom"
    }
```

The following example shows how the PARAMETER of the TYPE `Type` with the value of `@cat` is moved
from `Body` to `Request`:

```jsight
# Full notation

POST /cats // Add a cat.
  Request 
    Body @cat

# Shorthand notation — the DIRECTIVE `Body` is omitted, the PARAMETER `Type` has moved to the DIRECTIVE `Request`

POST /cats // Add a cat.
  Request @cat
```

##### ANNOTATION

ANNOTATION is NOT allowed to the DIRECTIVE `Request`.

##### BODY

The DIRECTIVE `Request` may contain two CHILD DIRECTIVES:

- `Headers` (not more than one),
- `Body` (one and only one, can also be omitted) is the CHILD DIRECTIVE by default (see the section
  [“CHILD DIRECTIVES by default”](#child-directives-by-default)).

The BODY of the DIRECTIVE `Request` must contain the DIRECTIVE `Body` in one way or another. There
are three possible cases:

1. The DIRECTIVE `Request` contains an explicitly declared DIRECTIVE `Body`.
2. The DIRECTIVE `Request` contains the BODY of the DIRECTIVE `Body` due to the fact that the
   declaration of the DIRECTIVE `Body` is omitted since it is CHILD DIRECTIVE by default.
3. The DIRECTIVE `Request` implicitly contains the omitted DIRECTIVE `Body`, while the BODY of the
   DIRECTIVE `Body` is empty. In this case, the DIRECTIVE `Request` must include the PARAMETER
   `Type`, which moved into it from the omitted DIRECTIVE `Body`.

Here are examples for each case.

**Example 1.** The directive `Body` is specified explicitly.

```jsight
POST /cats // Add a cat.
  Request 
    Body
      {
        "id" : 1,
        "name" : "Tom"
      }
```

**Example 2.** The directive `Body` is omitted, and its BODY is moved to the DIRECTIVE `Request`.

```jsight
POST /cats // Add a cat.
  Request 
    {
      "id" : 1,
      "name" : "Tom"
    }
```

**Example 3.** The directive `Body` is omitted. The BODY of the omitted DIRECTIVE `Body` is empty,
but its PARAMETER `Type` with the value `@cat` moved to the DIRECTIVE `Request`.

```jsight
POST /cats // Add a cat.
  Request @cat
```

The CHILD DIRECTIVE `Headers` describes the headers of the request part of the HTTP REQUEST (for
more information, see the section [“DIRECTIVE "Headers"”](#directive-headers)).

Example of using the DIRECTIVE `Headers`:

```jsight
POST /cats // Add a cat.
  Request 
    Headers 
      {
        "X-Header": "my Very Important Header"
      }
    Body
      @cat
```

Please note that in the given example, the DIRECTIVE by default `Body` cannot be omitted, since it
is not the only CHILD DIRECTIVE in the context of the DIRECTIVE `Request` (for more information, see
the section [“CHILD DIRECTIVES by default”](#child-directives-by-default)).

### DIRECTIVES-**RESPONSES**

> Requirement: req.japi.http_method.response 0.3.  
> Status: APPROVED 10/12/2021.

DIRECTIVES-RESPONSES describe responses to HTTP-REQUESTS. Keywords of DIRECTIVES-RESPONSES contain
three digits that match possible HTTP status codes, for example, `200`, `301`, `404`, `500`, etc.
(see https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

|       DIRECTIVES-RESPONSES   |                         |
| :--------------------------: | :---------------------: |
| KEYWORD                      |    HTTP status codes    |
| PARENT DIRECTIVES            | DIRECTIVES-HTTP-METHODS |
| PARAMETERS                   | <ol align="left" style={{margin: 0}}><li>`Type`</li><li>`SchemaNotation`</li></ol> |
| ANNOTATION                   |         Allowed         |
| BODY                         |    CHILD DIRECTIVES     |
| CHILD DIRECTIVES (possible)  | <ul align="left" style={{margin: 0}}><li>`Headers`</li><li>`Body` *</li></ul> |
| Default CHILD DIRECTIVE      |          `Body`         | 

##### PARENT DIRECTIVES

DIRECTIVES-RESPONSES can only be placed in the CONTEXT of DIRECTIVE-HTTP-METHODS.

Also, DIRECTIVES-RESPONSES can be placed in the macro defined by the DIRECTIVE `MACRO` (see the
section [“DIRECTIVE "MACRO"”](#directive-macro)).

##### PARAMETERS

DIRECTIVES-RESPONSES do not have their own PARAMETERS.

However, if the declaration of the CHILD DIRECTIVE by default `Body` is omitted, then the PARAMETERS
of the omitted DIRECTIVE `Body` can be specified in the PARAMETERS of the DIRECTIVE-RESPONSE, namely
the PARAMETERS `Type` or `SchemaNotation` (for more information, see the section [“DIRECTIVE
"Body"”](#directive-body), and also the section [“CHILD DIRECTIVES by
default”](#child-directives-by-default)).

As an example, consider the following JSight API code:

```jsight
GET /cats/{id} // Get a cat.
  200
    Body jsight
      {
        "id" : 1,
        "name" : "Tom"
      }
```

If the CHILD DIRECTIVE by default `Body` is omitted, the PARAMETER `SchemaNotation` with the value
`jsight` should be moved to the DIRECTIVE `200` as follows:

```jsight
GET /cats/{id} // Get a cat.
  200 jsight
    {
      "id" : 1,
      "name" : "Tom"
    }
```

The below example shows how the PARAMETER of the TYPE `Type` with the value of `@cat` is moved from
`Body` in `200`:

```jsight
# Full notation

POST /cats/{id} // Get a cat.
  200 
    Body @cat

# Shorthand notation —  the DIRECTIVE `Body` is omitted, the PARAMETER `Type` has moved to the DIRECTIVE `200`.

POST /cats/{id} // Get a cat.
  200 @cat
```

##### ANNOTATION

It is allowed to input an ANNOTATION to the DIRECTIVES-RESPONSES, which in this case is intended for
HTTP-RESPONSE descriptions.

##### BODY

The DIRECTIVE-RESPONSE may contain two CHILD DIRECTIVES:

- `Headers` (not more than one),
- `Body` (one and only one, can be omitted) — is the CHILD DIRECTIVE by default (for more
  information, see the section [“CHILD DIRECTIVES by default”](#child-directives-by-default)).

The BODY of the DIRECTIVE-RESPONSE must contain the DIRECTIVE `Body` in one way or another. There
are three possible cases:

1. The DIRECTIVE-RESPONSE contains the explicitly declared DIRECTIVE `Body`.
2. The DIRECTIVE-RESPONSE contains the BODY of the DIRECTIVE `Body` due to the fact that the
   declaration of the DIRECTIVE `Body` is omitted since it is CHILD DIRECTIVE by default.
3. The DIRECTIVE-RESPONSE implicitly contains the omitted DIRECTIVE `Body`, while the BODY of the
   DERECTIVE `Body` is empty. In this case, the DIRECTIVE-RESPONSE must contain the PARAMETER
   `Type`, which moved into it from the omitted DIRECTIVE `Body`.

Here are examples for each case.

**Example 1.** The directive `Body` is specified explicitly.

```jsight
GET /cats/{id} // Get a cat.
  200 
    Body
      {
        "id" : 1,
        "name" : "Tom"
      }
```

**Example 2.** The directive `Body` is omitted, and its BODY has moved to the DIRECTIVE `200`.

```jsight
GET /cats/{id} // Get a cat.
  200 
    {
      "id" : 1,
      "name" : "Tom"
    }
```

**Example 3.** The directive `Body` is omitted. The BODY of the omitted DIRECTIVE `Body` is empty,
but its PARAMETER `Type` with the value `@cat` has moved to DIRECTIVE `200`.

```jsight
GET /cats/{id} // Get a cat.
  200 @cat
```

**Example 4.** The developer made a mistake by not specifying the DIRECTIVE `Body`.

```jsight
POST /cats/counter
  200                  # --ERROR! It is not right.
```

Most probably, in this case the developer wanted to specify that in response to a REQUEST, any
HTTP-BODY could come. It is correct to do by specifying the PARAMETER `SchemaNotation` with the
value `any` (for more information, see the section [“DIRECTIVE "Body"”](#directive-body)):

```jsight
POST /cats/counter
  200 any              # Now it is correct.
```

In addition to the DIRECTIVE `Body`, the DIRECTIVE-RESPONSE may include an optional DIRECTIVE
`Headers`, which describes the headers of the RESPONSE to the HTTP REQUEST (for more information,
see the section [“DIRECTIVE "Headers"”](#directive-headers)).

Example of using the DIRECTIVE `Headers`:

```jsight
GET /cats/{id} // Get a cat.
  200 
    Headers 
      {
        "X-Header": "my Very Important Header"
      }
    Body @cat
```

Please note that in the given example, the DIRECTIVE by default `Body` cannot be omitted, since it
is not the only CHILD DIRECTIVE in the context of the DIRECTIVE `Request` (for more information, see
the section [“CHILD DIRECTIVES by default”](#child-directives-by-default)).

### DIRECTIVE **"SERVER"**

> Requirement: req.japi.server 0.3.  
> Status: APPROVED 10/12/2021.

The DIRECTIVE `SERVER` is intended to describe the server where the API is physically located.

|   DIRECTIVE "SERVER"    |                   |
| ----------------------- | :---------------: |
| KEYWORD                 |     `SERVER`      |
| PARENT DIRECTIVES       |   ROOT CONTEXT    |
| PARAMETERS              |      `Name` *     |
| ANNOTATION              |       Allowed     |
| BODY                    |  CHILD DIRECTIVES |
| CHILD DIRECTIVES        |      `BaseUrl` *  |
| Default CHILD DIRECTIVE |         No        |

In the ROOT CONTEXT any number of DIRECTIVES `SERVER` is allowed.

##### PARENT DIRECTIVES

The DIRECTIVE `SERVER` can only be placed in the ROOT CONTEXT.

Technically you can place DIRECTIVE `SERVER` in DIRECTIVE `MACRO`, but it does not make sense (see
the section [“DIRECTIVE "MACRO"”](#directive-macro)).

##### PARAMETERS

The  DIRECTIVE `SERVER` has one required PARAMETER `Name`, in which the server name is specified.
The name must start with the symbol `@`.

The server name must be distinct from all other server names within the PROJECT.

For other requirements for server name, see the subsection [“Names”](#names) of the section “Common
Syntax Rules”.

Example:

```jsight
SERVER @CATS_API_SERVER
  BaseUrl "https://catsbook.com/api"
```

##### ANNOTATION

The DIRECTIVE `SERVER` may have an ANNOTATION that briefly describes this server.

Example:

```jsight
SERVER @CATS_API_SERVER // Real server catsbook.com API
  BaseUrl "https://catsbook.com/api"
```

##### BODY

The BODY of the DIRECTIVE `SERVER` may contain only one mandatory CHILD DIRECTIVE `BaseUrl`, which
specifies the server URL.

Example:

```jsight
SERVER @CATS_API_SERVER
  BaseUrl "https://catsbook.com/api"
```

### DIRECTIVE **"Title"**

> Requirement: req.japi.info.title 0.3.  
> Status: APPROVED 10/12/2021.

The DIRECTIVE `Title` is intended to indicate the title of the PROJECT.

|    DIRECTIVE "Title"    |                    |
| ----------------------- | :----------------: |
| KEYWORD                 |      `Title`       |
| PARENT DIRECTIVE        |      `INFO`        |
| PARAMETERS              |     `Title` *      |
| ANNOTATION              |     Not allowed    |
| BODY                    |        No          |
| CHILD DIRECTIVES        |        No          |
| Default CHILD DIRECTIVE |        No          | 

##### PARENT DIRECTIVES

The DIRECTIVE `Title` can be placed only inside the DIRECTIVE `INFO`.

Technically you can place DIRECTIVE `Title` in DIRECTIVE `MACRO`, but it does not make sense (see
the section [“DIRECTIVE "MACRO"”](#directive-macro)).

##### PARAMETERS

The DIRECTIVE `Title` has one mandatory PARAMETER `Title`, in which the title of the PROJECT must be
specified.

Example:

```jsight
INFO
  Title "Catsbook API"
```

##### ANNOTATION

The DIRECTIVE `Title` CANNOT have ANNOTATION.

##### BODY

The DIRECTIVE `Title` has no BODY.

### DIRECTIVE **"TYPE"**

> Requirement: req.japi.type 0.3.  
> Status: APPROVED 10/12/2021.

The DIRECTIVE `TYPE` is intended to declare and describe named USER TYPES of data that can be
referred to from different places of the PROJECT.

|        DIRECTIVE "TYPE"            |                    |
| ---------------------------------- | :----------------: |
| KEYWORD                            |       `TYPE`       |
| PARENT DIRECTIVE                   |    ROOT CONTEXT    |
| PARAMETERS                         | <ul align="left" style={{margin: 0}}><li>`Name` *</li><li>`SchemaNotation`</li></ul> |
| ANNOTATION                         |       Allowed      |
| BODY                               | BODY of CHILD DIRECTIVE by default `Schema` |
| CHILD DIRECTIVES (possible)        | <p align="left" style={{margin: 0}}>In this version no.<br />In future versions: `Description`, `Schema`.</p> |
| Default CHILD DIRECTIVE            | <p align="left" style={{margin: 0}}>In future versions: `Schema`</p> |

In the ROOT CONTEXT any number of DIRECTIVES `TYPE` is allowed.

##### PARENT DIRECTIVES

The DIRECTIVE `TYPE` can only be placed in the ROOT CONTEXT.

Technically you can place DIRECTIVE `TYPE` in DIRECTIVE `MACRO`, but it does not make sense (see
the section [“DIRECTIVE "MACRO"”](#directive-macro)).

##### PARAMETERS

The DIRECTIVE `TYPE` has the following PARAMETERS:

1. `Name` is mandatory PARAMETER and represents the name of the USER TYPE. The name must start with
   the symbol `@`, for example, `@cat`. A USER TYPE name must be distinct from all other names used
   by USER TYPES within the PROJECT. Other name requirements are specified in the subsection
   [“Names”](#names) of the section “Common Syntax Rules”.

2. `SchemaNotation` is an optional PARAMETER that specifies the NOTATION of the SCHEMA, which is
   used to describe the TYPE data SCHEMA in the BODY of the DIRECTIVE `TYPE`. The default value is
   `jsight`.

##### ANNOTATION

The DIRECTIVE `TYPE` may have an ANNOTATION, which is intended to briefly describe a USER TYPE.

##### BODY

Conceptually, the BODY of the DIRECTIVE `TYPE` may contain two CHILD DIRECTIVES: `Description` and
`Schema`. In this case, the DIRECTIVE `Schema` is the CHILD DIRECTIVE by default. However, these
two CHILD DIRECTIVES are not supported inside the DIRECTIVE `TYPE` in this language version,
therefore the DIRECTIVE `TYPE` may currently contain the BODY of a hypothetical default CHILD
DIRECTIVE `Schema`, which will only be implemented in future language versions.

In other words, in the current language version, the BODY of the DIRECTIVE `TYPE` must contain the
SCHEMA of the USER TYPE data in the NOTATION, which is set explicitly or by default by the PARAMETER
`SchemaNotation`.

Let's consider some examples.

**Example 1.**

```jsight
TYPE @cat
  {
    "id"  : 12,
    "name": "Tom"
  }
```

In the given example, the TYPE `@cat` is declared and its SCHEMA is set in the default NOTATION
`jsight` (in the [JSight Schema language](jsight-schema-0-3)).

**Example 2.**

```jsight
TYPE @catsName regex
  /[A-Z][a-z]*/
```

In the given example, the TYPE `@catsName` is declared and its SCHEMA is set in the NOTATION
`regex`.

##### References to USER TYPES by name

From different parts of the PROJECT, you can refer to USER TYPES by name. References by name can be
placed:

- In PARAMETERS of DIRECTIVES (for example, in PARAMETER `Type` of the DIRECTIVE `Body`).
- In SCHEMAS (for example, in SCHEMAS of the NOTATION `jsight`, see the specification for [JSight
  Schema language](jsight-schema-0-3)).

### DIRECTIVE **"URL"**

> Requirement: req.japi.url 0.4.  
> Status: APPROVED 10/12/2021.

The DIRECTIVE `URL` declares new RESOURCE of API.

| DIRECTIVE "URL"             |                                                            |
|-----------------------------|:----------------------------------------------------------:|
| KEYWORD                     |                                   `URL`                    |
| PARENT DIRECTIVE            |                               ROOT CONTEXT                 |
| PARAMETERS                  |                                 `Path` *                   |
| ANNOTATION                  |                                Not allowed                 |
| BODY                        |                             CHILD DIRECTIVES               |
| CHILD DIRECTIVES (possible) | <ul align="left"><li>HTTP-METHODS</li><li>`Path`</li></ul> |
| Default CHILD DIRECTIVE     |                                    No                      |

Any number of DIRECTIVES `URL` is allowed in the ROOT CONTEXT.

When describing the JSON-RPC 2.0 protocol, the set of child DIRECTIVES for the `URL` changes (see
section ["Language extensions for describing the JSON-RPC 2.0
protocol"](#language-extensions-for-describing-the-json-rpc-20-protocol))

##### PARENT DIRECTIVES

The DIRECTIVE `URL` can only be placed in the ROOT CONTEXT.

Technically you can place DIRECTIVE `URL` in DIRECTIVE `MACRO`, but it does not make sense (see
the section [“DIRECTIVE "MACRO"”](#directive-macro)).

##### PARAMETERS

The DIRECTIVE `URL` has only one mandatory PARAMETER `Path`, which must contain RESOURCE PATH. The
`Path` may contain PATH PARAMETERS in curly braces, for example: `/cats/{id}`. For more information
on the PARAMETER of the DIRECTIVE `Path`, see the section “Reference of common PARAMETERS of
DIRECTIVES”, subsection [“PARAMETER "Path"”](#parameter-path).

Example of the directive `URL` with mandatory PARAMETER `Path`:

```jsight
URL /cats
```

##### ANNOTATION

It is not allowed to input an ANNOTATION to the DIRECTIVE `URL`.

##### BODY

A BODY of the DIRECTIVE `URL` is made up of one or more nested DIRECTIVES. There must be at least
one CHILD DIRECTIVE in the BODY.

DIRECTIVE `URL` allows the following CHILD DIRECTIVES:

- DIRECTIVES-HTTP-METHODS (any number) — describe the HTTP-METHODS of this RESOURCE. HTTP-METHODS
  cannot be repeated inside one DIRECTIVE `URL` (for example, one `URL` cannot have two directives
  `GET`).
- DIRECTIVE `Path` (not more than one) — describes the PARAMETERS of the PATH, declared in the
  PARAMETER `Path`.

For DIRECTIVE `URL`, the default CHILD DIRECTIVE is not defined (see the section [“CHILD DIRECTIVES
by default”](#child-directives-by-default)).

Example of using the directive `URL`:

```jsight
URL /cats
  GET // Get the list of cats.
    200 [@cat]
  POST // Create a new cat.
    200 @cat

URL /cats/{id}
  Path
    {
      "id": 123 // {min: 1} - Cat identifier.
    }
  GET // Get a cat by its id. 
    200 @cat
  PUT // Change a cat by its id. 
    200 @cat
```

In the given example:

- The DIRECTIVE `URL /cats` declares that the API has a RESOURCE specified in the PARAMETER of the
  DIRECTIVE `Path`, that is, the RESOURCE `/cats`.
- The RESOURCE `/cats` accepts two HTTP-methods: `GET` and `POST`.
- The DIRECTIVE `URL /cats/{id}` declares that the API has a RESOURCE `/cats/{id}`.
- The DIRECTIVE `Path` specifies the requirements for the PARAMETER of the PATH `id`.
- The resource `/cats/{id}` accepts two HTTP-methods: `GET` and `PUT`.

While describing a RESOURCE tree using the DIRECTIVE `URL`, keep in mind the prohibition of
duplication of PARAMETERS of RESOURCE PATH (see the section [“PARAMETER "Path"”](#parameter-path)).

### DIRECTIVE **"Version"**

> Requirement: req.japi.info.version 0.3.  
> Status: APPROVED 10/12/2021.

The directive `Version` is intended to indicate the version of the API PROJECT.

|   DIRECTIVE "Version"   |                    |
| ----------------------- | :----------------: |
| KEYWORD                 |     `Version`      |
| PARENT DIRECTIVE        |      `INFO`        |
| PARAMETERS              |    `Version` *     |
| ANNOTATION              |      Not allowed   |
| BODY                    |        No          |
| CHILD DIRECTIVES        |        No          |
| Default CHILD DIRECTIVE |        No          | 

##### PARENT DIRECTIVES

The DIRECTIVE `Version` can only be placed inside the DIRECTIVE `INFO`.

Technically you can place DIRECTIVE `Version` in DIRECTIVE `MACRO`, but it does not make sense (see
the section [“DIRECTIVE "MACRO"”](#directive-macro)).

##### PARAMETERS

The DIRECTIVE `Version` has one mandatory PARAMETER `Version`, in which the version of API PROJECT
must be specified. The version can be of any string.

Example:

```jsight
INFO
  Version 1.0
```

##### ANNOTATION

The DIRECTIVE `Version` CANNOT have ANNOTATION.

##### BODY

The DIRECTIVE `Version` does not have a BODY.

## Reference of common PARAMETERS of DIRECTIVES

### PARAMETER "Path"

> Requirement: req.japi.path_rules 0.3.  
> Status: APPROVED 01/10/2022.

The PARAMETER `Path` is used in the DIRECTIVE `URL` and in DIRECTIVES-HTTP-METHODS (`GET`, `POST`,
etc.).

This PARAMETER is intended to indicate the PATH or part of the PATH to a particular RESOURCE.

The PATH must start with a slash symbol `/`, for example, `/cats/{id}`.

Inside the PARAMETER `Path`, PATH PARAMETERS are specified using curly braces. For example, in the
PATH `/cats/{id}/friends/{friend_id}` two PATH PARAMETERS are declared: `id` and `friend_id`. The
PATH PARAMETER is a changeable part of the specified RESOURCE PATH. In real URL, instead of a PATH
PARAMETER, precise values of PATH PARAMETERS are specified. For example, `/cats/123/friends/14`.
Requirements for a value of PATH PARAMETERS are set using the DIRECTIVE `Path` (see the section
[“DIRECTIVE "Path"”](#directive-path)). If requirements for the PATH PARAMETER are not specified
using the DIRECTIVE `Path`, then the value of the PATH PARAMETER can be any string.

The PATH PARAMETER is uniquely identified not only by its name, but also by the part of the PATH
that is located to its left. For example, the PARAMETER of the PATH `id` in THE PATH `/cats/{id}` is
identical to the PARAMETER `id` in THE PATH `/cats/{id}/friends` and is not identical to the
PARAMETER of the PATH `id` in THE PATH `/dogs/{id}`.

A PATH can theoretically start directly with a PATH PARAMETER. For example:

```jsight
GET /{id}/cats // Get a cat by its id
  200 @cat
```

Let's formulate several important rules related to the parameter `Path`.

**Rule 1. PATHS with the same hierarchy but different PARAMETER names must not exist as they are
identical.**

For example, the following paths are considered identical and invalid:

- /cats/{id}
- /cats/{catId}

An example of a violation of this rule:

```jsight
URL /cats/{id}
  GET
    200 @cat

GET /cats/{name}   # --ERROR!!! This PATH contradicts the PATH `/cats/{id}`,
  200 @cat         #            which has already been set earlier.
```

**Rule 2. Within the same PROJECT, one and the same PATH cannot be used twice in the same
DIRECTIVE-HTTP-METHOD.**

Let's consider an example of a violation of this rule:

```jsight
URL /cats/{id}
  GET
    200 @cat

GET /cats/{id}   # --ERROR!!! This HTTP-method with this PATH has already been declared earlier.
  200 @cat

POST /cats/{id}  # No error — this HTTP-method is introduced in this PATH for the first time. 
  Request @cat
  200 any
```

**Rule 3. Within the same PROJECT, one and the same PATH cannot be used twice
in the DIRECTIVE `URL`.**

Let's consider an example of a violation of this rule:

```jsight
URL /cats/{id}
  GET
    200 @cat
  
URL /cats/{id}  # --ERROR!!! This PATH has already been set for the DIRECTIVE `URL` earlier.
  POST
    Request
      @cat
    200 any
```

**Rule 4. Names of PATH PARAMETERS cannot be repeated in the PATH.**

Let's consider an example with an error:

```jsight
URL /cats/{id}/friends/{id} # --ERROR!!! The PARAMETER `id` is duplicated in THE PATH.
```

In the given example, the last string contains an error, since the PARAMETER of the PATH `id` is
duplicated in the PATH of the DIRECTIVE `GET /cats/{id}/friends/{id}`. In this case, the second
parameter should be named, for example, `friendId` or `friend:id`, or something else.

**Rule 5. If requirements for the PATH PARAMETER are set using the DIRECTIVE `Path`, then those
requirements apply to all cases when this PATH PARAMETER occurs in other PATHS.**

Example:

```jsight
URL /cats/{id}/friends
  Path
    {
      "id": 12 // {min: 0}
    }
  
  GET
    200 @cat

GET /cats/{id}/enemies
  200 @cat
```

In the given example, the DIRECTIVE `Path` describes the requirements for the PARAMETER `id` in the
PATH `/cats/{id}/friends`. This means that these requirements apply to all PARAMETERS of the PATH
`id` in all PATHS that start with the string `/cats/{id}`. For example, in the given code, these
requirements will also be applied to the PARAMETER `id` in the path `/cats/{id}/enemies`.

If the requirements for the PATH PARAMETER are described after it has been used in the PATH of some
DIRECTIVE, then these requirements continue to apply to this PATH PARAMETER. To demonstrate this,
let's modify the previous example as follows:

```jsight
GET /cats/{id}/friends      # Requirements for `id` are valid, although they are described below. 
  200 @cat

URL /cats/{id}/enemies
  Path
    {
      "id": 12 // {min: 0}
    }

  GET
    200 @cat
```

:::caution Important!

Once again, we emphasize that PARAMETER requirements do not apply to PARAMETERS with
the same name, but with a different PATH to the left of the PARAMETER.

:::

For example:

```jsight
GET /cats/{id}
  Path
    {
      "id": 12
    }

  200 @cat

GET /dogs/{id}   # The requirements for the PARAMETER of the PATH `id`
  200 @dog       # described above do not apply here!
```

In the given example, the PARAMETER `id` in THE PATH `/cats/{id}` and the PARAMETER `id` in THE PATH
`/dogs/{id}` *are different PARAMETERS*! These PARAMETERS have the same name, but are located in
different PATHS, therefore they are completely independent of each other and the requirements for
one PARAMETER `id` along the PATH `/cats/{id}` do not apply to another PARAMETER `id` along the PATH
`/dogs/{id}`.

**Rule 6. For one PATH PARAMETER to which the same sub-PATH leads, you can specify the requirements
using the DIRECTIVE `Path` only once.**

Let's consider an example of a violation of this rule:

```jsight
URL /cats/{id}
  Path
    {
      "id": 12
    }
  GET
    200 @cat

GET /cats/{id}/friends
  Path
    {
      "id": 12 # --ERROR!!!  The requirements to the PARAMETER of the PATH `id` along
    }          #             the PATH `/cats/{id}` have already been specified earlier. 
    
  200 @cat
```

:::caution Important!

Once again, we emphasize that if the PARAMETERS of a PATH with the same name have different PATHS to
their left, then these PATH PARAMETERS are different and do not depend on each other. You can apply
the DIRECTIVE `Path` to each of them.

:::

For example, the following SCHEMA will be correct:

```jsight
GET /cats/{id}
  Path
    {
      "id": "CAT-123" // {regex: "CAT-\\d+"}
    }

  200 @cat

GET /dogs/{id}
  Path
    {
      "id": "DOG-123" // {regex: "DOG-\\d+"}  # --IT IS CORRECT! 
    }

  200 @dog
```

### PARAMETER "SchemaNotation"

> Requirement: req.japi.schema_notation 0.1.  
> Status: APPROVED 10/12/2021.  
> It is planned:
>
> - To add the NOTATION multipart and the DIRECTIVE Part.

Several DIRECTIVES use the PARAMETER `SchemaNotation`, in which name of SCHEMA NOTATION must be
specified. (For example, DIRECTIVES `Request`, `Body`, `TYPE`, DIRECTIVES-RESPONSES `200`, `301`,
etc.). This language version supports the following SCHEMA NOTATIONS:

- `jsight`,
- `regex`,
- `any`,
- `empty`.

In future language versions, other NOTATIONS will be also supported, for example `multipart`.

Given NOTATIONS are briefly described below.

#### NOTATION "jsight"

> Requirement: req.japi.schema_notation.jsight 0.3.  
> Status: APPROVED 10/12/2021.

The NOTATION `jsight` implies the use of the [JSight Schema language](jsight-schema-0-3).

The NOTATION `jsight` is the base SCHEMA NOTATION in the JSight API language. In all DIRECTIVES that
implies specifying a NOTATION, the PARAMETER `SchemaNotation` is `jsight` by default.

An example of using SCHEMA NOTATION `jsight`:

```jsight
TYPE @cat jsight
{
  "id"  : 1,
  "name": "Tom",
  "email": "tom@catsbook.com" // {type: "email"}
}
```

More information about the NOTATION `jsight` can be found in the official [JSight Schema language
specification](jsight-schema-0-3).

#### NOTATION "regex"

> Requirement: req.japi.schema_notation.regex 0.3.  
> Status: APPROVED 10/12/2021.

The NOTATION `regex` is created using regular expressions.

It is used when the data being described is a simple string.

The SCHEMA in the NOTATION `regex` represents a string of regular expressions. The line should begin
and end with a slash `/`.

An example of using SCHEMA NOTATION `regex`:

```jsight
TYPE @okResponse regex
  /OK/
```

#### Pseudo-NOTATION "any"

> Requirement: req.japi.schema_notation.any 0.3.  
> Status: APPROVED 10/12/2021.

The NOTATION `any` is a “pseudo”-NOTATION in the sense that it has no rules for describing the
SCHEMA. The NOTATION `any` means that data can have absolutely any structure, therefore, the SCHEMA
is not required when specifying this NOTATION.

An example of using the NOTATION `any`:

```jsight
GET /cats
  200 @cat
  400 any
```

This example indicates that if the RESOURCE `/cat` returns the 400 error, it can be data with any
structure in the RESPONSE BODY.

#### Pseudo-NOTATION "empty"

> Requirement: req.japi.schema_notation.empty 0.3.  
> Status: APPROVED 10/12/2021.

The NOTATION `empty` is a “pseudo”-NOTATION in the sense that it has no rules for describing the
SCHEMA. The NOTATION `empty` means that data must be completely empty, therefore, the SCHEMA is not
required when specifying this NOTATION.

An example of using the NOTATION `empty`:

```jsight
GET /cats
  200 @cat
  400 empty
```

This example indicates that if the RESOURCE `/cat` returns the 400 error, the RESPONSE BODY should
be strictly empty.

## Language extensions for describing the JSON-RPC 2.0 protocol

The previous sections demonstrated how to describe HTTP REST JSON based APIs using the JSight
language. This section will show additional features for describing/documenting an API built using
the JSON-RPC 2.0 protocol.

To describe the JSON-RPC 2.0 protocol, you must use a DIRECTIVE `URL` with a child DIRECTIVE
`Protocol` that has the `json-rpc-2.0` parameter specified. In this case, the DIRECTIVE URL changes
the set of allowed child directives (see the next section).

### DIRECTIVE **"URL"** (for JSON-RPC protocol)

> Requirement: req.japi.url.json-rpc 0.1.  
> Status: APPROVED 05/04/2022.

The DIRECTIVE `URL` along with the child DIRECTIVE `Protocol json-rpc-2.0` declares a new JSON-RPC
endpoint.

| DIRECTIVE "URL"             |                                                            |
|-----------------------------|:----------------------------------------------------------:|
| KEYWORD                     |                                   `URL`                    |
| PARENT DIRECTIVE            |                               ROOT CONTEXT                 |
| PARAMETERS                  |                                 `Path` *                   |
| ANNOTATION                  |                                Not allowed                 |
| BODY                        |                             CHILD DIRECTIVES               |
| CHILD DIRECTIVES (possible) | <ul align="left"><li>`Protocol`</li><li>`Method`</li></ul> |
| Default CHILD DIRECTIVE     |                                    No                      |

Any number of DIRECTIVES `URL` is allowed in the ROOT CONTEXT.

##### PARENT DIRECTIVES

The DIRECTIVE `URL` can only be placed in the ROOT CONTEXT.

Technically you can place DIRECTIVE `URL` in DIRECTIVE `MACRO`, but it does not make sense (see
the section [“DIRECTIVE "MACRO"”](#directive-macro)).

##### PARAMETERS

The DIRECTIVE `URL` has only one mandatory PARAMETER `Path`, which must contain path to the JSON-RPC
endpoint. The `Path` may contain PATH PARAMETERS in curly braces, for example: `/cats/{id}`. For
more information on the PARAMETERS of the DIRECTIVE `Path`, see the section “Reference of common
PARAMETERS of DIRECTIVES”, subsection [“PARAMETER "Path"”](#parameter-path).

Example of the directive `URL` with mandatory PARAMETER `Path`:

```jsight
URL /cats
```

##### ANNOTATION

It is not allowed to input an ANNOTATION to the DIRECTIVE `URL`.

##### BODY

A BODY of the DIRECTIVE `URL` is made up of one or more nested DIRECTIVES. There must be at least
one CHILD DIRECTIVE in the BODY.

DIRECTIVE `URL` allows the following CHILD DIRECTIVES:

- DIRECTIVE `Protocol` (not more than one) — defines the protocol that is used in the current BODY
  of the DIRECTIVE `URL`.
- DIRECTIVE `Method` (one or more) — defines JSON-RPC method.

For DIRECTIVE `URL`, the default CHILD DIRECTIVE is not defined (see the section [“CHILD DIRECTIVES
by default”](#child-directives-by-default)).

Example of using the DIRECTIVE `URL` to describe an API created using the JSON-RPC 2.0 protocol:

```jsight
JSIGHT 0.3

URL /api/rpc
  Protocol json-rpc-2.0

  Method createCat // Create a cat.
    Description
        The method creates a cat.
    Params
      {
        "cat": @cat
      }
    Result
      {
        "id": 1 // Cat’s id.
      }

  Method getCat // Get a cat by its id.
    Params
      {
        "id": 1 // Cat’s id.
      }
    Result
      @cat

TYPE @cat
{
  "id": 1,
  "name": "Tom"
}
```

In the given example:

- The DIRECTIVE `URL /api/rpc` declares that the API has an endpoint specified in the PARAMETER 
  of the DIRECTIVE.
- The RESOURCE `/api/rpc` accepts two JSON-RPC methods: `createCat` and `getCat`.
- The methods `createCat` and `getCat` have their parameters described, as well as return result.

:::note

*Note that the JSight language does not describe the member `id` for the JSON RPC 2.0 request
object (see [JSON-RPC 2.0 Specification](https://www.jsonrpc.org/specification#request_object)).
According to the JSON RPC 2.0 specification, the member can input any `id` containing a String,
Number, or NULL if he expects to get a response.*

:::

### DIRECTIVE **"Protocol"**

> Requirement: req.japi.protocol 0.1.  
> Status: APPROVED 05/04/2022.

The DIRECTIVE `Protocol` is located in the CONTEXT of the DIRECTIVE `URL`. The DIRECTIVE specifies
the protocol that is described in the current BODY of the DIRECTIVE URL.

:::note

<i>Currently only one protocol is supported: JSON-RPC 2.0. Other protocols will be supported in the
future (for example: gRPC, Kafka, RabbitMQ, WebSocket, etc.).</i>

:::

|   DIRECTIVE "Protocol"  |                  |
| ----------------------- |:----------------:|
| KEYWORD                 |    `Protocol`    |
| PARENT DIRECTIVE        |      `URL`       |
| PARAMETERS              | `ProtocolName` * |
| ANNOTATION              |   Not allowed    |
| BODY                    |        No        |
| CHILD DIRECTIVES        |        No        |
| Default CHILD DIRECTIVE |        No        |

##### PARENT DIRECTIVES

The DIRECTIVE `Protocol` can only be placed inside the DIRECTIVE `URL`.

Also, the DIRECTIVE `Protocol` can be placed in the macro defined by the DIRECTIVE `MACRO` (see the
section [“DIRECTIVE "MACRO"”](#directive-macro)).

##### PARAMETERS

The DIRECTIVE `Protocol` has one mandatory parameter `ProtocolName`, which contains the protocol
name. There is currently only one possible value for this parameter: `json-rpc-2.0`.

##### ANNOTATION

The DIRECTIVE `Protocol` CANNOT have ANNOTATION.

##### BODY

The DIRECTIVE `Protocol` does not have a BODY.

Example of using the DIRECTIVE `Protocol`:

```jsight
JSIGHT 0.3

URL /api/rpc
  Protocol json-rpc-2.0
```

### DIRECTIVE **"Method"**

> Requirement: req.japi.method 0.1.  
> Status: APPROVED 05/04/2022.

The DIRECTIVE `Method` defines RPC method.

| DIRECTIVE "Method"      |                                                                                |
|-------------------------|:------------------------------------------------------------------------------:|
| KEYWORD                 |                                           `Method`                             |
| PARENT DIRECTIVE        |                                             `URL`                              |
| PARAMETERS              |                                        `MethodName` *                          |
| ANNOTATION              |                                            Allowed                             |
| BODY                    |                                       CHILD DIRECTIVES                         |
| CHILD DIRECTIVES        | <ul align="left"><li>`Description`</li><li>`Params`</li><li>`Result`</li></ul> |
| Default CHILD DIRECTIVE |                                              No                                |

##### PARENT DIRECTIVES

The DIRECTIVE `Method` can only be placed inside the DIRECTIVE `URL`.

Also, the DIRECTIVE `Method` can be placed in the macro defined by the DIRECTIVE `MACRO` (see the
section [“DIRECTIVE "MACRO"”](#directive-macro)).

##### PARAMETERS

The DIRECTIVE `Method` has one mandatory parameter `MethodName`,  which contains a string containing
the name of the method to be invoked.

Example:

```jsight
URL /api/rpc
  Protocol json-rpc-2.0
  Method createCat // Create a cat.
```

##### ANNOTATION

The DIRECTIVE `Method` may have an ANNOTATION that briefly describes this method.

##### BODY

The BODY of the DIRECTIVE `Method` may contain the following CHILD DIRECTIVES:

- DIRECTIVE `Description` (not more than one) — gives a detailed description of the method.
- DIRECTIVE `Params` (not more than one) — describes the parameters that can be used during a method
  call.
- DIRECTIVE `Result` (not more than one) —  describes the SCHEME of the data that should be returned
  in case of a successful method call.

If the child DIRECTIVE `Result` is not specified, this means that we are describing a Notification
request (we are not interested in the server’s response).

Example:

```jsight
JSIGHT 0.3

URL /api/rpc
  Protocol json-rpc-2.0

  Method createCat // Create a cat.
    Description
        The method creates a cat.
    Params
      {
        "cat": @cat
      }
    Result
      {
        "id": 1 // Cat’s id.
      }

  Method removeCat // Remove a cat.
    Description
        The method removes a cat.
    Params
      {
        "id": 1 // Cat’s id.
      }
```

### DIRECTIVE **"Params"**

> Requirement: req.japi.params 0.1.  
> Status: APPROVED 05/04/2022.

The DIRECTIVE `Params` describes a set of input parameters for a method defined by the parent
DIRECTIVE `Method`.

|   DIRECTIVE "Params"    |                                 |
| ----------------------- |:-------------------------------:|
| KEYWORD                 |            `Params`             |
| PARENT DIRECTIVE        |            `Method`             |
| PARAMETERS              |               No                |
| ANNOTATION              |           Not allowed           |
| BODY                    | SCHEMA of the NOTATION `jsight` |
| CHILD DIRECTIVES        |               No                |
| Default CHILD DIRECTIVE |               No                |

##### PARENT DIRECTIVES

The DIRECTIVE `Params` can only be placed inside the DIRECTIVE `Method`.

Also, the DIRECTIVE `Params` can be placed in the macro defined by the DIRECTIVE `MACRO` (see the
section [“DIRECTIVE "MACRO"”](#directive-macro)).

##### PARAMETERS

The DIRECTIVE `Params` has no PARAMETERS.

##### ANNOTATION

The DIRECTIVE `Params` CANNOT have ANNOTATION.

##### BODY

The BODY of the DIRECTIVE `Params` must contain the SCHEMA of the NOTATION `jsight` (in the 
[JSight Schema language](jsight-schema-0-3)).

Example:

```jsight
JSIGHT 0.3

URL /api/rpc
  Protocol json-rpc-2.0

  Method createCat // Create a cat.
    Description
        The method creates a cat.
    Params
      {
        "cat": @cat
      }
    Result
      {
        "id": 1 // Cat’s id.
      }

  Method getCatsByIds // Get a list of cats by their IDs.
    Params
      [1,2,3]
    Result
      [@cat]

TYPE @cat
{
  "id": 1,
  "name": "Tom"
}
```

### DIRECTIVE **"Result"**

> Requirement: req.japi.result 0.1.  
> Status: APPROVED 05/04/2022.

The DIRECTIVE `Result` describes the SCHEMA of the data that should be returned in the event of a
successful method call.

|   DIRECTIVE "Result"    |                                 |
| ----------------------- |:-------------------------------:|
| KEYWORD                 |            `Result`             |
| PARENT DIRECTIVE        |            `Method`             |
| PARAMETERS              |               No                |
| ANNOTATION              |           Not allowed           |
| BODY                    | SCHEMA of the NOTATION `jsight` |
| CHILD DIRECTIVES        |               No                |
| Default CHILD DIRECTIVE |               No                |

##### PARENT DIRECTIVES

The DIRECTIVE `Result` can only be placed inside the DIRECTIVE `Method`.

Also, the DIRECTIVE `Result` can be placed in the macro defined by the DIRECTIVE `MACRO` (see the
section [“DIRECTIVE "MACRO"”](#directive-macro)).

##### PARAMETERS

The DIRECTIVE `Result` has no PARAMETERS.

##### ANNOTATION

The DIRECTIVE `Result` CANNOT have ANNOTATION.

##### BODY

The BODY of the DIRECTIVE `Result` must contain the SCHEMA of the NOTATION `jsight` (in the [JSight
Schema language](jsight-schema-0-3)). The SCHEMA describes the value that the server should return
if the JSON-RPC 2.0 server method is successfully called.

Example:

```jsight
JSIGHT 0.3

URL /api/rpc
  Protocol json-rpc-2.0

  Method createCat // Create a cat.
    Description
        The method creates a cat.
    Params
      {
        "cat": @cat
      }
    Result
      {
        "id": 1 // Cat’s id.
      }

  Method getCat // Get a cat by its id.
    Params
      {
        "id": 1 // Cat’s id.
      }
    Result
      @cat

  Method getCatName // Get a cat's name by its id.
    Params
      {
        "id": 1 // Cat’s id.
      }
    Result
      "Tom"

  Method removeCat // Remove a cat.
    Description
        The method removes a cat.
    Params
      {
        "id": 1 // Cat’s id.
      }

TYPE @cat
{
  "id": 1,
  "name": "Tom"
}
```

## Edit history

|    Date    | JSight API Version | Document version | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |       Author        |
|:----------:|:------------------:|:----------------:|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------:|
| 07/12/2022 |       0.3.7        |      0.1.20      | <ul><li>Added “Language extensions for describing the JSON-RPC 2.0 protocol”.</li><li>Added DIRECTIVE `INCLUDE`.</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |    Andrei Ivanov    |
| 02/11/2022 |       0.3.6        |      0.1.19      | <ul><li>The DIRECTIVE `DIR` has been replaced by DIRECTIVES `MACRO` and `PASTE`.</li><li>Minor improvements to COMMENTS.</li><li>Changed dependency to JSight Schema 0.3.5.</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |    Andrei Ivanov    |
| 01/13/2022 |       0.3.5        |      0.1.18      | Changed dependency on JSight Schema 0.3.4.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | Konstantin Malyshev |
| 01/12/2022 |       0.3.4        |      0.1.17      | Minor change in principle 12.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | Konstantin Malyshev |
| 01/11/2022 |       0.3.4        |      0.1.16      | An example was added in section “PARAMETER "Path"”.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Konstantin Malyshev |
| 01/10/2022 |       0.3.4        |      0.1.15      | <ul><li>The requirement req.japi.http_method.query 0.3 was changed, the new version is 0.4. DIRECTIVE `Query` now must have a DIRECTIVE `Body`.</li><li>The requirement req.japi.jsight 0.3 was changed, the new version is 0.4. DIRECTIVE `JSIGHT` now cannot be omitted.</li><li>The requirement req.japi.server.base_url 0.3 was changed, the new version is 0.4. DIRECTIVE `BaseUrl` now does not have a body.</li><li>The requirement req.japi.path_rules 0.3 was changed, the new version is 0.4. Paths `/cats/{id}` and `/cats/{catId}` now are considered identical and invalid.</li><li>Minor edits according to docusaurus format.</li></ul>                                                                                                                                                                                                                                            | Konstantin Malyshev |
| 01/06/2022 |       0.3.3        |      0.1.14      | Changed dependency on JSight Schema 0.3.3. Changes in formatting for Docusaurus. Small fixes.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | Konstantin Malyshev |
| 12/29/2021 |       0.3.2        |      0.1.13      | Minor edits.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | Konstantin Malyshev |
| 12/22/2021 |       0.3.2        |      0.1.12      | Changed dependency to JSight Schema 0.3.2. Clarifications of some wordings without changing the meaning.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | Konstantin Malyshev |
| 12/17/2021 |       0.3.2        |      0.1.11      | Minor refinements about Markdown and comments.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Konstantin Malyshev |
| 12/14/2021 |       0.3.2        |      0.1.10      | Minor clarifications of wordings within the same content.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | Konstantin Malyshev |
| 12/06/2021 |       0.3.2        |      0.1.9       | Minor edits beside the point.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | Konstantin Malyshev |
| 12/03/2021 |       0.3.2        |      0.1.8       | <ul><li>The principle of language design 12 (OpenAPI compliance) was added.</li><li>The concept of “relative PATH” has been abolished. All paths are now absolute. </li><li>The requirement req.japi.context.default_child was changed, the new version is 0.4. Now it is possible to omit the default DIRECTIVE only if it is the only one. </li><li>The requirement req.japi.url was changed,  the new version is 0.4. The change is due to the cancellation of relative paths.</li><li>The requirement req.japi.http_method was changed, the new version is 0.4. The change is due to the cancellation of relative paths.</li><li>The status of the requirement req.japi.enum 0.3 was changed to POSTPONED.</li><li>The requirement req.japi.path_rules was changed, the new version is 0.2. The change is due to the cancellation of relative paths.</li><li>Minor blots were fixed</li></ul> | Konstantin Malyshev |
| 12/03/2021 |       0.3.1        |      0.1.7       | Clarifications of the rules for parentheses (explicit body boundaries).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | Konstantin Malyshev |
| 11/19/2021 |       0.3.1        |      0.1.6       | Adding TODO comments                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | Konstantin Malyshev |
| 11/12/2021 |       0.3.1        |      0.1.5       | Typo is fixed.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Konstantin Malyshev |
| 11/09/2021 |       0.3.1        |      0.1.4       | Minor inconsistencies in the use of quotes in directive parameters have been fixed.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Konstantin Malyshev |
| 11/02/2021 |       0.3.1        |      0.1.3       | Changed dependency to JSight Schema 0.3.1 (was 0.3.0).<br /> Minor edits beside the point.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | Konstantin Malyshev |
| 10/19/2021 |       0.3.0        |      0.1.2       | Typos are fixed, small clarifications are done.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Konstantin Malyshev |
| 10/14/2021 |       0.3.0        |      0.1.1       | Typos are fixed.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Konstantin Malyshev |
| 10/13/2021 |       0.3.0        |      0.1.0       | Many changes have been made based on the testing results.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | Konstantin Malyshev |
| 12/13/2019 |        0.1         |        -         | The full language version that was approved for the development of a validator prototype.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |   Maxim Reznitsky   |
