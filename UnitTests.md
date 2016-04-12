#Thoughts on Unit Testing

If you want to be more effective unit testing there are a few tips which can help you.

###Create a Test List
  * Keep yourself organized by taking some time to create a test list. know ahead of time what you want to test
  * Synch to Pen and Paper will help you. Create quick notes and reminders while you go. 

###Name Things Well
  * Spend some time and give your tests a good name. A good solid name helps you understand what is being tested and the gaps in your test coverage
  * Name Format - [UnitOfWork_StateUnderTest_ExpectedBehavior[

###Structure Tests
  * Setup
  * Act
  * Assert

###Write Assert First and work Backwards
  * Doing this you will write less code to setup tests, be to the point, and wont get lost as often in your tests
  * This, along with Test Lists may be the single most important ways to improve the efficiency of your unit tests

###Fail your test before passing
  * You want to see your test fail before passing. Make sure it can fail
  * flip the assert() to see it fail

###Don't Test too Much
  * Make sure you pick the proper scope for your test
  * If the test is too big, this indicates you are either 
     * Testing too much
     * you have a code smell

###Humble Object
  * If a class is too hard to test, there is a change you can refactor out a humble object which may simplify your tests
  * Humble Objects are mostly identified on the ends of your application. Dom, Services, Entry points to your appication are normally the hardest to test
  * Create an abstraction on the hard parts to test. These new abstractions are your Humble Objects

###Refactor Tests
  * Because a unit test is working, does not mean it isn't fragile or hard to maintain. Take time to refactor the test after you are done to ensure it is easy to read, well structured and easy to maintain.
  * Boyscout Rule
