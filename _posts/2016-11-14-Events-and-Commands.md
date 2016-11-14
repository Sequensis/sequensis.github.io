---
layout: post
title:  "Events and Commands"
date:   2016-11-14 13:00:00 +0100
categories: NServiceBus
---

# Commands and Events

## Purpose

To provide clear guidelines on the usage of events and commands, and the Sequensis approach to this. 

*Note that this is a living document and should continue to evolve and change in line with improvements to bets practice within Sequensis*

### This article will:
- Help new starters get up to speed with events and commands at Sequensis
- Clarify what we mean by events and commands
- Provide a point of reference for PR's
- Provide a point of reference for new service implementations

### This article will not
- Address message versioning. This will be addressed in a separate document.
- Provide low-level technical implementation details. This is covered by the [NServiceBus Website] and book
- *Cover the topic of context interchange and domains that span multiple components, although it may be updated in the future to do so.*

## Definition of events and commands
Refer to the [NServiceBus Website on events and commands] for a more detailed guide on what events and commands are, and how to implement them. Some key features are that:

 - **Events**: Published in one place, handled in many; Can be subscribed to in a pub/sub pattern.
 - **Commands**: Sent from many places, handled in only one place;  Cannot be subscribed to.

## Guidelines

### Preference for Events
In general we should prefer using events over commands, although there are specific use cases where commands should be used. The reasons for this are as follows:

- **Flexibility**: Events give more flexibility and extensibility, as they can be subscribed to make other services in future use cases. Commands are much stricter and only satisfy a single use case.
- **Domain Centric Language**:  Events encourage a richer, more domain driven language, describing things that have happened / are happening. Commands tend to be more asking specific components to do specific things.
- **Coupling**: Whilst using a service bus does technically decouple our implementations,  commands tend to drive a more tightly coupled architecture, as service depend on one another more. Events encourage more loosely coupled architecture as components have less knowledge of one another. 

### Event Guidelines
The following is some high level guidelines on the usage of events:

- **Domain Naming Representation**: Thought needs to be given to the naming of events, to clearly explain what has happened in the domain. For example 
	- It is ok to raise an event called ApplicantUnsubscribed before the action has taken place in the database as it represents that the applicant has indicated they wish to unsubscribe. This is an event representing something that has happened in the domain, even if the wider system has not yet processed it.
	- It is not ok to raise an event saying EmailSent before an email has actually been sent, as this is not representing what has actually happened in the domain.
- **Context Boundary**: The namespace portion of the event is actually part of the event name that implies context. For example:
	- A TopUpLoanService.ApplicationCreated event is when an application is created in the context of the top-up loan service. In this case the event is ApplicationCreated and the context boundary is the TopUpLoanService.
- **Naming Specificity**: A preference for specific messages vs general message with discriminator, for example:
	- A general message with discriminator might be ApplicationCreated with a property of ApplicationType (PersonalLoan, TopUpLoan, HirePurchase etc).
	- Specific messages would be PersonalLoanApplicationCreated, TopUpLoanApplicationCreated etc

### Command Guidelines
Given that commands are limited in flexibility they should always:

- **Atomic Operations**: Be atomic and transactional (i.e. used to wrap low level operations)
- **Single Action**: Wrap a single action. (where action represents something that changes a resource or impacts pricing, whether external or internal)

The following is some guidelines to when it might be appropriate to use commands.

- **Anti-Corruption Layers**: The usage of commands in ACLs is ok as they're designed to act as a bridge between different contexts. For example:
	- The DecisionDataTranslationService is an ACL that translates various domain centric events into commands for the decision engine.
- **Multi-Step Process**: The use of internal commands to break down multi-step in event handlers is ok. For example
	- The HighLevelChecksPassedEventHandler might call the Experian bureau service, but persist the data using an internal StoreExperianBureauResultCommand. This isolates the performance of the request, from any failures in the saving of the data.
- **Isolate Resource**: The use of commands to isolate an specific resource is also ok, for example:
	- A command such as SendEmail to isolate the email sending functionality.
- **Co-ordinating Event Handler**:  The use of internal commands with a co-ordinating event handler as opposed to multiple event handlers is preferred. In this case there would be a single event handler that would raise several internal commands, and therefore just act as a co-ordinator. For example:
	- The ExperianBureauService handles the HighLevelChecksPassed event and raises commands to perform the ExperianBureauSearch and ExperianAuthenticateSearch.

### Other Considerations

- **Idempotency**: All handlers should be idempotent, where through the NServiceBus outbox, the idempotency manager or other means.
- **Retry policies**: Consideration should be given to retry policies and what exceptions are thrown within handlers. For example, a timeout exception might be ok as a retry might fix the issue, whereas an argument null exception probably isn't as retrying wouldn't fix the null argument.
- **Poison message handling**: *Need some input here!!*
- **Raise an event from a handler**: Raising a data rich event at the end of a handler to broadcast what has been done is generally good practice, but not a requirement.
- **Raising multiple events from a handler**: Raising multiple events from a single handler is generally considered a code smell, as it would indicate that the handler is completing more than one action.

## Use your judgement
The final thing to note is that these are guidelines and should not be followed dogmatically. Use your common sense and depart from the guidelines if necessary - as with all guidelines, should you do so it would make sense to make note of the departure and why in the project (either near the code or in the readme).

[NServiceBus Website]:https://docs.particular.net/nservicebus/
[NServiceBus Website on events and commands]: https://docs.particular.net/nservicebus/messaging/messages-events-commands






