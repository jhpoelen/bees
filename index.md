---
layout: default
---
<h1>Welcome to Content-based iDigBio!</h1> 

The following table was created by:

1. creating a new blank Jekyll site:
```
jekyll new [site_dir] --blank
```
1. archiving (meta-)data and media of an iDigBio registered [UAM Insect Collection (Arctos)](https://www.idigbio.org/portal/recordsets/eaa5f19e-ff6f-4d09-8b55-4a6810e77a6c) running 
```
cd [site_dir]
preston track "https://search.idigbio.org/v2/search/records/?rq=%7B%22family%22%3A%22Andrenidae%22%2c%22hasImage%22%3A%22true%22%7D&limit=10&offset=0"
``` 
using [iDigBio API](https://www.idigbio.org/wiki/index.php/IDigBio_API) and [Preston](https://preston.guoda.bio). 
3. generating a static Jekyll site content from Preston archive using:
```
preston copyTo --type jekyll .
```
4. launch website using:
```
jekyll s
``` 
5. opening generated static site in browser at 
```
http://localhost:4000
```

<div style="display: flex; flex-direction: column; row-gap: 2em;">
  
  {%- assign records = site.pages | where: "layout", "record" -%}
  {%- for record in records -%}
  <div style="display: flex; flex-align: column; border: solid;">
  <a href="{{ record.idigbio.uuid }}">iDigBio Specimen Record {{ record.idigbio.uuid | slice: 0,7 }}</a>
  {%-   for uuid in record.idigbio.indexTerms.mediarecords -%}
  {%-     assign mediarecord = site.pages | where: "id", uuid | first -%}
  {%      include media.html mediarecord=mediarecord.idigbio %}
  {%-   endfor -%}
  </div>
  {%- endfor -%}

</div>
