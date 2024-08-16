<!-- This liquid script creates a US Core subscription topic requirements table using input data from input/data/scopes.csv
include parameters: conformance='SHALL'|'SHOULD'|'MAY'see below for how used and crud='cruds'<<< not used -->

{% assign rows = site.data.scopes -%}
{%- assign resource_scopes = '' -%}

<table class="grid">
<thead>
<tr>
<th>Subscription Topic</th>
<th>Subscription Topic Canonical URL</th>
<th>Resource Type</th>
<th>US Core Profile</th>
</tr>
</thead>
<tbody>
{%- for item in rows -%}
{%- if item.topic_conformance == include.conformance -%}
<tr>
<td><a href="{{item.topic_path}}">{{item.topic}}</a></td>
<td><pre><span class="copy-text">{{item.topic_url}}<button data-clipboard-text="{{item.topic_url}}" title="Click to copy URL" class="btn-copy" data-original-title="Click to copy URL"></button></span></pre></td>
<td>{{item.resource_type}}</td>
<td>{{item.profile_title}}</td>
</tr>
{%- endif -%}
{% endfor %}
</tbody>
</table>



http://hl7.org/fhir/us/core/SubscriptionTopic/us-core-problems-and-health-concerns