# Unit Testing

This article focuses on unit testing in general. Examples are written for a Django backend service.

Developers write tests to prove that *under certain conditions* their code will perform as expected. In writing tests a developer does not try to prove their code is correct - the tested parts of the code are not mathematical theorems. Instead, with enough representative examples, they might ensure if code contains errors they will be found by one or more tests.

It is for this reason **a test suite should contain many, small tests**, where each test proves one specific thing only. These tests are heuristics, not proofs.

With all that in mind, a good test should be:

- readable 
- deterministic
- fast
- well designed.

### Readable Tests

Tests not only provide the programmer with a degree of certainty that their code works, but also serve as documentation. The ideal mindset is to imagine you are coming onto the project for the very first time. Ideally you would want each test to tell a story.

```python
def test_should_disable_logged_in_user(self):
    """Test API sets User.disabled = True. Refetch from database to prove."""
    user = self.create_user()
    self.login(user)
    response = self.client.post(self.disable_url)
    db_user = self.fetch_db_user(user.id)
    self.assertTrue(db_user.disabled)
```

The above test is very easy to follow as each line has descriptive variable/method names. The rule of writing descriptive names should not just apply to test code, but to the entire project. Having a variable named `a` in any file is not helpful for someone not familiar with the codebase. In fact, all code should be as readable as possible, but it's even more important in tests.

Due to the importance of readability, tests are the only place in code where we feel [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)ness is not necessarily always the right approach. But if extracting common behaviour can be done in a way to let a test story flow, then it is preferred to do so. One example is `self.login(user)` from above. Here a common behaviour will be made part of the class or perhaps a base class containing helper functions. In fact, it actually helps with readability here, as the previous line was originally `self.client.login(username=user.username, password=USER_PASSWORD)`.

To hammer home the point: if you can make a test both more readable and DRYer then 100% do so!

### Deterministic Tests

Writing deterministic tests normally comes down to one evil in webapp - avoiding dynamically generated data.

One such example is testing against dates/times. When doing something like `datetime.today()`, you are playing with fire unless you are very careful with data arrangements and assertions. On top of that, relying on a specific day means you can't really test for edge cases e.g. 29th February. The solution for this is **mock objects**.

Mocks allow you to program parts of your application with pre-defined behaviour/data, so you can run your test many times within the same controlled environment.

```python
@mock.patch('some.app.models.timezone')
def test_should_set_the_expiry_date(self, mock_timezone):
    now =  timezone.make_aware(timezone.datetime(2016, 1, 1))
    mock_timezone.now.return_value = now
    expected_expiry_date = now + timedelta(days=1)
    token = Token.objects.create()
    self.assertEqual(token.expiry_date, expected_expiry_date)
```

Another common issue with test determinism is using hardcoded values, especially primary keys associated with a database. You should never use hardcoded values where possible.

```python
def test_id_bad(self):
    user = User.objects.create()
    update_user(user)
    # Refetch user from DB to show changes have saved
    # Bad, cannot assume ID is 1
    user_from_db = User.objects.get(id=1)
    self.assertEqual(user_from_db.field, 'updated')

def test_id_good(self):
    user = User.objects.create()
    update_user(user)
    # Refetch user from DB to show changes have saved
    # Good, makes no assumptions about the User's id
    user_from_db = User.objects.get(id=user.id)
    self.assertEqual(user_from_db.field, 'updated')
```

### Fast Tests

Tests should be fast for obvious reasons - nobody wants to wait around for an hour while they prove the one line of code the changed didn't break the entire system.

There are a couple of things you can do to ensure they can be as fast as possible.

**Don't create more test data than you need to**

To prove correctness for your specific test, how many objects do you really need to create? 

If you are proving that a maximum of 2 objects are returned from a list, then add 3. 

Adding more data than necessary to prove a specific case is pointless and slows down the system.

**Mock out "slow" interfaces**

In unit testing, slow running parts of a system tend to involve network requests such as external API calls or file transfers. Not only are these slow, but they can be non-deterministic, if for instance, an external service is down. Unit testing is about testing **your** code, and for many reasons, mocking should almost always be done for any kind of stream handling or external service.

Please note that integration tests still need to be run, and 100% should be. There is no guarantee than an external service may change their API unexpectedly, and your application should find that out before deploying. But for development purposes, unit tests should be run more often to allow for better development speed.

### Correct Tests

A correct test is one which tests what it should be testing - very meta :-). Or better put, it's one in which the initial state and final assertions can provide a high enough certainty that the code works, relative to the contract of the test name.

In short:

* Have meaningful test names
* Create an initial state which can help to prove assertions
* Be sure to make specific assertions which can prove your test

Let's show some examples:

```python
def test_should_return_no_users_if_hide_is_True(self):
    """
    Here we are not proving that hide has any effect on the implementation
    because we have not created any users. The initial state of the system
    provides a false positive. 

    A good developer would make sure to also add another test proving users
    are returned if hide=False, so as to further negate other false assumptions.
    """
    users = service.get_users(hide=True)
    self.assertEqual(users.count(), 0)

def test_should_update_user_when_endpoint_PATCHed_done_badly(self):
    """
    Here we are arranging the initial data correctly, but our assertions are too
    weak. The tests says SHOULD UPDATE therefore proving the response comes back
    is not enough.

    See the next test for additional improvements.
    """
    user = User.objects.create()
    patch_data = {
        "password": "secure"
    }
    self.login(user)
    response = self.client.PATCH(self.url, patch_data)
    self.assertEqual(response.status_code, status.HTTP_200_OK)

def test_should_update_user_when_endpoint_PATCHed_done_better(self):
    """
    Now we add in a few subtle assertions that give us higher certainty
    that our code is correct.
    """
    user = User.objects.create()
    # After creating the user, prove their password is not already secure
    # as if our implementation just did nothing, this test would still pass
    self.assertNotEqual(user.password, 'secure')
    patch_data = {
        "password": "secure"
    }
    self.login(user)
    response = self.client.PATCH(self.url, patch_data)
    self.assertEqual(response.status_code, status.HTTP_200_OK)
    # After the status_code assertion, we need to prove the User has been
    # updated.
    db_user = User.objects.get(id=user.id)
    self.assertEqual(db_user.password, 'secure')

def test_should_showcase_a_good_way_to_test(self):
    """
    This test shows a good pattern to follow that helps in creating your tests
    by following the Arrange/Act/Assert method.

    The steps involve:
    1. Arrange your test data (as well as make assertions if necessary)
    2. Act on your system under test
    3. Assert the system is in the correct state after executing
    """
    # Arrange
    user = User.objects.create()
    self.assertEqual(user.username, 'user')
    self.login(user) # IMO belongs in Arrange as stateful

    # Act
    response = service.call(user)

    # Assert
    self.assertEqual(response.data, "something specific")

```

As said previously, creating many small, specific tests is a great way to focus effort and produce tests that are effective. Test Driven Development also helps by thinking about one test at a time.


### Other Help

This part is for Winter Circle advice. The above will eventually be moved to our book.

Unless for a good reason, all TestCases using DRF views should subclass **wintercircle.common.tests.WinterCircleAPITestCase**.

This provides some utility methods and fields to make tests easier to write, and more importantly **easier to read** and as said above this should always be at the front of your thoughts when writing tests.

Please inspect the code and see what utility methods are available to you. Even better would be to add your own if you feel they can help with "story tests" and DRY.

Thanks for reading, happy testing!


### Python

* Tests should start with test_