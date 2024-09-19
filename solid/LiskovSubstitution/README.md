# L - Liskov Substitution Principle (LSP)

**Liskov Substitution Principle states:**

**Types should be able to be replaced by their subtypes without altering the reliability of the program.**

This means that every subclass or derived class should be substitutable for their base or parent class. To make the Liskov substitution principle a little more concrete, let's dive back into Calendar application example:

```javascript
class ImportantEvent extends Event {
  renderNotification() {
    return `Urgent! ${super.renderNotification()}`;
  }
}
```

The assumption implicit in our doing this is that our Calendar class and its implementation will not be concerned with whether events are Events or subclasses of Events. We expect that it will treat them the same. The Calendar class may have a notifyUpcomingEvents method, for example, that iterates through all upcoming events and calls renderNotification on each event:

```javascript
class Calendar {
  getEventsWithinMinutes(minutes) {
    return this.events.filter((event) => {
      return event.startsWithinMinutes(minutes);
    });
  }
  notifiyUpcomingEvents() {
    this.getEventsWithinMinutes(10).forEach((event) => {
      this.sendNotification(event.renderNotification());
    });
  }
  // ...
}
```

What's crucial here is that the Calendar implementation makes no deliberations as to the type of Event instance that it is dealing with.
