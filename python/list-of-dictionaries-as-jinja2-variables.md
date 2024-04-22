# Using a list of dictionaries as variables to render a jinja2 template

```python
from jinja2 import Environment, FileSystemLoader

environment = Environment(loader=FileSystemLoader("./"))
template = environment.get_template("template.j2")

list_of_dicts = [{"a":1, "b":2}, {"a":3, "b":4}]

print(template.render(variables=list_of_dicts))
```

The important part is the `variables` kwarg.

```jinja2
{% for var in variables %}
    {{ var.a }}
    {{ var.b }}
{% endfor%}
```
