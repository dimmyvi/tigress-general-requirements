---
title: "Transfer Digital Credentials Securely - Sample Implementation - Signal Protocol"
abbrev: "tigress-signal-sample-implementation"
category: info

docname: draft-tigress-signal-sample-implementation-latest
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
    ins: D. Vinokurov
    name: Dmitry Vinokurov
    organization: Apple Inc
    email: dvinokurov@apple.com

normative:

informative:
  Signal-20:
    author:
    -
      fullname: Moxie Marlinspike
    -
      fullname: Trevor Perrin
    title: "Signal Protocol. The Sesame Algorithm: Session Management for Asynchronous Message Encryption. Revision 2, 2017-04-143"
    date: 2017-04
    target: https://signal.org/docs/specifications/sesame/

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

This document describes a sample implementation of transferring digital credentials securily (Tigress) using Signal protocol.

--- middle

# Introduction

Prevously Tigress reviewed an implementation of digital credentials transfer using Tigress protocol (https://datatracker.ietf.org/doc/draft-art-tigress/). In previous IETF meetings community asked to review other possible solutions using alternative standards to illustrate how Tigress problem can be solved differently.
In this document we are trying to describe how an alternative potential implementation of a solution to Tigress {{Tigress-req-02}} problem of transferring digital credentials securily can be done using Signal Protocol.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Signal Protocol Sample Implementation

 The Signal Protocol {{Signal-20}} is a secure messaging protocol that provides end-to-end encryption for instant messaging and voice/video calls. The Signal Protocol is primarily used through the Signal app, which is available for both Android and iOS devices. Users are free to implement Signal Protocol on their own in a custom application or integrate with Signal application via set of documented APIs. For this document, we will focus on this implementation of the Signal Protocol.

 ## Secure Credential Transfer with Signal Protocol:

 For Secure Credential Transfer {{Tigress-req-02}} with Signal Protocol, the message sequence and message contents between Sender and Receiver devices will be the same as the other approaches.
 Per Signal protocol specification {{Signal-20}  there is a requirement to implement an intermediary server that is used to store  user and device records containing security keys (one-time prekeys and signed prekeys), identity keys, user and device identities and all encrypted messages stored in mailboxes. In real system these functions might be distributed across multiple servers.

 Once the common secret is established using X3DH agreement and secure session is created between sender and receiver devices using Double Ratchet session, the credential application on the sender’s device will encrypt the provisioning information using sender encryption key and send it to the intermediary server. Receiver device will receive the encrypted message from signal intermediary server, using X3DH and Double Ratchet algorithms will create the receiver encryption key  will decrypt the message and get the provisioning data. Credential management application (digital wallet)  on both sender and receiver devices will have to implement Signal Protocol or integrate with Signal application API in order to exchange encrypted messages. For stateless sharing, the receiver digital wallet will take that provisioning information and provision the credential.

 For the stateful sharing flow, the receiving credential application will encrypt modified data (e.g. CCC Key Signing Request) and send it to Signal intermediary server. Sender device will read the encrypted data, re-create receiving decryption key and decrypt the message.  Sender will then generate CCC Key Import Request, encrypt the data with sending encryption key and upload it to intermediary server. Receiver device will read the encrypted message from intermediary server, generate receiver decryption key and decrypt the data. Now receiver device can provision new credential in the digital wallet .

 While this is guaranteed to be a secure method of transferring data between users, there are a few considerations.

 * Both sender and receiver devices have to implement support of Signal Protocol in credential management application (digital wallet) or have Signal application installed and Signal API supported in digital wallet.
 * Intermediate Signal server has to be implemented to support Signal Protocol or user accounts have to be created within Signal Application servers.
 * Intermediate server (servers) in Signal Protocol require user identities / authentication and device identities. Signal intermediate server, even though may not decrypt the content of the messages, may correlate the fact of information exchanges between certain users and certain devices by their identities.
 * More security considerations are listed in “Security considerations” section of Signal specification: https://signal.org/docs/specifications/sesame/#security-considerations

 Signal application is currently available on iOS and Android, so most users are able to make accounts for free. However, it may limit sharing for sender and receiver pairs that only have one method of contact outside of Signal. It is worth noting that while the Signal Protocol is open source, the Signal Protocol libraries for iOS and Android are not. The libraries are licensed under the GPLv3 license, which allows for use and modification, but they also include some proprietary components that are not open source.


# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
