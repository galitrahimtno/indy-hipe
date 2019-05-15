# DIDCom over XMPP
- Name: didcom-over-xmpp
- Authors: Oskar van Deventer (oskar.vandeventer@tno.nl), Galit Rahim (galit.rahim@tno.nl), Alexander Blom (alexander.blom@bloqzone.com)
- Start Date: 2019-04-23
- PR: 
- Jira Issue: 

## Summary
[summary]: #summary

While DIDCom leaves its users free to choose any underlying communication protocol, for peer-to-peer DID relationships with one or both parties behind a firewall actually getting the messages to the other party is not straightforward.  

Fortunately this is a classical problem, encountered by all realtime communication protocols, and it is therefore natural to use one of these protocols to deal with the obstacles posed by firewalls. The DIDCom-over-XMPP feature provides an architecture to exchange DIDCom connection protocol messages over XMPP, using XMPP to solve any firewall issues.

DIDCom-over-XMPP enables:

- Initiation, use, maintenance and termination of a trusted electronic relationship
- DIDCom agents being available for incoming DIDCom messages despite being behind a firewall
- Binding of that relationship to a human-to-human communication channel

and all of this in spite of the presence of firewalls.

## Motivation
[motivation]: #motivation

Currently, all examples of service endpoint in the W3C DID specification use HTTP. This assumes that the endpoint is running an HTTP server and firewalls have been opened to allow this traffic to pass through. This assumption typically fails for DIDCom agents behind LAN firewalls or using cellular networks. As a consequence, such DIDCom agents can be expected to be unavailable for incoming DIDCom messages, whereas several use cases require this. The following is an example of this.

A consumer contacts a customer service agent of his health insurance company, and is subsequently asked for proof of identity before getting answers to his personal health related questions. DIDcom could be of use here, replacing privacy sensitive and time consuming questions in order to establish the consumers' identity with an exchange of verifiable credentials using DIDcom.  In that case, the agent would just send a DIDCom message to the caller to link the ongoing human-to-human communication session to a DIDCom agent-to-agent communication session. The DIDCom connection protocol would then enable the setting up and maintenance of a trusted electronic relationship, to be used to exchange verifiable credentials. Replace insurance company with any sizeable business to consumer company and one realizes that this use case is far from insignificant.

Unfortunately, by itself, the parties DIDcom agents will be unable to bypass the firewalls involved and exchange DIDcom messages. Therefore XMPP is called to the rescue to serve as the transport protocol which is capable with firewalls. Once the firewalls issue is solved, DIDcom can be put to use in all of these cases.  

- The XMPP protocol is a popular protocol for chat and messaging. It has a client-server structure that bypasses any firewall issues.

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

```
- xmpp:alice@foo.com
- xmpp:alice@foo.com/phone
--> xmpp:alice@did.foo.com

- xmpp:bob@bar.com
- xmpp:bob@bar.com/laptop
- xmpp:bob@bar.com/phone
 --> xmpp:bob@did.bar.com
```

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

## Reference
[reference]: #reference

*Editor's note: add references*

## Drawbacks
[drawbacks]: #drawbacks

Having a human-readable userpart may be a strong correlation point, especially if the userpart is identical to one used for human-to-human XMPP chat. Two privacy-enhancing approaches have been suggested.

- Use the DID in the userpart. The XMPP URL is not meant to human-readable anyway. Potential issue: mixes addressing between layers.
- Provide large numbers of one-time-use service endpoints in the DID Document. Potential issue: adds complexity and dependencies.

*Editor's note: further analysis is needed*

## Rationale and alternatives
[alternatives]: #alternatives

All service endpoint examples from W3C's [Data Model and Syntaxes for Decentralized Identifiers (DIDs)](https://w3c-ccg.github.io/did-spec/) are HTTP. So if a consumer would want to be reachable for incoming DIDCom messages, then it should run an HTTP service on its consumer device and take actions to open firewalls (and handle network-address translations) towards its device. Such scenario is technically completely unrealistic, not to mention the security implications of such scenario.

XMPP was specifically designed for icoming messages to consumer devices. XMPP's client-server structure overcomes any firewall issues.

## Prior art
[prior-art]: #prior-art

*Editor's note: add prior art*

## Unresolved questions
[unresolved]: #unresolved-questions

See "Drawbacks" above

## Security considerations
[security]: #security-considerations

*Editor's note: add security considerations*
