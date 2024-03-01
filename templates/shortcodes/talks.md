{% set data = load_data(path=path) -%}
{% for talk in data.talks %}
- {{ talk.date }}. [{{ talk.title }}](/{{ talk.url }}).
  _{{ talk.venue }}_. {{ talk.location }}.
{% endfor %}
