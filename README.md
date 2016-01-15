## Objectives

  1. Understand the value of removing logic from unexpected places
  2. Know the definition of presentation logic

## Outline

We cover MVC early on in the Rails topic but by now (Check out our previous example here https://github.com/learn-co-curriculum/sinatra-intro-to-mvc) it's been a while. The following lesson should just give them a high level reminder on what MVC is, and then our focus is on how we want our code to be in the expected places. Keep it high level and conceptual.

  * Quick briefer on MVC using the restaurant analogy
  * Reminder of Separation of Concerns
    * Models should really be doing most of the computation, heavy lifting.
    * Controller kinda chooses which model methods to pass along to view
    * View only cares about presentation
  * If run `rails s` and see a list of posts posted today...and then I see another list of posts posted yesterday that is the same *presentation logic* but different *business logic* behind it. 
    * They won't know the term business logic, but should learn so just introduce it here
    * As applications get larger and larger developers need to have a better intution for where code will live. By following MVC as strictly as possible we give our developers a fighting chance at that.
  * Your view should not have the code to hit the DB and filter the posts. That sounds like a job for the model. That isn't presentation logic
  * Separately as your presentations get more complex you'll start having presentation logic (things like present username if logged in)
    * Initially we can put that logic in our view just directly, but eventually we might want to share code across views. 

