
###### Step 1 - Subscribe to US Core Immunization Subscription Topic

The Client (Data Consumer) *subscribes* to the EHR's US Core Immunization Subscription Topic to get notifications when there is an immunization event for a patient. The subscription resource uses the canonical URL `http://hl7.org/fhir/us/core/SubscriptionTopic/us-core-immunization`. The EHR accepts the Subscription and returns it in the response body with an "active" status. 

This operation is done once. To unsubscribe, the Client deletes the Subscription from the server or nominates a fixed end date, and the server automatically deletes it at the specified time.


**Request**

~~~

Post [base]/Subscription

~~~


{% include request-headers.md %}


**Request Body**


~~~

<!-- {% raw %} {% include_relative Subscription-cdex-task-inline-scenario1-subscription-requested.json %} {% endraw %} -->

...Example Subscription ...

~~~


~~~

HTTP/1.1 200 OK

Server: CDEX Example Server

Location: http://example.org/FHIR/Subscription/cdex-example1-query-subscription/_history/1

...(other headers)

~~~


**Response Body**


~~~

<!-- {% raw %} {% include_relative Subscription-cdex-task-inline-scenario1-subscription-active.json %} {% endraw %} -->

...Subscription...

~~~


###### Step 2 - Clinical Event Triggers Notification



...clinical event...



###### Step 3 - Receive Notification

In this example, the clinical event is a patient refusing a booster vaccination which triggers a notification to the Client. As defined in the [Subscription R5 Backport Implementation Guide], the notification is a *history Bundle*. The first entry of the bundle is a Parameters resource that communicates the subscription status information. In addition to the subscription status information, Task IDs are listed in the "focus" part parameter.


**Post From EHR**

~~~

Post http://example.org/FHIR/Client/immunization-watch

~~~

{% include request-headers.md %}


**Request Body**

<!-- {% raw %} ~~~
{% include_relative Bundle-cdex-task-inline-scenario1-subscription-notification.json %}
~~~ {% endraw %} -->
... request body ...

{% include response-headers.md %}


###### Step 4 - Fetch Task

When notified, the Client can fetch the patient's Immunization resource referenced in the notification using a FHIR RESTful `GET` interaction.

**Request**

~~~

GET [base]Immunization/immunization-example2

~~~


{% include request-headers.md %}


{% include response-headers.md %}


**Response Body**


~~~

<!-- {% raw %} {% include_relative Task-cdex-task-example2.json %} {% endraw %} -->
... response body ...

~~~

