# Django

## Tutorials
Coming soon

## Common issues
Coming soon

### Multiple inheritance, mixins
Coming soon (Combine classes in a super class created for this reason to keep things dry) 

### Migrations (to be written up properly)
Migrations are basically translated into (in our case) SQL queries
When you apply a migration, django executes some SQL queries on the DB instance
So if you apply migration 0002, it will create columns etc on that table
Then you switch back to another branch, that has 0001 as the latest migration
0001 is applied
No further migrations are required
-> `No migrations to apply`
If you want to go back to another branch, what you need to do is the following:
```
git checkout <branch>
./manage.py migrate <package name> <migrations number to go back to e.g. 0001>
git checkout development
```
I.e. you need to reverse the migration
Reversing the migration is, again, a set of SQL commands that are run on the DB instance
If you created a field, the reverse will remove it etc.

On the other issue about removing `null=True` from an existing field (or adding a new required field to the model for that matter):
When you remove `null=True` the migration would create a requirement in the DB such that a field cannot be NULL.
But that would cause issues, if you already had some NULL values in the table
So it offers you a chance to assign a defult value ​_in the migration_​ (it does this interactively when you run `makemigrations`) (edited)
This default value is very different from declaring `default=<something>` in the model
That would be written to the DB, and would affect the actual data structure
By declaring the default in the migration, you can have the strongest requirements possible (in this case: a user type must be selected in all cases) in the DB level
But if the migration is run on a legacy database, it will first run a query to update the existing NULL values to whatever you provided
It is a subtle but important distinction

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