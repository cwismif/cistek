## Testing
---
I’ve read that if functionality has not been thoroughly unit-tested, then it hasn’t been implemented. The idea of putting poorly unit tested application in *production* is dangerous.  Despite there being checkpoints further along the CI pipeline such as end-to-end tests in *staging*, they should not be relied upon and should fail rarely because the testing in prior build steps has caught the issue already.  

#### BDD
Preferable application black-box tests are written first. It helps me identify any gaps in my understanding of the requirements and shape the API of the service early.

#### TDD 
##### Method
Write a small, simple test for the functionality required, watch it fail, write just enough code to make it pass, refactor, make sure all tests are still passing, repeat.  
###### Why?
* Gives fast feedback
* Leaves you with a set of regression tests
* Allowing the client to be considered first. The unit tests act as clients, so in writing them you have their perspective in mind.
* Helps clarify the appropriate abstraction (a good abstraction is easier for me to clearly define when thinking as the client)  
* Identifies tight coupling (caused by leaky abstractions that your test will have to deal with)
* Keeps responsibilities granular and separate (you have to mock both a database client and http client and tests include business logic)
* Identified failures in the law of demeter (if a unit test has many mocked classes) 
* If the tests are focussed and precise, the code is probably more likely to be the same. So by writing clean tests, you write clean code. 
* Common helper functions, factories for test objects and mocks

##### Trade offs / complications

* Tests that are tightly-coupled with the production code they tests can make refactoring a lot more time-consuming, and developing the skills to write loosely coupled tests will take time.
* Unit tests are often neglected in terms of clean code. You often have replication of tests or tests that overlap.  The solution is to keep tests clean, use common libraries, good testing practices.
* If a class has complex logic, then finely-grained unit tests are invaluable in covering all of the branches of execution that can occur because of that logic. Although, it is important not to write unit tests that essentially test a library or other dependency. You should test that the use of the library is correct by mocking the library or if not possible due to final classes or static functions, wrap the library in a thin object that just delegates. Then mock and assert on the delegate.

Please see ./dev-ops.md for how I would integrate tests into builds and CI workflows.

#### Integration tests - TODO
#### Contract tests - TODO
#### E2E tests in staging - TODO
