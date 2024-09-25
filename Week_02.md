## Guidance
Answer the following questions considering the learning outcomes for
- [Week 02](https://learn.foundersandcoders.com/course/syllabus/developer/week02-project02-chatbot/learning-outcomes/)

Make sure to record evidence of your processes. You can use code snippets, screenshots or any other material to support your answers.

Do not fill in the feedback section. The Founders and Coders team will update this with feedback on your progress.

## Assessment
 ### 1. Learning outcomes
 * Writing complex asynchronous code with GET and POST requests using syntax from API documentation - example of openAI API syntax of how we make a POST request sending chat requests from Discord
 ```javascript
    // Make a request to OpenAI to generate a response based on the provided model and conversation history
    const response = await openai.chat.completions.create({
      model: config.openaiModel, // Use the AI model specified in the config (e.g., GPT-3.5, GPT-4)
      messages: conversationHistory, // Pass conversation history as expected
    });
```
 * Using array methods learned on execute program and recognising when to use them, or when it is not necessary too, example using filter method to select only javascript files in our commands directory and then iterating through each adding each command to available commands (a map method could have been used here too)
```javascript
// Load all command files dynamically
const commands = new Map();
const commandFiles = fs
  .readdirSync(path.join(__dirname, "../commands"))
  .filter((file) => file.endsWith(".js"));

for (const file of commandFiles) {
  const command = require(`../commands/${file}`);
  commands.set(command.name, command);
  console.log(`Loaded command: ${command.name}`);  // Log each command that is loaded
}
```
 * Updating my node.js using fnm... Could not get volta to work as in guide from FAC
 * Setting up secure node.js environment using dotenv module storing safely API keys in a .env, calling them in a config.js file for authentication centrally, which proved useful for debugging as the following code allowed us to print which specific API connection may have an issue
```javascript
requiredEnvVariables.forEach((envVar) => {
  if (!process.env[envVar]) {
    console.error(`Error: Missing required environment variable: ${envVar}`);
    process.exit(1); // Exit the application with an error code if a required variable is missing
  }
});
```
 * Setting up npm and package.json file with dependencies and scripts, our scripts in here included running the bot and testing:
```JSON
"scripts": {
    "test": "node --test",
    "bot": "nodemon src/bot.js",
    "slash": "node src/slashCommands.js"
  }
```
 * Testing testing testing, I wrote a few unit tests to test that our AI bot responds as it should inside Discord, to do so I designed a createMockMessage function which simulates conversations between user and bot, one example of these tests is testing that it responds to DMs
```javascript
  // Test 2: Handle DM (no prefix required)
  const test2Message = createMockMessage('explain recursion', true); // Simulate DM
  await handleMessage(test2Message);
  assert.ok(test2Message.author.lastMessageSent, 'Test 2 Failed: Bot did not respond to a DM.');
  console.log('Test 2 Passed: Bot responded correctly to a DM.');
```
 * Also learned how to implement functionality from openAI such as prompting using system and user roles, how to use conversation history so the bot remembers while being mindful and demure about tokens
 * Implementing commands in Discord API

 ### 2. Show an example of some of the learning outcomes you have struggled with and/or would like to re-visit.
 * I struggled with assessing the project from the get-go and understanding my role as well as the role of everyone in my team
 * Struggled taking the time to read documentation fully before getting started with the project

## Feedback (For CF's)
> [**Course Facilitator name**]

Alexander

> [*What went well*]

Very good example of a progress log. I love the short snippets, that you covered all the basics and you went further integrating learnings from the workshops in your project

> [*Even better if*]

Nothing to mention.
