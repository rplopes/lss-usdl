LSS-USDL
=========

Linked Service Systems for USDL (LSS-USDL) is an ontology for modeling service systems in RDF. This brings many advantages to organizations that make use of this ontology:

 - The resulting service models may be used as documentation for the service operations or to generate service descriptions for various stakeholders
 - A freely available and complete service description presents a solid evidence of an organization's effort towards transparency
 - After modeling a complete service system it is possible to identify previously unknown bottlenecks and fail points and to study how to overcome them
 - After all operations of a service system are identified it is possible to execute automation tasks that can greatly reduce costs
 - It is also possible to run simulations based on the service model, which aid managerial and operational decisions
 - Because this ontology uses Semantic Web tools and integrates with the Linked Data Cloud, a strong data integration is also ensured
 - Service models may create custom service descriptions aimed at customers that could be used in a generic online services marketplace

## Model Explanation

### 6-Point Interaction Star

A service system can be expressed as the flow of its interactions. Service interactions take place when any actor interacts with the service system. We use the journalism interrogative pronouns (who, how, why, where, when, what) to give a better context to service interactions.

The elements that represent each of those answers are, respectively, the role of the actor that is interacting, the process that describes how the service works, the goal behind why such an interaction is taking place, the location, the time and the resources that enter or leave the service system during that interaction.

This contextualization of service interactions is the core of LSS-USDL. It is called 6-point interaction star and can be viewed in the images directory of the project.

### Extending Interactions and Resources

For those familiarized with the service blueprint, it is also possible to further describe an interaction based on its area of action. This means that an interaction in LSS-USDL can be classified as customer interaction, onstage interaction, backstage interaction and support interaction.

Resources may also be further described according to their nature. A resource in LSS-USDL can be classified as a physical resource (such as a package), a knowledge resource (such as a customer's profile) or a financial resource (such as a payment).

These classifications are depicted in the images directory of the project. Note, however, that their use is not mandatory, and because they are defined as SKOS concept schemes, you can replace them with your own.

### Ontology Elements

A graph of the full ontology may be viewed in the images directory of the project. This subsection explains its elements and their relations.

`ServiceSystem` is the entity that represents the service system that is being modeled. A `ServiceSystem` is connected to an `Interaction`, which represents service interactions, through the property `hasInteraction`.

`Interaction` is connected to the following elements:

 - `Role`, which describes the role of the actor interacting with the service, through the property `isPerformedBy`
 - `Process`, which describes the process of the interaction, through the property `belongsToProcess`
 - `Goal`, which describes the goal behind the service interaction, through the property `hasGoal`
 - `Location`, which describes the interaction's location, through the property `hasLocation`
 - `Time`, which holds the interaction's temporal data, through the property `hasTime`
 - `Resource`, which describes the resources interacting with the service system at that moment, through the properties `receivesResource` (if it's an input from outside), `createsResource` (if it was created internally), `consumesResource` (if it was consumed internally) and `returnsResource` (if it's an output to outside)

These core elements can also be connected to other elements of the Linked Data Cloud, in order to provide richer information and better disambiguations.

A `Role` can also be connected to a `BusinessEntity` of the ontology [GoodRelations](http://www.heppnetz.de/ontologies/goodrelations/v1) through the property `hasBusinessEntity`. This makes it possible to explain which company or other stakeholder is responsible for that role.

A `Process` can be connected to a `Process` of the [BPMN 2.0](http://www.scch.at/en/Page56-8330.aspx) ontology through the property `hasBPMN`. This enables linking the service model with the previously defined process model.

A `Location` can have a broader location through the property `isLocatedIn`. This enables a hierarchy that lest us express knowledge such as "Room A is located in Building 1, therefore an interaction happening in Room A is also happening in Building 1". A `Location` can also be connected to a `Feature` of the [Geonames](http://www.geonames.org/ontology/documentation.html) ontology through the property `isLoationFrom` to assign it an unambiguous geographical meaning.

`Time` is connected to a `TemporalEntity` of the [OWL-Time](http://www.w3.org/TR/owl-time) ontology through the property `hasTemporalEnity`. This enables a very rich temporal description, such as the duration of the interaction, its date or its temporal relation to other interactions.

A `Resource` is connected to a `QuantitativeValue` of the ontology [GoodRelations](http://www.heppnetz.de/ontologies/goodrelations/v1) through the property `hasQuantitativeValue` to assign it values such as quantities, units, etc. It is also connected to a [DBpedia](http://dbpedia.org) `Resource` through the property `hasDBpediaResource` to get an unambiguous semantic meaning.

## Getting Started Tutorial

A service system modeled in LSS-USDL is represented by RDF statements. We will use the Turtle notation because it's cleaner and easy to read and edit.

The first step is to create a file that will hold the service model. For an express mail delivery service system we may create the file maildelivery.ttl. These are the RDF prefixes we need for this tutorial (you may add others and remove any that you might not use):

```
@prefix : <http://genssiz.org/lss-usdl/expressmail#> . # this is the prefix for our example, change the URL to match yours

@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
@prefix lss-usdl: <http://genssiz.dei.uc.pt/lss-usdl#> .
```

The first element to add is the service system. We can use RDF properties to give any element a label and a comment:

```
# Add the service system
:ExpressMailDelivery a lss-usdl:ServiceSystem;
  rdfs:label "Express Mail Delivery";
  rdfs:comment "A service system for delivering express mails".
```

Note that, if we were using the RDF/XML notation, the same data would look like the following:

```
<lss-usdl:ServiceSystem rdf:about="#ExpressMailDelivery">
  <rdfs:label>Express Mail Delivery</rdfs:label>
  <rdfs:comment>A service system for delivering express mails</rdfs:comment>
</lss-usdl:ServiceSystem>
```

Now we can start adding interactions. Every time we add an interaction, it should be added to the service system's list of interactions:

```
# Change the service system
:ExpressMailDelivery a lss-usdl:ServiceSystem;
  rdfs:label "Express Mail Delivery";
  rdfs:comment "A service system for delivering express mails";
  lss-usdl:hasInteraction :CustomerCalls .

# Add the interaction
:CustomerCalls a lss-usdl:CustomerInteraction;
  rdfs:label "Customer calls" .
```

That interaction still has no information, so the next step is to provide more useful context data. For new entities we need to create them and make the interaction point to them:

```
# Change the interaction
:CustomerCalls a lss-usdl:CustomerInteraction;
  rdfs:label "Customer calls";
  lss-usdl:hasGoal :SendMail;
  lss-usdl:isPerformedBy :Sender;
  lss-usdl:hasLocation :SenderHome .

# Add the goal
:SendMail a lss-usdl:Goal;
  rdfs:label "Send mail" .

# Add the role
:Sender a lss-usdl:Role;
  rdfs:label "Sender" .

# Add the location
:SenderHome a lss-usdl:Location;
  rdfs:label "Sender's home" .
```

For entities that have already been created we only need to point at them. So if we create a new interaction performed by an actor with the role "sender" we only need to point to it:

```
# Change the service system
:ExpressMailDelivery a lss-usdl:ServiceSystem;
  rdfs:label "Express Mail Delivery";
  rdfs:comment "A service system for delivering express mails";
  lss-usdl:hasInteraction :CustomerCalls,
    :CustomerDeliversPackages .

# Add the interaction
:CustomerDeliversPackages a lss-usdl:CustomerInteraction;
  rdfs:label "Customer delivers packages";
  lss-usdl:isPerformedBy :Sender .
```

The code we have so far should now look like this:

```
@prefix : <http://genssiz.org/lss-usdl/expressmail#> .

@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
@prefix lss-usdl: <http://genssiz.dei.uc.pt/lss-usdl#> .

:ExpressMailDelivery a lss-usdl:ServiceSystem;
  rdfs:label "Express Mail Delivery";
  rdfs:comment "A service system for delivering express mails";
  lss-usdl:hasInteraction :CustomerCalls,
    :CustomerDeliversPackages .

:CustomerCalls a lss-usdl:CustomerInteraction;
  rdfs:label "Customer calls";
  lss-usdl:hasGoal :SendMail;
  lss-usdl:isPerformedBy :Sender;
  lss-usdl:hasLocation :SenderHome .

:CustomerDeliversPackages a lss-usdl:CustomerInteraction;
  rdfs:label "Customer delivers packages";
  lss-usdl:isPerformedBy :Sender .

:SendMail a lss-usdl:Goal;
  rdfs:label "Send mail" .

:Sender a lss-usdl:Role;
  rdfs:label "Sender" .

:SenderHome a lss-usdl:Location;
  rdfs:label "Sender's home" .
```

This was just a short getting start guide to explain how to use the LSS-USDL ontology to model service systems. The full code of this express mail delivery example is available in the use cases directory of the project, among with other examples to help you understand how to use this ontology.

## Useful links

 - [USDL Incubator Group](http://www.w3.org/2005/Incubator/usdl): LSS-USDL is part of the research for service systems by the USDL research group.
 - [Linked USDL](http://www.linked-usdl.org): Similar project, focusing on service descriptions for customers. The third use case found in LSS-USDL's repository shows a service system modeled both in LSS-USDL and Linked USDL.
 - [Linked USDL core](https://github.com/linked-usdl/usdl-core): Repository for the core module of Linked USDL. The other modules may be found under the same Github profile.
 - [Semantic Web](http://semanticweb.org/wiki/Main_Page): Technologies such as RDF are a core component of LSS-USDL.