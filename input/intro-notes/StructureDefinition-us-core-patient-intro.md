
**Example Usage Scenarios:**

The following are example usage scenarios for this profile:

-   Query for Patient demographic information using Medical Record
    Number (MRN), which is a type of identifier. The MRN is identifiable
    by `identifier.system` and may be location specific.
-   Query for Patient demographic information using first name, last
    name, birthdate, and gender.

### Mandatory and Must Support Data Elements


The following data elements must always be present ([Mandatory] definition) or must be supported if the data is present in the sending system ([Must Support] definition). They are presented below in a simple human-readable explanation. Profile specific guidance and examples are provided as well. The [Formal Views] below provides the formal summary, definitions, and terminology requirements.  

**Each Patient Must Have:**

1. a patient identifier (e.g., MRN)
1. a patient name
1. a gender*

**Each Patient Must Support:**

1. a birth date
1. an address*

{% include additional-requirements-intro.md type="Patient" plural="true" %}

1. contact detail (e.g., a telephone number or an email address)
2. a communication language*
3. <span class="bg-success" markdown="1">interpreter required flag*</span><!-- new-content -->
4. a race*
5. an ethnicity*
6. a tribal affiliation
7. sex*
8. <span class="bg-success" markdown="1">sex parameter for clinical use*</span><!-- new-content -->
9. gender identity*
10. <span class="bg-success" markdown="1">personal pronouns</span><!-- new-content -->
11. date of death*
12. previous address*
13. previous name*
14. suffix*

*see guidance below

**Profile Specific Implementation Guidance:**
- Notes for *Race*, *Ethnicity*, *Date of Death*, *Previous Name*, *Suffix*, *Previous address*, and *Preferred Language* USCDI Data Elements: 
  - The Complex Extensions for Race and Ethnicity allow for one or more codes of which:
    - [Must Support] at least one category code from the OMB Race and Ethnicity Categories
    - **MAY** include additional detailed codes from CDC Race and Ethnicity Codes
    - **SHALL** include a text description
  - Date of Death is communicated using the `Patient.deceasedDateTime` element.
    - Although `Patient.deceased[x]` is marked as 𝗔𝗗𝗗𝗜𝗧𝗜𝗢𝗡𝗔𝗟 𝗨𝗦𝗖𝗗𝗜, certifying systems are not required to support both, but **SHALL** support at least `Patient.deceasedDateTime`
  - Previous name is represented by setting `Patient.name.use` to "old" or providing an end date in `Patient.name.period` or doing both.
  - Suffix is represented using the `Patient.name.suffix` element.
  - Previous address is represented by setting `Patient.address.use` to "old" or providing an end date in `Patient.address.period` or doing both.
  - `Communication.preferred` **MAY** designate a preferred language when multiple languages are represented. 
  - The [Patient example] demonstrates how these elements are represented.

- [Certifying systems] **SHALL** and non-certifying systems **SHOULD** follow the [Project US@ Technical Specification for Patient Addresses Final Version 1.0] as the standard style guide for `Patient.address.line` and  `Patient.address.city` for new and updated records.

   - For certifying systems, this requirement does not apply to historical records or documents that are exposed through FHIR-based APIs.

- \*US Core aligns with the [HL7 Gender Harmony Project] gender and sex information, which includes data elements, value sets, and code systems. Refer to it and the [FHIR R5 Patient Resource Gender and Sex Notes] for additional guidance and background for representing Administrative Gender, Sex Assigned at Birth, Gender Identity, <span class="bg-success" markdown="1"> and Sex Parameter For Clinical Use (SPCU)</span><!-- new-content --> Note that:
  - The US Core Sex Extension <span class="bg-success" markdown="1">represents</span><!-- new-content --> the [U.S. Core Data for Interoperability (USCDI)] data element "Sex".
  - <span class="bg-success" markdown="1">The FHIR extension [Patient Sex Parameter For Clinical Use] extension represents the [U.S. Core Data for Interoperability (USCDI)] Observation Data Class' Data Element "Sex Parameter for Clinical Use"</span><!-- new-content -->
  - The US Core Birth Sex Extension is no longer a USCDI Requirement.
  
  <div class="bg-success" markdown="1">
  US Core certified systems are required to support the [Patient Sex Parameter For Clinical Use] extension on US Core Patient to support at least a patient level context. However, they **MAY** use the the extension on other US Core Profiles for specific clinical contexts, for example, on US Core Observation when interpreting a result. Future versions of US Core may require additional contexts (US Core Profiles) where this extension is required.
  {:.stu-note}
  </div><!-- new-content -->


- [Provenance] and the FHIR Extension [Target Element] can document how individual patient demographic data was captured. See [Element Level Provenance] on the [Basic Provenance] page for more information.
- The Patient's Social Security Numbers **SHOULD NOT** be used as a patient identifier in `Patient.identifier.value`. There is increasing concern over using Social Security Numbers in healthcare due to the risk of identity theft and related issues. Many payers and providers have purged them from their systems and filtered them out of incoming data.
  
<div class="bg-success" markdown="1">

- \*Servers can use the US Core Interpreter Required Extension on the US Core Patient or [US Core Encounter Profiles] to communicate whether a patient needs an interpreter. Although the extension is marked as an *Additional USCDI Requirements* on both US Core Patient and US Core Encounter Profiles, the certifying server system is not required to support both, but **SHALL** support the extension on at least one of these profiles. The certifying client application **SHALL** support the extension on both profiles.
  - System can communicate the patient's language preferences in the `Patient.language` element and the optional [Patient Proficiency Extension](https://hl7.org/fhir/extensions/StructureDefinition-patient-proficiency.html) and infer a patient's language service needs from it.

</div><!-- new-content -->

{% include link-list.md %}
