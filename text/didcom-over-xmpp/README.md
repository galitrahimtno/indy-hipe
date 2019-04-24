# DIDCom over XMPP
- Name: didcom-over-xmpp
- Author: Oskar van Deventer (oskar.vandeventer@tno.nl)
- Start Date: 2019-04-23
- PR: 
- Jira Issue: 

## Summary
[summary]: #summary

The DIDCom-over-XMPP feature provides an architecture to exchange DIDCom Connection Protocol messages over the XMPP chat protocol. It enables:

- Initiation, use, maintenance and termination of a trusted electronic relationship
- Binding of that relationship to a human-to-human communication channel 

## Motivation
[motivation]: #motivation

When a consumer contacts a business, then often identity proof is required during the conversation. For example, when a patient calls a health insurer, then the insurer can share any public information freely, like the insurance coverage of medical supplies for its various insurance products. However, the caller may next ask about its specific case, "does my current insurance cover a wheelchair". Before sharing such privacy-sensitive information with the caller, it needs to identify and authenticate the caller, and verify the caller's authorization to receive that information.

This identication and authentication is currently cumbersome. On the phone, the health insurer may ask for patient number, date-of-birth, maiden-name-of-mother, and more. Such checks take valuable call-center time and they are error-prone. Moreover, they are insufficient for some transactions, like the patient's commitment to pay for the other half of the wheelchair. In the latter case, the caller needs to contact the health insurer via another, more secure channel and explain the whole story again to a different person.

The problem is quickly getting worse, given the explosion of communication channels through which patients are contacting their health insurer. Phone, voice chat, text chat, video calls, Facebook chat, WhatsApp and Instagram are just some of the channels through which patient contact health insurers. Also all kinds of types and brands of personal health monitors are getting supported, each of which sends data to the health insurer that needs to be reliabily linked to the patient, and demonstrable authorization from the patient is needed before the health data can be collected, stored and used. Moreover, patients will be requisting information from the health insurer via virtual assistents like Amazon Alexa, Google Assistent, Apple Siri.

![Many_communication_channels.jpg](Many_communication_channels.jpg)

The above problem space applies to any sector with intensive consumer-to-business communication, including health services, energy service, telecommunication services, television services, internet services, tax office services, parcel services, emergency services, etcetera.

The [DIDCom connection protocol](https://github.com/hyperledger/indy-hipe/blob/b3f5c388/text/connection-protocol/README.md) enables the setting up and maintenance of a trusted electronic relationship, i.e. a pairwise DID relationship. The [XMPP protocol](https://en.wikipedia.org/wiki/XMPP) is a popular protocol for messaging. Their combination is expected to be beneficial here.

The DIDCom-over-XMPP feature supports the following use cases.

- Use case 1: A new pairwise DID relationship is initiated during an electronic human-to-human communication.
- Use case 2a: An existing pairwise DID relationship is used during an electronic human-to-human communication to authenticate it.
- Use case 2b: An existing pairwise DID relationship is used to switch to another an electronic human-to-human communication without losing the call history.

## Tutorial
[tutorial]: #tutorial

Explain the proposal as if it were already implemented and you
were teaching it to another Indy contributor or Indy consumer. That generally
means:

- Introducing new named concepts.
- Explaining the feature largely in terms of examples.
- Explaining how Indy contributors and/or consumers should *think* about the
feature, and how it should impact the way they use the ecosystem.
- If applicable, provide sample error messages, deprecation warnings, or
migration guidance.

Some enhancement proposals may be more aimed at contributors (e.g. for
consensus internals); others may be more aimed at consumers.

## Reference
[reference]: #reference

Provide guidance for implementers, procedures to inform testing,
interface definitions, formal function prototypes, error codes,
diagrams, and other technical details that might be looked up.
Strive to guarantee that:

- Interactions with other features are clear.
- Implementation trajectory is well defined.
- Corner cases are dissected by example.

## Drawbacks
[drawbacks]: #drawbacks

Why should we *not* do this?

## Rationale and alternatives
[alternatives]: #alternatives

- Why is this design the best in the space of possible designs?
- What other designs have been considered and what is the rationale for not
choosing them?
- What is the impact of not doing this?

## Prior art
[prior-art]: #prior-art

Discuss prior art, both the good and the bad, in relation to this proposal.
A few examples of what this can include are:

- Does this feature exist in other SSI ecosystems and what experience have
their community had?
- For other teams: What lessons can we learn from other attempts?
- Papers: Are there any published papers or great posts that discuss this?
If you have some relevant papers to refer to, this can serve as a more detailed
theoretical background.

This section is intended to encourage you as an author to think about the
lessons from other implementers, provide readers of your proposal with a
fuller picture. If there is no prior art, that is fine - your ideas are
interesting to us whether they are brand new or if they are an adaptation
from other communities.

Note that while precedent set by other communities is some motivation, it
does not on its own motivate an enhancement proposal here. Please also take
into consideration that Indy sometimes intentionally diverges from common
identity features.

## Unresolved questions
[unresolved]: #unresolved-questions

- What parts of the design do you expect to resolve through the
enhancement proposal process before this gets merged?
- What parts of the design do you expect to resolve through the
implementation of this feature before stabilization?
- What related issues do you consider out of scope for this 
proposal that could be addressed in the future independently of the
solution that comes out of this doc?