---
title: "Mendix & Integration"
parent: "integration-overview"
menu_order: 1
draft: true
---

Mendix is optimized for the easiest and fastest way to develop and maintain Apps
that fulfil a business function. Internal integration from GUI to Logic to Data
within a Mendix App is handled out of the box, in one Model and one deployable.

But most solutions also require integration with other Apps and systems.
Integration is easy in Mendix, but there are many options to choose from, as
should be the case, because integration is like the glue between components in a
solution and needs to be adaptable to all possible functional scenarios.

As more Core systems are being built using Mendix Microservices, the integration
part is increasingly important. This document discusses different types of
integration and when one way to solve the problem may be better than another.

Mendix makes it easy to build, update and maintain an App/Microservice that
fulfils a business function. Usually a business functions need a GUI, some logic
and data. The internal integration of those layers is handled by Mendix, but for
most Apps, and in Microservice architectures, there is need for integration with
other systems.

![](attachments/mendix-integration/feature-requirements.png)

So the Mendix developer does not have to worry about internal App layer’s
integration, but the solution architect or dev-lead needs to design good
Microservices and good interfaces to integrate seamlessly with other Apps and
systems in the enterprise.

Mendix handles a large array of formats and protocols, see [Mendix 7 How To’s -
integration](https://docs.mendix.com/howto/integration/)*.*

The problem is often to choose the right option amongst a lot of possibilities.
This document will give an overview of Integration Methods and typical Use
cases. A specific document will be created for major use-cases.

Always think Functional First
-----------------------------

The first recommendation is to have an open mind to the integration requirement.
Think about what the integration need really is and consider more than one
option for the solution.

Remember that an interface starts inside one system and ends inside one or more
other systems, and there could be integration components in the middle, so the
number of options to solve something is quite large.

Consider: What triggers the interface? Who needs what data when? Who should
handle functional errors occurring? Who should handle technical errors? How can
I minimize integration? How do I build a UX that requires very few service calls
to load?

Then consider which use case it may be, which options are available, and make a
conscious choice on why one method is chosen over another.

Use Case Categories
-------------------

Integration means any kind of interaction between any person, thing, App or
system for any type of purpose. The scope is enormous. There are hundreds of use
cases. Scoping out a new project there is a fair chance of finding a new
use-case type even after years working on integration. So, there is reason to
have eyes open for what is similar and what is different from cases we already
know.

Still some Use Cases are very typical and Mendix is working on creating an
example implementation, and Best Practices related to the most common ones:

![](attachments/mendix-integration/common-use-cases.png)

In each of these use-cases we consider different Solution Methods and which
option is more suitable than others under which condition.

Basic Solution Categories
-------------------------

For most integration related to Mendix, there are 5 solution categories, that
are almost always used. Sometimes one, sometimes a combination:

![](attachments/mendix-integration/solution-categories.png)

1.  UI Integration, means e.g. using a DeepLink from the UI of one App, opening
    the UI of another App, either in the same Tab or another Tab. It also
    entails CMS integration with e.g. Akamai and other content management
    systems

2.  Service Based Integration, or RPC style integration, uses request-reply and
    is almost always synchronous

3.  Event Driven Integration, usually does not have a response, and it used to
    distribute data large scale, large distance or simply in a decoupled way

4.  Batch Oriented includes, exporting, moving and importing files

5.  Central Data is a pattern where data is landed and combined in a central
    place before it is distributed, e.g. this could be an ODS, ETL, BI or Data
    lake solution.

Overview of Options
-------------------

Plotting the Functional Use Cases against the basic methods of integration,
there are several common options available. That is good, because integration is
the flexible part of any solution, adapting to how systems, things or people
communicate. Turn to the Use-Case documents (link docs) for more detail. In the
table below:

-   Capital “X” indicates common or preferred use of the method

-   Lower case “x” indicates possible use in some cases

-   In some of the latter cases, e.g. IoT, the solution will require several
    methods and then there is also several capital “X”

| *Use Case*                                 | *UI Integration* | *RPC / Services* | *Events / Queues* | *Export, Import, Batch* | *Central Data* |
|--------------------------------------------|------------------|------------------|-------------------|-------------------------|----------------|
| 1. SSO, AD and Identity integration        | x                | X                |                   |                         |                |
| 2. Import and Distribute Reference Data    |                  | X                | x                 | X                       | x              |
| 3. View and Search Data in another system  | x                | X                |                   |                         |                |
| 4. Use and refer to Data in another system |                  | X                |                   |                         | x              |
| 5. Process integration - Continue workflow | X                | x                | x                 |                         |                |
| 6. Export, Import & Batch integration      |                  | x                | x                 | X                       | x              |
| 7. Update Data in the Master App           | X                | X                | x                 |                         |                |
| 8. Distribute Master & Transactional Data  |                  | X                | x                 | x                       | x              |
| 9. Integration with BI and Reporting       |                  | x                | x                 | X                       | x              |
| 10. Mobile integration and Off-line        |                  | X                | x                 |                         |                |
| 11. CMS and CDN Integration                | X                | x                |                   |                         |                |
| 12. Process Orchestration & State Engines  |                  | X                | x                 |                         | X              |
| 13. Integration with Ops and Monitoring    | x                | x                | X                 | x                       | X              |
| 14. Integration with IoT solutions         |                  | X                | X                 | x                       | X              |
| 14. Integration with AI, Machine Learning  |                  | X                | x                 |                         | X              |

Integration Styles
==================

Request Reply most Frequently Used
----------------------------------

Request-Reply is a collaboration style that means that who-ever initiates
integration will expect a response back from the destination. For most standard
interfaces a request-reply scenario is the easiest way to integrate because the
side of the interface that starts the integration knows directly if the call
worked or not.

I.e. Request-reply is more deterministic and therefore easier to think about. If
it times out, it’s possible to try later, if that is relevant. If it there is an
error message, the calling system can react directly: set a flag, start an error
workflow, or show a good error message on the screen that helps the end user
correct the problem immediately.

UI Integration
--------------

Integration via a UI link is becoming more and more common. It allows to develop
a UI only once in the App where it belongs, and link other users there when they
need to perform that process.

In Microservice systems there is often a Dashboard App or portal / landing page
where people sign in via e.g. SSO. It often contains workflow, overviews and
status, while when a user wants to perform real work in an area, he is
deep-linked into another App and works there.

Depending on the business requirements, the second App is opened in the same tab
and the user is un-aware of working in several Apps, or if parallel work in two
areas is preferred, there could be a separate Tab.

UI integration can also have an advantage for mastering data, since the process
and UX validations of information is always done in the same way. Then when the
work is done a relevant part of the new data can be copied back to the other
App.

Event Driven Trend
------------------

At this moment Event driven architectures are making their return into
mainstream of Integration. It follows an increased interest and focus on e.g.
IoT solutions, distributed networks of actors, central monitoring etcetera.
Several solution providers promote new, modern paradigms for managing large,
distributed, high-volume event driven architectures.

The key with event-streams is that they (often) only flow in one direction. A
device leaving metrics to an IoT system, does not expect an immediate answer to
the data it ships. Additionally, there could be very many devices,
geographically distributed and shipping a lot of data. Request-reply is neither
needed not practical for inbound IoT, while for e.g. commanding a drone, or
other device it’s highly recommended.

IoT, AI and Big Data integration is only in the beginning of an expected
explosion of new IT that will be built next to the existing IT landscape, in the
coming years and Mendix will together with Siemens invest heavily in this area,
to be a leader. In that perspective Kafka and other event-based architecture
will play an important role in the coming years.

IoT, AI and BI solutions will probably count as professional systems and require
professional developers, that handle queues or Kafka, large databases etcetera.

Batch Oriented, Export and Import
---------------------------------

Batch oriented integration runs a large set of data at a certain moment.
Interfaces towards DWH and BI are often bulk and/or snap-shot oriented. The same
is true for initial loads of systems, or distribution of reference data.

More and more processes become real-time, but a lot of business processes are
still periodic in nature, e.g. salary payments, interest calculations. These
use-cases are best implemented in batch-oriented interfaces.

Files and DB dumps will stay important in the future. The advantage of batch is
that systems are decoupled, and the export and import can run at different
times. The interface can be “re-run” again, and/or use a workflow handles
errors.

Processing data in bulk is also more CPU efficient, and if it is periodic, it
can usually be done at night, when other load is much lower.

API Management and ESBs
-----------------------

All real-time interfaces can be routed via an ESB or API management. It does not
change very much for the publisher and the consumer except there is a technical
decoupling point and/or a queueing system.

API management or ESBs start making sense when there are \>100 systems and
several business domains that are organizationally separated. It creates a level
of stability for services used across the enterprise

This document will not separate that from the case of direct service
interaction, because functionally it is almost the same thing. Microservice
recommendation is to use a very thin integration layer, or no layer at all.

Many people use direct integration within a system, business domain or area,
while having some API management for external and intra-domain integration.

Integration Layers and Data Hubs
--------------------------------

If the integration layer in the middle stores and combines business data before
re-distributing it to other parties, it is a “central data” integration pattern.

A typical use-case is if a company has 10 business lines with all different
ordering systems, but only one single support desk. It then makes sense to store
all orders in a central point where they can be searched together using SQL
instead of composite service calls.

Maybe this is the Support system itself, but it could also be an Integration App
or ODS (Operational Data Store), serving via services a global search on orders.

Many people use Mendix Apps to create this pattern for a specific area. It is
easy to define the data, the logic and lookups and any human workflow to manage
errors.

ETL systems also work in a similar way, extracting data from one system, storing
the data from last extract, maybe combining with reference data, and then
sending it on.

Data Lakes are like an enterprise wide ODS, that doubles as a BI and DWH system.
It is a big undertaking to make this work, and there is an issue when trying to
combine operational data, and snap-shot type data that is used for statistics.

The statistics part of Data Lakes can feed BI and AI solutions of the future,
but we do not recommend using Data Lakes as Operational Data, because of the
varying time-stamps, and various layers the data goes through. For Operational
Data a relational DB, ODS is preferred, as mentioned above.

Ops Integration and Test Services
---------------------------------

A new trend, part of Microservices and DevOps, is also to build services and
interfaces from live systems that are specifically oriented towards automated
testing and health checks on live systems.

A service that is used to test things in CICD pipelines, may later be re-used to
verify a production deployment, or to check a live system, or to collect user
metrics to a dashboard etcetera.

For professional Ops solutions there is often an agent per node, shipping in
near-real-time, data towards an APM (Application Performance Monitoring) system,
used for root-cause analysis, trend analysis, sizing metrics and alarms

Microservices also often have an Admin page with collected important
information, both technival and functional KPIs that help maintain the solution,
and from a local Ops dashboard or App management module, one can deep-link into
these pages.

Recommendations
===============

Minimize Integration
--------------------

It may seem obvious, but it is still worth pointing out that the overall
Solution Design of 1-N Apps working together, should always attempt at finding
the functional components and interfaces with the least amount of integration
need, and the least complicated integration.

That will make the overall solution easier to build and maintain, and it
simplifies dependencies between Apps. This means that even the decision on which
Microservices/Apps to build should incorporate integration analyses

Dependencies are there – Learn to Work with them
------------------------------------------------

Apps working together are dependent on each other. That is part of the business
process and can not be avoided. Trying to eliminate a functional dependency
between two Apps, by a technical solution, is not recommended, because it
usually creates other functional issues with more complex error handling.

E.g. sending data from App A to App B, we may put them on a queue in between in
an asynchronous event, which seems to eliminate an on-line dependency from a
synchronous request reply. But in fact error handling becomes much more
difficult since neither App is aware of the entire travel path of the event, and
if it goes wrong in the middle nobody is properly notified. A better solution is
often to poll from App B to App A and get all recently updated records. The App
that needs the data is in control of the entire interface, and error handling is
confined to one single place.

I.e. in the case above the development dependency is almost eliminated, but the
run-time situation is less optimal with events than request reply.

Still there are many cases when events make more sense, e.g. real event streams
from e.g. IoT, Logging or metrics, where data should only flow one way and a
message can be lost without breaking the business.

There are also technical reasons to go for events, e.g. very high volume,
distributed infrastructure, poor network connectivity, many-to-many situations.
To guarantee delivery one can make asynchronous request replies or use a state /
process engine monitoring all events in a large supply chain process

Keep it Simple
--------------

Event Driven integration increase drastically in the future, where Linked in is
using Kafka to distribute posts, metrics and user statistics, and Siemens and
the rest of the world is bracing for the era of IoT, when almost all devices
will be connected.

But the event driven trend does not change what is already working well for
normal business Apps. The regular App developer, integrating a few systems for
regular business processes should keep it as simple as possible.

That usually means request-reply using e.g. REST over Http(s). It allows control
of the case of non-delivery of information or events, which for normal business
processes should be managed.

Overall Recommendations
-----------------------

Apps should act as actors in a business process. They typically do different
things, and often they have a different view of the data.

E.g. the customer portal, the sales funnel, the support and the operations will
all have product and customer data, but they will have very different views of
the data. This is good, because they specialize in what is their specific task.
When specialization is local, we allow smaller interfaces, and thereby more
autonomous services.

It is good to endorse some basic recommendations:

1.  Seek the overall solution that minimizes Integration, because integration is
    complexity and it creates dependencies in releases and operations

2.  Think functionally first, do not start from the solution, rather define what
    is really needed, and consider more than one technical solution option

3.  Use explicit contracts that only transfer the data required, in order to
    make dependencies smaller, and shelter Apps from each other’s data-models

4.  Use request-reply when possible, for easier error handling

5.  Do not be afraid of copying data from one App to another, because it
    increases processing speed, and removes on-line dependencies

    1.  Only copy the data that is required, to limit dependencies

6.  Consider consumer specific contracts if a service is not truly generic and
    stable, because they augment autonomy and flexibility in releases

Now continue to look into the specific *Integration Use Cases \<link\>*, and
example provided.
