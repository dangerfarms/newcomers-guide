# Django and Django Rest Framework

## Tutorials
Make sure do go through these tutorials first. They explain the basic concepts and help with initial setup as well.

[Official Django Tutorial (v1.9)](https://docs.djangoproject.com/en/1.9/intro/tutorial01/)  
[Official Django Rest Framework Tutorial](http://www.django-rest-framework.org/tutorial/1-serialization/)

## Common issues
In this section you will find pointers and solutions for common issues. Please read this section carefully and check back for updates.

### Multiple inheritance, mixins
Coming soon (Combine classes in a super class created for this reason to keep things dry) 

### Migrations
Migrations are created automatically by Django when running the `makemigrations` management command. 
Migrations can be applied by running the `migrate` management command.
Read more about migrations in [the official docs](https://docs.djangoproject.com/en/1.9/topics/migrations/) (v 1.9).

When you apply a migration, Django generates some SQL queries and executes them on the DB instance.

#### Working with migrations on different branches
As the database is not part of the git architecture, it will not change when you switch branches. 

For example you are working on a branch `1-add-new-column-to-example`. 

You create and apply a certain migration (`0002`) of app `examples` which creates a column `new_column` on the database table `examples_example`.

Then you switch back to another branch, that has `0001` as the latest migration file for the `example` app. If you run `./manage.py migrate` now, no migrations will be applied as that is already in the database. The `migrate` management command doesn't know that it should revert something.

This can lead to inconsistencies and database errors.

Therefore before switching branches, you should always revert any migrations that only exist on your current branch.  
You can revert a migration by running `./manage.py migrate` with a specific pacakge name and the target migration number. In this example:
```
./manage.py migrate examples 0001
```

Then you can switch back to another branch safely.


### Setting defaults when adding new fields
If adding a new field to a model (or changing an existing field) that has no default value and isn't nullable, the `makemigrations` management command will prompt you to either change the model definition and add `default=<value>` or provide a default value for the migration. It might also be tempting to add `null=True` to the model field definition.

It is important to clearly understand the difference between the three approaches.

The first approach is to add `default=<value>` to the field's definition in the model.  
You should choose this if and only if it makes sense from a data architecture point of view. E.g. it might make sense for a boolean field to default to `True` and be set to `False` if some condition is met.  
You shouldn't assign a default value if that is not what is expected in the database. E.g. if there is a `name` field, it might make little sense to set a default value.

The second approach is to set `null=True` on the model field definition. This might be tempting to get rid of the migration prompt, however once again, you should think about what data structure is expected.  
Do you really want the field to sometimes be `NULL` in the database?  
Unless your answer to that question is a definitive "yes", supperted with a good argument, you shoud not choose this approach.

The third, and usually best approach is to provide a one off default value in the migration (the prompt allows you to do this when running `makemigration`). 
With this approach your selection for a default value will be recorded in the migration file itself.  
This makes it possible to apply the new migration to existing (e.g. production) databases, while not makeing any compromises on future data quality.


## Unit testing

Coming soon

### Naming conventions

Methods in a class that subclasses `TestCase` are run automatically if and only if their name starts with `test`.

We therefore preface each test method with `test_` (to confirm to django's default test runner's conventions).

To aid naming tests, our convention is to start each test with `should_`, resulting in test names matching the pattern `test_should_<action>_<when/if>_<condition>`.

Example:
```python
def client_profile_should_not_be_set_to_completed_if_required_fields_are_blank(self):
    # This will NOT be picked up by the test runner, therefore it won't fail
    self.asssertTrue(False)
    
def test_should_not_set_completed_if_required_fields_are_blank(self):
    # This will be run as expected
    # The subject (client profile) is clear from the context, 
    # since we are testing client profiles only in this test case.
```