# Makers Aceplay Front-End - React, TypeScript and Vite

In this module, you will build a front-end to integrate with the back-end you built last week. 

## Learning Objectives:

- Be able to explain the benefits of TypeScript, and typical use cases. 

- Master the basics of TypeScript and how to use it in the context of a React Application. 

- Improve on existing front-end designing skills:
	- create a React application that is organised thoughtfully and shows understanding of the core principles of React (parent/child relationships that are well thought-out, avoiding excessive props-drilling, etc.).

	- make use of TDD to support your development and component design process, for instance using your tests to outline the expected behaviour of a component.  

	- show awareness of user experience (using prompts when necessary for instance).

	- create an API layer to centralise all the API calls in one place.  

## Phase One - TypeScript Basics: 

[On our first day together, we will focus on the basics of Typescript.](https://github.com/ClaireMakers/typescript-react-vite-accelerator/blob/main/introduction-to-typescript.md)

## Phase Two - Improve your front-end TDD skills: 

[On the second day, we will work on TDD with React and TypeScript](https://github.com/makersacademy/frontend-tdd-challenges). 


## Phase Three - Setting-up your front-end project: 

From the second day onwards, you'll be building the front-end to your back-end project in Java. Here are the set-up steps you should follow:

### Vite, React and Cypress: 

First, create a Vite project - Vite is a lot like create-react-app, and has been better maintained recently. The file architecture will be very similar to what you are used to. 

```
npm create vite@latest

```

Follow the prompts, and choose a project name, then select React as a framework,and TypeScript as a variant. 

Then, cd into your project and run: 

```
npm install

```

Your Vite project is now set-up, and you can run the following to start the dev server: 

```
npm run dev

```

In addition to Vite, we will install a plugin that will monitor your code for Type errors, and throw an error when it finds one: 

```
npm --save-dev vite-plugin-checker

```

Then, modify your vite.config.ts to include it there as well - your file should look something like that at this point:

```typescript
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import checker from 'vite-plugin-checker'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [
	react(),
	checker({ typescript: true, })],
})

```

We will also use Cypress to write tests, so we also need to install it as a dev dependency: 

```
npm --save-dev cypress
 
```

We will also need this package to write our component tests with React and Vite: 

```

npm --save-dev @cypress/react18

```
[Click here to find some documentation on how to use the cypress React API.](https://docs.cypress.io/guides/component-testing/react/api)

Now, open the cypress gui to configure cypress: 

```
npx cypress open

```

Configure both the e2e tests and the component testing from there. These steps will add the relevant folders/config files to your project. 

On your cypress.config.ts file, add a line to specify your baseUrl for you end2end tests: 

```typescript
e2e: {
    setupNodeEvents(on, config) {
      // implement node event listeners here
    },
    baseUrl: "your base url here"
  },
```


### Creating an API layer: 
As applications grow, not separating UI code from data-fetching functionalities can get messy, and result in less maintanable code. For instance, you might end up re-writing the same fetching functionality over several components that rely on the same data; if something changes on your back-end, you then need to re-factor your front-end code in several places. This is time-consuming and not ideal from a design perspective. One way in which you can address that is by keeping your API calls in a separate file, and importing your functions across your components to re-use them as and when needed. 

In practice, this can mean creating one or several files that will act as function libraries for your API calls. In your case, you may decide whether one file is enough to contain the API calls you know will be necessary for your front-end, or you can choose to split them into several files, one for each endpoint, for instance. 

[Here is some extra reading on the subject, everything done with axios can be done with fetch too.](https://semaphoreci.com/blog/api-layer-react)

## User Stories: 

You should use these as a guidance for your MVP, and then build upon them depending on what you have implemented in your back-end in the week prior to this one. 
As a user of Makers Aceplay, I should be able to:

- Create and account.
- Sign-in to my account. 
- View my own personalised dashboard with my saved data. 
- Upload a track from my computer to my Library.
- Play the track from my library.
- Create a playlist.
- Add a track from my library to my playlist. 
- Play my playlist.

## Resources: 

1. [TypeScript Documentation](https://www.typescriptlang.org/docs/)
1. [React TypeScript Cheatsheet](https://react-typescript-cheatsheet.netlify.app/docs/basic/getting-started/class_components)
