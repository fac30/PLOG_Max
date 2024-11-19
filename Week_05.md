## Guidance
Answer the following questions considering the learning outcomes for
- [Week 05](https://learn.foundersandcoders.com/course/syllabus/developer/week05-project03-test-deploy/learning-outcomes/)

Make sure to record evidence of your processes. You can use code snippets, screenshots or any other material to support your answers.

Do not fill in the feedback section. The Founders and Coders team will update this with feedback on your progress.

## Assessment
 ### 1. Learning outcomes
 * Learned about testing through the cypress workshops and reviewing Jason's PRs of our app tests in cypress, I thought it was particularly useful to automate user inputs but also repeat actions after each test as our app has no menu and has a sequential order of pages, we used cypress' beforeEach method like this:
```typescript
	context ('Input Page', () => {
		beforeEach(() => {
			cy.get( app.input.name ).type( user.valid );
			cy.get( app.button.landing ).click();
		});
```
 * Useful to look into static and 'dynamic' deployment methodologies using S3 bucket for static and EC2 instance for dynamic in AWS, managed to use AWS' secrets manager to deploy our backend on an EC2 instance without using the `.env` file, this is a simple version of it (without error handling for simplicity)
```typescript
const secret_name = 'moodtimesecrets';
const client = new SecretsManagerClient({
  region: 'eu-west-2',
});
let response = await client.send(
  new GetSecretValueCommand({
    SecretId: secret_name,
    VersionStage: 'AWSCURRENT',
  })
);
const secretString = response.SecretString;
const secrets = JSON.parse(secretString); // from here retrieve specific keys from JSON object
```
 * I worked on a github actions workflow that would build our front end react app and deploy it on our S3 bucket inside AWS but got stuck with permissions on AWS side, perhaps there is more investigation to be had looking into web identity IAM roles in AWS and how it can give necessary permissions to our github repo (tbc), but build part of the github workflow was working as expected

 ### 2. Show an example of some of the learning outcomes you have struggled with and/or would like to re-visit.
 * I didn't find the time to update our API documentation in swagger with our latest changes to our endpoints and data schemas
 * It would have been useful to get involved in cypress testing myself this week but we decided early on in the week that we needed a proper division of labour to get through all pending tasks and make our app functional, and that we would learn by properly reviewing each other's PRs, which was reasonably successful but not as insightful as developing myself in my opinion
 * I had trouble with CI/CD deployment using github actions but was happy to see there are other ways to do it using AWS CDK thanks to Jack, to be investigated!

## Feedback (For CF's)
> [**Course Facilitator name**]

Alexander

> [*What went well*]

Strong implementation of AWS Secrets Manager for secure deployment. Good understanding of Cypress test automation with clear examples of beforeEach usage. Practical approach to team collaboration through PR reviews.

> [*Even better if*]

Document your GitHub Actions workflow attempts, even if they failed. The AWS permissions issues you encountered would be valuable to track for future reference.
