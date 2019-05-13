# DIDCom over XMPP
- Name: didcom-over-xmpp
- Author: Oskar van Deventer (oskar.vandeventer@tno.nl), Galit Rahim (galit.rahim@tno.nl), Alexander Blom (alexander.blom@bloqzone.com)
- Start Date: 2019-04-23
- PR: 
- Jira Issue: 

## Summary
[summary]: #summary

Firewalls are a major issue for peer-to-peer DID relationships. The DIDCom-over-XMPP feature provides an architecture to exchange DIDCom connection protocol messages over the XMPP chat protocol, bypassing any firewall issues.

DIDCom-over-XMPP enables, unburdened of any firwall issues:

- Initiation, use, maintenance and termination of a trusted electronic relationship
- DIDCom agents being available for incoming DIDCom messages
- Binding of that relationship to a human-to-human communication channel 

## Motivation
[motivation]: #motivation

Firewalls are a major issue for peer-to-peer DID relationships. All examples of service endpoint in the W3C DID specification use HTTP. This assumes that the endpoint is running an HTTP server and firewalls have been opened to pass this traffic. This assumption typically fails for smartphone DIDCom agents, as well as private internet connections of consumers and small busineses. As a consequence, such DIDCom agents are unavailable for incoming DIDCom messages, whereas several use cases require this. The following are examples of this.

When a consumer contacts a business, then often identity proof is required during the conversation. For example, when a patient calls a health insurer, then the insurer can share any public information freely, like the insurance coverage of medical supplies for its various insurance products. However, the caller may next ask about its specific case, "does my current insurance cover a wheelchair". Before sharing such privacy-sensitive information with the caller, it needs to identify and authenticate the caller, and verify the caller's authorization to receive that information. This identication and authentication is currently cumbersome. On the phone, the health insurer may ask for patient number, date-of-birth, maiden-name-of-mother, and more. Such checks take valuable call-center time and they are error-prone. Moreover, they are insufficient for some transactions, like the patient's commitment to pay for the other half of the wheelchair. In the latter case, the caller needs to contact the health insurer via another, more secure channel and explain the whole story again to a different person. The problem is quickly getting worse, given the explosion of communication channels through which patients are contacting their health insurer. Phone, voice chat, text chat, video calls, Facebook chat, WhatsApp and Instagram are just some of the channels through which patient contact health insurers. Also all kinds of types and brands of personal health monitors are getting supported, each of which sends data to the health insurer that needs to be reliabily linked to the patient, and demonstrable authorization from the patient is needed before the health data can be collected, stored and used. Moreover, patients will be requisting information from the health insurer via virtual assistents like Amazon Alexa, Google Assistent, Apple Siri.

![Communication_channels.jpg](Communication_channels.jpg)

The above problem space applies to any sector with intensive consumer-to-business communication, including health services, energy service, telecommunication services, television services, internet services, tax office services, parcel services, emergency services, etcetera.

In the ideal case, the insurer just sends a DIDCom message to the caller to link the current human-to-human communication to the DIDCom agent-to-agent communication, bypassing any firewall issues. The combination of DIDCom and XMPP achieves this.

- The [DIDCom connection protocol](https://github.com/hyperledger/indy-hipe/tree/master/text/0031-connection-protocol) enables the setting up and maintenance of a trusted electronic relationship.
- The [XMPP protocol](https://en.wikipedia.org/wiki/XMPP) is a popular protocol for chat and messaging. It has a client-server structure that bypasses any firewall issues.

The DIDCom-over-XMPP feature supports the following use cases.

- Use case 1: A new trusted electronic relationship is initiated during an electronic human-to-human communication.
- Use case 2a: An existing trusted electronic relationship is used during an electronic human-to-human communication to authenticate it.
- Use case 2b: An existing trusted electronic relationship is used to switch to another an electronic human-to-human communication without losing the call history.

## Tutorial
[tutorial]: #tutorial

The DIDCom-over-XMPP feature provides an architecture for the transport of DIDCom messages over an XMPP network, using XMPP to bypass any firewalls at the receiving side.

### DIDCom

The DIDCom connection protocol is specified in [Hyperledger Indy Hipe 0031](https://github.com/hyperledger/indy-hipe/tree/master/text/0031-connection-protocol). The purpose of the protocol is to set up a trusted electronic relationship between two parties (natural person, legal person, ...). Technically, the trust relationship involves the following

- Univocal identification of the parties within the context of the relationship
- Secure exchange of keys to encrypt and verify messages between agents of the parties
- Secure exchange of service end points to be reachable at in the future

W3C specifies [Data Model and Syntaxes for Decentralized Identifiers (DIDs)](https://w3c-ccg.github.io/did-spec/). This specification introduces Decentralized Identifiers, DIDs, for identification. A DID can be resolved into a DID Document that contains the associated keys and service endpoints, see also W3C's [A Primer for Decentralized Identifiers](https://w3c-ccg.github.io/did-primer/). W3C provides a [DID Method Registry](https://w3c-ccg.github.io/did-method-registry/) for a complete list of all known DID Method specifications. Many of the DID methods use an unambiguous source of truth to resolve a DID Document, e.g. a well governed public blockchain. An exception is the [Peer DID method](https://dhh1128.github.io/peer-did-method-spec/index.html) that relies on the peers, i.e. parties in the trusted electronic relationship to maintain the DID Document.

The DIDCom connection protocol has several steps to create a trusted electronic relationship.
- Step 0: Invitation to Connect, an out-of-band (open) invitation to connect from the *invitor*
- Step 1: Connection Request, an encrypted request from the *invitee* to the *invitor* containg a pairwise DID and the associated DID Document
- Step 2: Connected Response, an encrypted response from the *invitor* to the *invitee* containg a pairwise DID and the associated DID Document
- Step 3: Trust building, any further verification steps and exchange of verifiable credentials to enhance the trust in the newly created electronic relationship. 

The result is a trusted electronic relationship with a DID pair and associated DID Documents, ready for use in transactions. The DIDCom connecton protocol also includes steps to read (get current DID Document from the other), update (e.g. key rotation) and delete.

### XMPP

Extensible Messaging and Presence Protocol (XMPP) is a communication protocol for message-oriented middleware based on XML (Extensible Markup Language). It enables the near-real-time exchange of structured yet extensible data between any two or more network entities. Designed to be extensible, the protocol has been used also for publish-subscribe systems, signalling for VoIP, video, file transfer, gaming, the Internet of Things applications such as the smart grid, and social networking services.

Unlike most instant messaging protocols, XMPP is defined in an open standard and uses an open systems approach of development and application, by which anyone may implement an XMPP service and interoperate with other organizations' implementations. Because XMPP is an open protocol, implementations can be developed using any software license and many server, client, and library implementations are distributed as free and open-source software. Numerous freeware and commercial software implementations also exist.

XMPP uses 3 types of messages:

| Message Type  | Description                               |
| ------------- |:-------------:                            |
| PRESENSE      | Inform listeners that agent is online     |
| MESSAGE       | Sending message to other agent            |
| IQ MESSAGE    | Asking for response from other agent      |


![XMPP_architecture.jpg](XMPP_architecture.jpg)

### DIDCom over XMPP

For use of XMPP, it is recommended to use [Openfire Server](https://www.igniterealtime.org/projects/openfire/) open source project, including 2 plugins to enable server caching and message carbon copy. This will enable sending DIDcom to mulitple endpoints of the same person.

*Editor's note: add proper references to the 2 plugins*

#### Use of MESSAGE

The DIDCom JSON text is send as plain text as XMPP MESSAGE, without any additional identifiers.

#### Service endpoint

The service end point of a DIDCom-over-XMPP service is derived from the XMPP address by preceding the domain part of the XMPP address with "did." and remove the resources part, i.e. the "/" and anything behind it. The reason for removing the resources part is that DIDCom messages are addressed to the person/entity associated with the DID, and not to any particular device. 

Here are some examples of this convention.

'''
- xmpp:alice@foo.com
- xmpp:alice@foo.com/phone
--> xmpp:alice@did.foo.com

- xmpp:bob@bar.com
- xmpp:bob@bar.com/laptop
- xmpp:bob@bar.com/phone
 --> xmpp:bob@did.bar.com
'''

Here is an example of the "service" property of a DID Document with an XMPP service endpoint. Note that there is no resource (like "/laptop") indicated. In normal XMPP a message can only be send to one resource, even if there are multiple resources online (for example /phone and /tablet). If a message is send to a XMPP address without using an specific resource, the server chooses which resource gets the message. The is usually based on importance of the resources (which the user can configure). However the routing logic of the XMPP server can be changed to follow different rules.

```
{
  "service": [{
    "id": "did:example:123456789abcdefghi;xmpp",
    "type": "XmppService",
    "serviceEndpoint": "xmpp:bob@did.bar.com"
  }]
}
```

XMPP servers handle messages sent to a user@host (or "bare") JID with no resource by delivering that message only to the resource with the highest priority for the target user. Some server implementations, however, have chosen to send these messages to all of the online resources for the target user. If the target user is online with multiple resources when the original message is sent, a conversation ensues on one of the user's devices; if the user subsequently switches devices, parts of the conversation may end up on the alternate device, causing the user to be confused, misled, or annoyed.

To solve this the plugin "Message Carbons" will be used. It will ensure that all of target user devices get both sides of all conversations in order to avoid user confusion. As a pleasant side-effect, information about the current state of a conversation is shared between all of a user's clients that implement this protocol.

#### ABNF

Here is a formal [ABNF](ftp://ftp.rfc-editor.org/in-notes/std/std68.txt) description of the XMPP service endpoint syntax.

```
xmpp-service-endpoint = "xmpp:" userpart "@did." domainpart
  userpart = 1\*CHAR
  domainpart = 1\*CHAR 1\*("." 1\*char)
  CHAR = %x01-7F
```

### Use cases

Here are three use cases where DIDCom over XMPP is used.

#### Use case 1: A new trusted electronic relationship is initiated during an electronic human-to-human communication

Patient Alice (*xmpp:alice@foo.com/phone*) is having an XMPP chat session with Bob (*xmpp:bob@bar.com/laptop*), an employee of the local hospital. At some point in time, Bob proposes to Alice to establish a trusted electronic communication channel for future use. Bob uses the popular marketing term for "DIDCom", which Alice recognizes and accepts. Next Bob sends Alice a DIDCom Invitation to Connect via the chat channel. After Alice has confirmed that she understands the instructions, they disconnect the chat session.

Following the instructions, Alice's agent creates a new XMPP service end point *xmpp:alice@did.foo.com*. It sets up an XMPP chat session to the received address of Bob's agent, *xmpp:bob@did.bar.com*. The two agents perform the remainder of the DIDCom connection protocol to create a pairwise-DID-based trusted electronic communication channel.

#### Use case 2a: An existing trusted electronic relationship is used during an electronic human-to-human communication to authenticate it

Patient Alice is on the phone with Bob, an employee of the local hospital. At some point in time, Bob recogmizes that he needs to authenticate Alice. Bob sees that he already has a trusted electronic communication channel with Alice. Bob asks Alice to repeat a six-digit code that she is about to receive via the trusated communication channel. Next, Bob instructs his agent with XMPP service end point *xmpp:bob@did.bar.com*, to send a six-digit code to the XMPP service end point of Alice's agent, *xmpp:alice@did.foo.com*. Alice's agent receives the 6-digit code and presents it to Alice. Alice reads it back to Bob, upon which Bob can share the privacy-sensitive information with Alice.

#### Use case 2b: An existing trusted electronic relationship is used to switch to another an electronic human-to-human communication without losing the call history

Patient Alice has a Facebook chat with Bob, an employee of the local hospital. At some point in time, Bob recognizes that the information that he needs to share requires Alice to use a different, more secure communication channel. Bob provides Alice with a reference, which Alice uses in the new more secure communication channel.

*Editor's note: The use of DIDCom for use case 2b needs to be clarified.*

## Reference
[reference]: #reference

*Editor's note: add references*

## Drawbacks
[drawbacks]: #drawbacks

So far, no drawbacks have been identified.

## Rationale and alternatives
[alternatives]: #alternatives

All service endpoint examples from W3C's [Data Model and Syntaxes for Decentralized Identifiers (DIDs)](https://w3c-ccg.github.io/did-spec/) are HTTP. So if a consumer would want to be reachable for incoming DIDCom messages, then it should run an HTTP service on its consumer device and take actions to open firewalls (and handle network-address translations) towards its device. Such scenario is technically completely unrealistic, not to mention the security implications of such scenario.

XMPP was specifically designed for icoming messages to consumer devices. XMPP's client-server structure overcomes any firewall issues.

## Prior art
[prior-art]: #prior-art

*Editor's note: add prior art*

## Unresolved questions
[unresolved]: #unresolved-questions

*Editor's note: add unresolved questions*

## Security considerations
[security]: #security-considerations

*Editor's note: add security considerations*