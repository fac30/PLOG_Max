## Guidance
Answer the following questions considering the learning outcomes for
- [Week 06](https://learn.foundersandcoders.com/course/syllabus/developer/week06-project04-databases/learning-outcomes/)

Make sure to record evidence of your processes. You can use code snippets, screenshots or any other material to support your answers.

Do not fill in the feedback section. The Founders and Coders team will update this with feedback on your progress.

## Assessment
 ### 1. Learning outcomes
 * Having some background in database design, I found it a very useful exercise to go through ontology with Jason and the whole group of how an e-commerce database ecosystem might be set up and named
 * As a group, we explored this further with our idea of a vinyl store and made quite a detailed database structure, our overall schema included some one to many and mny to many relationships which we fully explored by decoupling those into tables and making explicit foreign key relationships:
 ![image](https://github.com/user-attachments/assets/d5f4b57a-10dc-4a54-8c54-47aef6ca6e92)
 * I delved deeper into databses and learned more about how to trigger functions for calculated columns which I used for automated categorisation of vinyls based on price, release date and whether it is a new release or not using thresholds from other tables in the database. Here is an example of the SQL function and trigger for one of those:
 ```sql
CREATE OR REPLACE FUNCTION update_time_period() RETURNS TRIGGER AS $$
BEGIN
    NEW.time_period_id := (SELECT id FROM time_periods
                          WHERE EXTRACT(YEAR FROM NEW.release_date) BETWEEN period_start AND period_end);
    RETURN NEW;
END;

CREATE TRIGGER trigger_update_time_period
    BEFORE INSERT OR UPDATE ON vinyls
    FOR EACH ROW
    EXECUTE FUNCTION update_time_period();
```
 * My next step with databases and access to it from our API is looking at how we can dynamically generate endpoints every time the server starts to read the database names and create the endpoint for those which could work like this (no error handling for simplicity):
 ```typescript
async function getTableNames(): Promise<string[]> {
  const queryText = `SELECT tablename FROM pg_tables WHERE schemaname = 'public';`;
  const result = await pool.query(queryText);
  return result.rows.map(row => row.tablename);
}

async function createDynamicRoutes() {
  const tables = await getTableNames();
  tables.forEach((table) => {
    app.get(`/${table}`, async (req: Request, res: Response) => {
      const queryText = `SELECT * FROM ${table};`;
      const result = await pool.query(queryText);
      res.json(result.rows); // Return all rows as JSON
    })
  })
}
```
 * Once this is establish, I have in mind to also query the tables dynamically with some get parameters like columns, where, on and 'and' for which we can populate any values and generate an SQL query to limit columns, rows, joins etc based on any user inputs.
 * Although I didn't directly work on the React front-end, I did alot of code reviews and gave feedback on code, learning more about asynchronous React operations with Josh's fetchData code and how the database data is accessed inside the React app, ensuring that as we scale up our database data, the React card components are passed down a key prop to keep track.

 ### 2. Show an example of some of the learning outcomes you have struggled with and/or would like to re-visit.
 * I spent a long time to come up with the simple calculated solution in SQL, my first pass with this was actually a very complicated SQL mapping and mathematical operations being performed on every single element I input into the database. However, this meant that this would only work for the specific SQL query I had written and was less adaptive for data coming in from different sources, as well as being less efficient.
 * I had discussion with our consultant Ben about this and would like to revisit a solution that he suggested using Views at some point (perhaps not for this project though given time constraints) - Views are like temporary tables that are used for performing calculations on other tables

## Feedback (For CF's)
> [**Course Facilitator name**]
 
Alexander

> [*What went well*]  

I absolutely love your approach of trying to make your solutions as generic as possible. In this case I am not sure if it makes sense but, in any case, I love it. It's simple and sophisticated.
