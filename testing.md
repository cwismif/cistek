## Testing
---
I’ve read that if functionality has not been thoroughly unit-tested, then it hasn’t been implemented. The idea of putting poorly unit tested application in *production* is dangerous.  Despite there being checkpoints further along the CI pipeline such as end-to-end tests in *staging*, they should not be relied upon and should fail rarely because the testing in prior build steps has caught the issue already.  

I use two methodologies to write tests:

#### BDD
Preferable application black-box tests are written first. It helps me identify any gaps in my understanding of the requirements and shape the API of the service early.

#### TDD 
Write a small, simple test for the functionality required, watch it fail, write just enough code to make it pass, refactor, make sure all tests are still passing, repeat.  This has great advantages in:
* Granting fast feedback
* Leaving you with a set of regression tests
* Allowing the client to be considered first. The unit tests act as clients, so in writing them you have their perspective which can:
  * clarify the appropriate abstraction 
  * identify tight coupling (caused by leaky abstractions)
  * keep responsibilities granular and separate 
  * identify failures in the law of demeter (if a unit test has many mocked classes) 
  * if the tests are focussed and precise, the code is probably more likely to be the same. So by writing clean tests, you write clean code. 
  * Common helper functions, factories for test objects and mocks  

However,  tests can be written in a way that couples them closely with the implementation, this means refactoring is 90% changing unit tests. Unit testing abstractions helps decouple and requires fewer changes to tests. 

A common complaint against TDD is that refactoring the production code means you have to change lots of unit tests, often leading to the vast majority of the work being these changes. This happens when the unit tests are tightly coupled with implementation of the production code.  However, by writing unit tests of abstractions, not implementations, then refactoring results in fewer changes to the unit tests. It also promotes cleaner abstractions in the production code.  

If a class has complex logic, then finely-grained unit tests are invaluable in covering all of the branches of execution that can occur because of that logic. Although, it is important not to write unit tests that essentially test a library or other dependency. You should test that the use of the library is correct by mocking the library.  

If the library contains static functions hence are difficult to mock, you can write a class of your own to wrap the library and test that. 

