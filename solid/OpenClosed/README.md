# O - Open Closed Principle (OCP)

**Open-closed Principle (OCP) states:**

**Software entities (classes, modules, functions, and so on) should be open for extension, but
closed for modification**

So what this principle wants to say is: We should be able to add new functionality without touching the existing code for the class. This is because whenever we modify the existing code, we are taking the risk of creating potential bugs.

Consider the following Event class and renderNotification method from a Calendar application:

```javascript
class Event {
  renderNotification() {
    return `You have an event occurring in
    ${this.calcMinutesUntil()} minutes!`;
  }
  // ...
}
```

We may wish to have a separate type of event that renders a notification prefixed with the word Urgent! to ensure that the user pays more attention to it. The simplest way to achieve this adaptation is via inheritance of the Event class, as follows:

```javascript
class ImportantEvent extends Event {
  renderNotification() {
    return `Urgent! ${super.renderNotification()}`;
  }
}
```

Here, via inheritance, we have achieved extension, adapting the Event class to our needs. Inheritance is only one way that extension can be achieved. There are various other approaches that we could take. One possibility is that, in the original implementation of Event, we foresee the need for custom notification strings and implement a way to configure a **renderCustomNotifcation** function.

```javascript
class Event {
  renderNotification() {
    const defaultNotification = `
    You have an event occurring in
    ${this.calcMinutesUntil()} minutes!
    `;
    return this.config.renderCustomNotification
      ? this.config.renderCustomNotification(defaultNotification)
      : defaultNotification;
  }
  // ...
}
```

This code presumes that there is a config object available. his is crucially different from the inheritance approach in that the Event class itself is prescribing the itself is prescribing the possibilities possibilities extension that exist. Providing adaptability via configuration means that users don't need to worry about the internal implementation knowledge required when extending classes.

```javascript
new Event({
  title: "Doctor Appointment",
  config: {
    renderCustomNotification: (defaultNotification) => {
      return `Urgent! ${defaultNotifcation}`;
    },
  },
});
```

This approach requires that your implementation can foresee its most likely adaptations and that those adaptations are predictably internalized into the abstraction itself. There is a balance to strike here: adaptability is a good thing, but we also must balance it
with a focused and cohesive abstraction that has a constrained purpose.
