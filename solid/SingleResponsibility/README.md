# S - Single Responsibility Principle (SRP)

**Single-responsibility Principle (SRP) states:**

- _A class should have one and only one reason to change, meaning that a class should have only one job._

  Responsibility, in this context, refers to the purpose and area of concern that abstraction covers. A function that validates phone numbers can be said to have a singular responsibility. A function that both validates and normalizes those numbers with their country codes, however, can be said to have two responsibilities. The SRP would tell us that we need to split that abstraction into two separate ones. This will be explored using example of small calendar application:

  **Bad Approach:**

  ```javascript
  class Calendar {
    addEvent(event) {}
    removeEvent(event) {}
    getEventsBetween(stateDate, endDate) {}
    setTimeOfEvent(event, startTime, endTime) {}
    setTitleOfEvent(event, title) {}
    exportFilteredEventsToXML(filter) {}
    exportFilteredEventsToJSON(filter) {}
  }
  ```

  In this example the caldendar class contain a lot of methods that are technically related to the calendar functionality. Howerver, the addition of all of these methods make class too complex with too many reasons to change like:

  - The way time is defined on events may need to change
  - The way titles are defined on events may need to change
  - The way events are searched for may need to change
  - The XML schema may need to change
  - The JSON schema may need to change

  So it makes sense to slip the code in more appropriate abstractions.

  **Good Approach:**

```javascript
class Event {
  setTime(startTime, endTime) {}
  setTitle(title) {}
}

class Calendar {
  addEvent(event) {}
  removeEvent(event) {}
  getEventsBetween(stateDate, endDate) {}
}

class CalendarExporter {
  exportFilteredEventsToXML(filter) {}
  exportFilteredEventsToJSON(filter) {}
}
```

Here each abstraction encapsulates its own responsibilitie.
