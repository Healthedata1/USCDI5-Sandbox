**Example Usage Scenarios:**

The following are example usage scenarios for this profile:

- Query for the existence and location of any advance directive documents the patient may have.
- [Record or update] observations to indicate whether a patient has an advance directive document.

### Mandatory and Must Support Data Elements

The following data elements must always be present ([Mandatory] definition) or must be supported if the data is present in the sending system ([Must Support] definition). They are presented below in a simple human-readable explanation. Profile specific guidance and examples are provided as well. The [Formal Views] below provides the formal summary, definitions, and terminology requirements.

**Each Observation Must Have:**

1. a status
2. a code for presence of advance directives
3. a patient
  
**Each Observation Must Support:**

1. references to ADI documents if they exist*
2. a category code of "advance-directive-observation"
3. when this verified observation was made available
4. one or more performers of the Observation*
5. a “yes/no/unknown” value confirming or refuting the code

*See guidance below

**Profile Specific Implementation Guidance:**


- \* The [Supporting info Extension](https://hl7.org/fhir/extensions/StructureDefinition-workflow-supportingInfo.html) references US Core DocumentReference Profiles which support the exchange of clinical notes including ADI documents.
   - The type of ADI document, period that a document is clinically effective (in force) and other actors (e.g. Author, Custodian) is communicated in the referenced DocumentReference or the document itself.
- \* An `Observation.performer` of type Practitioner or Organization typically made the Observation, and an `Observation.performer` of Patient or RelatedPerson usually is the source of information (for example, a next of kin who answers questions about the patient's advance directives) Systems may use the standard [Performer function Extension ](http://hl7.org/fhir/StructureDefinition/event-performerFunction) to distinguish the type of involvement of the performer in the Observation.

{% include link-list.md %}
