---

title: "Workload Identity Use Cases"
abbrev: "WIMSE Use Cases"
category: info

docname: draft-gilman-wimse-use-cases-latest
submissiontype: IETF  # also: "independent", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
# area: SEC
# workgroup: WIMSE
keyword:
 - identity
 - security
 - cloud computing

author:
 -
    fullname: Evan Gilman
    organization: SPIRL
    email: evan@spirl.com
    role: editor
 -
    fullname: Justin Richer
    organization: Bespoke Engineering
    email: ietf@justin.richer.org
 -
    fullname: Pieter Kasselman
    organization: Microsoft
    email: pieter.kasselman@microsoft.com
 -
    fullname: Joseph Salowey
    organization: Venafi
    email: joe@salowey.net

normative:

informative:


--- abstract

Workload identity systems like SPIFFE provide a unique set of security challenges, constraints, and possibilities that affect the larger systems they are a part of. This document seeks to collect use cases within that space, with a specific look at both the OAuth and SPIFFE technologies.

--- middle

# Introduction

The OAuth and SPIFFE communities have historically been fairly disjoint. The former is a set of identity standards shepherded by the IETF and is (mostly) human-centric, while the latter is a set of identity standards shepherded by the CNCF and is (mostly) workload-centric. Recently, members of both communities have begun to discuss a set of common challenges that they are facing, which they believe could be evidence of a gap in the broader ecosystem of identity standards.

This document captures those challenges as a set of use cases as a first step towards exploring that gap, should it in fact exist.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Use Cases

This section captures the underserved use cases identified. To start, we organized use cases by the person contributing them to this doc. Then, we attempted to bucket them by use case - this is still a WIP. Once finished, we will see what patterns emerge (e.g. policy enforcement, operational, etc) and prioritize them.

## Constrained Credential Security

As a security engineer, I’d like to mitigate the unconstrained re-use of a credential by those who are able to observe it in use (e.g. a proxy, a log message, or a workload processing the request)

1.    As a security engineer, I’d like to prevent token replay in the event that one of my internal services is compromised.
1.    If a workload credential is compromised, I can’t re-use it.
1.    Workload authentication using asymmetric credentials

## Cross-workload Access

As a [SPIFFE,OAuth] workload owner, I’d like to access other workloads that are using [SPIFFE,OAuth] in a simple and consistent way, regardless of their location, platform, or domain.

1.    As a SPIFFE user, I’d like to access OAuth protected resources without having to provide any additional secrets (as a SPIFFE user with more than 10k workloads, I’d like to access OAuth protected resources without having to manage 10k OAuth Clients).
1.    Access workloads from different service providers (access across different trust domains workloads to workload from different companies).
1.    Access workloads running in different cloud services (Multi-cloud deployments).

## Chain of Custody for Requests

As a security engineer, I’d like a verifiable chain of custody for each request transiting my system, starting with the request initiator, which may be a human or a workload.

1.    As a security engineer, I’d like to authorize data access RPCs iff the data owner issued the original request (a requests made by the data owner transit many backend services prior to reaching the data access layer).
2.    Authenticating and authorizing a service that is operating on behalf of a logged in user.
3.    Authenticating and authorizing a service that is operating on behalf of a user as a schedule job.
4.    As a security engineer, I’d like to authorize payment RPCs iff the request has transited our fraud detection service.
5.    As a security engineer, I’d like to authorize an RPC iff the request entered our infrastructure via a specific front end system.

## Local Authentication and Authorization Decisions

As a security engineer, I’d like to be able to make local authentication and authorization decisions in order to meet my performance and availability requirements.

1.    As a developer, I’d like security-related hot path delays to not exceed <<10ms.
2.    As a developer, I’d like things to continue working through (potentially asymmetric) network disruption.
3.    Lookup of info/keys related to an entity's identity needs to work when the entity is disconnected from the rest of the system (particularly “upstream” entities that trust comes from).
4.    Account onboarding is not strictly time-sensitive (can use networks), but local account use is (day-to-day authentication needs to stay local).

## Audit Logs

As a security engineer, I’d like a place to record information about an entity for the purposes of remediation, reconciliation, audit and forensics.

1.    If a workload is compromised, I can remediate that specific workload without impacting others.
2.    If an account is onboarded based on info from another entity, we need to write that down into the account and carry it through the network, especially if the account is used to onboard onto an entity further down the call stack.
3.    Reconcile logs when a disconnected entity is re-connected to the overall network fabric.

## Consistent Entity Identification

I need to be able to identify different entities uniquely and deterministically within the system.

1.    Each network entity needs to be identified uniquely (and http urls don’t quite give us all the aspects we need)
2.    As a SPIFFE user, I’d like a standard way to learn the bundle endpoint parameters of a remote trust domain

## Authorization

As a security engineer, I’d like a place to record information about an entity for use in authorization decisions.

1.    As a security engineer, I’d like to authorize an RPC iff the software calling it matches a specific SHA value.
2.    Authentication based on credential service provider (CSP), infrastructure or workload identity documents.
3.    Ability to carry rights/policies/privileges with a verifiable artifact to a disconnected entity for that entity to verify without having to reconnect.
4.    Transporting capabilities to transfer the permission to execute an operation from caller to service.
5.    Record should be append-only as it goes through the call chain. Participation (adding to the record) is not mandatory for every node in the chain.

## General requirements

In addition to the above use cases, the authors have determined the following general requirements:

AI: Observability should be a requirement. The credential should have a meaningful identifier that can be logged etc.

Accountability: Workloads need to be able to make a localized decision but still be accountable to the overarching policy and framework that provisioned them.

The system owner/operator should be able to effect changes in the system (the control plane) based on signals from the application plane.

Definition of information encapsulated in the document (e.g. capability transmission).


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
