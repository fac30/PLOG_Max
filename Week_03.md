## Guidance
Answer the following questions considering the learning outcomes for
- [Week 03](https://learn.foundersandcoders.com/course/syllabus/developer/week03-project03-server/learning-outcomes/)

Make sure to record evidence of your processes. You can use code snippets, screenshots or any other material to support your answers.

Do not fill in the feedback section. The Founders and Coders team will update this with feedback on your progress.

## Assessment
 ### 1. Learning outcomes
 * In the express workshop frm the previous week, I learned the fundamentals of setting up a server, it wa sinteresting this week to work alongside Jason and set this up using Typescript from a configuration and QA perspective, I had to understand and document it
 * Configuring the project for typescript was more effort than I thought but I am now looking back understanding better how to do it next time, current configuration file could be improved probably by using commonJS instead of NodeNext:
```json
{
  "compilerOptions": {
    "target": "es2016",
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true,
    "strict": false,
    "skipLibCheck": true,
    "outDir": "dist",
    "rootDir": "./src",
    "resolveJsonModule": true
  },
  "include": ["src"],
  "exclude": ["node_modules", "dist", "docs", ".github", ".vscode", ".gitignore", ".prettierrc.json", "eslint.config.mjs", "package-lock.json", "package.json", "README.md"]
}

```
 * In configuration, I defined the data schemas coming in and out of our API endpoints, this was particularly well suited using typescript as I could do this and define our custom interfaces/types for the project at the same time and really meant I could get to grips with typescript syntax in practice, an example of a custom interface and type I set up is:
 ```typescript
export interface track {
    title: string,
    artist: string,
    album: string,
    releaseDate: Date,
    duration: number
};

export type spotifyResponse = track[];
```
 * Having set that up from the start made documenting the API using Swagger easier as we had endpoints and data schemas cleary defined, thismeant my swagger documentation object definitions for endpoints could reference those directly
```typescript
'/spotify': {
        get: {
          tags: ['Dev Endpoints'],
          summary: 'Search for tracks and return a playlist based on a predefined genre, date and spotify feature metric settings.',
          requestBody: {
            required: true,
            content: {
              'application/json': {
                schema: { $ref: '#/components/schemas/SpotifyQuery' },
              },
            },
          },
          responses: {
            '200': {
              description: 'Return a playlist to be delivered to the user.',
              content: {
                'application/json': {
                  schema: { $ref: '#/components/schemas/SpotifyResponse' },
                },
              },
            },
            '404': {
              description: 'No songs found.',
            },
          },
        },
      }
```
 * vdfv

 ### 2. Show an example of some of the learning outcomes you have struggled with and/or would like to re-visit.
 * I struggled with typescript configuration but I think we achieved something that works well in the end and I have lessons learned for next time
 * It was a bit difficult being "hands off" on actually building functionality into our API and sticking to the role of QA, which was a lot of work on its own
 * Yet to use postman but have set it up and ready for when our server is up and running

## Feedback (For CF's)
> [**Course Facilitator name**]

Alexander

> [*What went well*]

You give different examples of the topics covered on this week and really good snippets

> [*Even better if*]

You could add something project planning related. No more to add to be honest
