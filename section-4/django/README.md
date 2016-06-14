# Django

## Tutorials
Coming soon

## Bits and bobs
Coming soon

### Multiple inheritance, mixins
Coming soon (Combine classes in a super class created for this reason to keep things dry) 

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