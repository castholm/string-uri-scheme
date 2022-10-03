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

This document defines string URIs and the "string" URI scheme. A string URI identifies a string contained within the
URI itself and can be used as a method of specifying an arbitrary string in a context where a URI is required.

In addition, this document also defines algorithms for obtaining the string contained within a string URI and for
determining whether two string URIs are equivalent.

--- middle

# Introduction

TODO Introduction

# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Syntax

Uses the syntax and the "ALPHA" and "DIGIT" rules defined by {{!RFC2234}}.

Uses the "pct-encoded" rule defined by {{!RFC3986}}.

```abnf

string-uri = "string:" string-data *( ALPHA / DIGIT / "-" / "_" / pct-encoded )

```

Additional conformance rules that can not be effectively described in ABNF form are also employed.

# Security Considerations

TODO Security Considerations

# IANA Considerations

TODO IANA Considerations

--- back

# Acknowledgments
{:numbered="false"}

TODO Acknowledgments
