# Map of the DIRECTIVES hierarchy

Version: JSight API 0.3.6.

- JSIGHT (Version) 1–1
- INFO 0-1
  - Title (Title) 0-1
  - Version (Version) 0-1
  - Description 0-1
- SERVER (Name *) // annotation is allowed
  - BaseUrl (Path *) 1–1
- URL (Path *) 0-\*
  - Path 0-1
  - HTTP-METHODS 0-* // annotation is allowed
    - ...
- HTTP-METHODS (Path) 0-* // annotation is allowed
  - Description 0-1
  - Path 0-1
  - Query (QueryExample, Format) 0-1
  - Authorization \[Coming soon\]
  - Cookies \[Coming soon\]
  - Request (Type, SchemaNotation) 0-1
    - Headers 0-1
    - Body (Type, SchemaNotation) 1-1 Def
  - DIRECTIVES-RESPONSES (Type, SchemaNotation) 0-* // annotation is allowed
    - Headers 0-1
    - Body (Type, SchemaNotation) 1-1 Def
- TYPE (Name *, SchemaNotation) // annotation is allowed
  - Description \[Coming soon\]
  - Schema \[Coming soon\]
- RULE (Name, RuleType) \[Coming soon\]
  - Description \[Coming soon\]
  - RuleValue \[Coming soon\]
- ENUM (Name *) \[Coming soon\]
  - Description \[Coming soon\]
  - EnumValue \[Coming soon\]
- MACRO (Name \*) 0-\*
  - Any directives, except MACRO
- INCLUDE (File) \[Coming soon\]