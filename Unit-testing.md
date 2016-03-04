LMMS as a unit test framework.

It's currently almost empty, but it's just waiting for you to add more tests.

# Running the tests

To run the tests, you must first compile them.

Simply follow the same steps as for compiling LMMS. But, when it comes to compiling LMMS type the following command instead:

```
make tests
```

Then, type the following command to run the tests:

```
tests/tests
```

You should get an output similar to the following:

```
cyr@lmms-dev:~/lmms/build$ tests/tests
>> Will run 1 test suites
********* Start testing of ProjectVersionTest *********
Config: Using QTest library 4.8.6, Qt 4.8.6
PASS   : ProjectVersionTest::initTestCase()
PASS   : ProjectVersionTest::test()
PASS   : ProjectVersionTest::cleanupTestCase()
Totals: 3 passed, 0 failed, 0 skipped
********* Finished testing of ProjectVersionTest *********
<< 0 out of 1 test suites failed.
cyr@lmms-dev:~/lmms/build$
```