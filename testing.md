## Testing
---
I’ve read that if functionality has not been thoroughly unit-tested, then it hasn’t been implemented. The idea of putting such applications in *production* is very comfortable.  Despite there being checkpoints further along the CI pipeline such as end-to-end tests in *staging*, they should not be relied upon and should fail rarely because the testing in prior build steps has caught the issue already.  

I use two methodologies are used to write tests:

#### BDD

Preferable application black-box tests are written first. It helps me identify any gaps in my understanding of the requirements early.  Although they won’t pass until you’ve finished lots of TDD unit tests and integration tests hence implemented the functionality.

#### TDD 

Write a small, simple test for the functionality required, watch it fail, write just enough code to make it pass, refactor, make sure all tests are still passing, repeat. This has great advantages in:

* Granting fast feedback and comprehensive regression tests  
* Allowing the client to be considered first. The unit tests act as clients, so in writing them you have another perspective which can highlight clear abstractions to use, coupling, multiple responsibilities,  law of demeter  
* Keeping code focused and concise, as long as the test is, so keep the tests similarly as clean as production code.  Use common helper functions, factories for test objects and mocks.

However,  tests can be written in a way that couples them closely with the implementation, this means refactoring is 90% changing unit tests. Unit testing abstractions helps decouple and requires fewer changes to tests. 
It also promotes cleaner abstractions in the production code. 

If a class has complex logic, then finely-grained unit tests are invaluable, but many unit tests exist that essentially test language or library features and add no value but a little cost.

