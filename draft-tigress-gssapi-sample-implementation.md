---
title: "Transfer Digital Credentials Securely - Sample Implementation with GSS API"
abbrev: "tigress-gssapi-sample-implementation"
category: info

docname: draft-tigress-gssapi-sample-implementation-latest
submissiontype: IETF
number:
date:
consensus: true
v: 3
area: ART
workgroup: TIGRESS
keyword: Internet-Draft
venue:
#  group: WG
#  type: Working Group
#  mail: WG@example.com
#  arch: https://example.com/WG
  github: "dimmyvi/tigress-general-requirements"
  latest: "https://datatracker.ietf.org/doc/draft-tigress-general-requirements/"

author:
 -
    ins: C. Astiz
    name: Casey Astiz
    organization: Apple Inc
    email: castiz@apple.com
 -
    ins: A. Pelletier
    name: Alex Pelletier
    organization: Apple Inc
    email: a_pelletier@apple.com


normative:

informative:
  Tigress-req-02:
    author:
    -
      ins: D. Vinokurov
      name: Dmitry Vinokurov
    -
      ins: A. Pelletier
      name: Alex Pelletier
    -
      ins: C. Astiz
      name: Casey Astiz
    -
      ins: B Lassey
      name: Brad Lassey
    title: "Tigress general requirements"
    date: 2023-02
    target: https://github.com/dimmyvi/tigress-general-requirements/


--- abstract

This document describes a sample implementation of transferring digital credentials securily (Tigress) using GSS API.

--- middle

# Introduction

Prevously Tigress reviewed an implementation of digital credentials transfer using Tigress protocol (https://datatracker.ietf.org/doc/draft-art-tigress/). In previous IETF meetings community asked to review other possible solutions using alternative standards to illustrate how Tigress problem can be solved differently.
In this document we are trying to describe how an alternative potential implementation of a solution to Tigress {{Tigress-req-02}} problem of transferring digital credentials securily can be done using GSS API {{!RFC2743}}.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

# GSS-Api Proposal

 General security service application program interface, or GSS-API, from {{!RFC2743}} defines a generic protocol for the security of messages being transferred and can provide authentication, integrity, and confidentiality. GSS-API does not define how the messages are sent between parties.

 Leveraging GSS-API provides flexibility to easily change the security of how a credential is transferred, but a lot of work to define the communication channel between two devices is still required. GSS-API also requires that each party have auth credentials before the communication occurs, which isn’t a requirement for our use case.

 ## Secure Credential Transfer with GSS-API

 Because GSS-API does not define the communication channel we will assume the devices are able to communicate via an arbitrary intermediary server. An example transfer using GSS-API + Tigress could like like:

 1. Sender creates a single use auth credential and encrypts it with a symmetric key that will be a shared secret.
     1. *Tigress*: The creation, structure, and validation of this single use auth credential would need to be defined by Tigress.
 2. Sender creates a GSS-API security context token with the encrypted credential.
 3. Sender sends security context token + shared secret to receiver.
     1. If a mailbox style intermediate server is used this can be done via a url where the shared secret is include in the url fragment.
     2. *Tigress*: How this information is sent to the intermediary server would need to be defined by tigress.
 4. Receiver gets information for communicating with the sender and the shared secret.
     1. This information could be transferred via a url, a file, a QR code, etc.
     2. *Tigress*: The high level format of this information would need to be defined by tigress so that devices could parse the data and extract the GSS-API specific parts.
     3. *Tigress:* We could recommend that this information have a nice preview, but that wouldn’t be required.
 5. Receiver accepts the security context token, and uses the shared secret to validate the credential.
 6. Receiver sends back security context token to sender. This process is repeated until the security context is full established.
     1. *Tigress*: How the receiver sends the opaque blob to the sender via the intermediary server would need to be defined by tigress.
 7. Sender creates credential to share, use GSS-API to create a message token, send message token to receiver.
 8. Receiver gets message token and uses GSS-API to extract the underlying credential.
     1. If the receiver is done they can terminate the transfer and send a GSS-API termination back to the sender.
         1. *Tigress*: How the session with the intermediary server between the sender and receiver is terminated would need to be defined by tigress.
     2. Or, the receiver can perform additional calls with the sender to complete transferring the credential.


# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
