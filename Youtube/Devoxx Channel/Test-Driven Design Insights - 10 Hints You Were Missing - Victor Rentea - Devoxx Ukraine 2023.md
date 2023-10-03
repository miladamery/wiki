<h2>Why we love Mocks</h2>
1. Isolated Tests
2. Fast
3. Test less logic

<h2>Why we hate Mocks</h2>
1. Uncaught bugs in production 😨
2. Fragile Tests 💔
3. Unreadable Tests

Sometimes we are not testing any behavior. We are testing a couple of other dependencies in other words, we are testing Mocks. One reason some people hate mocks is that they are writing so many lines of code just to setup mocks and actually not testing anything. Problem is not `Mocks`, But is the design and architecture.

In methods/functions that has a lot of infrastructure code or are completely made of infrastructure code its better 1. not to test them in isolation 2. write integration-test for them.

<h2>Honeycomb Testing Strategy</h2>
This strategy says write more integration tests instead of mocking. Use `TestContainer`, `WireMock` etc to create all these external dependencies you need. But when you face a challenging logic from domain. Isolate  that, extract it and <b>Unit Test</b> that challenging logic. In this strategy and in unit tests use Mocks for external dependencies.

* Unit Tests give you most <span style="color: skyblue">Design Feedback</span>
* More Complexity => Better Design
* Mock Roles, Not Objects
* Before mocking a dependency, <b>Clarify its responsibility</b>

<h2> No one said unit test means targeting a single class</h2>
Do not write <b>Solitary Unit Tests</b> (testing each class in isolation), Write <b>Social Unit Tests</b> (for "components" (group of objects) with clear responsibilities).
<h3>Robust Unit Testing requires identifying responsibilities</h3>

<h2>Do not start mocking things around unless you clarify their responsibility</h2>


<h2>Hints</h2>
* Unit Tests tell you to Simplify signatures and data structures
	* Tailored Data Structures
	* Agnostic Domain
* Unit Tests puts design pressure on <b>Highly Complex Login</b>
	* Split by layers of Abstraction
		* Vertically
		* Horizontally (Split Unrelated Complexity)
* Unit Tests promotes Functional Programming (Pure function + Immutable Objects)


