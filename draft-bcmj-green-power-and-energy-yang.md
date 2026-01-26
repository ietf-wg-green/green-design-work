---
title: Power and Energy YANG Module
abbrev: GREEN-PEM-YANG
docname: draft-bcmj-green-power-and-energy-yang-latest
category: std
submissionType: IETF

# Status

ipr: trust200902
area: OPS
workgroup: GREEN
keyword: ["Internet-Draft", "GREEN", "YANG", "Power", "Energy"]

# Publication dates (auto-generated if not specified)

date:

# Document controls

stand_alone: yes
pi: [toc, sortrefs, symrefs, docmapping]

# Authors

author:

- ins: C. Benoit
  fullname: Benoit Claise
  org: Everything OPS
  email: benoit@everything-ops.net
- ins: C. Gen
  fullname: Gen Chen
  org: Huawei
  email: chengen@huawei.com
- ins: M. Palmero
  fullname: Marisol Palmero
  org: Individual
  email: marisol.ietf@gmail.com
- ins: J. Lindblad
  fullname: Jan Lindblad
  org: All For Eco
  email: jan.lindblad@for.eco

normative:

informative:

--- abstract

This document defines the YANG data model for Power and Energy
monitoring of devices within or connected to communication networks.

{::boilerplate bcp14-tagged}

--- middle

# Introduction

This document defines a YANG data model for Power and Energy
Monitoring and control of devices within or connected to communication
networks, for the use cases document in
{{?I-D.ietf-green-use-cases-00}}.

The data model includes both the monitoring and control of Energy
Objects for networked devices.

This YANG data model is based on the the "GREEN framework"
{{?I-D.belmq-green-framework-06}}, following the "GREEN terminology"
{{!I-D.ietf-green-terminology-00}}.

Power and Energy Monitoring and Control can be applied to devices in
communication networks. All identifiable devices with measurable or
representable Power and Energy characteristics fall within the scope
of this specification. Target devices include (but are not limited to)
routers, switches, Power over Ethernet (PoE) endpoints, smart PDU,
storage and compute servers, etc.

Where applicable, device monitoring extends to the components of the
device as well as software and service running on the device. As a
result, the metrics to be monitored include Device Level Energy
Efficiency (DLEE), Component Level Energy Efficiency (CLEE) and
potential Service Level Energy Efficiency (SLEE) at the
orchestrator-level, etc. For example, a router can contain components
such as Line Processing Unit (LPU), Switch Fabric Unit (SFU), Main
Processing Unit (MPU).

## Terminology

This document makes use of the terms defined in
{{!I-D.ietf-green-terminology-00}}:

    - Power
    - Energy
    - Energy Management
    - Energy Monitoring
    - Energy Control
    - Energy Efficiency/Energy Efficiency Ratio
    - Device Level Energy Efficiency (DLEE)
    - Component Level Energy Efficiency (CLEE)
    - Service Level Energy Efficiency (SLEE)

This document makes use of the terms defined in
{{?I-D.belmq-green-framework-06}}

    - Energy Object

The terms reused from {{!I-D.ietf-green-terminology-00}} and
{{?I-D.belmq-green-framework-06}} are capitalized in this
specification.

This document uses the terms Power and Energy in accordance with
{{!I-D.ietf-green-terminology-00}}. Power refers to the instantaneous
rate at which a device consumes or produces electrical energy
(typically expressed in Watts). Energy, by contrast, represents the
cumulative amount of work performed over time (typically expressed in
Joules or Watt-hours). Both concepts are required within this YANG
module. Power enables real-time monitoring, control, and optimization
of device operation, while Energy provides a time-integrated view
necessary for accounting, reporting, and even for sustainability
analysis. This specification includes both Power and Energy
attributes.

The terminology for describing YANG modules is defined in [RFC7950].
The meanings of the symbols in the YANG tree diagrams are defined in
[RFC8340].

# The GREEN Framework

The "GREEN framework" described in {{?I-D.belmq-green-framework-06}}
covers monitoring and controlling devices and components where
monitoring includes measuring Power, Energy, demand and attributes of
Power.

For the whole picture of the monitoring interfaces and the relevant
requirements, please refer to "GREEN reference model" in section 4 in
{{?I-D.belmq-green-framework-06}}.

# Power and Energy Data Model

The Power and Energy Data Model reports the Power and Energy
consumption of each Energy Object as well as the units, sign,
measurement accuracy, etc. A containment tree view of the Power and
Energy Monitoring is presented.

~~~~ yangtree
{::include yang/ietf-power-and-energy.txt}
~~~~
{: markers="true" name="ietf-power-and-energy@2025-12.txt”}

# Relationship to the Hardware YANG Data Model

The ietf-hardware YANG module {{!RFC8348}} is required by the Power
and Energy YANG module. In the ietf-hardware YANG model, there are
three identifiers for hardware components, which are "name",
"physical-index" and "uuid". Among them, "name" is the key to "List of
components", "physical-index" matches entPhysicalIndex in the legacy
Entity MIB {{?RFC6933}} if it exists, and UUID is the Universally
Unified IDentifier {{?RFC4122}} of the component.

In the Power and Energy YANG Module defined in this specification,
there is a leaf named "source-component-id" which refers to the
component name in the ietf-hardware model. The "source-component-id"
can in turn reuse the UUID in the ietf-hardware YANG module.

There are also cases where the controllers also generate its own set
of UUIDs for the hardware (components). In such a case, it might be
necessary to document the mappings between the UUIDs generated on the
hardware side and the UUIDs on the controller side. Basically, the
devices (such as routers) generate the UUID and the controller can
query it.

The ietf-hardware YANG module {{!RFC8348}} allows to discover all the
device components, including the containment tree, and the parent/child
relationship, which is important for energy/power aggregation (see the
contains-child relationship in RFC 8348).

# Relationship to the EMAN Work

The EMAN IETF Working Group
(https://datatracker.ietf.org/wg/eman/about/) is a concluded Working
Group that produces a couple of RFCs in the domain of Power and
Energy. The Working Group produced MIB modules for monitoring and
control for power and energy, for the context information, for battery
monitoring, and an extension to the ENITY-MIB to add the UUID
definition {{?RFC6933}}.

For various reasons, those MIB modules were not implemented by
vendors.

The Power and Energy data model defined in this specification use the
Monitoring and Control MIB for Power and Energy {{!RFC7460}} as a
starting point to discuss the solution to the different use cases in
{{?I-D.ietf-green-use-cases-00}}.

However, it has not been the goal to simply map the MIB module to a
YANG module. The changes compared to the EMAN MIB modules are mainly
due to the alignment with the up-to-date requirements of the network
carriers on Energy Efficiency. Compared to the MIB modules, some
definitions and types are optimized, some new Energy Objects are added
and some legacy Energy Objects are removed accordingly.

# Power and Energy YANG Module

This YANG Module is used to monitor and control Power and Energy usage
of network devices and the components on these devices.

~~~~ yang
{::include yang/ietf-power-and-energy.yang}
~~~~
{: sourcecode-markers="true" sourcecode-name="ietf-power-and-energy@2025-12.yang”}

# Operational Considerations

Heterogeneous sensor capabilities across components complicate power
and energy aggregation. Operators must use the data-source-accuracy
identities (e.g., accuracy-measured-bronze vs. accuracy-estimated) to
weight data reliability carefully before aggregating Power
(current-power) and Energy (total-energy-consumed and/or
total-energy-produced) values to avoid skewing Device-Level Energy
Efficiency (DLEE) metrics.

Operators might not always be interested to get the individual component
accuracy. What counts is the device level or domain level, identity
accuracy-like-parent is introduced to meet their demands. From an
implementation point of view, to facilitate data collection and
aggregation on runtime and avoid post-aggregation data confidence
interval issues, operators and implementers should use as much as
possible this accuracy-like-parent identity.

YANG Push support eliminates device-side bucket storage by streaming
energy telemetry directly to controller-side via subscriptions.
Operators must verify the 'yang-push' bundle is enabled and validate
push-max-operational limits accommodate all component subscriptions,
preventing notification flooding while avoiding memory overhead on the
device.

# Security Considerations

This section will be completed once the YANG module is complete,
according to https://wiki.ietf.org/group/ops/yang-security-guidelines.

This section is modeled after the template described in Section 3.7.1
of [RFC-to-be draft-ietf-netmod-rfc8407bis].

The Power and Energy YANG module defines a data model that is designed
to be accessed via YANG-based management protocols, such as NETCONF
[RFC6241] and RESTCONF [RFC8040]. These YANG-based management
protocols (1) have to use a secure transport layer (e.g., SSH
[RFC4252], TLS [RFC8446], and QUIC [RFC9000]) and (2) have to use
mutual authentication.

The Network Configuration Access Control Model (NACM) [RFC8341]
provides the means to restrict access for particular NETCONF or
RESTCONF users to a preconfigured subset of all available NETCONF or
RESTCONF protocol operations and content.

# IANA Considerations

This document requests IANA to register the YANG module
"ietf-power-energy-monitoring".

Note to IANA: RFC XXXX must be replaced by the newly assigned RFC
number.

# Acknowledgments

This work has benefited from the regular discussions on the GREEN
Design Meetings. The authors wish to thank the WG chairs, Rob Wilton
and Diego Lopez, for organizing the recurring calls and progressing
the work. The authors also wish to thank the following individuals,
who provided helpful comments and reviews to this document.
