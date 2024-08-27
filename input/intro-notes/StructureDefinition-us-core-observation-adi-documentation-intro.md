**Example Usage Scenarios:**

The following are example usage scenarios for this profile:

- Query for the existence of any advance directive documents the patient may have.
- [Record or update] observations to indicate a patient's advance directive documents

### Mandatory and Must Support Data Elements

The following data elements must always be present ([Mandatory] definition) or must be supported if the data is present in the sending system ([Must Support] definition). They are presented below in a simple human-readable explanation. Profile specific guidance and examples are provided as well. The [Formal Views] below provides the formal summary, definitions, and terminology requirements.

**Each Observation Must Have:**

1. a status
2. a fixed code to indicate if advance directive documents exist
3. a patient
  
**Each Observation Must Support:**

1. a category code of "advance-directive-observation" *☞ ☞ ☞ See Question about category in notes ☜ ☜ ☜*
2. reference to the ADI documents this observation is about
3. *☞ ☞ ☞ when this verified observation was made available  ☜ ☜ ☜*
4. *☞ ☞ ☞ who reviewed and verified the observation ☜ ☜ ☜*
5. advance directive observation value*

*See guidance below

**Profile Specific Implementation Guidance:**

- *The advance directive observation value is a code from the [Advance Healthcare Directive Categories Grouper](https://vsac.nlm.nih.gov/valueset/2.16.840.1.113762.1.4.1115.25/expansion/Latest)(*) ValueSet representing the specific type of documents present (for example, physician order for life sustaining treatment (MOLST or POLST)). It also contains a "No advance directive" concept to address when no ADI documents exist for a patient.
- The period that a document is clinically effective (in force) and other actors (e.g. Author, Custodian) is communicated in the referenced DocumentReference or the document.

{% include link-list.md %}
