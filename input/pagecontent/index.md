### Health eData Sandbox

testbed for ideas....


<!-- This liquid script creates a US Core subscription topic requirements table using input data from input/data/input/data/LoincUniversalLabOrdersValueSet.csv
include parameters: conformance='SHALL'|'SHOULD'|'MAY'see below for how used and crud='cruds'<<< not used -->



### LOINC Universal Lab Order Codes Value Set

Overview:
The LOINC Universal Lab Order Codes Value Set, also known as the "Common Lab Orders", is a collection of the most frequent lab orders. It was created for order entry system developers so that they could deliver them in HL7 messages to laboratories. On receipt, laboratories could understand and translate these universal codes into their internal processes for fulfillment.

This value set was developed iteratively through both empirical and consensus-driven approaches. The original version was developed by the NLM, Regenstrief Institute, and several organizations and contained an empirical list of the most commonly ordered tests. We often called that list the "Top 300", because it contained about 300 codes that accounted for the majority of the volume of five nationally-representative data sources.

In 2014, S&I Framework Initiative called "aLOINC Order Code" launched with the goal of enhancing and expanding the original list to more comprehensively cover tests commonly ordered in ambulatory care settings and for tests ordered in public health settings. The initative used a combination of empiric and consensus based approaches to identify additional LOINC codes to be added to the original list.

Columns:
 - LOINC_NUM - the LOINC term identifier, which is a unique numeric identifier with a check digit
 - LONG_COMMON_NAME - the LOINC Long Common Name for the term =
 - ORDER_OBS - whether the LOINC term can be used as an Order, an Observation, or Both. In this file, the only values contained in the ORDER_OBS column are "Both" and "Order".

Sort order:
This file is sorted alphabetically by LOINC Long Common Name.

*Note: This file does not include any deprecated terms.

{% assign rows = site.data.LoincUniversalLabOrdersValueSet -%}

<table class="grid">
<thead>
<tr>
<th>Row</th>
<th>LOINC</th>
<th>Long Common Name</th>
</tr>
</thead>
<tbody>
{%- for item in rows -%}
{%- unless forloop.first -%}
<tr>
<td>{{forloop.index0}}</td>
<td>{{item.LOINC_NUM}}</td>
<td>{{item.LONG_COMMON_NAME}}</td>
</tr>
{%- endunless -%}
{% endfor %}
</tbody>
</table>


