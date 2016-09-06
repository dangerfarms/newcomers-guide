# Django and Django Rest Framework

## Tutorials
Make sure to go through these tutorials first. They explain the basic concepts and help with initial setup.

[Official Django Tutorial (v1.9)](https://docs.djangoproject.com/en/1.9/intro/tutorial01/)  
[Official Django Rest Framework Tutorial](http://www.django-rest-framework.org/tutorial/1-serialization/)

## Best Practices

##### Never directly import `User`.

Use `django.contrib.auth.get_user_model()` instead. We use custom User models. Using this method ensures that all our code points the correct model.

##### Use DRF's `status` module.

Instead of hardcoding status codes like `401`, use eg. `rest_framework.status.HTTP_401_UNAUTHORIZED` instead. This makes code more readable and reduces the chance of typos. 

Hardcoded constant strings are generally a bad idea in programming. You should try to assign constant strings to variables, then reference that variable instead. This is more DRY.

## Examples

Here is some idiomatic backend code, using Django and Django Rest Framework to implement a `GET` endpoint for a single Profile object.

https://github.com/dangerfarms/cc-api/commit/059c328b259b0fbfe970feff7535cfb2b77d42b4

## Common issues
In this section you will find pointers and solutions for common issues. Please read this section carefully and check back for updates.

### Multiple inheritance (and mixins)
[Multiple inheritance](https://docs.python.org/2/tutorial/classes.html#multiple-inheritance) can be a bit confusing. What overwrites what?

Multiple inheritance patterns often repeat throughout the code.

For the above reasons, the best practice is to create a class that is only responsible for combining the classes.

Example:

```python
class SomethingBehaviourClass:
    def something(self):
        pass


class Mixin:
    def something(self):
        pass

    def other(self):
        pass


class SomethingBehaviourClassWithMixin(SomethingBehaviourClass, Mixin):
    pass


class ActualClass(SomethingBehaviourClassWithMixin):
    pass
```

### Migrations
Migrations are created automatically by Django when running the `makemigrations` management command. 

Migrations can be applied by running the `migrate` management command.
Read more about migrations in [the official docs](https://docs.djangoproject.com/en/1.9/topics/migrations/) (v1.9).

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

#### A tale about why you should never remove or edit migrations
Schema migrations are database management system (DBMS, such as e.g. PostgreSQL, MySQL, sqlite, etc) independent code snippets, that describe change to the database schema.

When a schema migration gets applied, it is essentially translated to some flavour of SQL (we usually use PostgreSQL), or other database language.

Lets say you start with an easy migration:  
0001:  
`CREATE TABLE something COLUMNS name VARCHAR(20);`  

(Note that this is not real sql, but enough to get my point accross.)

Then someone adds a field and the following migration is created:  
0002:  
`ALTER TABLE something ADD COLUMN age INTEGER;`  

Then someone modifies that field:  
0003:  
`ALTER TABLE something ALTER COLUMN RENAME age TO old_age;` 

Now imagine our production server with all our very important data about Somethings is at a stage where it applied 0001 and 0002 but not 0003.

Django saves this information (the fact that something 0001 and something 0002 both been applied) in another database table. That is where the info comes from when you run `./manage.py showmigrations`

Now someone comes along and removes all migrations, and recreates a new one:  
0001:  
`CREATE TABLE something COLUMNS name VARCHAR(20), old_age INTEGER;`  

The production server actually has table `something` with columns `name` and `old_age`

And it "knows" it had already applied `something 0001`, so will not apply it (even if it would, it would break, as the table already exists...).

Now it tries to run some code, like retrieving a `Something`, and this sql command is created by django from the model code, which is correct:

`SELECT name, old_age FROM something WHERE id=1;`

However now the database complains, because `table 'something' has no column 'old_age'`.

And now you have a nice 500 error and nothing works any more.

Don't remove or edit existing migrations.

The end. 

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


### Random bits
- Minimize the number of joins, avoid overcomplicated serializers, in particular be aware of serializers that nest more than two levels, and especially if they can be searched/filtered on.

- Minimize the number of SQL queries executed.

- When defining a ForeignKey field, think about, and explicitly set the `on_delete` parameter.

## Unit testing

### Naming conventions

#### File structure
Coming soon.

#### Classes (Test cases)

##### Use DRF's verbs to name tests

Django Rest Framework uses some useful verbs to name [mixins and generic views](http://www.django-rest-framework.org/api-guide/generic-views/#mixins).

* List
* Create
* Retrieve
* Update
* Destroy

Try to use these words in your test class names.

Use "list" when you `GET` a collection. Use "retrieve" when `GET` a single entry in the collection.

For example, if you have a `/users` view you might write these test classes.

* `ListUsersTestCase` (`GET /users`)
* `CreateUserTestCase` (`POST /users`)

For `/users/:id` (ie. a detail view), you'd probably write these.

* `RetrieveUserTestCase` (`GET /users/12`)
* `UpdateUserTestCase` (`PUT/PATCH /users/12`)
* `DestroyUserTestCase` (`DELETE /users/12`)



#### Methods (Unit tests)

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

### Best Practices

##### Use `django.test.TestCase` instead of `unittest.TestCase`

Django's `TestCase` class is based on the one in the `unittest` package, but it also clears the DB between each test.

If we don't clear the DB between tests, tests might fail depending on which tests were ran before it. Therefore any test class which uses the database **must** extend `django.test.TestCase`.

### Troubleshooting

#### `ImportError` when running tests.

##### Error
`ImportError: 'tests' module incorrectly imported from 'blah/tests'. Expected 'blah'. Is this module globally installed?` 

##### Reason
The error is due to the directory `blah` having both a `tests.py` file and `tests/__init__.py` file, resulting in two possible sources for `blah.tests`. 

##### Solution
Move the contents from `tests.py` inside one of the test files in `tests/`, and remove `tests.py`.

