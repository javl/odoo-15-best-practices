# Odoo 15 best practices

*This repo is a work in progress.*
 
I'm planning on using this repo to write down best practices and other types of useful info on using Odoo 15, 
as well as some more generic suggestions for working in Python. Feel free to contribute!

## Names for relational fields (Many2one / One2many / Many2Many)

When using relational fields use the related model name as field name and add `_id` when using `Many2one` (as this relation points to a single object) or `_ids` when using `One2many` or `Many2many` (which can point to multiple objects).

Example: 

```
    user_id = fields.Many2one(’res.users', default=lambda self: self.env.user)
    datapoint_ids = fields.Many2many(’impact_monitor.datapoint')
```

***Note:*** if you don’t specify a name for your field `string`, Odoo will automatically use the given field name without `_id` or `_ids`. 

For example: the automatically generated label for `user_id` will be `user`.

## Methods

### Private methods

When a method is only called from within the model itself (not from a button in the UI for example) this
method should be made private. You can do this by starting the method name with an underscore `_`, for example: 

```python
def _get_demo_data(self):
    # ...
```

### Special methods

Methods that are connected to one of Odoo's built-in special method types (`onchange`, `compute`, `constrains`, etc.) 
should start with an underscore and the method type, for example:

```python
def _onchange_user(self):
   # ...
   
def _constraint_partners_chat(self):
	# ...

def _compute_name(self):
	# ...
```

## Naming and doctypes
Methods names should be short but descriptive: you should easily be able to tell what it is the method does. 
But even with a descriptive name it can be helpful to add some more info on what kind of arguments the methods excepts or what it returns. To do this, add a docstring to the method like so:

```python
def check_if_numeric(value):
    """
    Takes a string and checks if it's value is numeric.
    Returns True if numeric, or False if not numeric or input was not a string.
    """
    try:
        return value.isnumeric()
    except AttributeError:
        return False
```

Using a docstring has the added benefit of easily getting info on a method when using debuggers or tools like ipython. For example, in ipython, typing `help(check_if_numeric)` will return the following:

```python
Help on function check_if_numeric in module __main__:

check_if_numeric(value)
    Takes a string and checks if it's value is numeric.
    Returns True if numeric, or False if not numeric or input was not a string.
```
