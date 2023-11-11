---
title:
  "{ title }": 
citekey:
  "{ citekey }": 
authors: {{creators|replace("\n","\n- ")}}
keywords: {{hashTags|replace("\n","\n- ")}}
---

# {{title}}

```table-of-contents
style: nestedList # TOC style (nestedList|inlineFirstLevel)
maxLevel: 0 # Include headings up to the speficied level
includeLinks: true # Make headings clickable
debugInConsole: false # Print debug info in Obsidian console
```
> [!ABSTRACT]- ABSTRACT
> {% if abstractNote %} 
> {{abstractNote|replace("\n"," ")}}
> {% endif %}

> [!KEYWORDS]- KEYWORDS
> {% if hashTags %} 
> {{hashTags|replace("\n"," ")}}
> {% endif %}
## Metadata
> [!meta]- Metadata – {% for attachment in attachments | filterby("path", "endswith", ".pdf") %}[PDF{% if not loop.first %} {{loop.index}}{% endif %}]({{attachment.desktopURI|replace("/select/", "/open-pdf/")}}){% if not loop.last %}, {% endif %}{% endfor %}
> **Title**:: {{title}}  
> **Authors**:: {%- for creator in creators %} {%- if creator.name == null %} {{creator.firstName}} {{creator.lastName}}, {%- endif -%} {%- if creator.name %}**{{creator.creatorType | capitalize}}**:: {{creator.name}}{%- endif -%}{%- endfor %}
> **Year**:: {{date | format("YYYY")}} 
> {%- if itemType %}**ItemType**:: {{itemType}}{%- endif %}
> {%- if itemType == "journalArticle" %}**Journal**:: *{{publicationTitle}}* {%- endif %} 
> {%- if itemType == "bookSection" %}**Book**:: {{publicationTitle}} {%- endif %}
> {% if allTags %}**Keywords**:: {{allTags}}{% endif %}
> {%- if bibliography %}**Bibliography:** {{bibliography}}{%- endif %}
> **Related**:: {% for relation in relations -%} {%- if relation.title -%} [[{{relation.title}}]], {% endif -%} {%- endfor%}


## Related

```dataview
TABLE created, updated as modified, tags, type
FROM ""
WHERE related != null
AND contains(related, "{{citekey}}")
```
## Reading notes

### Notes :
```python
# RUN TO GENERATE, OR RELOAD OBSIDIAN NOTES
import os
os.chdir("/home/jules/Documents/phd/Obsidian_Vault/script/")
from note_loading import *
loading(@title)
```

<hr>

__long_note_insertion__

<hr>

```python
# SAVE NOTES
import os
os.chdir("/home/jules/Documents/phd/Obsidian_Vault/script/")
from note_loading import *
saving(@title)
```
<br>
{%-
    set zoteroColors = {
        "#ff6666": "red",
        "#f19837": "orange",
        "#5fb236": "green",
        "#ffd400": "yellow",
        "#2ea8e5": "blue",
        "#aaaaaa": "grey",
        "#a28ae5": "purple"
    }
-%}

{%-
   set colorHeading = {
		"green": "✅ Comment, noticeable point",
		"blue": "💙 To keep in mind/usefull/good reference",
		"purple": "🗝️ Key point",
		"orange": " ✴️ Critical/Main point of the article",
	    "yellow": " 🌟 Critical point for personal/work interest",
	    "red": " 🚨 Disagreement with authors/doubting point",
	    "grey":" 🗒️ new vocabulary, details,whatever"
   }
-%}

{%- macro calloutHeader(type) -%}
    {%- switch type -%}
        {%- case "highlight" -%}
        Highlight
        {%- case "image" -%}
        Image
        {%- default -%}
        Note
    {%- endswitch -%}
{%- endmacro %}

{%- set newAnnot = [] -%}
{%- set newAnnotations = [] -%}
{%- set annotations = annotations | filterby("date", "dateafter", lastImportDate) %}

{% if annotations.length > 0 %}
*Imported: {{importDate | format("YYYY-MM-DD HH:mm") }}*


{%- for annot in annotations -%}

    {%- if annot.color in zoteroColors -%}
        {%- set customColor = zoteroColors[annot.color] -%}
    {%- elif annot.colorCategory|lower in colorHeading -%}
    	{%- set customColor = annot.colorCategory|lower -%}
    {%- else -%}
	    {%- set customColor = "other" -%}
    {%- endif -%}

    {%- set newAnnotations = (newAnnotations.push({"annotation": annot, "customColor": customColor}), newAnnotations) -%}

{%- endfor -%}

{#- INSERT ANNOTATIONS -#}
{#- Loops through each of the available colors and only inserts matching annotations -#}
{#- This is a workaround for inserting categories in a predefined order (instead of using groupby & the order in which they appear in the PDF) -#}
<br>

{#- INSERT ANNOTATIONS -#}
{#- Loops through each of the available colors and only inserts matching annotations -#}
{#- This is a workaround for inserting categories in a predefined order (instead of using groupby & the order in which they appear in the PDF) -#}

{%- for color, heading in colorHeading -%}
{%- for entry in newAnnotations | filterby ("customColor", "startswith", color) -%}
{%- set annot = entry.annotation -%}

{%- if entry and loop.first %}

### {{colorHeading[color]}}
{%- endif %}

> [!quote{{"|" + color if color != "other"}}]+ {{calloutHeader(annot.type)}} ([page. {{annot.pageLabel}}](zotero://open-pdf/library/items/{{annot.attachment.itemKey}}?page={{annot.pageLabel}}&annotation={{annot.id}}))

{%- if annot.annotatedText %}
> {{annot.annotatedText|nl2br}} {% if annot.hashTags %}{{annot.hashTags}}{% endif -%}
{%- endif %}

{%- if annot.imageRelativePath %}
> ![[{{annot.imageRelativePath}}]]
{%- endif %}

{%- if annot.ocrText %}
> {{annot.ocrText}}
{%- endif %}

{%- if annot.comment %}
> - **{{annot.comment|nl2br}}**
{%- endif -%}

{%- endfor -%}
{%- endfor -%}
{% endif %}