Models
======

First of all, let's clarify one important thing.

Despite that you have seen model classes in many popular frameworks, there are no models in MVC. Those frameworks call classes that extend ActiveRecord implementation as "models".

Krystal is different. It respects SOLID, follows SoC and avoids ActiveRecord.

# What is a model?

A model itself a concept of data/logic abstraction. When it comes to web-applications, it consist of these parts:

- Data Mapper

It provides a set of methods that abstract table access.

- Domain object/layer

It provides an object that performs calculation or processes business rules. A domain object is optional. For example, if your form has an ability to upload images putting a watermarks on them, then a class that does image processing would be domain object.

- Service

A service is just a brigde between domain objects (if you have them) and data mappers. When instantiating a service class, its constructor usually accepts a data mapper and a domain object (if present).

All service objects must be registered within `getServiceProviders()` in module definition class. There's no a generalized answer on creating models (services). It's always up to you to decide how to build that.