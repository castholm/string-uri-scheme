---
title: The "string" URI Scheme
category: exp

docname: draft-castholm-string-uri-scheme-latest
submissiontype: independent
number:
date:
v: 3
# area: AREA
# workgroup: WG Working Group
keyword:
  - string
  - uri
  - scheme
venue:
#  group: WG
#  type: Working Group
#  mail: WG@example.com
#  arch: https://example.com/WG
  github: castholm/string-uri-scheme
  latest: 'https://castholm.github.io/string-uri-scheme/draft-castholm-string-uri-scheme.html'

author:
  - fullname: Carl Åstholm
    email: carl@astholm.se

normative:

informative:

--- abstract

This document defines string URIs and the "string" URI scheme. A string URI identifies an application-dependent value
(most commonly, but not necessarily, a string of characters) and can be used as a method of specifying such a value in
a context where a URI is required. String URIs have a strict syntax and well-defined rules for processing and
comparing that make it simple for applications to consume them.

--- middle

# Introduction

A string URI is a URI that identifies an application-dependent value, such as a string of characters. It uses the
"string" URI scheme, which is designed to meet the following requirements:

- Identifiers have a strict yet simple and familiar syntax that is easy for humans to remember.
- Identifiers are convenient for developers and users of applications to define and use.
- An identifier that identifies a string consisting of only alphanumeric ASCII characters may safely include that
  string as a component of the identifier itself without requiring any preprocessing.
- An identifier has exactly one valid representation, making testing two identifiers for equality very simple.

# Terminology

{::boilerplate bcp14-tagged}

# Syntax {#syntax}

The string URI syntax is a subset of the generic URI syntax defined in {{!rfc3986}}.

Using ABNF and the "DIGIT" and "ALPHA" rules defined in {{!RFC2234}}, the string URI syntax is defined by the
"string-URI" rule defined below:

~~~ abnf
string-URI = %x73.74.72.69.6E.67 ":" data
data       = *( DIGIT / ALPHA / "-" / "_" / pct )

pct        = "%" ( pct-1 / pct-2 / pct-3 / pct-4 )

pct-1      = %x30-31 hex
           / "2" ( DIGIT / %x41-43 / %x45-46 )
           / "3" %x41-46
           / ( "4" / "6" ) "0"
           / "5" %x42-45
           / "7" %x42-46

pct-2      = pct-2-1 pct-cont
pct-2-1    = %x43 ( %x32-39 / %x41-46 )
           / %x44 hex

pct-3      = pct-3-1-2 pct-cont
pct-3-1-2  = %x45 ( "0%" %x41-42 hex
                  / ( %x31-39 / %x41-43 / %x45-46 ) pct-cont
                  / %x44 "%" %x38-39 hex
                  )

pct-4      = pct-4-1-2 pct-cont pct-cont
pct-4-1-2  = %x46 ( "0%" ( "9" / %x41-42 ) hex
                  / %x31-33 pct-cont
                  / "4%8" hex
                  )

pct-cont   = "%" ( %x38-39 / %x41-42 ) hex

hex        = %x30-39 / %x41-46
~~~

The string URI syntax can also be described by the following POSIX regular expression (whitespace included for
readability but should be ignored):

~~~
^string\:(([0-9A-Za-z\-_]|%(
 [01][0-9A-F]
 |2[0-9ABCEF]
 |3[A-F]
 |[46]0
 |5[B-E]
 |7[B-F]
 |(
  (C[2-9A-F]|D[0-9A-F])
  |E(
   0%[AB][0-9A-F]
   |[1-9ABCEF]%[89AB][0-9A-F]
   |D%[89][0-9A-F]
  )
  |F(
   0%[9AB][0-9A-F]
   |[123]%[89AB][0-9A-F]
   |4%8[0-9A-F]
  )%[89AB][0-9A-F]
 )%[89AB][0-9A-F]
))*)$
~~~

In slightly less technical terms, the string URI syntax can be described as follows (non-normative):

- The URI must begin with a case-sensitive match for "string" in all lowercase immediately followed by a colon (":").
- Everything following the colon must consist solely of any combination of the digits 0 to 9, the letters A to Z in
  both uppercase and lowercase, the hyphen-minus ("-"), the low line ("_") or percent-encoded octets (see
  {{Section 2.1 of ?RFC3986}}).
- Percent-encoded octets must use uppercase hexadecimal digits.
- Percent-encoded octets must decode to valid UTF-8.
- Percent-encoded octets must not decode to 0-9, A-Z (case-insensitive), "-" or "_".

# Usage

## Defining and Using String URIs

TODO Defining and Using String URIs

## Comparing String URIs

TODO Comparing String URIs

## Locating Resources Identified by String URIs

TODO Locating Resources Identified by String URIs

# Conformance

## Conformance Requirements

A string URI MUST conform to the syntax described in {{#syntax}}.

## Handling Nonconforming String URIs

TODO Handling Nonconforming String URIs

# Examples

TODO Examples

# Security Considerations

TODO Security Considerations

# IANA Considerations

## Registration of the "string" URI Scheme

{:vspace}
Scheme name:
: string

Status:
: Provisional

Applications/protocols that use this scheme name:
: Any application or protocol may use this scheme for any purpose, provided that its use conforms to the rules defined
  in this document.

Contact:
: N/A

Change controller:
: N/A

References:
: This document

--- back

# Acknowledgments
{:numbered="false"}

TODO Acknowledgments
