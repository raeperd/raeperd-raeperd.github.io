# DAO vs Repository

Category: java
Created: January 29, 2021 6:46 PM
Created By: RaeCheol Park
Last Edited Time: January 29, 2021 6:46 PM
Status: DONE
URL: https://www.baeldung.com/java-dao-vs-repository

Often, the implementations of repository and DAO are considered interchangeable, especially in data-centric apps. This creates confusion about their differences.

## DAO pattern

- DAO Pattern, is an abstraction of data persistence and is considered closer to the underlying storage, which is often table-centric.

## Repository pattern

- As per Eric Evans' book Domain-Driven Design, the **“repository is a mechanism for encapsulating storage, retrieval, and search behavior, which emulates a collection of objects.”**
- Likewise, according to Patterns of Enterprise Application Architecture, **it “mediates between the domain and data mapping layers using a collection-like interface for accessing domain objects.”**

So far, we can say that the implementations of DAO and repository look very similar. However, DAO seems a perfect candidate to access the data, and a repository is an ideal way to implement a business use-case.

## Difference between DAO and Repository

- DAO is an abstraction of data persistence. However, a repository is an abstraction of a collection of objects
- **DAO is a lower-level concept, closer to the storage systems. However, Repository is a higher-level concept, closer to the Domain objects**
- DAO works as a data mapping/access layer, hiding ugly queries. However, a repository is a layer between domains and data access layers, hiding the complexity of collating data and preparing a domain object
- DAO can't be implemented using a repository. However, a repository can use a DAO for accessing underlying storage

Also, if we have an anemic domain, the repository will be just a DAO.

Additionally, **the repository pattern encourages a domain-driven design, providing an easy understanding of the data structure for non-technical team members, too**.

[DAO vs Repository Patterns | Baeldung](https://www.baeldung.com/java-dao-vs-repository)

[The DAO Pattern in Java | Baeldung](https://www.baeldung.com/java-dao-pattern)

[DAO와 REPOSITORY 논쟁](http://egloos.zum.com/aeternum/v/1160846)