## Guidance
Answer the following questions considering the learning outcomes for
- [Week 010](https://learn.foundersandcoders.com/course/syllabus/developer/week10-project05-DOTNET-intro/learning-outcomes/)

Make sure to record evidence of your processes. You can use code snippets, screenshots or any other material to support your answers.

Do not fill in the feedback section. The Founders and Coders team will update this with feedback on your progress.

## Assessment
 ### 1. Learning outcomes
 * I learned how to create and run a .NET project in the workshops last week, Gaj set us up this week for a minimal API project by running the command `dotnet new web`. Subsequently, to simply run an .NET project you run the command `dotnet build` (optional) followed by `dotnet run` (which will build the project anyway). Our project containing a postgreSQL database and EF meant a few extra commands to get it going, initialise the database and run it, it went like this:
```bash
dotnet ef migrations add InitialCreate
dotnet ef database update
dotnet build
dotnet run
```
 * C# basics I also learned in the previous week with the workshops (and already had some working knowledge of previously), I learned some more fundamentals this week though, one example of this is defining namespaces within which to define classes, which can then be imported into other files using the namespace itself, so if a collection of classes are defined within the same namespace (even accross multiple files for a modularised approach), they can all be recalled using that namespace. This is also best practice rather than defining classes outside namespaces which would be accessible within all files. I used 2 namespaces for this project, one for models (my database schemas essentially), and one for the database context itself, an example of this:
```csharp
namespace PokeLikeAPI.Models
{
    public class BaseEntity
    {
        public int Id { get; set; }
        public DateTime CreatedAt { get; set; }
    }
}
```
 * It has been interesting working with swagger within a .NET project where it can automatically detect your endpoints but not only that it can serve as a testing tool like postman also without the need to recopy the url and data schema as payload into another platform (postman) so it is very quick and efficient to test that your code works, this has been hugely beneficial compared to the approach of using swagger in javascript which is just documentation and seperate from your actual code. All you need for swagger to keep trakc of your code is to set up the environment first before it builds:
```csharp
if (builder.Environment.IsDevelopment())
{
    builder.Services.AddSwaggerGen(c =>
{
    c.SwaggerDoc("v1", new OpenApiInfo
    {
        Title = "PokeLike",
        Description = "Keep track of what pokemon you think is cute 🥰",
        Version = "v1"
    });
});
}
```
And after the app builds make an endpoint and define some parameters like Title to be displayed on your swagger page (I'm sure it is doing a lot mroe than this behind the abstraction but the benefit of OOP with C# is that I don't necessarily need to get into that level of detail for it to work how it should):
```csharp
if (builder.Environment.IsDevelopment())
{
    builder.Services.AddSwaggerGen(c =>
{
    c.SwaggerDoc("v1", new OpenApiInfo
    {
        Title = "PokeLike",
        Description = "Keep track of what pokemon you think is cute 🥰",
        Version = "v1"
    });
});
}
```
 * Although we have not yet linked up our front end and backend, we have made preparations for the frontend to receive the data in the format that was defined within the swagger documentation. Given that an API connects with its frontend through urls, whether the backend is a typescript or .NET project does not change ho that connection will occur on the frontend so I feel like I understand how that will be done next week.
 * Communication and PRs this week have been good, I have written clear PRs and included documentation with my commits to explai n how the code works. I have also sat with my team members and explained in some depth how everything works for the backend. I have also been able to contribute to Khalos' PRs on the frontend with helpful suggestions.

 ### 2. Show an example of some of the learning outcomes you have struggled with and/or would like to re-visit.
 * Not much struggle this week has been quite well organised and relaxed, I pushed myself in the previous week to get ahead with C#, along with the workshops Alex suggested from the beginning of the week with minimal API has meant I was well equipped to get through the project this week. The only issue being we did not find the time to connect frontend and backend so far but as mentionned above, I am not worried about this so all good from me!

## Feedback (For CF's)
> [**Course Facilitator name**]

Alexander

> [*What went well*]

Strong grasp of .NET project structure and C# fundamentals, particularly namespace organization. Good understanding of Swagger integration in .NET with practical examples. Clear communication with team through well-documented PRs.

> [*Even better if*]

Since frontend-backend integration wasn't completed, outline your planned approach for connecting them. You mentioned understanding how it would work - show that understanding with code examples.
