---
title: "Green Core Model Design Work"
abbrev: "Core Models DT"
category: std

docname: draft-gdt-green-design-work-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: "Operations and Management"
workgroup: "Getting Ready for Energy-Efficient Networking"
keyword:
 - green working group design work
venue:
  group: "Getting Ready for Energy-Efficient Networking"
  type: "Working Group"
  mail: "green@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/green/"
  github: "ietf-wg-green/green-design-work"
  latest: "https://ietf-wg-green.github.io/green-design-work/draft-gdt-green-design-work.html"

author:
 -
    fullname: Robert Wilton
    organization: Cisco
    email: rwilton@cisco.com

normative:

informative:

...

--- abstract

TODO Abstract


--- middle

# Introduction

TODO Introduction


# Conventions and Definitions

{::boilerplate bcp14-tagged}


# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.


# Acknowledgments
{:numbered="false"}

TODO acknowledge.


--- back


# Examples

## Example including YANG tree
~~~~ yangtree
{::include generated-tree-output/ietf-gdt-model-tree.txt}
~~~~
{: align="left" title="ietf-gdt-model.yang tree diagram"}

## Example including a YANG file
~~~~ yang
{::include yang/ietf-gdt-model.yang}
~~~~
{: align="left" sourcecode-markers="true"
sourcecode-name="ietf-gdt-model.yang#0.1.0" title="YANG module ietf-gdt-model"}

## Example including a JSON example
~~~~ json
{::include examples/full-notification.json.txt}
~~~~
{: align="left" title="Example of update notification including notification envelope"}
