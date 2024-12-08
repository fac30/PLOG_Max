## Guidance
Answer the following questions considering the learning outcomes for
- [Week 013](https://learn.foundersandcoders.com/course/syllabus/developer/week13-TFB-design/learning-outcomes/)

Make sure to record evidence of your processes. You can use code snippets, screenshots or any other material to support your answers.

Do not fill in the feedback section. The Founders and Coders team will update this with feedback on your progress.

## Assessment
 ### 1. Learning outcomes
 * Expanded learning to work with a client, communicating effectively with the product owner through user stories and prioritisation - we asked the product owner at the start of the week to consolidate the user stories discussed the previous week and set priorities to each of them between low, medium and high. The idea being that we focus our MVP for the 4 weeks on the high priority items. By doing this, we were able to set the client's expectations to just that list of high priority items, which we reviewed together and moved some blocked user stories in the high priority list down the list of priorities and out of our backlog for the MVP, by mutual agreement.
 * We followed an agile methodology from the start of this project, were able to produce a sprint plan for this week based on what was achieved in the previous week and asses some kind of "team velocity". We re-assessed at the end of the week and, being very strict, we calculated that we had met 50% of the user stories we set out to at the start of the week. However, this assessment did not consider user stories that were almost finished and since then many of them have crossed the finish line, adjusting out completion to roughly 80% of what we set out instead. This will be taken into consideration to calculate our velocity for next week and plan that sprint.
 * My role in the team was QA and I was very busy reviewing everyone's code (with Jack's help). We did make a blunder and, in order to show the latest deployed to the product owner, skipped proper QA processes before our meeting and managed to introduce a bug into main. We later discovered that actually this was not a bug in the code but in the deployment. The point still stands though that the QA process is extremely important and if that were to happen in a professional setting it would be unacceptable. Since I will continue to take the job of QA very seriously even in the excitment of wanting to have the latest changes to deployed to show off to a client or stakeholder.
 * We also introduced testing in quite a serious way this week as part of a QA initiative on the back of the workshop we did last Friday. Everytime someone implements a new feature they ened to write and pass tests before making a PR. Every time someone makes a new PR they need to run all tests and ensure they all pass before making the pull request. If tests do not pass, they need to investigate why and either fix the code or update the tests. Our tests are written using Jest and RTL, they vary in complexity and essentially test every feature and important elements on the page are rendered, this is an example of a very simple example for the home page:
```typescript
import "@testing-library/jest-dom";
import { render, screen } from "@testing-library/react";
import Home from "../src/app/page";

describe("Home", () => {
  it("renders a heading", () => {
    render(<Home />);
    const heading = screen.getByRole("heading", { level: 1 });
    expect(heading).toBeInTheDocument();
  });
});
```

 ### 2. Show an example of some of the learning outcomes you have struggled with and/or would like to re-visit.
 * We struggled with the database initialisation, Jack volunteered early on the fix it using a database manager class to collect all methods we need to initialise, seed, access and write to the database. However, it still appears we are getting errors and trying to initialise the database multiple times, in loops. My input on this has only been through PR reviews until now and I was trying to make helpful suggestions but it seemed to create different issues:
![image](https://github.com/user-attachments/assets/b996f9a2-8cc6-4bcc-9dbe-4804925a8c4a)
The problem is that we are calling seed methods inside the initialiseDb method but then in the seed methods themselves, the initialiseDb method is called, creating a loop:
```typescript
class DatabaseManager {
  dbInstance: RxDatabase | null = null; // we need to check for a value inside here before ever calling inistialiseDb

  async initialiseDatabase() {
    if (!this.dbInstance) {
      this.dbInstance = await createRxDatabase({
        name: "database",
        storage: getRxStorageDexie(),
      });
      ...
      await this.seedCategories();
      await this.seedToolkitItems();
      ...
    }
    return this.dbInstance;
  }
  
  async seedCategories() {
  try {
      const db = await this.initialiseDatabase(); //HERE we are calling inituialiseDb again!
      if (!db) {
        console.error("Database initialisation failed");
        return;
      }
  ...
  }
  ...
}
```
The idea being that we split initialiseDb into 2 methods, one initalising and another accessing only (which will check if it is initialised and if not run the initialise method again. Then the seeding methods can only run the accessing method and not the initialise method.


## Feedback (For CF's)
> [**Course Facilitator name**]  
> [*What went well*]  
> [*Even better if*]
