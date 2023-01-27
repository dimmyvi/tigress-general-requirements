---
title: "Transfer Digital Credentials Securely - General Requirements"
abbrev: "tigress-general-requirements"
category: info

docname: draft-tigress-general-requirements-latest
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
    ins: D. Vinokurov
    name: Dmitry Vinokurov
    organization: Apple Inc
    email: dvinokurov@apple.com
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
 -
    ins: B Lassey
    name: Brad Lassey
    organization: Alphabet Inc
    email: lassey@google.com

normative:

informative:


--- abstract


This document describes the use cases necessitating the secure transfer of digital credentials, residing in a digital wallet, between two devices and defines general assumptions, requirements and the scope for possible solutions to this problem.

--- middle

# Introduction

In this document we are identifying a problem of transferring digital credentials (e.g. a digital car key, a digital key a a hotel room or a digital key to a private house) from a wallet on one device (smartphone) to another, particularly, if these devices belong to two different platform (e.g. one is iOS, another - Android). 
Today, there is no widely accepted way of transferring digital credentials securely between two digital wallets independent of hardware and software manufacturer. This document describes the problem space and the requirements for the solution the working group creates.

A Working Group, called Tigress has been established to find a solution to a given problem. 
Within the WG an initial solution was presented (https://datatracker.ietf.org/doc/draft-art-tigress). The community decided to generalize the requirements to the solution and consider alternative solutions within the WG.

THis document presents the general requirements to possible solutions and  specifies certain privacy requirements in order to maintain a high level of user privacy.

# General Setting

When sharing digital secure credentials, there are several actors involved. This document will focus on sharing information between two digital wallets, directly or through an intermediary server.

Digital credentials provide an access to property owned and operated by 3-rd party companies, such as hotel or residential building owners. The companies that are providing the digital credential for consumption by a digital wallet are the Provisioning Partners.
Provisioning Partner require to have control over digital credential issuance and life time management. Each digital wallet has a preexisting trust relationship between itself and the Provisioning Partner.

The interface between the devices and the Provisioning Partner can be proprietary or a part of published specifications. The sender wallet obtains provisioning information from the provisioning partner, then shares it to the recipient using a solution defined in Tigress WG. The recipient then takes that data and sends it to the Provisioning Partner to redeem a credential for consumption in a digital wallet.

For some credential types the Provisioning Partner who issues new credentials is actually the sender wallet. In that scenario the receiver will generate a new key material at the request of the sender, and then communicate with the sender over Tigress to have its key material signed by the sender.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

General terms:

- Credential information - data used to authenticate the user with an access point.
- Provisioning information - data transferred from Sender to Receiver device that is both necessary and sufficient for the Receiver to request a new credential from Provisioning Partner to provision it to the Receiver device.
- Provisioning - A process of adding a new credential to the device.
- Provisioning Partner - an entity which facilitates Credential Information lifecycle on a device. Lifecycle may include provisioning of credential, credential termination, credential update.
- Sender (device) - a device initiating a transfer of Provisioning Information to a Receiver that can provision this credential.
- Receiver (device) - a device that receives Provisioning Information and uses it to provision a new credential.
- Intermediary (server) - an optionl intermediary server that provides a standardized and platform-independent way of transferring provisioning information between Sender and Receiver devices.
- Digital Wallet - A device, service, and/or software that faciliates transactions either online or in-person via a technology like NFC. Digital Wallet's typically support payments, drivers licenses, loyalty cards, access credentials and more.

# Use Cases

- Let's say Ben owns a vehicle that supports digital keys which comply with the CCC specification {{CCC-Digital-Key-30}}. Ben would like to let Ryan borrow the car for the weekend. Ryan and Ben are using two different mobile phones with different operating systems. In order for Ben to share his digital car key to Ryan for a weekend, he must transfer some data to the receiver device. The data structure shared between the two participants is defined in the {{CCC-Digital-Key-30}}. In addition, the {{CCC-Digital-Key-30}} requires the receiver to generate required key material and return it to the sender to sign and return back to the receiver. At this point, the receiver now has a token that will allow them to provision their new key with the car.

- Bob booked a room at a hotel for the weekend, but will be arriving late at night. Alice, his partner, comes to the hotel first, so Bob wants to share his digital room key with Alice. Bob and Alice are using two different mobile phones with different operating systems. In order for Bob to share his digital room key to Alice for a weekend, he must transfer some data to her device. The data structure shared between the two participants is proprietary to the given hotel chain (or Provisioning Partner). This data transfer is a one-time, unidirectional transfer from Bob’s device to Alice’s. Once Alice receives this data, she can provision a new key to her digital wallet, making a call to Provisioning Partner to receive new credential information.


# Assumptions

- Original credential information (with cryptographic key material) MUST NOT be sent or shared. Instead, sender SHALL be transferring its approval token for Receiver to acquire new credential information. The reason for that is that the Provisioning Partner needs to control who the new credential is issued to, control new credential life cycle and manage the credentials on sender and receiver devices independently (e.g. receiver device may have just a subset of the sender's device access rights or receiver's device credential may be revoked independently of the sender's device credential).
- Provisioning Partner SHALL NOT allow for two users to use the same credential / cryptographic keys - this requirenet is a direct result of the previous assumption. Credentials on sender's and receiver's devices shall be different to be managed independently. If the same cryptographic key is copied and provisioned on a new device, in case of key revocation both devices will lose access.  
- Security: Communication between Sender / Receiver and Provisioning Partner SHOULD be trusted. Since new credential key matherial is generated by Provisioning Partner the channel between the device and Provisioning Provider shall be secure and trusted by both parties.
- In case if an intermediary server used during the credential transfer from sender device to receiver device, the choice of intermediary SHALL be defined by the application initiating the credential transfer. The wallet or another application that manages credentials on sender device shall make the decision regarding the channel to be used to sent the Provisioning Information.
- Sender and Receiver SHALL both be able to manage the shared credential at any point by communicating with the Provisioning Partner.
- Any device OEM with a digital credential implementation adherent to Tigress solution SHALL be able to receive shared provisioning information, whether or not they can originate provisioning information themselves. We defined the digital credential transfer as platform-independant, therefore, if the receiver device can recognize the data format of the received Provisioning Information, it should be able to provision the new credential to the Digital Wallet. 
- Provisioning a credential on the Receiver MAY require multiple round trips. In case the Provisioning Partner is not used for the certan type of credentials, both sender and receiver devices are used to generate new credentials' cryptographic key matherial and sign on the new cryptographic key for it to be trusted by the access point.


# Requirements


# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
