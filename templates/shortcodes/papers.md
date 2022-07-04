{% set data = load_data(path=path) -%}
{% for paper in data.papers %}
1. [{{ paper.title }}]({{ paper.url }}).<br />
   _{{ paper.authors }}_. {{ paper.journal }} ({{ paper.year }})
{% endfor %}
