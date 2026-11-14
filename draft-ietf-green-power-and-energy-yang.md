---
title: Power and Energy YANG Module
abbrev: GREEN-PEM-YANG
docname: draft-ietf-green-power-and-energy-yang-latest
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

- ins: B. Claise
  fullname: Benoit Claise
  org: Everything OPS & Arrcus
  email: benoit@everything-ops.net
- ins: G. Chen
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

   RFC7950:
    title: The YANG 1.1 Data Modeling Language
    date: 2016-08
    target: https://datatracker.ietf.org/doc/html/rfc7950

   RFC8340:
    title: YANG Tree Diagrams
    date: 2018-03
    target: https://datatracker.ietf.org/doc/html/rfc8340

   RFC6241:
    title: Network Configuration Protocol (NETCONF)
    date: 2011-06
    target: https://datatracker.ietf.org/doc/html/rfc6241

   RFC8040:
    title: RESTCONF Protocol
    date: 2017-06
    target: https://datatracker.ietf.org/doc/html/rfc8040

   RFC4252:
    title: The Secure Shell (SSH) Authentication Protocol
    date: 2006-01
    target: https://datatracker.ietf.org/doc/html/rfc4252

   RFC9846:
    title: The Transport Layer Security (TLS) Protocol Version 1.3
    date: 2026-07
    target: https://datatracker.ietf.org/doc/html/rfc9846

   RFC9000:
    title: QUIC - A UDP-Based Multiplexed and Secure Transport
    date: 2021-05
    target: https://datatracker.ietf.org/doc/html/rfc9000

   RFC8341:
    title: Network Configuration Access Control Model
    date: 2018-03
    target: https://datatracker.ietf.org/doc/html/rfc8341

   RFC3688:
    title: The IETF XML Registry
    date: 2004-01
    target: https://datatracker.ietf.org/doc/html/rfc3688

   RFC6020:
    title: YANG - A Data Modeling Language for the Network Configuration Protocol (NETCONF)
    date: 2010-10
    target: https://datatracker.ietf.org/doc/html/rfc6020

   RFC9890:
    title: An Update to YANG Module Names Registration
    date: 2025-10
    target: https://datatracker.ietf.org/doc/html/rfc9890

informative:

   RFC9907:
    title: Guidelines for Authors and Reviewers of Documents Containing YANG Data Models
    date: 2026-03
    target: https://datatracker.ietf.org/doc/html/rfc9907



--- abstract

This document defines a YANG data model for Power and Energy
monitoring and control of devices within or connected to communication
networks.

--- middle

# Introduction

{::boilerplate bcp14-tagged}

This document defines a YANG data model for Power and Energy
Monitoring and control of devices within or connected to communication
networks, for the use cases document in
{{?I-D.ietf-green-use-cases}}.

The data model includes both the monitoring and control of Energy
Objects for networked devices.

This YANG data model is based on the "GREEN framework"
{{?I-D.ietf-green-framework}}, following the "GREEN terminology"
{{!I-D.ietf-green-terminology}}.

Power and Energy Monitoring and Control can be applied to devices in
communication networks. All identifiable devices with measurable or
representable Power and Energy characteristics fall within the scope
of this specification. Target devices include, but are not limited to,
routers, switches, Power over Ethernet (PoE) endpoints, smart Power
Distribution Units (PDUs), and storage and compute servers.

Where applicable, device monitoring extends to the Components of the
device as well as software and services running on the device. As a
result, the metrics to be monitored include Device Level Energy
Efficiency (DLEE), Component Level Energy Efficiency (CLEE), and
potential Service Level Energy Efficiency (SLEE) at the orchestrator
level. For example, a router can contain Components such as a Line
Processing Unit (LPU), a Switch Fabric Unit (SFU), and a Main
Processing Unit (MPU).

## Terminology

This document makes use of the terms defined in
{{!I-D.ietf-green-terminology}}:

- Power
- Nameplate Power
- Power State
- Power Interface
- Power Inlet
- Power Outlet
- Power Factor
- Energy
- Energy Object
- Component
- Energy Monitoring
- Energy Efficiency
- Device Level Energy Efficiency (DLEE)
- Component Level Energy Efficiency (CLEE)
- Service Level Energy Efficiency (SLEE)

The terms reused from {{!I-D.ietf-green-terminology}} and
{{?I-D.ietf-green-framework}} are capitalized in this
specification.

This document defines the following term, which is also capitalized:

Power Cap:
: An upper limit on the Power that an Energy Object receives, set by
  an administrator or a controller and enforced by the device, for
  example by reducing the clock frequency of a Component. Unlike the
  Nameplate Power, which is a fixed design value, a Power Cap can
  change during operation.

This document uses the terms Power and Energy in accordance with
{{!I-D.ietf-green-terminology}}. Power refers to the instantaneous
rate at which a device consumes or produces electrical energy
(typically expressed in watts). Energy, by contrast, represents the
cumulative amount of work performed over time (typically expressed in
joules or watt-hours). Both concepts are required within this YANG
module. Power enables real-time monitoring, control, and optimization
of device operation, while Energy provides a time-integrated view
necessary for accounting, reporting, and even for sustainability
analysis. This specification includes both Power and Energy
attributes.

The terminology for describing YANG modules is defined in [RFC7950].
The meanings of the symbols in the YANG tree diagrams are defined in
[RFC8340].

# The GREEN Framework

The "GREEN framework" described in {{?I-D.ietf-green-framework}}
covers monitoring and controlling devices and Components where
monitoring includes measuring Power, Energy, demand and attributes of
Power.

For the whole picture of the monitoring interfaces and the relevant
requirements, please refer to the "GREEN reference model" in
{{?I-D.ietf-green-framework}}.

# Power and Energy Data Model

The Power and Energy Data Model reports the Power and Energy of each
Energy Object, in fixed units per leaf (milliwatts for instantaneous
Power and the Power Cap, watts for the Nameplate Power, and
milliwatt-hours for Energy), together with the measurement accuracy,
etc.

Power and Energy are reported per direction, as unsigned values.
Received is the Power or Energy flowing into the Energy Object through
its Power Inlets, and provided is the Power or Energy flowing out of
it to other Energy Objects through its Power Outlets. Both can be
non-zero at the same time: a power supply receives Power from its
source and provides Power to the device, and the difference is its
loss. The same Power Interface can act as a Power Inlet and a Power
Outlet: a battery receives Power while charging and provides Power
while discharging. The state of a battery, such as its charge and
capacity, is out of scope of this document; it is covered by the
Battery MIB {{?RFC7577}}. For Power over Ethernet, the Power provided
by a switch is measured at its PoE ports, and the Power received by a
powered device at its own Power Inlet; the difference is dissipated
mainly in the cable and connectors.

The net Power of an Energy Object is its received Power
minus its provided Power, so that Power passed from one Energy Object
to another is not counted twice when the net Power of several Energy
Objects is summed. An example is given in {{psu-example}}.

A containment tree view of the Power and Energy Monitoring is presented.
The model uses the admin and oper leaves within the power-state
container to represent the intended (administrative) and operational
(actual) Power States respectively. The two leaves together form the
complete Power State management interface. The operational tree
('container energy-objects') will typically contain a significantly
larger number of instances than the configuration tree
('container energy-control'). The configuration tree, which is limited
to explicitly provisioned entries, provides a compact self-contained
view of the intent. For this reason, although an NMDA
(Network Management Datastore Architecture) design with a single
"state" leaf (per {{?RFC8342}}) was considered, it is not adopted in
this document.

The model also allows an administrator or controller to request a
Power Cap on an Energy Object, for example to cap the Power of a GPU
in a cluster. The requested limit is
configured in the power-cap container in /energy-control. The device
enforces the requested limit, or the closest value that the Energy
Object supports, and reports the limit in effect in the power-cap
container of the Power attributes in /energy-objects. Discovering the
range of limits that an Energy Object supports is out of scope of this
document.

The existence of a Power Cap and its effect are reported separately. The
power-cap container in /energy-objects is present only while a cap is
enforced, and the Energy Object then does not draw more Power than the
enforced limit. The limiting leaf indicates whether the cap is
constraining the Energy Object at this time, that is, whether the
Energy Object would draw more Power without the cap, so that its
operation is reduced to stay within the limit. For example, with a cap
of 300 W, a GPU that would draw 80 W when idle, or 200 W with a light
workload, is not constrained (limiting is false), whereas a GPU that
would draw 450 W during a training job is held at 300 W and runs more
slowly (limiting is true).

A management system cannot reliably derive the limiting indication by
comparing the received Power with the enforced limit: the Power
that the Energy Object would draw without the cap is not observable,
caps are typically enforced as an average over a time window, and the
measurement accuracy blurs the comparison. The device, which applies
the cap, knows whether it is constraining the Energy Object.

The id of an entry in /energy-control/energy-objects/energy-object
SHOULD be the id of an existing or anticipated entry in
/energy-objects/energy-object. This allows an administrator or
controller to correlate a configured Power State intent with the Energy
Object being monitored. The id is a leafref with require-instance
false, so that the configuration can be created before the Energy
Object is introduced and remains after it has disappeared from the
system. If the referred Energy Object does not currently exist, the
configuration item for it has no effect.

The relationship list models the relationship between an Energy
Object and its peer Energy Objects, using the
`energy-relationship-type` identities: `powered-by` and `powering`
(Power Source Relationship), `metered-by` and `metering` (Metering
Relationship), `aggregated-by` and `aggregating` (Aggregation
Relationship), and `enabled-by` and `enabling` (Functional Enablement
Relationship).

Each pair of identities expresses the same relationship from the
perspective of each participant (e.g., if Energy Object A is powered-by
Energy Object B, then Energy Object B is powering Energy Object A).
These relationship categories, including their use for power/metering
topology discovery and for preventing double-counting of Energy values,
are defined in {{?I-D.ietf-green-framework}}. For each relationship
type, one or more peer Energy Objects can be identified via the `id`
leaf within the `peer` list, a string value that is preferably the
peer's UUID; if the UUID is not known, another identifier can be used,
together with human-readable details captured in the `details` leaf.

Regarding relationships among Energy Objects, this document does not
provide a mechanism to configure relationships on the device (i.e.,
there is no relationship list under /energy-control); the
relationship list under /energy-objects/energy-object is read-only
operational data (config false).

For relationships between Components within the same device (e.g.,
between a Line Processing Unit (LPU) and a Switch Fabric Unit (SFU)),
the device can typically determine and populate this data directly,
without requiring external configuration.

Relationships between Energy Objects located on different devices are
generally established and maintained at the controller or Energy
Management System (EnMS) level, which has visibility into the
broader network topology, as discussed in
{{?I-D.ietf-green-framework}}. A device may still report a known
inter-device relationship (e.g., using the peer's network-level UUID)
when it has been made aware of it, but this module does not provide a
mechanism to configure such relationships on the device itself.

~~~~ yangtree
{::include yang/ietf-power-and-energy.txt}
~~~~
{: markers="true" name="ietf-power-and-energy.txt"}

# Relationship to the Hardware YANG Data Model

The ietf-hardware YANG Data Model for Hardware Management [RFC8348]
is required in connection with the 'source-component-id' leaf.

In the Power and Energy YANG Module defined in this specification,
the leaf 'source-component-id' refers to a hardware component
defined in the ietf-hardware module [RFC8348].

The ietf-hardware module provides three identifiers for hardware
components: 'name', 'physical-index', and 'uuid'.  The 'name'
leaf is the list key and uniquely identifies a component within
a single device, but carries no uniqueness guarantee across
different devices or management systems.  The 'physical-index'
leaf optionally matches entPhysicalIndex in the Entity MIB
[RFC6933] when the entity-mib feature is supported.  The 'uuid'
leaf is a Universally Unique Identifier [RFC9562], intended to
be unique across space and time, and therefore suitable for
stable identification of a component across management systems.

The GREEN YANG module imports the ietf-hardware module [RFC8348]
and uses the 'name' leaf as the value of 'source-component-id'. The
'name' leaf is the list key of the hardware components and uniquely
identifies a component within a single device. For component
identification across devices and management systems, the device
SHOULD also report the 'uuid' leaf of the referenced component.

The mapping between energy-object entries in this YANG Module and the
hardware-components in ietf-hardware YANG module {{!RFC8348}} is
designed to be 1:1, architecturally aligning each energy-object entry
with exactly one physical hardware component via source-component-id.
As no hardware class exists for hardware accelerators such as GPUs,
this document requests a new "accelerator" hardware class (see
{{accelerator-class}}).

There are also cases where controllers generate their own set of UUIDs
for the hardware components. In such a case, it might be necessary to
document the mappings between the UUIDs generated on the hardware side
and the UUIDs on the controller side. In general, the devices (such as
routers) generate the UUID, and the controller can query it.

The ietf-hardware YANG module {{!RFC8348}} allows discovering all the
device components, including the containment tree, and the parent/child
relationship, which is important for Energy/Power aggregation (see the
contains-child relationship in RFC 8348).

# Relationship to the EMAN Work

The EMAN IETF Working Group
(https://datatracker.ietf.org/wg/eman/about/) is a concluded Working
Group that produced several RFCs in the domain of Power and
Energy. The Working Group produced MIB modules for monitoring and
control for Power and Energy, for the context information, for battery
monitoring {{?RFC7577}}, and an extension to the ENTITY-MIB to add the
UUID definition {{!RFC6933}}.

For various reasons, those MIB modules were not implemented by
vendors.

The Power and Energy data model defined in this specification uses the
Monitoring and Control MIB for Power and Energy {{?RFC7460}} as a
starting point to discuss the solution to the different use cases in
{{?I-D.ietf-green-use-cases}}.

However, it has not been the goal to simply map the MIB module to a
YANG module. The changes compared to the EMAN MIB modules are mainly
due to the alignment with the up-to-date requirements of the network
carriers on Energy Efficiency. Compared to the MIB modules, some
definitions and types are optimized, some new data nodes are added,
and some legacy MIB objects are removed accordingly.
For example, the signed eoPower object of {{?RFC7460}} is replaced by
the Power received and the Power provided, as unsigned values, in the
same way as the Energy counters.

# Power and Energy YANG Module

Note to the RFC Editor: Please replace all occurrences of "XXXX" in
this document with the RFC number assigned to this document, and the
revision date of both YANG modules with the date of publication of
this document. Please remove this note before publication.

This YANG Module is used to monitor and control Power and Energy usage
of network devices and the Components on these devices.

~~~~ yang
{::include yang/ietf-power-and-energy.yang}
~~~~
{: sourcecode-markers="true" sourcecode-name="ietf-power-and-energy@2026-11-01.yang"}

The IANA-maintained identities for certification types are defined in
the following module.

~~~~ yang
{::include yang/iana-power-and-energy.yang}
~~~~
{: sourcecode-markers="true" sourcecode-name="iana-power-and-energy@2026-11-01.yang"}

# Operational Considerations

Heterogeneous sensor capabilities across Components complicate Power
and Energy aggregation. Operators must use the data-source-accuracy
identities (e.g., accuracy-measured-bronze vs. accuracy-estimated) to
weight data reliability carefully before aggregating Power
(instantaneous-power-received and instantaneous-power-provided) and
Energy (total-energy-received and total-energy-provided) values to
avoid skewing Device Level Energy Efficiency (DLEE) metrics.

The device reports the Power and Energy that each Energy Object
receives and provides, not a single consumption value. The consumption
of an Energy Object is its net value, received minus provided, which a
management system or a controller computes when needed. For a power
supply, this net value is its loss (see {{psu-example}}).

For implementers familiar with {{?RFC7460}}, the following table maps
its Power and Energy objects to this module. The values in {{?RFC7460}}
are scaled by a unit multiplier, while this module uses fixed units;
the Energy objects of {{?RFC7460}} cover a configured interval, while
the Energy counters of this module are cumulative; and the
discontinuity time of {{?RFC7460}} is a sysUpTime value, while this
module reports a date and time.

| RFC 7460 object  | This module |
|------------------|-------------|
| eoPower          | instantaneous-power-received minus instantaneous-power-provided |
| eoEnergyConsumed | total-energy-received |
| eoEnergyProvided | total-energy-provided |
| eoEnergyDiscontinuityTime | discontinuity-time |
{: title="Mapping of RFC 7460 Power and Energy Objects"}

The Energy counters are of type counter64 {{!RFC9911}}. With a unit
of one milliwatt-hour, a counter64 wraps around only after about 18
billion MWh, which does not happen in practice. The risk is rather a
reset that goes unnoticed: an Energy counter can restart at times
other than the re-initialization of the management system, for
example when a power supply is hot-swapped, when a line card is
reinserted, or when the Energy measurement is reset. A client that
computes the Energy over a period from two readings of a counter
therefore needs to check the discontinuity-time leaf: if it has
changed between the two readings, the difference between them is not
valid.

Operators might not always be interested in the accuracy of individual
Components; what matters is the accuracy at the device or domain
level. The accuracy-like-parent identity is introduced for this
purpose. The parent Energy Object is determined by the containment
tree {{RFC6933}}, which ietf-hardware {{RFC8348}} represents with the
parent leaf: starting from the Component of an Energy Object and
following the parent leaf up the containment tree towards the root,
the first Component that has an Energy Object provides the parent
Energy Object. The Energy Object at the top of the containment tree
has no parent, so its accuracy should be reported explicitly. A module
that reuses the power-values or energy-values groupings outside a
device, such as a controller-side module, can refine the default of
data-source-accuracy, for example to accuracy-unknown. From an
implementation point of view, to facilitate data collection and
aggregation at runtime and to avoid post-aggregation data confidence
interval issues, operators and implementers should use the
accuracy-like-parent identity as much as possible.

YANG Push support eliminates device-side bucket storage by streaming
Energy telemetry directly to controller-side via subscriptions.
Operators must verify the 'yang-push' bundle is enabled and validate
push-max-operational limits accommodate all Component subscriptions,
preventing notification flooding while avoiding memory overhead on the
device.

## Hardware Component Identification

The GREEN YANG module relies on the ietf-hardware module {{!RFC8348}}
for hardware component identification.  Among the three identifiers
that ietf-hardware provides for each component: 'name',
'physical-index', and 'uuid', only 'uuid' is intended to be
unique beyond a single device.
The 'name' leaf, while serving as the list key within a device,
is meaningful only in the context of that specific device.  The
'physical-index' leaf, where present, provides a mapping to the
legacy Entity MIB {{!RFC6933}}, but its availability depends on
whether the entity-mib feature is supported.

The 'source-component-id' leaf in this module is bound to the 'name'
leaf of the corresponding ietf-hardware component, since 'name' is
the list key and always present, whereas 'uuid' is optional in
{{!RFC8348}}. To correlate Energy Objects across devices, management
systems, and administrative domains without relying on device-local
naming conventions, a client uses the 'uuid' leaf {{!RFC9562}} of the
referenced component.

## Measurement Accuracy and Data Source Classification

Power and Energy metrics may originate from a wide range of sources and
estimation methods, each with different levels of reliability. These
include direct sensor measurements, manufacturer-provided
specifications, historical observations, and predictive models. Without
explicit characterization of data quality, comparisons and aggregations
may be misleading. The GREEN YANG data model therefore requires all
Power and Energy values to be associated with an accuracy
classification.

The model defines the following primary accuracy categories using YANG
identities:

- Unknown Accuracy: Data accuracy cannot be determined, or measurements
  are unavailable due to sensor failures, powered-off Components, or
  other operational constraints.

- Estimated Data: Values derived through indirect methods:
  - Static estimates: From manufacturer datasheets, nameplate ratings
    (critical for legacy devices, as described in the "Incremental
    Application of the GREEN Framework" use case in
    {{?I-D.ietf-green-use-cases}})
    - Identity: `accuracy-static`
  - Historic estimates: Based on prior measurements of this specific
    system under similar conditions
    - Identity: `accuracy-historic`
  - Learned estimates: Generated by machine learning models predicting
    consumption from workload patterns (see the "Energy-Efficient
    Management of Distributed AI Training Workloads" use case in
    {{?I-D.ietf-green-use-cases}})
    - Identity: `accuracy-learned`

- Measured Data: Direct, real-time sensor measurements with quantified
  precision:
  - Bronze: +/-30% accuracy for typical values.
  - Silver: +/-10% accuracy for typical values.
  - Gold: +/-5% accuracy for typical values.
  - Red: +/-2% accuracy for typical values.
  - Ones: All digits are valid, except trailing zeros.

Percentage-based accuracy fails for small values. For example, +/-5% of
100 mW is only 5 mW, which may be smaller than sensor noise. Industry
standards (IEC 62053, IEC 61850-7-4) address this by specifying:
Accuracy = MAX(percentage_error, absolute_threshold)

The absolute threshold suffixes (`-1`, `-10`, `-100`, `-1000`) are
expressed in the unit of the qualified value: milliwatts for Power,
and milliwatt-hours for Energy. For example, `-10` means +/-10
milliwatts for a Power value.

Example - A sensor with `accuracy-measured-gold-10` reports:

- 16.25 W -> actual value between 15.4375 W and 17.0625 W
  (5% = 0.8125 W > 0.010 W threshold)
- 0.15 W -> actual value between 0.140 W and 0.160 W
  (5% = 0.0075 W < 0.010 W threshold, so +/-10 mW applies)

Explicit accuracy reporting enables:

- Weighted aggregation: High-precision measurements carry appropriate
  weight when calculating network-wide Energy consumption
- Upgrade prioritization: Identify devices with low-accuracy reporting
  for sensor upgrades or replacement
- Compliance validation: Automated verification against regulatory
  thresholds requiring specific measurement precision
- Double-accounting prevention: Understand when PDU-level measurements
  (+/-2%) should override device estimates (+/-30%) to avoid counting
  the same Energy twice (see the "Double Accounting" use case in
  {{?I-D.ietf-green-use-cases}})
- Cross-domain correlation: Map accuracy expectations when integrating
  with external systems like 3GPP energy KPIs (see the "Consideration
  of other domains for obtention of end-to-end metrics" use case in
  {{?I-D.ietf-green-use-cases}})

The accuracy hierarchy uses YANG identities for extensibility, allowing
vendors to define manufacturer-specific accuracy classes while
maintaining interoperability through standardized base types.

## Industry-Standard Certifications

Energy Efficiency certifications issued by recognized testing
organizations provide standardized benchmarks for the expected
performance of equipment and Components. These certifications are
typically based on controlled laboratory measurements and formal
evaluation procedures. The GREEN YANG data model supports reporting of
such certifications in order to complement operational measurement data.

Common Certifications:

- 80 PLUS (Power Supply Units): Bronze/Silver/Gold/Platinum/Titanium
  tiers based on efficiency at 20%/50%/100% load
- ENERGY STAR: Government-backed program certifying energy-efficient
  products
- EPEAT: Electronic Product Environmental Assessment Tool ratings
  (Bronze/Silver/Gold)
- EU Energy Label: European efficiency ratings
- CN Energy Label: China efficiency ratings
- CQC: China Quality Certification for energy efficiency

Additional certification schemes may be supported through extensible
identities.

Certification data and measurement accuracy serve complementary
functions within the model.

Certification information describes the verified design-time efficiency
characteristics of a device or Component, as established through
independent testing. Measurement accuracy describes the precision and
reliability of reported operational data obtained from sensors or
estimation mechanisms.

Key differences include:

- Certification is typically applied at manufacturing time and remains
  stable throughout the product lifecycle.
- Measurement accuracy may vary over time due to calibration,
  environmental conditions, or sensor degradation.
- Certification is generally associated with discrete Components,
  such as power supply units.
- Measurement accuracy applies to individual metrics at Component,
  subsystem, or system level.

Both types of information may be reported simultaneously for the same
Energy Object.

Example: A power supply might have:

- Certification: `c80-plus`
  (80 PLUS certified; at the Platinum level, >=92% efficient at 50%
  load, independently verified. The identity does not convey the
  level.)
- Measurement Accuracy: `accuracy-measured-silver`
  (+/-10% sensor precision on real-time Power readings)

The certification tells operators that the Energy Object (for example,
a power supply unit) is designed to be efficient; the measurement
accuracy tells them how precisely they can monitor its actual
performance.


# Security Considerations

This section is modeled after the template described in Section 3.7.1
of {{RFC9907}}.

The Power and Energy YANG module defines a data model that is designed
to be accessed via YANG-based management protocols, such as NETCONF
[RFC6241] and RESTCONF [RFC8040]. These YANG-based management
protocols (1) have to use a secure transport layer (e.g., SSH
[RFC4252], TLS [RFC9846], and QUIC [RFC9000]) and (2) have to use
mutual authentication.

The Network Configuration Access Control Model (NACM) [RFC8341]
provides the means to restrict access for particular NETCONF or
RESTCONF users to a preconfigured subset of all available NETCONF or
RESTCONF protocol operations and content.

There are two writable data nodes defined in this YANG module that
may be considered sensitive or vulnerable in some network environments.
Write operations (e.g., edit-config) to these data nodes without proper
protection can have a negative effect on network operations:

- /energy-control/energy-objects/energy-object/power-state/admin:
  Unauthorized write access to this leaf allows an attacker to change
  the administratively requested Power State of an Energy Object.
  Depending on the target device or Component, this could be used to
  power down critical network infrastructure (resulting in denial of
  service), force a Component into a state that damages hardware, or
  mask an ongoing attack by cycling Power States to disrupt
  monitoring. Access to this data node SHOULD be limited to
  authorized administrators via NACM.

- /energy-control/energy-objects/energy-object/power-cap/limit:
  Unauthorized write access to this leaf allows an attacker to set a
  very low Power Cap on an Energy Object, degrading or disrupting its
  operation (resulting in denial of service), or to remove a cap that
  protects the power budget of a site. Access to this data node SHOULD
  be limited to authorized administrators via NACM.

Some of the readable data nodes in this YANG module may be considered
sensitive or vulnerable in some network environments. It is thus
important to control read access (e.g., via get, get-config, or
notification) to these data nodes. Specifically, the following
subtrees and data nodes have particular sensitivities:

- /energy-objects/energy-object/power and
  /energy-objects/energy-object/energy: These subtrees expose
  real-time and cumulative Power and Energy consumption for
  individual hardware components. Fine-grained, time-correlated
  Power/Energy telemetry can reveal operational patterns, such as
  workload levels, traffic volume, or usage schedules, of a device
  or of the network behind it. In some environments, this
  information could be leveraged as a side channel to infer
  sensitive operational or business information (e.g., data center
  utilization, capacity, or customer activity patterns).

- /energy-objects/energy-object/relationship: This list exposes
  relationships (e.g., powered-by, powering, metered-by) and UUIDs
  between Energy Objects, which can reveal the physical and logical
  power topology of a site. Disclosure of this information could
  assist an attacker in identifying high-value targets (e.g., shared
  power infrastructure whose disruption has a broad impact) or in
  correlating Energy Objects across administrative domains.

This document does not define any RPC operations or YANG
notifications.

# IANA Considerations

This document requests IANA to perform the following actions.

## YANG Module Name and URI Registry

IANA is requested to register the following URIs in the "IETF XML
Registry" [RFC3688].

| Field | Value                                                 |
|-------|-------------------------------------------------------|
| URI   | urn:ietf:params:xml:ns:yang:ietf-power-and-energy     |
| Registrant Contact | The IESG                                 |
| XML   | N/A; the requested URI is an XML namespace.           |
{: title="IETF URI Registry: ietf-power-and-energy"}

| Field | Value                                                 |
|-------|-------------------------------------------------------|
| URI   | urn:ietf:params:xml:ns:yang:iana-power-and-energy     |
| Registrant Contact | The IESG                                 |
| XML   | N/A; the requested URI is an XML namespace.           |
{: title="IETF URI Registry: iana-power-and-energy"}

IANA is also requested to register the following YANG modules in the
"YANG Module Names" registry {{RFC6020}} {{RFC9890}} within the "YANG
Parameters" registry group.

| Field               | Value                                   |
|---------------------|-----------------------------------------|
| Name                | ietf-power-and-energy                   |
| Maintained by IANA? | N                                       |
| Namespace | urn:ietf:params:xml:ns:yang:ietf-power-and-energy |
| Prefix    | eo                                                |
| Reference | RFC XXXX                                          |
{: title="IETF YANG Module Name Registry: ietf-power-and-energy"}

| Field               | Value                                   |
|---------------------|-----------------------------------------|
| Name                | iana-power-and-energy                   |
| Maintained by IANA? | Y                                       |
| Namespace | urn:ietf:params:xml:ns:yang:iana-power-and-energy |
| Prefix    | ianaeo                                            |
| Reference | RFC XXXX                                          |
{: title="IETF YANG Module Name Registry: iana-power-and-energy"}

## Accelerator Hardware Class {#accelerator-class}

IANA is requested to add the following enumeration to the
IANAPhysicalClass textual convention in the IANA-maintained
IANA-ENTITY-MIB module {{!RFC6933}}:

~~~~
   accelerator(TBD1)   -- e.g., GPU, TPU, NPU, or FPGA card
~~~~

As the iana-hardware YANG module reflects IANA-ENTITY-MIB {{!RFC8348}},
IANA is also requested to add the following identity to the
iana-hardware YANG module, with a new revision statement:

~~~~
   identity accelerator {
     base hardware-class;
     description
       "This identity is applicable if the hardware class is some
        sort of hardware accelerator, such as a Graphics Processing
        Unit (GPU), a Tensor Processing Unit (TPU), a Neural
        Processing Unit (NPU), or an FPGA-based accelerator card.";
     reference
       "RFC XXXX: Power and Energy YANG Module";
   }
~~~~

Note to IANA: the value 16, the next unassigned IANAPhysicalClass
value, is suggested for TBD1.

## Power and Energy Certification Types Registry

IANA is requested to create a new registry named
"Power and Energy Certification Types" within the "Power and Energy"
registry group.

This document defines the initial version of the IANA-maintained
`certification-type` identity in the `iana-power-and-energy` YANG
module. The registry assigns string identity names for power and energy
efficiency certification types, for use as identityref values in
"ietf-power-and-energy" YANG module. The registered value is the
unqualified identity name (e.g., energy-star or c80-plus). No
numeric code points are assigned by this registry. Registry Template is
as follows.

| Field                  | Value                                 |
|------------------------|---------------------------------------|
| Registry Name          | Power and Energy Certification Types  |
| Reference              | RFC XXXX                              |
| Registration Procedure | Expert Review [RFC8126]               |
| Available Values       | Unqualified identity names as strings |
{: title="IETF Power and Energy Certification Types Registry"}

| Identity Name |	Description	                       | Reference |
|---------------|------------------------------------|-----------|
| c80-plus      | 80 PLUS Power Supply Certification | RFC XXXX  |
| energy-star	  | ENERGY STAR certification          | RFC XXXX  |
| epeat | Electronic Product Environmental Assessment Tool ratings | RFC XXXX |
| eu-energy-label |	EU Energy Label: European efficiency ratings   | RFC XXXX |
| cn-energy-label | CN Energy Label: China efficiency ratings      | RFC XXXX |
| cqc             | China Quality Certification for energy efficiency | RFC XXXX |
{: title="Initial Registries"}

New entries to "Power and Energy Certification Types" registry require
Expert Review {{!RFC8126}}. The Designated Expert(s) should verify that:

- The certification is issued by a recognized and independent
  standards body, testing laboratory, regulatory authority, or
  equivalent organization.
- The certification has a stable, publicly accessible reference.
- The proposed identity name SHOULD be a short mnemonic derived
  from the official certification name.

When a new certification type is added to the registry, a new
`identity` statement MUST be added to the `iana-power-and-energy` YANG
module. The following substatements to the `identity` statement MUST be
defined:

- `base`: MUST contain the value `certification-type`.
- `status`: Include only if a registration has been deprecated (use the
  value `deprecated`) or obsoleted (use the value `obsolete`).
- `description`: MUST include the full name of the certification
  program and a brief description of its energy efficiency scope.
  Lines MUST NOT exceed 72 characters.
- `reference`: MUST include a stable URI to the certification program's
  official documentation or registry.

Unassigned or reserved values MUST NOT be present in the module.

When the "Power and Energy Certification Types" registry is updated
with a new entry, a corresponding new `identity` statement MUST be
added to the `iana-power-and-energy` YANG module, and a new revision
statement MUST be added in front of the existing revision statements.

IANA is requested to add the following note to the "Power and Energy
Certification Types" registry:

Certification types MUST NOT be directly added to the
iana-power-and-energy YANG module. They MUST instead be added to the
"Power and Energy Certification Types" registry. When this registry is
updated, the iana-power-and-energy YANG module MUST be updated as
defined in RFC XXXX.

# Acknowledgments

This work has benefited from the regular discussions on the GREEN
Design Meetings. The authors wish to thank the WG chairs, Rob Wilton
and Diego Lopez, for organizing the recurring calls and progressing
the work. The authors also wish to thank Rob Wilton, Diego Lopez,
Tony Li, Per Andersson, Qin Wu, Chih-Tsung Huang, Alex Huang Feng,
and Amanda Baber for their helpful comments and reviews of this
document.

--- back

# Example: Modeling a 750 W Power Supply {#psu-example}

This appendix shows how a 750 W power supply is modeled. The power
supply, psu-1, is fed by outlet 7 of a PDU and powers the chassis of
the device. Both are Components in the ietf-hardware module
{{!RFC8348}}, and psu-1 is contained in the chassis. The values are
illustrative: the power supply receives 400 W from the PDU and
provides 376 W to the chassis, an efficiency of 94% at about half its
Nameplate Power, and the Energy counters cover the 30 days at that load
since their last discontinuity, reported in discontinuity-time.

The following instance data is encoded in JSON {{?RFC7951}}. Power is
in milliwatts and Energy in milliwatt-hours, and the uint64 values are
encoded as JSON strings.

~~~~ json
{
  "ietf-power-and-energy:energy-objects": {
    "energy-object": [
      {
        "id": "psu-1",
        "source-component-id": "psu-1",
        "power": {
          "instantaneous-power-received": "400000",
          "instantaneous-power-provided": "376000",
          "nameplate-power": 750,
          "data-source-accuracy": "accuracy-measured-gold",
          "power-factor": 98
        },
        "energy": {
          "total-energy-received": "288000000",
          "total-energy-provided": "270720000",
          "discontinuity-time": "2026-10-01T00:00:00Z"
        },
        "certifications": [ "iana-power-and-energy:c80-plus" ],
        "relationship": [
          { "type": "powered-by",
            "peer": [ { "id": "pdu-a-outlet-7",
                        "details": "PDU A, outlet 7" } ] },
          { "type": "powering",
            "peer": [ { "id": "chassis" } ] }
        ]
      },
      {
        "id": "chassis",
        "source-component-id": "chassis",
        "power": {
          "instantaneous-power-received": "376000"
        },
        "energy": {
          "total-energy-received": "270720000",
          "discontinuity-time": "2026-10-01T00:00:00Z"
        },
        "relationship": [
          { "type": "powered-by",
            "peer": [ { "id": "psu-1" } ] }
        ]
      }
    ]
  }
}
~~~~

From these values, a management system derives:

- the loss of the power supply: 400 W - 376 W = 24 W;
- its efficiency: 376 W / 400 W = 94%, which the Energy counters
  confirm over the period (270,720 Wh / 288,000 Wh);
- the Power drawn from the PDU, by summing the net Power of the Energy
  Objects: 24 W for the power supply plus 376 W for the chassis, that
  is 400 W, without counting twice the Power passed from the power
  supply to the chassis.

The Power Factor and the efficiency are distinct metrics. The Power
Factor is the ratio of real power to apparent power at an Alternating
Current (AC) Power Inlet, and is considered to be 100% for Direct
Current (DC). The efficiency of an Energy Object that provides Power,
such as a power supply, is the ratio of the Power it provides to the
Power it receives. A high Power Factor does not imply a high
efficiency: in this example, the Power Factor is 98% and the
efficiency is 94%.

A battery is modeled in the same way: it reports received Power and
Energy while charging, and provided Power and Energy while
discharging.

# Changes {#changes}

Note to the RFC Editor: please remove this appendix before
publication.

## Changes from 03 to 04

- Removed the unit-multiplier leaves: each leaf now has a fixed unit;
  instantaneous-power is in milliwatts, int64, and optional.
- Renamed energy-entry to energy-object, object-id to id, and
  power-state-admin and power-state-oper to admin and oper.
- Based the admin and oper leaves on the power-state identity, so
  that both can take the Power State values.
- /energy-control mirrors /energy-objects, and its id references the
  Energy Object with require-instance false.
- Relationship to the Hardware YANG Data Model: explained the three
  ietf-hardware identifiers; added the Hardware Component
  Identification section.
- Accuracy thresholds are expressed in milliwatts.
- IANA Considerations: addressed the IANA early review: registration
  in the IETF XML Registry and the YANG Module Names registry, and the
  template and initial entries of the Power and Energy Certification
  Types registry.
- Took the Energy Object term from draft-ietf-green-terminology, and
  referenced the latest versions of draft-ietf-green-terminology and
  draft-ietf-green-framework.
- Editorial changes.

## Changes from 04 to 05

- Reported Power and Energy per direction, as unsigned values:
  instantaneous-power is replaced by instantaneous-power-received and
  instantaneous-power-provided; total-energy-consumed and
  total-energy-delivered are renamed total-energy-received and
  total-energy-provided.
- Added the Power Cap: definition, requested limit in /energy-control,
  and enforced limit in /energy-objects.
- Moved certifications to the energy-object level.
- Fixed the Energy units (milliwatt-hours).
- Added the power-values, energy-values, relationships, and
  certifications groupings.
- Defined the parent of accuracy-like-parent.
- Operational Considerations: consumption as the net Power, and the
  mapping of the RFC 7460 objects.
- Added the reporting rule for measurable values, and text on Power
  over Ethernet and batteries.
- Added Appendix A, an example of a 750 W power supply.
- IANA Considerations: registered the ietf-power-and-energy module,
  requested an "accelerator" hardware class, renamed eu-energy-level
  and cn-energy-level to eu-energy-label and cn-energy-label, and fixed
  the registry table (c80-plus, cqc).
- Capitalized the terms defined in draft-ietf-green-terminology.
- Updated references, and editorial changes.

## Changes from 05 to 06

- Added Power Factor to the Terminology.
- Appendix A: distinguished the Power Factor from the efficiency.
- Relationship to the Hardware YANG Data Model and Operational
  Considerations: source-component-id is bound to the 'name' leaf of
  the hardware component; the device SHOULD also report the 'uuid'
  leaf.
- Energy counters: yang:counter64 instead of uint64, and a
  discontinuity-time leaf, mapped from eoEnergyDiscontinuityTime of
  RFC 7460.
