## Guidance
Answer the following questions considering the learning outcomes for
- [Week 07](https://learn.foundersandcoders.com/course/syllabus/developer/week07-project04-authentication/learning-outcomes/)

Make sure to record evidence of your processes. You can use code snippets, screenshots or any other material to support your answers.

Do not fill in the feedback section. The Founders and Coders team will update this with feedback on your progress.

## Assessment
 ### 1. Learning outcomes
 * Working on the backend with a reviewing role for the frontend, I learned from the workshops initially how to set up authentication more or less from scratch using only cookie parser and SQL tables
 * My implementation of of this on the backend became quite complex because of my use (or misuse - see in the next section) of supabase postgresql. I set up customers and sessions tables with 4 endpoints that would mean the frontend could easily authenticate each case - register, login, logout and check-session.
 * Taking the example of one endpoint for logging in - the frontend would send a POST request to our backend at /login with a payload containing username and password_hash (we decided to has on the frontend perhaps not best practice for security), in the backedn we check these credentials are correct, create a session on the sessions table and return the whole customer object and session object:
```typescript
const { username, password_hash } = req.body;
const customers: Customer[] = await dbGet<Customer>('customers', {
  where: { username },
  join: {
    relatedTable: 'locations',
    foreignKey: 'location_id',
    columns: ['region', 'country']
  }
});
 /*BELOW is the security check for successful login! Does the customer exist and is the password correct? Either way we want to return the same message to not display which part of the combination is causing errors in case a malicious actor is behind the screen on the other side and can use this information to try again */
if (customers.length === 0 || customers[0].password_hash != password_hash) {
  res.status(401).json({ message: 'Invalid username or password' });
  return;
}
const customer = customers[0];
const sessions = await dbGet<Session>('sessions', { where: { customer_id: customer.id } });
if (sessions.length > 0) {
  const session = sessions[0];
  await dbDelete('sessions', session.id); // DELETE previous session if one existed
}
// CREATE new user session
const newSession = await dbPost('sessions', {
  customer_id: customer.id,
  expires_at: new Date(Date.now() + 7 * 24 * 60 * 60 * 1000),
});
res.status(201).json({ message: 'Login successful', customer, session: newSession });
```
 * Mitigating security threats (cross-site forgeries), I understand the priciple and implementation of this in the workshop (which was a mix of frontend and backend in the same server) using cookie parser, signing cookies and checking that they match the session information on our database. However, with a full-stack project seperated frontend and backend it was not immediately clear which application should be serving the cookies. Eventually - it is the backend because the frontend is static and cannot parse cookies directly. This is still to be implemented and will be part of the refactoring of the backend as the project will transition to using express sessions rather than doing everything from scratch.
 * As a reviewer of the frontend, I did not directly interact with it but managed to understand and make contructive comments throughout. I also developed a side project for a react template repo in which I implemented React router for a very generic use case. I kept it very simple for a template so my App.tsx returns the below and then a very simple Navbar to link to each route.
```tsx
<>
   <Header />
     <Router>
       <Routes>
         <Route path="/" element={<Home />} />
         <Route path="/about" element={<About />} />
         <Route path="/contact" element={<Contact />} />
       </Routes>
     </Router>
   <Footer />
 </>
```
 * Use context I wrapped my head around and implemented on the previous project alongside Jason to store playlist data in an unresolved promise that gets resolved 2 pages after the request is sent (slow API). This week we used local storage because it was what the frontend team knew how to use best. In hindsight I should have provided more support to get use context working and I will strive to do this in he next week.

 ### 2. Show an example of some of the learning outcomes you have struggled with and/or would like to re-visit.
 * It was my impression that using supabase and postgresql somehow limited what you can do in simple SQL and meant that I had to write very long functions (sometimes making 2 calls to the database for many-to-many relationships).
 * I have now found out that this struggle could have been avoided as in fact it was just the structure of my many-to-many tables were not formatted exactly write to use natively supported joins when those foreign keys are properly defined in postgresql. My many-to-many table looked like this:
```sql
CREATE TABLE transactions_vinyls (
  id SERIAL PRIMARY KEY,
  transaction_id INTEGER REFERENCES transactions(id) ON DELETE CASCADE,
  vinyl_id INTEGER REFERENCES vinyls(id) ON DELETE CASCADE,
  UNIQUE (transaction_id, vinyl_id)
);
```
Whereas it should have looked like this:
```sql
CREATE TABLE transactions_vinyls (
  transaction_id INTEGER REFERENCES transactions,
  vinyl_id INTEGER REFERENCES vinyls,
  PRIMARY KEY (transaction_id, vinyl_id),
);
```
 * And this means that a complicated join that I was using 2 calls to the database to perform and multiple functions to for example call the transactions by customer_id and bring up all the associated vinyls (which connects to vinyls through the transactions_vinyls table above) could be simply achieved with the native functionality like this
```typescript
const data = await supabase
 .from('transactions')
 .select(`
     *, 
     vinyls ( * ) //THIS is very simply calling all the vinyls listed in the transactions_vinyls table that are associated to the transaction
 `)
 .eq('customer_id', 1);
```

## Feedback (For CF's)
> [**Course Facilitator name**]
 
Alexander

> [*What went well*]

Thorough implementation of authentication system with security considerations. Good understanding of cookie handling between frontend and backend. Clear explanation of database relationship issues with specific examples of both problematic and correct table structures.

> [*Even better if*]

Implement and document the express-session migration you mentioned, since you identified it as a better solution than your current setup. Your useContext experience from the previous project could have been valuable - show how you would implement it here.
