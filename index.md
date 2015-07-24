---
layout: page
title: Nexus Developer Hub
tagline: API Documentaion and More!
---
{% include JB/setup %}

## API Params

### Result format

The API returns results as [JSON](http://www.json.org/) by default. The JSON object contains:

| Section | Description |
|-|-|
| `meta` | Metadata about the query, including a disclaimer, link to data license, last-updated date, and total matching records, if applicable. |
| `data` | List of matching results, dependent on which endpoint was queried. |
{: class="table"}

### Query parameters

The API supports four query parameters. The basic building block of queries is the `search` parameter. Use it to "filter" requests to the API by looking in specific fields for matches. Each endpoint has its own unique fields that can be searched.

| Parameter | Description | Example |
|-|-|-|
| `search` | What to search for, in which fields. If you don't specify a field to search, the API will search in every field. | `search=maj_agency_cat:"Department+of+Defense"` |
| `count` | Count the number of unique values of a certain field, for all the records that matched the `search` parameter. By default, the API returns the 100 most frequent values. | `count=maj_agency_cat` |
| `limit` | Return *up to* this number of records that match the `search` parameter. Large numbers (above 100) could take a very long time, or crash your browser. | `limit=25` |
| `offset` | offset this number of records that match the `search` parameter, then return the matching records that follow. Use in combination with `limit` to paginate results. | To get 25 records at a time, use:<br /><br />`limit=25&offset=0` to get the first 25,<br />`limit=25&offset=25` to get the next 25,<br />`limit=25&offset=50` to get the next 25, and so on. |
{: class="table table-code"}

### Query syntax

Queries to the API are made up of *parameters* joined by an ampersand `&`. Each parameter is followed by an equals sign `=` and an argument.

Searches have a special syntax: `search=field:term`. Note that there is only one equals sign `=` and there is a colon `:` between the field to search, and the term to search for.

Here are a few syntax patterns that may help if you're new to this API.

| Pattern | Description |
|-|-|
| `search=field:term` | Search within a specific `field` for a `term`. |
| `search=field:term+AND+field:term` | Search for records that match two terms. |
| `search=field:term+field:term` | Search for records that match either of two terms. |
| `search=field:term&count=field.exact` | Search for matching records, then count within that set the number of records that match the unique values of a field. |
{: class="table table-code"}

#### Spaces

Queries use the plus sign `+` in place of the space character. Wherever you would use a space character, use a plus sign instead.

#### Exact matches

For exact matches, use double quotation marks `" "` around the words. For example, `"Department+of+Defense"`.

#### Grouping

To group several terms together, use parentheses `(` `)`. For example, `(maj_agency_cat:DOD+DOJ+DHS)`. Terms separated by plus signs `+` are treated as in a boolean OR.

To join terms as in a boolean AND, use the term `+AND+`. For example, `(maj_agency_cat:DOD+DOJ+DHS)+AND+localgovernmentflag:N` requires that *any* of the drug names match *and* that the field `localgovernmentflag` also match.

#### Dates and ranges

The API supports searching by *range* in date, numeric, or string fields.

 - Specify an *inclusive* range by using square brackets `[min+TO+max]`. These include the values in the range. For example, `[1+TO+5]` will match **1** through **5**.
 - Dates are simple to search by via range. For instance, `[2004-01-01+TO+2005-01-01]` will search for records between Jan 1, 2004 and Jan 1, 2005.

### Timeseries

The API supports `count` on date fields, which produces a timeseries at the granularity of day. The API returns a complete timeseries.


## Contracts

### Data Downloads
[All 2015 Contracts](https://s3.amazonaws.com/data-act-demo/contract_all_2015.csv)


### Example API Call


```
GET http://ec2-52-6-163-137.compute-1.amazonaws.com/contract?count=maj_agency_cat
```

{::nomarkdown}
<table id="results"class="table table-bordered table-striped">
<thead>
<tr>
  <th>Count</th>
  <th>Agency</th>
</tr>
</thead>
<tbody></tbody>    
</table>
{:/}
<script type="text/javascript">
$(function() {

    $.get('http://ec2-52-6-163-137.compute-1.amazonaws.com/contract?count=maj_agency_cat',function(data,textStatus, jqXHR){
        
        printTable(data.data);
    
    });
    
    function printTable(data) {
    
        var output = '';
        $.each(data,function(k,v){
            output += '<tr><td>' + v.count + '</td><td>' + v.term + '</td></tr>';
        });
        
        console.log(output);
        
        $('#results tbody').html(output);
    
    }



});

</script>    
    
## Assistance

## Sub-Awards

