+++
title = "Internet Security Association and Key Management Protocol (ISAKMP)"
author = ["svejk"]
draft = false
+++

## Internet Security Association and Key Management Protocol (ISAKMP) {#internet-security-association-and-key-management-protocol--isakmp}


### Wiki Overview {#wiki-overview}

ISAKMP defines the procedures for authenticating a communicating peer, creation and management of Security Associations, key generation techniques and threat mitigation (e.g. denial of service and replay attacks). As a framework,[1] ISAKMP typically utilizes IKE for key exchange, although other methods have been implemented such as Kerberized Internet Negotiation of Keys. A Preliminary SA is formed using this protocol; later a fresh keying is done.

ISAKMP defines procedures and packet formats to establish, negotiate, modify and delete Security Associations. SAs contain all the information required for execution of various network security services, such as the IP layer services (such as header authentication and payload encapsulation), transport or application layer services or self-protection of negotiation traffic. ISAKMP defines payloads for exchanging key generation and authentication data. These formats provide a consistent framework for transferring key and authentication data which is independent of the key generation technique, encryption algorithm and authentication mechanism.

ISAKMP is distinct from key exchange protocols in order to cleanly separate the details of security association management (and key management) from the details of key exchange. There may be many different key exchange protocols, each with different security properties. However, a common framework is required for agreeing to the format of SA attributes and for negotiating, modifying and deleting SAs. ISAKMP serves as this common framework.

ISAKMP can be implemented over any transport protocol. All implementations must include send and receive capability for ISAKMP using UDP on port 500.


### From [ComptTIA Security+]({{<relref "securityplus.md#" >}}) {#from-compttia-security-plus--securityplus-dot-md}

Internet Security Association and Key Management Protocol (ISAKMP), an element of Internet Key Exchange (IKE), is used to organize and manage the encryption keys that have been generated and exchanged by Oakley and SKEME. A security association is the agreed-on method of authentication and encryption used by two entities (a bit like a digital keyring). ISAKMPs use of security associations is what enables IPSec to support multiple simultaneous VPNs from each host. Encapsulating Security Payload (ESP) provides confidentiality and integrity of packet contents. ESP provides encryption, limited authentication, and prevents replay attacks. Secure Key Exchange MEchanism (SKEME), an element of Internet Key Exchange (IKE), is a means to ex-change keys securely. Authentication Header (AH) provides assurances of message integrity and nonrepudiation. AH also provides authentication, access control, and prevents replay attacks.