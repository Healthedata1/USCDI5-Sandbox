**Example Usage Scenarios:**

The following are example usage scenarios for this profile:

-  Query for the existence of any advance directive documents the patient may have.
- [Record or update] observations to indicate a patient's advance directive documents

### Mandatory and Must Support Data Elements

The following data elements must always be present ([Mandatory] definition) or must be supported if the data is present in the sending system ([Must Support] definition). They are presented below in a simple human-readable explanation. Profile specific guidance and examples are provided as well. The [Formal Views] below provides the formal summary, definitions, and terminology requirements.

**Each Observation Must Have:**

1. a status
2. a fixed code to indicate if advance directive documents exist
3. a patient
  
**Each Observation Must Support:**

1. a category code of "advance-directive-observation" *☞ ☞ ☞ See Question about category in notes ☜ ☜ ☜*
2. reference to the documents this observation is about
3. a time indicating when the observation was made
<!-- 3. who reported the preference -->
4. advance directive observation value*

*See guidance below

**Profile Specific Implementation Guidance:**

- *The advance directive observation value is a code from the [US Core ADI Finding](*) ValueSet representing the specific type of documents present (for example, physician order for life sustaining treatment (MOLST or POLST)). <!-- or a finding of no ADI documents? -->

{% include link-list.md %}
