---
title: The "string" URI Scheme
category: exp

docname: draft-castholm-string-uri-scheme-latest
submissiontype: independent
number:
date:
v: 3
keyword:
  - string
  - uri
  - scheme
venue:
  github: castholm/string-uri-scheme
  latest: 'https://castholm.github.io/string-uri-scheme/draft-castholm-string-uri-scheme.html'

author:
  - fullname: Carl Åstholm
    email: carl@astholm.se

normative:
  UNICODE:
    -: UNICODE
    title: 'The Unicode Consortium. The Unicode Standard, Version 15.0.0, (Mountain View, CA: The Unicode Consortium, 2022. ISBN 978-1-936213-32-0)'
    target: 'https://www.unicode.org/versions/Unicode15.0.0/'

informative:

--- abstract

This document defines the "string" URI scheme, string URIs and rules for applications that use string URIs. A string
URI includes a payload representing a string of characters that can be interpreted in an application-dependent manner
to identify and possibly locate a resource. This enables applications to specify such a string in a context where a
proper URI is required. String URIs have a strict syntax and well-defined rules for processing and comparing that make
them simple for applications to use.

--- middle

# Introduction

Sometimes, application developers find themselves in situations where a particular protocol mandates the use of URIs
(Uniform Resource Identifiers {{!RFC3986}}). Most popular URI schemes are designed for identifiers to have very
specific meanings and to be globally unique. This can be inconvenient when only a simple, locally unique identifier
with an application-dependent meaning is desired.

There exists a need for a simple URI (Uniform Resource Identifier) scheme that satisfies the following requirements:

- Identifiers include a string of characters that can be interpreted in an application-dependent manner to identify
  and possibly locate a resource.
- Identifiers have a strict yet simple and familiar syntax that is easy for humans to remember.
- Identifiers are convenient for application developers and users to use.
- Identifiers have well-defined rules for comparing that are trivial for application developers to implement.

Existing URI schemes satisfy some, but not all, of the above requirements.

- HTTP (Hypertext Transfer Protocol) URIs {{?RFC9110 (Section 4.2)}} are familiar to most users and can include
  arbitrary application-dependent data. However, minting new HTTP URIs require a registered domain name that may
  change owners in the future, making them inconvenient to use as stable identifiers, and the rules for comparing
  identifiers are poorly specified.
- URNs (Uniform Resource Names {{?RFC8141}}) require using pre-existing namespaces, which come with their own rules
  and restrictions, or registering new namespaces, which is often inconvenient. While URNs have well-defined rules for
  comparing, such rules are non-trivial to implement in practice.
- Data URIs {{?RFC2397}} are simple to use, but no normative rules for comparing data URIs have been specified and
  they technically only identify and locate their own included payload.
- Tag URIs {{?RFC4151}} have simple, well-defined rules for comparing, but because they are designed to be unique
  across time and space their structure may feel nedlessly complex for applications that only require local
  uniqueness.

This specification introduces the "string" URI scheme, which is designed to satisfy all of the above requirements.

The following is an example of an identifier using the "string" URI scheme:

~~~
   string:UserNotFoundError
~~~

Such an identifier might, for example, identify an application-specific error type representing an error indicating
that a user was not able to be found. The same identifier might also be used to locate more information about the
error type, using a method defined and specified by the application.

# Terminology

{::boilerplate bcp14-tagged}

For simplicity, this specification uses the term "character(s)" to mean "Unicode scalar value(s)" as defined by
{{definition D76 in Section 3.9 of -UNICODE}}. When this specification requires attention to be carefully directed
toward the specific implications of "Unicode scalar value(s)", that specific term is used in order to reduce
ambiguity.

# Structure and Usage

## Components of a String URI

TODO Components of a String URI

## Using String URIs

TODO Using String URIs

## Obtaining the String URI Payload

TODO Obtaining the String URI Payload

## Comparing String URIs

TODO Comparing String URIs

## Locating Resources Identified by String URIs

TODO Locating Resources Identified by String URIs

# Syntax {#syntax}

The string URI syntax is a subset of the generic URI syntax defined in {{!RFC3986}}.

Using ABNF and including the "DIGIT" and "ALPHA" rules defined in {{!RFC2234}}, the string URI syntax is defined by
the "string-URI" rule in the following ABNF specification:

~~~ abnf
   string-URI  = %x73.74.72.69.6E.67 ":" payload
   payload     = *( unencoded / pct-encoded )
   unencoded   = DIGIT / ALPHA / "-" / "_"
   pct-encoded = "%" ( pct-1 / pct-2 / pct-3 / pct-4 )

   pct-1       = %x30-31 upper-hex
               / "2" ( DIGIT / %x41-43 / %x45-46 )
               / "3" %x41-46
               / ( "4" / "6" ) "0"
               / "5" %x42-45
               / "7" %x42-46

   pct-2       = pct-2-1 pct-cont
   pct-2-1     = %x43 ( %x32-39 / %x41-46 )
               / %x44 upper-hex

   pct-3       = pct-3-1-2 pct-cont
   pct-3-1-2   = %x45 ( "0%" %x41-42 upper-hex
                      / ( %x31-39 / %x41-43 / %x45-46 ) pct-cont
                      / %x44 "%" %x38-39 upper-hex
                      )

   pct-4       = pct-4-1-2 pct-cont pct-cont
   pct-4-1-2   = %x46 ( "0%" ( "9" / %x41-42 ) upper-hex
                      / %x31-33 pct-cont
                      / "4%8" upper-hex
                      )

   pct-cont    = "%" ( %x38-39 / %x41-42 ) upper-hex

   upper-hex   = %x30-39 / %x41-46
~~~

Readers should note that the "pct-encoded" rule is NOT equivalent to the rule of the same name found in
{{Section 2.1 of ?RFC3986}} and instead describes a much more constrained syntax that ensures that percent-encoded
octets use only uppercase hexadecimal digits and do not decode to invalid UTF-8 or characters matched by the
"unencoded" rule.

Observant readers will also note that the "unencoded" rule describes the base64url alphabet found in
{{Section 2.1 of ?RFC4648}}, which may already be familiar.

The string URI syntax is also described by the following POSIX regular expression (where whitespace is included for
readability but should be ignored):

~~~
   ^string:(([0-9A-Za-z\-_]|%(
    [01][0-9A-F]|
    2[0-9ABCEF]|
    3[A-F]|
    [46]0|
    5[B-E]|
    7[B-F]|
    (
     (C[2-9A-F]|D[0-9A-F])|
     E(
      0%[AB][0-9A-F]|
      [1-9ABCEF]%[89AB][0-9A-F]|
      D%[89][0-9A-F]
     )|
     F(
      0%[9AB][0-9A-F]|
      [123]%[89AB][0-9A-F]|
      4%8[0-9A-F]
     )%[89AB][0-9A-F]
    )%[89AB][0-9A-F]
   ))*)$
~~~

# Conformance

## Conformance Requirements

A string URI MUST conform to the "string-URI" syntax rule defined in {{syntax}}.

## Recommendations for Handling Nonconforming String URIs

TODO Recommendations for Handling Nonconforming String URIs

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
  by this specification.

Contact:
: N/A

Change controller:
: N/A

References:
: This specification

--- back

# Acknowledgments
{:numbered="false"}

TODO Acknowledgments
