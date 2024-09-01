The USCDI *Sex Parameter for Clinical Use Observation Data Class* is defined as "Category based upon clinical observations typically associated with the designation of male and female." This section describes the guidance and conformance rules for using the FHIR extension [Patient Sex Parameter For Clinical Use] on US Core Profiles to represent this USCDI data element.

### Background

The [HL7 Cross Paradigm Implementation Guide: Gender Harmony - Sex and Gender Representation, Edition 1] discusses the approach on how to exchange clinical sex and gender information using FHIR including the FHIR extension Patient Sex Parameter For Clinical Use

>**4.6 Sex Parameter for Clinical Use (SPCU)**  
Sex Parameter for Clinical Use is provided for use in orders, observations, and other clinical uses. SPCU can be highly contextual and allows specific considerations to be provided for potential automated uses and records[^first].

>**13.11.1 Patient Level Sex Parameter for Clinical Use**  
A Sex Parameter for Clinical Use (SPCU) may be used in specific clinical contexts, for example, when placing an order or when interpreting a result. However, there are cases where having a context-free categorization of a patient can be useful...When using SPCU at a patient level, consider if any information is available suggesting that the patient is NOT male-typical or female-typical across all clinical contexts, then using **specified** [*Patient Sex Parameter For Clinical Use extension*] as the patient-level SPCU is most appropriate[^second].



>**13.11.2 Contextual Sex Parameter for Clinical Use**  
A Sex Parameter for Clinical Use (SPCU) may be used in specific clinical contexts, for example, when placing an order or when interpreting a result. In these contexts, consider whether using a categorization such as Sex Parameter for Clinical Use is sufficient, or if using a more specific clinical observation such as an Observation about the presence or absence of an organ is most appropriate. If a categorization is sufficient, then the patient-sexParameterForClinicalUse  [*Patient Sex Parameter For Clinical Use*] extension may be added to the resource that best represents the context. For example, if the context is a referral order or lab order, then the extension could be added to the ServiceRequest[^third].

### US Core Guidance and Conformance

Implementers seeking ONC certification **SHALL** be capable of using the  Patient Sex Parameter For Clinical Use extension on *at least one* appropriate US Core Profile to meet the USCDI *Sex Parameter for Clinical Use Observation Data Class*. 

For example, to represent SPCU for laboratory observation a server may either:

- Add the Patient Sex Parameter For Clinical Use extension the US Patient profile (See the [Patient Example])
- Add the  Patient Sex Parameter For Clinical Use extension to the US Core Laboratory Result Observation Profile (see the [GFR Observation Example])

For non-certifying systems this extension is optional. See the [USCDI page]() for more information about the US Core and USCDI relationship and a mapping between US Core Profiles and the USCDI Data Classes and Elements.

---

[^first]: https://hl7.org/xprod/ig/uv/gender-harmony/model.html#sex-parameter-for-clinical-use-spcu

[^second]: https://hl7.org/xprod/ig/uv/gender-harmony/fhirgenderharmony.html#patient-level-sex-parameter-for-clinical-use

[^third]: https://hl7.org/xprod/ig/uv/gender-harmony/fhirgenderharmony.html#contextual-sex-parameter-for-clinical-use

{% include link-list.md %}
