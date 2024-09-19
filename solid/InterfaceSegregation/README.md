# I - Interface Segregation Principle (ISP)

**The interface segregation principle states:**

- _A client should never be forced to implement an interface that it doesn’t use, or clients shouldn’t be forced to depend on methods they do not use._

This principle is similar in spirit to the principle of single responsibility: its goal is to ensure that you create focused and highly-cohesive abstractions that are only concerned with a single area of responsibility. But instead of making you consider the concept of
responsibility itself, it makes you look at the interfaces that you're creating and consider whether they're appropriately segregated.

Consider a local government office. They have a paper form (let's call it Form 1A) that it uses to change a person's details. It is a form that's existed for over 50 years. Via this form, a local citizen can change a number of their details, including, but not limited to, the
following:

- A change of name
- A change of marital status
- A change of address
- A change of council tax discount status (student/elderly)
- A change of disability status

As you can imagine, it's a very complex and dense form:

![alt text](image.png)

Form 1A provides a set of interfaces to the various change functions that are provided by the local government office. The interface segregation principle asks us to consider whether this form is forcing its clients to depend on methods that they don't use.

Form 1A does not follow the interface segregation principle very well. If we were to redesign it, we would likely separate out the individual
functions it serves into their own independent forms:

![alt text](image-1.png)

We can then choose to only include in each form what is needed to fulfill its function.
