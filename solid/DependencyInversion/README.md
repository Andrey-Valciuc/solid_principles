# D - Dependency Inversion Principle (DIP)

**Dependency inversion principle states:**

- _High-level modules should not depend on low-level modules. Both should
  depend on abstractions (that is, interfaces)_
- _Abstractions should not depend on details. Details (such as concrete
  implementations) should depend on abstractions_

Our abstractions should be separated (decoupled) in such a way that we can easily change low-level implementation details at a later date without having to refactor all of our code.

Considering our Calendar application once again, let's say that we wanted to implement a way to retrieve events happening within a specific radius of a fixed location. We may choose to implement it like so:

```javascript
class Calendar {
  getEventsAtLocation(targetLocation, kilometerRadius) {
    const geocoder = new GeoCoder();
    const distanceCalc = new DistanceCalculator();
    return this.events.filter((event) => {
      const eventLocation = event.location.address
        ? geocoder.geocode(event.location.address)
        : event.location.coords;
      return (
        distanceCalc.haversineFormulaDistance(eventLocation, targetLocation) <=
        kilometerRadius / 1000
      );
    });
  }
  // ...
}
```

The _Calendar_ class is a high-level abstraction, concerned with the broad concepts of a calendar and its events. The getEventsAtLocation method, however, contains a lot of location-related details that are more of a low-level concern. The Calendar class here is concerns itself with which formula to utilize on the DistanceCalculator and the units of measurement used in the calculation.

All of these details should not be the business of a high-level abstraction, such as _Calendar_ . The dependency inversion principle asks us to consider how we can abstract
away these concerns to an intermediary abstraction that acts as a bridge between high-level and low-level. One way in which we may accomplish this is via a new class, _EventLocationCalculator_:

```javascript
const distanceCalculator = new DistanceCalculator();
const geocoder = new GeoCoder();
const METRES_IN_KM = 1000;
class EventLocationCalculator {
  constructor(event) {
    this.event = event;
  }
  getCoords() {
    return this.event.location.address
      ? geocoder.geocode(this.event.location.address)
      : this.event.location.coords;
  }
  calculateDistanceInKilometers(targetLocation) {
    return (
      distanceCalculator.haversineFormulaDistance(
        this.getCoords(),
        targetLocation
      ) / METRES_IN_KM
    );
  }
}
```

This class could then be utilized by the Event class in its own _isEventWithinRadiusOf_ method. An example of this is shown in the following code:

```javascript
class Event {
  constructor() {
    // ...
    this.locationCalculator = new EventLocationCalculator();
  }
  isEventWithinRadiusOf(targetLocation, kilometerRadius) {
    return (
      locationCalculator.calculateDistanceInKilometers(targetLocation) <=
      kilometerRadius
    );
  }
  // ...
}
```

Therefore, all the Calendar class needs to concern itself with is the fact that Event instances have _isEventWithinRadiusOf_ methods. It needs no information and makes no prescriptions as to the specific implementation that determines distances; the details of that
are left to our lower-level _EventLocationCalculator_ class:

```javascript
class Calendar {
  getEventsAtLocation(targetLocation, kilometerRadius) {
    return this.events.filter((event) => {
      return event.isEventWithinRadiusOf(targetLocation, kilometerRadius);
    });
  }
  // ...
}
```

**Dependency graph:**

![alt text](image.png)
