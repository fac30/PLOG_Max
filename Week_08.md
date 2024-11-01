## Guidance
Answer the following questions considering the learning outcomes for
- [Week 08](https://learn.foundersandcoders.com/course/syllabus/developer/week08-project04-test-deploy/learning-outcomes/)

Make sure to record evidence of your processes. You can use code snippets, screenshots or any other material to support your answers.

Do not fill in the feedback section. The Founders and Coders team will update this with feedback on your progress.

## Assessment
 ### 1. Learning outcomes
 * I wrote endpoint tests in Jest and supertest before starting to design the endpoints and most of those tests were still valid by the time the app was designed, this is an example of a test to GET a vinykl by id:
```typescript
describe('GET /vinyls/:id', () => {
    it('should retrieve a vinyl by ID', async () => {
      const vinylId = 1;
      const response = await request(app).get(`/vinyls/${vinylId}`);
      expect(response.status).toBe(200);
      expect(response.body.id).toBe(vinylId);
    });
});
```
 * In typescript I started implementing a lot of the learnings from execute programme, one example of this was using Partial<> to call interfaces for deconstructed customer objects where sometimes only certain keys would be used for logging in (username and password), or different keys would be retrieved from a request that would be transformed before being sent to the database (password becomes password_hash):
```typescript
const { password, ...rest } = req.body as Partial<Customer>;
```
 * I could have spent more time setting up appropriate environments for staging and production as I had done in the previous project, I could have pushed this further on this project.
 * I learned AWS CDK deployment, infrastructure as code through the workshop and through Marika's work on deploying our app, similarly with CI/CD pipelines which was implemented through github actions that triggers the CDK deployment in our project.
 * Permissions, AWS deployment types I learned a lot of from the previous project and was able to provide some support to Marika from time to time (although most of the support came from Levi).
 * Other learnings this week included swagger documentation which I'm very happy with and think is supper useful, in hindsight this would have been useful to implement earlier so it could have been a reference for the team working on the frontend instead of me writing endpoint details in Discord
![image](https://github.com/user-attachments/assets/23a25b10-04fd-4dc8-a216-2ab88acb97d4)


 ### 2. Show an example of some of the learning outcomes you have struggled with and/or would like to re-visit.
 * This week we struggled with finishing up authorisation on the frontend and including credentials in fetch requests for express session to detect the cookies. Setting the CORS policy to accept credentials while also only accepting requests from urls of our deployed app and local environment, in the end the solution on the backend to enable this was a simple CORS policy (as below) while on the frontend the request needed a header of `credentials: 'include'`.
```typescript
const allowedOrigins = [
  "http://localhost:5173",
  "http://localhost:5174",
  "http://pro04cdkstack-sitebucket397a1860-yjssa5llco8j.s3-website.eu-west-2.amazonaws.com",
];

app.use(
  cors({
    origin: (origin: string | undefined, callback) => {
      if (!origin || allowedOrigins.includes(origin)) {
        callback(null, true);
      } else {
        callback(new Error("Not allowed by CORS"));
      }
    },
    credentials: true,
  })
);
```

## Feedback (For CF's)
> [**Course Facilitator name**]  
> [*What went well*]  
> [*Even better if*]
