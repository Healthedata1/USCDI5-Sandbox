
### Provenance in US Core
<div class="bg-success" markdown="1">

The FHIR [Provenance] Resource tracks information about the activity about a version of a resource, describing the entities and agents involved. FHIR resources also contain elements that represent "small-p provenance" information about how the resource was obtained which overlap with the functionality of the Provenance resource.

​This page is divided into two section documenting how the [US Core Provenance Profile] records changes at the *organizational level*, and how the US Core Profiles' "small-p provenance" elements record changes at the *individual level*. The US Core Provenance Profile asserts where the data came from at an organizational or system level.  Because systems typically do not use the Provenance Resource to represent this information at an *individual level* (in other words, activities by the patient or provider), various FHIR resource elements are identified that track the "small p provenance" information at the individual level. The baseline rules and guidance here do not preclude more advanced use cases such as [Using Provenance To Target Resource Elements].

</div><!-- new-content -->

### <span class="bg-success" markdown="1">Organizational Level</span><!-- new-content --> Provenance

<span class="bg-success" markdown="1">The [US Core Provenance Profile] focuses on a key subset of elements, the "last hop", and specific use cases used to assert changes to the record at the organizational level.<!-- new-content --> Full Provenance of a Resource requires details from the original resource creator and all intermediary actors that updated the Resource. Members of the Argonaut community and the HL7 security working group discussed the current sharing approaches and end-user display. They agreed the most important information is the last organization making a meaningful clinical update to the data and the prior system providing it - the 'last hop'. Participants didn't dispute the potential need to recreate the entire chain but didn't see this as relevant to the immediate end-user.

#### Key Provenance Elements

The guidance for Provenance in US Core focuses on six key elements: Timestamp, the Target Resource, Author, Author Organization, Transmitter, and Transmitter Organization. The Timestamp is the date and time the author created, updated, or deleted the data. The Target Resource is the Resource the Provenance record supports. The Author represents the person(s) responsible for the information. The Author Organization defines the organization the author is associated with when they created, updated, or deleted the data. The Transmitter represents the system responsible for transmitting the information. Finally, the Transmitter Organization defines the organization responsible for the transmission.

**Key Provenance elements:**

Element | Required | Must Support |Optional| FHIR Element|
---|---|---|---|---
Target | Resource Provenance <br>record supports | | | `Provenance.target`
Timestamp | Date | | Time with timezone offset | `Provenance.recorded`
Author | | Name<br>Identifier | NPI recommended, additional identifiers allowed| `Provenance.agent.who`
Author Organization | | Name<br>Identifier  | NPI recommended, additional identifiers allowed |`Provenance.agent.onBehalfOf`
Transmitter | | Name<br>Identifier | NPI recommended, additional identifiers allowed| `Provenance.agent.who`
Transmitter Organization | | Name<br>Identifier  | NPI recommended, additional identifiers allowed |`Provenance.agent.onBehalfOf`
{: .grid}

#### Use Cases <span class="warning" markdown="1">  NOTE THAT THE "USE CASES" SECTIONS HAVE ALL BEEN COMMENTED OUT.</span><!-- warning -->

The HL7 Basic Provenance Informative implementation guide outlines four use cases: Fax, Health Information Exchange (HIE) redistribution, HIE transformation, and Clinical Information Reconciliation and Incorporation (CIRI). While these use cases may have FHIR implications in the future, CIRI and HIE are the key use cases and are covered in detail here.

##### Clinical Information Reconciliation and Incorporation
<!-- {% raw %} 
[Clinical Information Reconciliation and Incorporation (CIRI)] is when a user reviews and updates or accepts information into their system. The data could come from a Health Information Exchange (HIE), a 3rd-party FHIR server, or a patient providing information. Figure 1 represents information from Good Health and Sunshine Health Organization. Dr. Reconciled reviews the data in Figure 1, updates the reaction to hives, and stores it in the Future Health Organization system. When a mobile app requests the information, it receives Dr. Reconciled as the Author and Future Health as the organization. This type of authorship change is only relevant for data suitable for reconciliation, such as medications, allergies, and problems. If Dr. Accepted had saved other clinical content into his EHR that is not reconciled but stored, such as clinical notes, that content must retain its original author.

The Figure below represents information from two different organizations to a clinician for reconciliation:

{% include img.html img="Provenance_Recon_Workflow_Step2.svg" caption="Figure 1: Clinical Information Reconciliation and Incorporation (CIRI) from two Organizations" %}

 Dr. Reconciled is the latest author after updating the reaction on 5/15/2019 and accepting it to the local data store. By accepting the information, Dr. Reconciled is taking over the responsibility.

 `GET /AllergyIntolerance?patient=[ID]&_revinclude=Provenance:target`

Good Health Organization Provenance
{% include examplebutton_default.html example="get-all-allergy-provenance-good-health" b_title = "Click Here to See 'Get Allergy Information for a Patient at Good Health' Example" %}

Sunshine Health Organization Provenance
{% include examplebutton_default.html example="get-all-allergy-provenance-sunshine-health" b_title = "Click Here to See 'Get Allergy Information for a Patient at Sunshine Health' Example" %}

After reconciliation, the Future Health Organization Provenance contains the latest information.
{% include examplebutton_default.html example="get-all-allergy-provenance-future-health" b_title = "Click Here to See 'Get Allergy Information for a Patient at Future Health' Example" %} {% endraw %} -->

##### Accepting information from a single site
<!-- {% raw %} 
The Figure below represents information after a provider accepted data from a single site:

{% include img.html img="Provenance_Recon_Single_Organization.svg" caption="Figure 2: CIRI After Accepting Reconciliation, No Changes" %}

Dr. Accepted is the latest author after verifying the problem on 5/2018 and accepting the problem data into their local data store since it went through an interactive reconciliation process.
 {% endraw %} -->
##### HIE Redistribution
<!-- {% raw %} 
A Health Information Exchange (HIE) is an organization and technology that facilitates information exchange between one and many partners. In specific HIE scenarios, they only redistribute information, while in others, they store, transform, and redistribute information. The HIE must maintain the clinical content fidelity (original author, author organization, and timestamp). In addition, the HIE must keep track of who sent them the information for auditing; however, they are not required to include the original transmitter when redistributing content.

{% include img.html img="Provenance_HIE_Single_Org_Device.svg" caption="Figure 3: HIE Redistribution - No clinical content transformation" %}

Since no clinical content is changed in the HIE redistribution, the best scenario is a Provenance Record with:
- `Provenance.agent.type` = author
  - `Provenance.agent.who` set to the practitioner who authored the content (i.e., not the HIE)
  - `Provenance.agent.onBehalfOf` set to the organization that the author acted on behalf of before sharing with the HIE
- `Provenance.agent.type` = transmitter
  - `Provenance.agent.who` set to the HIE organization

 The timestamp and pointer (i.e., target) to the appropriate Resource are required in all cases and must be included. This IG would note these as **SHALL** constraints if systems always had the Author and Author Organizations available. Participants in the development of this guide reported that the Author information provided to HIEs needs to be more consistent and reliable.  

 {% endraw %} -->
##### HIE Transformation
<!-- {% raw %} 
Unlike Use Case 3 - HIE Redistribution, Use Case 4 includes data transformation. Information is received (e.g., v2 lab, other CDs), transformed by a HIE, stored, and passed in a new format (e.g., CCD or FHIR).

{% include img.html img="Provenance_HIE_Multi_Org_Device.svg" caption="Figure 4: HIE Transformation - Clinical content is transformed" %}

Transformation of data from one format to another **MAY** change the authorship of the information, where the HIE is the author/author organization. The HIE must maintain the original data source. An `agent.type`="assembler", `agent.type`="transmitter", or other agents from [Provenance Agent Type] value set **MAY** also be included. Due to insufficient implementer guidance, the Basic Provenance guidance here does not specify how to assign authorship for this use case. HL7 plans to gather additional input and include it in the HL7 Basic Provenance Informative guide for C-CDA and FHIR.

 {% endraw %} -->

### <span class="bg-success" markdown="1">Individual Level Provenance</span><!-- new-content -->

<div class="bg-warning" markdown="1">
- Discuss:
  - Which elements should be added  as *Must Support/Add'l USCDI*
  - Which target resources should be added as *Must Support/Add'l USCDI*
    - US Core Patient
    - US Core Practitioner
    - US Core PractitionerRole
    - US Core RelatedPerson
    - US Core Organization
    - US Core Device
  - If not adding *Must Support/Add'l USCDI* then what guidance do we give to meet the requirement?

(source excel file is here: https://1drv.ms/x/c/deea5e002be8d274/EazOGawVCEBBjJmfJXcI5DQBKbAXNqHGcHr4ioRM6s2m0g)
</div><!-- new-content --> 
<div class="bg-success" markdown="1">

US Core Profiles contain elements that represent "small-p provenance" information about how the resource was obtained, which overlaps with the functionality of the Provenance resource discussed above. The table below identifies the author and author roles that meet the [U.S. Core Data for Interoperability (USCDI)] Provenance Author and Author Role Data Elements requirements for individuals. In addition, they are listed in each US Core Profile page's "Profile Specific Implementation Guidance" section. The author is communicated by the elements and the author's role by the target resource type referenced by it (for example, Patient, Practitioner/PractitionerRole, RelatedPerson, Device). Details about the author's role are contained in the target resource's contents. The **bold font** indicates the elements and target resources that are labeled as *Must Support* in the respective US Core Profiles. Elements and target resources not labeled as *Must Support* **SHOULD** be supported in the profiles when the system captures the data.  

##### Author and Author Role Data Elements

This table excludes these US Core Profiles, which are not typically associated with individual authorship:

- US Core Encounter
- US Core Location
- US Core Organization
- US Core Practitioner
- US Core PractitionerRole
- US Core RelatedPerson
- US Core Medication
- US Core Provenance
- US Core CareTeam
- US Core Coverage
- US Core Specimen

<span style="color: red;">Red text</span> indicates proposed new *Must Support* elements and targets

This information is also available as a [csv](tables/provenance-elements.csv) or [excel](tables/provenance-elements.xlsx) file:

{% include provenance-source-table-generator.md %}

\* US Core *Must Support* or *Additional USCDI* element

\** US Core *Must Support* Target Resource Type
</div><!-- new-content -->

{% include link-list.md %}
