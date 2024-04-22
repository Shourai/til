# How to open an jinja2 template from a file

```python
from jinja2 import Environment, FileSystemLoader

environment = Environment(loader=FileSystemLoader("./"))
template = environment.get_template("template.j2")

print(template.render(<variables>))
```
