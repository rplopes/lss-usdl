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

For those familiarized with the service blueprint, it is also possible to further describe an interaction, based on its area of action. This means that an interaction in LSS-USDL can be classified as customer interaction, onstage interaction, backstage interaction and support interaction.

Resources may also be further described according to their nature. A resource in LSS-USDL can be classified as a physical resource (such as a package), a knowledge resource (such as a customer's profile) or a financial resource (such as a payment).

These classifications are depicted in the images directory of the project.

### Ontology Elements

A graph of the full ontology may be viewed in the images directory of the project. This subsection explains its elements and their relations.

`ServiceSystem` is the entity that represents the service system that is being modeled. A `ServiceSystem` is connected to `Interaction`, which represents service interactions, through the property `hasInteraction`.

`Interaction` is connected to the following elements:

 - `Role`, which describes the role of the actor interacting with the service, through the property `isPerformedBy`
 - `Process`, which describes the process of the interaction, through the property `belongsToProcess`
 - `Goal`, which describes the goal behind the service interaction, through the property `hasGoal`
 - `Location`, which describes the interaction's location, through the property `hasLocation`
 - `Time`, which holds the interaction's temporal data, through the property `hasTime`
 - `Resource`, which describes the resources interacting with the service system at that moment, through the properties `receivesResource` (if it's an input from outside), `createsResource` (if it was created internally), `consumesResource` (if it was consumed internally) and `returnsResource` (if it's an output to outside)

These core elements can also be connected to other elements of the Linked Data Cloud, in order to provide richer information and better disambiguations.

A `Role` can also be connected to a `BusinessEntity` of the ontology GoodRelations through the property `hasBusinessEntity`. This makes it possible to explain which company or other stakeholder is responsible for that role.

A `Process` can be connected to a `Process` of the BPMN 2.0 ontology through the property `hasBPMN`. This enables linking the service model with the previously defined process model.

A `Location` can have a broader location through the property `isLocatedIn`. This enables a hierarchy that lest us express knowledge such as "Room A is located in Building 1, therefore an interaction happening in Room A is also happening in Building 1". A `Location` can also be connected to a `Feature` of the Geonames ontology through the property `isLoationFrom` to assign it an unambiguous geographical meaning.

`Time` is connected to a `TemporalEntity` of the OWL-Time ontology through the property `hasTemporalEnity`. This enables a very rich temporal description, such as the duration of the itneraction, its date or its temporal relation to other interactions.

A `Resource` is connected to a `QuantitativeValue` of the ontology GoodRelations through the property `hasQuantitativeValue` to assign it values such as quantities, units, etc. It is also connected to a DBpedia `Resource` through the property `hasDBpediaResource` to get an unambiguous semantic meaning.