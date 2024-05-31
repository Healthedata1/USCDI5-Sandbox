
​This section provides implementers with important definitions, requirements, and guidance on creating, using, and sharing Clinical Notes.

### Clinical Notes

Clinical notes are a key component in communicating a patient's current status. In the context of this implementation guide, the term "clinical notes" refers to the wide variety of documents generated on behalf of a patient in many care activities. They include notes to support transitions of care, care planning, quality reporting, billing, and even handwritten notes by providers. This implementation guide does not define new note types or set content requirements per note type. Instead, this implementation guide focuses on exposing clinical notes stored in existing systems.


This implementation guide defines how systems exchange <span class="bg-success" markdown="1">eleven</span><!-- new-content --> "Common Clinical Notes" and three DiagnosticReport categories.

Servers **SHALL** support, at *minimum*, these <span class="bg-success" markdown="1">eleven</span><!-- new-content --> "Common Clinical Notes":

  1. [Consultation Note (11488-4)]
  1. [Discharge Summary (18842-5)]
  1. [History & Physical Note (34117-2)]
  1. [Procedures Note (28570-0)] 
  1. [Progress Note (11506-3)]
  1. [Imaging Narrative (18748-4)]
  1. [Laboratory Report Narrative (11502-2)]
  1. [Pathology Report Narrative (11526-1)]
  1. <span class="bg-success" markdown="1">[Surgical Operation Note (11504-8)]</span><!-- new-content -->
  1. <span class="bg-success" markdown="1">[Emergency Department Note (34111-5)]</span><!-- new-content -->
  1. <span class="bg-success" markdown="1">[Advance directives (42348-3)]*☞ ☞ ☞ See Question about DNR(81351-9) and POLST(81351-9) in notes ☜ ☜ ☜*</span><!-- new-content -->

Servers **SHALL** support, at *minimum*, these three DiagnosticReport categories:

  1. [Cardiology (LP29708-2)]
  2. [Pathology (LP7839-6)]
  3. [Radiology (LP29684-5)]

A DiagnosticReport category query allows a Client to retrieve multiple documents in a single query (see [Support Requirements](#support-requirements)).

The Argonaut project team provided this initial list to HL7 after surveying the Argonaut and the US Veterans Administration (VA) participants. They represent the *minimum* set a system must support to claim conformance to this guide. In addition, systems are encouraged to support other common notes types, such as:

* [Referral Note (57133-1)]
* [Nurse Note (34746-8)]

The complete list of note (document) types is available in the [US Core DocumentReference Type Value Set].


●●●


Not all scanned information stored through DocumentReference will be exposed through DiagnosticReport since DocumentReference stores other non-clinical information. For example, DocumentReference can point to an insurance card.

#### Support Requirements

This guide requires systems to implement the [US Core DocumentReference Profile] and to support a *minimum* of all <span class="bg-success" markdown="1">eleven</span><!-- new-content -->  Common Clinical Notes listed above. Systems may extend their capabilities to the complete [US Core DocumentReference Type Value Set]. This requirement is necessary because some systems scan lab reports and don't store them in the DiagnosticReport resource. See [FHIR Resources to Exchange Clinical Notes](#fhir-resources-to-exchange-clinical-notes) for more detail.

●●●

{% include link-list.md %}
