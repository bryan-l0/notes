## Stakeholders
**Primary:** People who use the system.  
**Secondary:** People who provide input or receieve output from the system.  
**Tertiary:** People who do not interact with the system, but are still affected by it.  
**Facilitating:** People who are involved in the design, development, and maintenance of the system.

## Personas
- Users become tangible and personal - solving needs of real people.
- Different personas for the same user.
- Incorporate personas into user stories, that captures what the user wants to achieve.

## User Stories
**Card:** Stories can be written on notecards/post it notes.  
**Conversation:** Details behind the story come out during conversations with the product owner.  
**Confirmation:** Tests will confirm that the story was delivered properly.

## Waterfall - Requirements, Design, Implementation, Verification, Maintenance
**Disadvantages:** Requirements change, understanding of requirements change, people make up requirements, limiting the final deliverable. Risky by only having 1 big delivery at the end because risks are not assessed early. Produces a tickbox culture which may hide the actual intent. High complexity and overhead.

## Agile Manifesto
1. *Individuals and interaction* over process and tools.
1. *Working software* over comprehensive documentation.
1. *Customer collaboration* over contract negotiation.
1. *Responding to change* over following a plan.

## Agile Principles
**Customer Involvement:** Customer is closely involved throughout the development process, to prioritise new system requirements, and evaluate iterations.  
**Iterative, Incremental Delivery:** Delivers value to customer after each sprint, on time. Requires developers that have the ability to cope with intense involvement.  
**Adapt, Embrace Change:** Expect the system to change and adapt.  
**Manage Risk** and **Maintain Simplicity**, whilst **Eliminating Complexity**.

## Extreme Programming (XP) - Simplicity, Communication, Feedback, Respect, Courage
**Planning:** Customers, managers, and developers establish requirements at the start of each iteration, by using user stories and CRC story cards, understandable to all parties.  
**On-site Customer:** Development team has access to a customer who will be using the product.  
**Test Driven Development:** Developers write acceptance tests before implementing features. Customers write functional tests to test the features.  
**Pair Programming:** Developers pair up to tackle a user story in unison.  
**Small Releases:** Start with the smallest required feature set, release early and often with small improvements.  
**Continuous Integration:** Integrate new code into the system often, and if they fail the functional tests, they are discarded.  
**Refactoring:** The design should be evolved throughout the development to keep it as simple as possible.  
**Collective Ownership:** Code is owned by the entire team, and anyone can make changes anywhere if necessary.  
**Metaphor:** Customers, managers, and developers construct a metaphor to model the system after.  
**Simple Design:** Keep the design as simple as possible.  
**Coding Standards:** Use a strict and well defined coding standard throughout the team.  
**Sustainable Pace:** Pacing the user stories well to prevent team burnout.

## SCRUM
1. Envision the project and produce a product backlog, a collection of user stories.
1. Iterate:
    1. Sprint planning by producing sprint backlog, by prioritising user stories.
    1. Check progress and show progress via burndown charts.
    1. Review sprint.
1. Repeat iteration process until finished.

## Risk Management
**Project Risks** affect the project schedule or resources, e.g. loss of memebers.  
**Product Risks** affect the quality or performance, e.g. failure of a component to perform as expected.  
**Business Risks** affect organisation, e.g. company takeover.

## Model-View-Controller


### Advantages
1. Loose coupling.
1. Clear separation of concerns.
1. Easy to maintain (Especially view).
1. Promotes re-use.
1. Supports multiple views.

### Structure - All should work without the other components
**Model** contains domain data. Mapping to real world objects/concepts, and contains relevant logic.  
**View** renders data for display. Visualises state of model to user, can consist of many subviews. User input is passed to controller.  
**Controller** as the pillar to mediate between model and view. Translates inputs into operations onto the model.

### Models
**Passive Model:** Model responds to state change operations from view, but controller handles the update to view.  
**Active Model:** Model responds to state change operations and notifies view of change, using an observer pattern.  
**iOS Model:** Controller handles updates to both model and view.

## Testing
Try to identity and correct *software defects*. Ensure software is *fit for purpose*, which is when a system is good enough for its intended use.  
**Verification Testing:** Verifies software correctly implements a specific function.  
**Validation Testing:** Verifies software satisfies customer requirements.  

### Testing Methods
**Automated Tests:** Tests run automatically, and compares test input to expected output.  
**Manual Tests:** Testes interact with software to ensure it behaves as expected.  
**Code Inspection:** Source code is manually inspected and reviewed (static testing).

### Development Testing
Carried out by developers during implementation. Includes defect and validation testing, to ensure the software works as intended by developers. Mostly uses automated testing and code inspection. Emphasis on defect testing, interleaved with debugging.  
**Partitioning:** Covers a sensible range. Consider the coverage of a test. If two tests cover the same instructions at the same step, they are equivalent. If you have N partitions, you need >N tests. I.e. testing addition function, where a and b's positive/negative property creates the partitions.  
**Black Box Testing** assuming no knowledge of code. **White Box Testing** is where testing is done with inspection of code, to refine the equivalence partitions.  

### Coverage Metrics
Can be used to design tests.  
**Control Flow:** Function, Statement, Branch, Decision coverage.  
**Data Flow:** Computational Uses (C-Use) - used to compute other variables. Predicate Variable Uses (P-Use) - flow deciding use.  
**Mutation Testing:** Testing robustness and redundancy.

### Component Testing
Tests the subcomponents of the software, e.g. collections of interacting classes, especially for coding in teams. This focuses on component interfaces.  

- Base test cases on how the component is used in your system.
- Build test cases that cause the component to fail (invalid parameters, exceed specification).
- Explore states of components.
- Test concurrency and timing issues.

Usually JUnit and Code Inspection is used for this.

### System Testing
- Tests multiple interacting components of the system (integration testing) up to entire working software.
- Highlights problems caused by:
    - Emergent behaviour of interacting components.
    - Usability issues.
    - Missing requirements.

### Acceptance Testing
Carried out by developers, testers, and users. Focuses on validation testing, to ensure the software works as intended by users. Mostly uses manual testing.  
**User Story Acceptance Criteria:** Tests that user stories are completed fully, by defining a acceptance criteria, testing to them, in conjunction to unit tests.  
**Release Testing:** Includes final tests before a system is released to users. Typically black-box. Includes requirements-based testing, scenario testing, and performance testing.  
**User Testing:** Alpha testing by users with the development team. Beta testing by users in their environment. User acceptance testing where customers test the software to decide if the specifications have been met.  

## Test-Driven Development
1. You may not write production code until you have written a failing unit test.
1. You may not write more of a unit test than is sufficient to fail.
1. You may not write more productino code than is sufficient to pass the currently failing test.

## Agile Methods

### Scrum
- Empirical framework for developing and sustaining complex products.
- Lightweight, simple to understand, hard to master.
- Built on principles of inspection, adaption, and transparency.

**Roles:** Scrum Master, Team, Product Owner.  
**Artifacts & Tools:** Burndown Chart, Velocity Chart, Calendar, Story Map.  
**Backlogs:** Product Backlog, Release Backlog, Sprint Backlog.  
**Meetings:** Daily Scrum, Sprint Planning, Sprint Review, Retrospective, Backlog Grooming.

### Kanban

**Visualise Work:** Create a visual model of work to observe the flow of working through the Kanban system.  
**Limit Work in Progress:** Limiting unfinished work helps reduce the time taken for an item to travel through the Kanban system.  
**Focus on Flow:** By using WIP limits and developing team-driven policies, the Kanban system can be optimised to improve the flow of work.  
**Continuous Improvement:** Teams can meansure their effectiveness by measuring flow, quality, throughput, lead times, etc. Experiments and analysis can change the system to improve effectiveness.  
**Kanban Tasks:** Comprises of task name, description, time estimate, assigned members, type of task, and blocker.

### Feature Driven Development
- Includes a chief architect and chief programmer. Has a light, up-front architectural model.
1. Develop overall shape object model.
1. Build a feature list.
1. Plan by feature.
1. Iterative design by feature.

### Adaptive Software Development
- Leadership-collaboration management philosophy.
1. Iterative development.
1. Feature based planning.
1. Customer focus group reviews.

### Crystal Methods
- *Crystal* is a family of stretch to fit, human powered software development methodologies.
- Emphasis on collaboration, good citizenship, and cooperation.

**Frequent Delivery:** Deliverables may not have to go into productino, but stakeholders can see them to provide feedback.  
**Continual Feedback:** Entire team meets regularly to discuss project activities, and meets with stakeholders regularly to make sure the project is heading in the expected direction, and communicate any potential new project changing discoveries.  
**Constant Communication:** Small projects done in same room, large projects done in same facility. Frequent access to people defining the requirements.  
**Safety:** People being effective and communicate truth without fear of reprisal.  
**Focus:** Team members should know the top priority items each member should be working on, and should be given time to complete them wtihout interruption.  
**Access to Users:** Expects project team to have access to users of the system being built.  
**Automated Tests and Integration** 