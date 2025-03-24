---
id: Software Architechture and Design
aliases:
  - Software Architechture and Design
tags: []
---

# Software Architecture and Design

Software Architecture and Design of Modern Large Scale systems

## Learning Goals
- Software Architecture of large scale systems, capable of handling millions of requests/day
- Design highly scalable, highly available and performant software systems
- Apply industry proven software architectural patterns, building blocks and best practices
- Define the system's technical requirements, high level components and API
- Gain confidence for an upcoming System Design Interview

## Introduction

### Software Architecture Motivation
#### In the real world
- Everything we build has a structure.
- The more we progress with building something, the more difficult it becomes to change the structure.
- This structure defines
    - The intent of the product
    - The quality of the product

#### In software
- We can organize our code in various ways
- Each way gives us different properties
- Architecture will impact
    - Performance
    - Ease of adding new features
    - Response to failures
- Reorganization or redesign will take up significant time/money

### Software Architecture Definition
Software Architecture of a system is the high level design of the system structure, it's components and how they communicate with each other to fulfill the requirements and constraints.
It provides a **high level overview** and *abstracts away the implementation* details.
> [!hint]
> - Implementation decisions should be made at the very end of the design process. This will lead to the best decisions as the requirements and constraints are already thoroughly thought through by then.
> - The various components may have their own design depending on complexity and perhaps some communication protocol
> - The architecture describes how the system
>   - *does what it is supposed to do* (components meet the user requirements)
>   - *prevents what is not supposed to happen* (meet the constraints)

When talking about architecture, we can talk on various levels of abstraction
Some of these layers include
- Classes/Structures
- Modules/Libraries
- Services/Processes (talking about such services running on potentially different hosts/computers make the discussion relate to distributed systems) **This is the focus**

Considering all these points allows us to *design scalable systems* that *store large amounts of data* and *serve a large number of users*

### Software Development Cycle
[[Software architecture contribution to SDLC 2025-01-02 12.27.11.excalidraw]]

### Challenges
We **cannot** prove that an architecture is
- Correct
- Optimal
because the architecture is not of mathematical nature

We **can** follow some practices to guarantee success like
- Adopt a methodical design process
- Apply tried and tested architectural patterns
- Follow some best practices ( mentioned below )

## System requirements or architectural drivers
### Intro to topic
#### Challenges in gathering correct requirements
1. Abstraction
   Designing a few methods or classes is easier than designing an entire system. Most of the time, a user will be talking about the overall product(which abstracts away the details) when talking about requirements
2. Ambiguity
   User requirements often come is ambiguous terms. Sometimes, even a user might not have all requirements clearly stated unless we ask deeper questions

> [!tip]
> It is our job as engineers to translate these *abstracted ideas* and *ambiguous needs* into meaningful technical requirements

#### Risks of getting requirements wrong at the start
Wrong requirements may lead to
- A product which does not sufficiently meet the user expectations - which may need a redesign which is costly
- Waste of engineering hours
- Bad reputation

#### Types of requirements
##### Features of the system
  Functional requirements
- These describe the behavior of the system and what the system must do
- They are the main objective of the system we are designing
- They are called functional requirements because they describe our system as a *black box function* of user input or external events
- They describe the functioning of our system but do not directly determine it's architecture
- Usually any architecture(good or bad) can help realize a set of features, which is what makes the architecture design difficult

###### Examples
Take a hitchhiking app as a case study:
One feature of this system might be described as follows
- when the user logs in to the app, display a map of the area with nearby drivers marked on the map
  here, user login is the input and the map to be displayed is the output
Another feature of this system might be described as
- when ride is completed, charge the user with the bill amount depending upon whatever payment option is setup and pay the driver his charges after adjusting service charges.
  Here, the ride end event is the input and the payment of the bill is the input

##### Quality Attributes
  Non functional requirements
- These are the qualities/properties that the system must have
  Some examples are
  - Scalability
  - Fault tolerance
  - Error handling
  - Reliability and so on...
- Quality Attributes do determine the architecture of system.
  Another way to look at it is the architecture defines the attributes of our system
- Different architectures provide us with different set of quality attributes

##### System Constraints
  Limitations and boundaries
These can be constraints pertaining to
  - system behavior
    like the system should not charge users multiple times, or it should not ignore failed payments
  - financial issue 
    like limited budget of the company
  - manpower related
    like limited availability of engineers
Whatever the case, such constraints affect the architectural decisions

#### Section quiz
We received the following requirement from the client:

"We would like you to build a system that allows sharing of large files between users.
After a user uploads a file, they will get a unique link that they can share with other users. Any user with that link can download the file.
The link should become active no later than 1 second after the file is uploaded. Download speeds should be at least `50 Mbit/sec`.
You have to support at least `PDF` and `JPG` file formats, as well as the following web browsers: Google Chrome, Mozilla Firefox, and Microsoft Edge."

### Features requirements - step by step process
Some simple methods
Ask the user for every detail, hope they don't forget details.

But there are better methods like
1. Discuss use cases
   Use case is a scenario in an environment in which the system will be used
2. Talk about user flow
   This is nothing but a more detailed graphical representation of one or more use cases

#### Formal method for gathering functional requirements
1. Identify all actors/users in the system
2. Describe all possible use cases/scenarios
3. Expand the cases into a flowchart based on the flow of events
   For each event, outline the trigger *action* and make note of the *data* that should flow along with the event

##### Example
hitchhiking continued...

#### Example sequence diagram
- [ ] to do (review last section of [[#Features requirements - step by step process]])

### System quality attributes requirements

#### Motivation
Some motivation as to why it is important to get the quality attributes right in the initial design
Most of the time, systems are frequently redesigned
**NOT** because of change in functionality is desired,
but because the current system **IS** not
- scalable enough
- reliable enough
- available enough
- maintainable enough
and so on...
In fact, even after a major redesign due to such reasons, the resulting system often ends up providing the same functionality as before
So we want to get these qualities right during our design to save time further down the line

#### Definition
Quality Attributes are non functional requirements
They describe the qualities of the actual functional requirements and the overall properties of the system

- They provide a quality measure on how well our system performs on a given dimension
- They are directly correlated to our system architecture
> [!tip]
> These quality attributes may not be only related to the functional requirements, instead they pertain to the quality requirements of all stakeholders involved. See examples below.

##### Examples
###### Functional quality attributes
###### Development quality attributes

#### Quality Attributes considerations

1. Quality Attributes of our system need to be
    - measurable
    - testable

> [!hint]
> If we cannot measure and test for these attributes, i.e. We cannot prove that our system meets the required attributes, we cannot know if our system performs *well* or *poorly*

2. Trade offs
We need to trade off certain attributes in favor of others, as there is no single architecture which satisfies all attributes.
> This is due to the reason that some attributes contradict another attribute(s)

3. Feasible
We need to make sure that the quality attributes we are considering while designing the system are feasible i.e. They are realistic ( technically speaking ).
Some times, if a certain client is non technical, they may end up asking for qualities that are impossible to achieve and it is **our job to call out such requirements early** on in the design.

> [!tip]
> When approving such requirements we may need to consult a domain expert to evaluate what are the unfeasible requirements if any.

### System constraints
#### Introduction
System constraint is a decision which was already made(fully or partially) for us, which restricts our freedom in implementing our design
> These constraints usually show up when evaluating the previous steps, i.e. The functionality and features of the system

However this is not bad because once system constraints are in place, they provide us with solid foundation/starting point to build/design around

#### Types
1. Technical constraints
2. Business constraints
3. Legal/regulatory constraints

#### Considerations

## Most Important Quality Attributes in Large Scale Systems
A system can have a long list of possible quality attributes. However not all attributes are of equal important for every system.
Here we present some of the most important attributes usually required in large scale systems

### Performance

#### Response time
Response time is the time taken by the system to respond to a user request. It could consist of 2 parts ->
1. Processing time
2. Waiting time - time spent waiting in the queue, or transit time over the network and so on
    Sometimes this is also referred to as Latency.
> [!hint]
> Also, the combination of these two times is often referred to as the **Round Trip Time** or **End to End Latency**

####  Throughput
Throughput is the amount of work done by the system in a given time period. This can be measured in terms of requests/tasks handled per second, or data processed per second and so on.
More throughput indicates better performance in such cases.

#### Important Considerations
Below are some important considerations to keep in mind while analyzing/measuring quality attributes and setting goals around them.

1. Measuring Response time Correctly
    Failing to measure response time correctly can lead to wrong conclusions about the system's performance. In such case, our performance graphs may show that the system is performing well, but in reality, the users may experience our system to be slow.
2. Response time distribution
    - If we have various servers, which handle requests from various users, and we create a distribution of response times across these servers, ideally we want these samples to have the same response times. However, in reality, this is not the case. We may find that some responses took very short time to process, while others took a very long time to process. Keeping this in mind, it is important to decide what metric should we measure and set our goals around.
    - To answer this, we need to learn how to organize and analyze these samples of data. One way is to sort the response time samples and plot a histogram, which shows frequency of response times. From this we can understand the Xth percentile of response times, which is the time taken for X% of the requests to be processed.
    - In this percentile graph, the 50th percentile in the median, which is the time taken for 50% of the requests to be processed. And the **samples at the end of the graph are the outliers, which are the requests that took the longest time to process**. This is the tail latency of the distribution. This tail latency needs to be as short as possible.
    - So when we set our goals for response time, we need to define them using percentiles. Like we may say that 95% of the requests should be processed in less than 100ms.
3. Performance degradation
    - This consideration takes into account both response time and throughput. And has to do with identifying the performance degradation point of our system as the load increases.
    - We need to identify how steep or fast this degradation is after said point. Which may indicate that some of our resources are at their limit. These resources could be:
        - CPU
        - Memory
        - Network bandwidth
        - Disk I/O
        - Software Queue

### Scalability
#### Motivation and definition
Load on our system is never the same at all times. It can vary depending on our users, time of day and many such factors.
In the [[#Important Considerations]] section, we mentioned performance degradation as the load increases. To get around this, we need to design our system to be scalable.
There are 3 ways to scale a system, we will discuss one by one below.

#### Vertical Scalability - Scale up
Adding resources to a single server to increase it's capacity is called as Vertical Scalability. This could be done by adding more CPU, memory, disk space and so on.
#### Horizontal Scalability - Scale out
Adding more servers to our system to increase it's capacity is called as Horizontal Scalability. This could be done by adding more servers to our server farm, or by adding more servers to our data center.
#### Team/Organizational Scalability - Scale the team

##### Comparison between vertical and horizontal scalability

### Availability - Introduction and Measurement
### Fault Tolerance and High Availability
### SLA, SLO, SLI
These are the 3 terms which essentially define/aggregate the quality of service provided by a system. They are used to define the quality of service in a contract between a service provider and a customer.
#### SLA - Service Level Agreement

- This is the contract between the service provider and the customer. It defines the quality(i.e. the promises we made when we considered the [[#Most Important Quality Attributes in Large Scale Systems]]) of service that the customer can expect from the service provider.
- It is a legal document and is usually signed by both parties.
- It also states penalties if the service provider fails to meet the promises made in the SLA. The penalties may include:
    - Full/Partial Refunds
    - Trial extension
    - Future credits

> [!hint]
> Usually if service is provided for free, the provider may not publish an SLA.

#### SLO - Service Level Objectives
- These are the goals that the service provider sets for themselves to meet the promises made in the SLA. As an example we could say that our system has:
    - Availability SLO of 99.9% (3 nine's)
    - Response time SLO of 100ms at 95th percentile
    - Issue resolution SLO of 12 hours

> [!hint]
> Even if a provider does not have an SLA, they may still have SLOs to allow themselves and their users to measure their systems performance.

#### SLI - Service Level Indicators
- These are the metrics that we use to measure the SLOs. These are the actual data points that we collect from our system to measure the performance of our system.
- They can be later compared to our SLOs to see if we are meeting our goals or not.

#### Important considerations
- We should not take every SLI as an SLO. We should only take those SLIs as SLOs which are important to our users.
- Fewer SLOs are better than more SLOs. This is because more SLOs can lead to more complexity in our system and can lead to more chances of failure.
- We should set realistic goals which leave some room for error.
- We should have a recovery plan in place in case the SLI's show that we are not meeting our SLO's.

> [!warning]
> Our **quality attributes need to be testable and measurable**. If we cannot measure and test for these attributes, we cannot know if our system performs well or poorly and none of this matters!

### Real World Examples from the Industry
#### Cloud vendors SLA examples
[AWS SLA's](https://aws.amazon.com/legal/service-level-agreements/?aws-sla-cards.sort-by=item.additionalFields.serviceNameLower&aws-sla-cards.sort-order=asc&awsf.tech-category-filter=*all)
[GCP SLA's](https://cloud.google.com/terms/sla)
[Microsoft Azure SLA's](https://azure.microsoft.com/en-us/support/legal/sla/)
#### Other examples
[Github enterprise SLA](https://github.com/customer-terms/github-online-services-sla)
[Atlassian products SLA](https://support.atlassian.com/subscriptions-and-billing/docs/service-level-agreement-for-atlassian-cloud-products/)

## API Design
### Introduction
#### Introduction to API design
After we have established the funcitonal requirements of our system, we can go ahead and think of our system as a black box, which has 2 things:
- Certain behavior
- A certain interface
    This **interface is a contract** between *engineers who design the system and clients who use the system*
    Because this interface is used by other applications to interact with our system, it is called an API(Application Programming Interface)
> [!tip]
> To clarify, as we are building an entire large scale system, and not a class or a library meant to be used at a smaller scale, we are talking about a **System API** here which will be called by other systems remotely over the network.

The applications calling our api could be
- client applications either mobile or web based
- other services in our org or external systems belonging to other orgs
After we have planned our internal design and broken it down into components, each such component of the system may have its own API called a `component API`. These APIs will be used by other components of the system to interact with each other.

#### API categories
1. Public APIs
    - these are exposed to the general public
    - any dev can use these APIs to interact with our system
    - it is considered good practice to have any user of such public APIs to be registered and authenticated with us so that -
        - we have control over who is using our system
        - we have control over how user is using our system
        - security reasons
        - we can blacklist users who are misusing our system
2. Private/Internal APIs
    - these are exposed internally within our org
    - they allow other teams to interact with our system and provide services to their clients and value to the org without directly exposing our system to the public
3. Partner APIs
    - they are similar to public APIs but are exposed to a select group of partners

#### Benefits of well designed API
#### API best practices and patterns

### RPC
### REST API
