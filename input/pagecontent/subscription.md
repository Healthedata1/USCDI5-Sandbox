### Polling vs Subscriptions

Checking for resource updates in FHIR can take one of two forms - subscription or polling, as described in the [Exchanging with polling] and [Exchanging with FHIR Subscription] sections of the FHIR R5 core specification. The [Subscription Capabilities] section provides general guidance on polling vs. subscription.

#### Polling

Polling is a mechanism for conveying new data to a Client as (or shortly after) the data is created or updated without requiring the Server to be aware of the specific needs of the Client. The Client repeatedly queries the Server to see if there is new data. For example, if a Client wants to know a patient's vaccination status they could periodically Query for their Immunizations using the `_lastUpdated` search parameter (see [Searching Using lastUpdated](https://hl7.org/fhir/us/core/general-guidance.html#searching-using-lastupdated) for additional guidance and limitations).

Clients can poll for a single resource or across several resources. The frequency must be often enough that the time between when the relevant data is created or updated and when the Client receives it is sufficiently short for the Client's needs. However, it must be infrequent enough that the Server's resources are not over-taxed by the repeated query.
<!-- Clients **SHOULD** perform this operation in an automated/background manner after 1 minute to return automated responses and no more than every 5 minutes for the first 30 minutes and no more frequently than once every hour after that. -->



#### Subscription

Subscription is a mechanism designed to allow clients to request notifications when an event occurs or data changes. It is an active notification system; a FHIR server actively sends notifications as changes occur. The Client subscribes to the Server's Server [base]/Subscription endpoint for notifications when any resource that they created is updated.

Servers who support Subscription **SHALL** comply with the [Subscription R5 Backport Implementation Guide].  This topic-based subscription model supersedes the query-defined subscription model defined in FHIR R4.  Systems **SHALL** follow the conformance requirements specified in the Subscription R5 Backport Implementation Guide and the US Core-specific conformance requirements listed below. 

<!-- In the subscription mechanism, instead of the Client regularly querying the Server to see if there are changes to existing resources, the Client creates a Subscription instance on the Server server. (It's also possible for the data source to configure subscriptions for clients; in other words, it can create subscriptions administratively. However, these implementation details are out of scope for this guide.) The Subscription indicates that the Client wants to be notified about changes to resources and provides filters describing what subset of resources the Client is interested in. The Server will then push notifications when there are new or updated resources and the Client can then query for the specific resources that have changed.

Servers who support Subscription **SHALL** comply with the [Subscription R5 Backport Implementation Guide] and the Da Vinci Health Record Exchange (HRex) [Subscription requirements](https://build.fhir.org/ig/HL7/davinci-ehrx/resource.html#subscription) for subscribing to resource updates. These implementation guides "pre-adopt" the FHIR R5 topic-based subscription approach in R4 implementations since most U.S. EHR vendors have agreed to support it. -->

#### Subscription Topics

Subscription topics specify an event or change in data that is used to trigger a notification. Topic definitions also include the boundaries around what a Subscription can filter for and additional resources that MAY be included with notifications. The [SubscriptionTopic] resource, which is available in FHIR 4B and later versions, is used to define these topics conceptually and formally in a computable fashion.  

Note that supporting the FHIR SubscriptionTopic resource or the equivalent Basic resource versions described in the R5 Backport Implementation Guides is NOT required by this guide to support subscriptions.
{:.bg-warning}

The table below summarizes the US Core subscription topic requirements (**SHALL vs SHOULD ?**).<!-- and best practice recommendations (**SHOULD**) --> They allow subscribers to receive notifications for patient-related events that are represented by the corresponding US Core Profiles.  These subscription topics represent the minimum support for US Core. The events include creation, updates, and deletion interactions of resources. The US Core subscription topics include a trigger for a resource type and have a filter to allow Clients to restrict notifications to a particular patient. Servers **MAY** add filters to these topics (for example, filter by the code parameter for the US Core Laboratory Result SubscriptionTopic) and still satisfy the US Core conformance requirements. Servers **MAY** define other subscription for different triggers or that include other resources in the notifications (in other words, the "shape"). However, these topic definition would not support the US Core requirements even if they are formally derived from US Core subscriptions.

##### The Following Subscription Topics **SHALL** (vs **SHOULD** ?) Be Supported

<!-- US Core Laboratory Result SubscriptionTopic
US Core Laboratory Report SubscriptionTopic
US Core Problems and Health Concerns SubscriptionTopic
US Core Encounter SubscriptionTopic
US Core Immunization SubscriptionTopic -->


{% include resource-subscription-table.md conformance="SHALL" crud='rs'%}


<!-- 
###### The Following Subscription Topics **SHOULD** Be Supported
###### The Following Subscription Topics **SHOULD** Be Supported
-->

Based upon additional testing in future versions, we may update these SubscriptionTopics to add additional filter based on the on US Core required search parameters, and add other US Core SubscriptionTopic to this list.
{:.stu-note}

#### Discovery

 If a Server supports subscriptions, the Server **SHALL** support discovery of the US Core Subscription Topic's definition and  canonical URL by one or more of the following methods:

- Server CapabilityStatement's SubscriptionTopic Canonical Extension (see below).
- FHIR Restful query of SubscriptionTopic Resource
- Out-of-band published documentation

##### CapabilityStatement SubscriptionTopic Canonicals
  
  The R5 Backport Implementation Guide defines the [CapabilityStatement SubscriptionTopic Canonical] extension to allow US Core Clients to discover US Core Servers' supported subscription topics. This extension enables servers to advertise the canonical URLs of subscription topics available to clients and allows clients to see the list of supported topics on a server.

  The example CapabilityStatement snippet shows a Server advertising the US Core resource Update Subscription Topic canonical URL with the CapabilityStatement SubscriptionTopic Canonical extension:

  <!-- {% raw %} {% include examplebutton_default.html example="advertise-topic.md" b_title = 'Click Here To See the US Core Server CapabilityStatement Advertising the US Core Immunization Subscription Topic' %}
    {% endraw %} -->

  {% include examplebutton_default.html example="todo1.md" b_title = 'Click Here To See an Example Subscription to the US Core Immunization Subscription Topic' %}

#### Subscription Resource

To dynamically request notifications for US Core resource updates (and other topics the Server supports), Clients RESTfully POST a Subscription resource with the Server. The Subscription resource **SHALL** conform to the [R4/B Topic-Based Subscription Profile].

<div class="bg-info" markdown="1">
Subscriptions need not be created dynamically by the Client. It's also possible for the Server to configure subscriptions for clients, in other words, create subscriptions administratively. However, these implementation details are out of scope for this guide.
</div> 

##### Channel Types

The subscription requires information about where to send notifications, such as the type of channel and the URL that describes the endpoint.

##### Payload Types

When specifying the contents of a notification, there are three options available: "empty," "id-only," and "full-resource." Because the Server can not guarantee who has access to the nominated subscription endpoint, the notification typically uses "id-only" to return the resource resource ID. By omitting the payload, the Client is forced to authenticate before accessing the data using a FHIR RESTful read or search, which mitigates privacy and security risks for the Server.

- The Client **SHALL** use the canonical URL in the `Subscription.criteria` element to subscribe to the Server's Server [base]/Subscription endpoint'
- The Server **SHOULD** support the "rest-hook" channel and **MAY** support other channel types.
- The Server **SHALL** support ""id-only" payload type and **MAY** support other payload types.

{% include examplebutton_default.html example="todo2.md" b_title = 'Click Here To See an Example Subscription to the US Core Immunization Subscription Topic' %}

##### Subscription Notifications

For active US Core event notification subscriptions, when that clinical event is updated, the Server **SHALL** trigger a Subscription Notification to the endpoint supplied by the Client. This notification is a Bundle resource and **SHALL** conform to the [R4 Topic-Based Subscription Notification Bundle]. The first entry contains the subscription's status information, represented by a Parameters resource. For the "id-only" payload type, resource IDs are listed in the "focus" part parameter.


{% include examplebutton_default.html example="todo3.md" b_title = 'Click Here To See an Example Subscription Notification to the US Core Immunization Subscription Topic' %}

#### Security Considerations (Move to Security Page?)

- Servers are responsible for implementing appropriate access control and managing authorization for subscriptions and notifications.
- Existing security practices and standards for FHIR implementations should be applied to subscription management and notification delivery.

### Example resource Based Transaction using Subscription

In this example, the patient refuses a booster vaccination which triggers a subscription notification to a subscriber.

1. The Client (Client) *subscribes* to the US Core Immunization Subscription Topic to get notifications when there is an immunization event for the patient.
2. In this example, the clinical event is a patient refusing a booster vaccination.
3. The event triggers, the EHR (Server) to send notification to the subscriber referencing the id of the US Core Immunization resource.
4. When notified, the Client can fetch the patient's Immunization resource referenced in the notification.

{% include examplebutton_default.html example="immunization-subscription-scenario.md" b_title = 'Click Here To See Transaction using Subscription' %}

---