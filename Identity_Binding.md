# Binding of eIDAS identity to records in the Swedish population register

### 2023-06-07 (draft)

This document contains information about the binding process when an eIDAS 
identity is linked to a record in the Swedish population register.

---

<p class="copyright-statement">
Copyright &copy; <a href="https://www.digg.se">The Swedish Agency for Digital Government (DIGG)</a>, 2023. All Rights Reserved.
</p>

## Table of Contents

1. [**Introduction**](#introduction)

2. [**The Identity Binding Service**](#the-identity-binding-service)

    2.1. [Making a Binding](#making-a-binding)
    
    2.2. [eIDAS-node Queries](#eidas-node-queries)

3. [**Identity Binding Levels**](#identity-binding-levels)

    3.1. [Basic Binding](#basic-binding)
    
    3.2. [Enhanced Binding](#enhanced-binding)
    
    3.3. [Verified Binding](#verified-binding)
    
4. [**Identity Binding Processes**](#identity-binding-processes)

    4.1. [Matching Against the Population Register](#matching-against-the-population-register)
    
    4.2. [Use of Swedish eID](#use-of-swedish-eid)
    
    4.3. [Attestation from Relative](#attestation-from-relative)
    
    4.4. [Foreign Identity Present in the Population Register](#foreign-identity-present-in-the-population-register)

    4.5. [Passport or ID-card Scanning](#passport-or-id-card-scanning)
    
5. [**Versions**](#versions)
    
<a name="introduction"></a>
## 1. Introduction

Public relying parties in Sweden commonly use the Swedish personal 
identification number (a.k.a. personnummer) or the Swedish coordination 
number (a.k.a. samordningsnummer) as identifier to carry out the 
authorisation of the authenticated user. These records cannot be found
in attributes when user authenticatation is performed through eIDAS. 

The Identity Binding Service is an extension to Swedish eIDAS-node and 
a part of Sweden's digital infrastructure aimed at linking records in 
the Swedish population register to notified eIDs from member states 
outside of Sweden.

<a name="the-identity-binding-service"></a>
## 2. The Identity Binding Service
The Identity Binding Service can be used by end-users, who are 
authenticated through eIDAS, in those cases when:
- end-user have a registered record in terms of Swedish personal identification number (a.k.a. personnummer) or the Swedish coordination number (a.k.a. samordningsnummer) in the Swedish population register, and
- end-user have chosen to bound this record to her/his eIDAS notified eID.

When end-user's Swedish records are linked to her/his eID, the Swedish eIDAS-node can 
provide this information in the assertion to relying party.

<a name="making-a-binding"></a>
### 2.1. Making a Binding
Identity binding can be made through a number of different record matching processes. 
These processes are run separately or in combinations and can result in linked 
identifications based on three different levels of confidence. See also 
[Identity Binding Levels](#identity-binding-levels)

> TODO: Include links to https://elegitimation.se and other resources where we describe
the ID-matching service.
    
<a name="eidas-node-queries"></a>
### 2.2. eIDAS-node Queries

The identity bindings of the Identity Binding service will be accessible for the
Swedish eIDAS-node via a query-API. During an eIDAS authentication the Swedish eIDAS-node
will query this API for a binding between the attributes presented in the assertion 
received from the foreign node and a Swedish personal identity number. If such a
binding exists two attributes will be added to the resulting assertion delivered to
the Swedish relying party (service provider). These attributes are:

- `urn:oid:1.2.752.201.3.16` (mappedPersonalIdentityNumber) - Contains the Swedish
personal identity number that was bound to the eIDAS identity.

- `urn:oid:1.2.752.201.3.6` (personalIdentityNumberBinding) - Contains an URI that
represents the "binding level", see [section 3, "Identity Binding Levels"](#identity-binding-levels) below.

See sections 2.5, "eIDAS Natural Person Attribute Set", and 3.3.2, "The mappedPersonalIdentityNumber and personalIdentityNumberBinding Attributes", of [Attribute Specification for the Swedish eID Framework](https://docs.swedenconnect.se/technical-framework/updates/04_-_Attribute_Specification_for_the_Swedish_eID_Framework.html) for more information
about attribute release during an eIDAS authentication.

<a name="identity-binding-levels"></a>
## 3. Identity Binding Levels
Linked identifications through the Identity Binding Service can be provided in three different levels of confidence. 
These levels can be used in the authorization process by the relying party. 

<a name="basic-binding"></a>
### 3.1. Basic Binding

**URI:** `http://id.swedenconnect.se/id-binding/level/basic`

**Description:** A basic binding where the identity attributes received from the foreign
eIDAS-node matches an individual's record in the population register.

The process `http://id.swedenconnect.se/id-binding/process/populationregister` 
([4.1](#matching-against-the-population-register)) must have been applied.

    
<a name="enhanced-binding"></a>
### 3.2. Enhanced Binding

**URI:** `http://id.swedenconnect.se/id-binding/level/enhanced`

**Description:** Enhanced binding. Apart from meeting all requirements for the basic level, the binding has been strengthened with additional steps. 

The process `http://id.swedenconnect.se/id-binding/process/populationregister` 
([4.1](#matching-against-the-population-register)) must have been applied as well
as at least one of the following processes:

- `http://id.swedenconnect.se/id-binding/process/relative` ([4.3](#attestation-from-relative))
    
<a name="verified-binding"></a>
### 3.3. Verified Binding

**URI:** `http://id.swedenconnect.se/id-binding/level/verified`

**Description:** Verified binding. The binding has been verified with strong and trustworthy processes as listed below:

The process `http://id.swedenconnect.se/id-binding/process/populationregister` must have
been applied as well as at least one of the following processes:

- `http://id.swedenconnect.se/id-binding/process/swedish-eid` ([4.2](#use-of-swedish-eid))

- `http://id.swedenconnect.se/id-binding/process/presentbinding` ([4.4](#foreign-identity-present-in-the-population-register))

- `http://id.swedenconnect.se/id-binding/process/iddoc-scanning` ([4.5](#passport-or-id-card-scanning))
    
<a name="identity-binding-processes"></a>
## 4. Identity Binding Processes

This section contains a detailed description of the processes of binding identities that
are possible to perform. Each process is identified with a URI.

> Note: The process URI:s are not part of a resulting SAML assertion. However, they will
be stored in matching records and log entries.

<a name="matching-against-the-population-register"></a>
### 4.1. Matching Against the Population Register

**URI:** `http://id.swedenconnect.se/id-binding/process/populationregister`

**Description:** ...

> TODO: Based on the matching in Navet, there can be different strengths on this
matching. Should we have several URI:s? I suggest that we don't since it will be
too much to handle.

<a name="use-of-swedish-eid"></a>
### 4.2. Use of Swedish eID

**URI:** `http://id.swedenconnect.se/id-binding/process/swedish-eid`

**Description:** The individual making a binding holds an approved<sup>1</sup> Swedish 
eID that is used to sign an attestation of the correctness of the binding.

TODO: more

> **\[1\]:** The Swedish eID must .... (Svensk e-legitimation, godkänd av DIGG) ...
    
<a name="attestation-from-relative"></a>
### 4.3. Attestation from Relative

**URI:** `http://id.swedenconnect.se/id-binding/process/relative`

**Description:** A relative to the person making the binding uses an approved
Swedish eID (see [4.2](#use-of-swedish-eid) above) to attest the correctness
of the binding.

> Note: Only relationships registered in the Swedish population register will
be allowed.

<a name="foreign-identity-present-in-the-population-register"></a>
### 4.4. Foreign Identity Present in the Population Register

**URI:** `http://id.swedenconnect.se/id-binding/process/presentbinding`

**Description:** For some countries a foreign personal identity number may be
present in the Swedish population register. More ...

<a name="passport-or-id-card-scanning"></a>
### 4.5. Passport or ID-card Scanning

**URI:** `http://id.swedenconnect.se/id-binding/process/iddoc-scanning`

**Description:** A passport ...

<a name="versions"></a>
## 5. Versions

- 2023-04-24: First draft version


