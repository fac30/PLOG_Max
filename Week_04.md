## Guidance
Answer the following questions considering the learning outcomes for
- [Week 04](https://learn.foundersandcoders.com/course/syllabus/developer/week04-project03-frontend/learning-outcomes/)

Make sure to record evidence of your processes. You can use code snippets, screenshots or any other material to support your answers.

Do not fill in the feedback section. The Founders and Coders team will update this with feedback on your progress.

## Assessment
 ### 1. Learning outcomes
 * Understanding how a React Typescript project is set up in Vite, it was interesting to see how the tsconfig setting were segmented into multiple files and configurations for the node environment (if applicable) and the React app environment:
```bash
tsconfig.app.json //this is the React app tsconfig setting which is used for our .tsx files
tsconfig.app.tsbuildinfo //this is the app build info telling pointing to the .tsx files that need to be run
tsconfig.node.json //this is the node environment typescript setting which is used for our .ts files
tsconfig.node.tsbuildinfo //this is the vite configuration file with typescript, we used the very basic but this can be further utilised by adding intellisense configuration or even environment variables in a project whre those are necessary - for us this is being handled by our API
```
 *  Setting up Tailwind CSS in a Typescript project - more config files and modifying VS Codes settings.json to use Intellisense effectively like so:
 ```json
"files.associations": {
  "*.css": "tailwindcss"
}
```
 * My understanding of components, props and state can be encompassed all in my first task (which I took extremely seriously and made a component that could accept all scenarios even ones we don't need for our app) - a button that takes multiple props and 2 states (active/non-active), where a combination of state and props can render the button to be default, active, disabled, hover, loading or in focus
```tsx
import React, { useState } from 'react';

interface ButtonProps {
  label: string;
  disabled?: boolean;
  loading?: boolean;
}

const Button: React.FC<ButtonProps> = ({ label, disabled = false, loading = false }) => {
  const [isActive, setIsActive] = useState(false);

  return (
    <button
      type="submit"
      disabled={disabled || loading}
      onMouseDown={() => setIsActive(true)}
      onMouseUp={() => setIsActive(false)}
      className={`m-2 text-lg px-6 py-3 rounded-full transition duration-300 ease-in-out
        ${disabled ? 'bg-[var(--button-disabled)] text-gray-500 cursor-not-allowed' : ''}
        ${loading ? 'bg-[var(--button-loading)] text-black cursor-wait' : ''}
        ${!disabled && !loading && !isActive ? 'bg-[var(--button-default)] text-white hover:bg-[var(--button-hover)]' : ''}
        ${isActive && !loading && !disabled ? 'bg-[var(--button-hover)] text-white border-[3px] border-[var(--button-active)]' : ''}
        focus:outline-none focus:ring-[3px] focus:ring-[var(--button-active)] focus:bg-[var(--button-default)]`}
    >
      {loading ? 'Loading...' : label}
    </button>
  );
};

export default Button;
```
 * Routing between our page was managed by a component called Content and a function called onNext which would lay out the map for our app, the function in COntent controlling which page to display:
```tsx
	const renderPage = () => {
		switch (currentPage) {
			case 'dummy':
				return <DummyPage onNext={() => setCurrentPage('landing')} />;
			case 'landing':
				return <LandingPage onNext={() => setCurrentPage('input')} setUserName={setUserName} />;
			case 'input':
				return <InputPage onNext={() => setCurrentPage('loading')} />;
			case 'loading':
				return <LoadingPage onNext={() => setCurrentPage('playlist')} />;
			case 'playlist':
				return <PlaylistPage onNext={() => setCurrentPage('landing')} />;
			default:
				return <DummyPage onNext={() => setCurrentPage('landing')} />;
		}
	};
```
 * Thanks to Jason, our project planning was tight and easy to follow this week as we expanded on the previous week's JIRA board, adding more epics, issues and setting up a timeline with dependencies between issues, our timeline looks like this from an epics point of view (plenty of smaller issues within each one):
![image](https://github.com/user-attachments/assets/1d28cfbc-9e4b-4c04-b15e-ca56307a1fbc)


 ### 2. Show an example of some of the learning outcomes you have struggled with and/or would like to re-visit.
* React is pretty much completely new to me and so it was a bit of a learning curve how to write in React and the terminology of props and hooks, how to use hooks with callbacks wasn't the most intuitive to me
* We had some trouble making calls to our API from within our React app because of security settings and permissions and so had to go back to last week's project and fix those permissions to allow the app to make those calls, we used a module called cors to do this
* Tailwindcss gave me some trouble because we had not set it up properly while I worked on my button, I spent a long time changing the tailwind classes on it and seeing no results in the rendered app, this was fixed by reinstalling it with the types module too and copying into the tailwind config file:
```js
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./src/**/*.{js,jsx,ts,tsx}",
  ],
  theme: {},
  plugins: [],
};
```


## Feedback (For CF's)
> [**Course Facilitator name**]

Alexander

> [*What went well*]

Excellent, I love how you included a bit a project planning. I also like how you try to understand the most basic concepts like config files. Most of us just ignore them.

> [*Even better if*]

You could edit a bit your code snippets to only leave what is neccesary, but ,in any case, this is a great log
