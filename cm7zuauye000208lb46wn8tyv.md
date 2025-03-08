---
title: "React Testing on VS Code: Best Practices"
seoTitle: "Ultimate Guide for testing React Application on VS Code"
seoDescription: "Learn how to test your React app efficiently with Jest, React Testing Library, and Playwright. Discover how Keploy automates end-to-end testing, saving time"
datePublished: Sat Mar 08 2025 06:44:57 GMT+0000 (Coordinated Universal Time)
cuid: cm7zuauye000208lb46wn8tyv
slug: react-testing-on-vs-code
canonical: https://keploy.io/blog/community/react-testing-on-vs-code
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1740425499972/a237bf6d-8d98-44ab-bfb7-5ab9a1ec5a45.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1740425853965/b0e52459-3180-44fa-a44c-2bdeeb9aad48.png
tags: tools, react-js, reactjs, testing, jest, vs-code, vercel, testing-library, keploy, ai-tools, vs-code-extensions

---

Testing React applications in VS Code ensures better reliability, performance, and maintainability. Without proper testing, UI bugs, API failures, and performance bottlenecks can go unnoticed, affecting the user experience. This guide walks through setting up a CRUD application in React and testing it with Jest, Playwright, and Keploy, while optimizing debugging in VS Code.

We’ll start by building a simple React app with CRUD functionality, then cover unit tests with Jest, UI automation with Playwright, and API testing with Keploy. Finally, we’ll explore debugging tools in VS Code, ensuring a smooth development experience.

## Creating the CRUD Application

We’ll be creating a CRUD react application using vite (since react has recently decided to deprocate create-react-app). This would be a simple, easy to understand and basic CRUD application which everyone can build. We will then look into different ways to test the application following the best practices. Then we will try and automate this entire process in order to save time and effort as well because manually testing your own app is for rookies! We'll automate everything because, well, why not?

### Tech Stack

* **React** for building UI components.
    
* **TailwindCSS** (optional) for styling.
    
* **JSON Server** for a mock API.
    

### Features

* **Create**: Add a new item.
    
* **Read**: Display a list of items.
    
* **Update**: Edit an existing item.
    
* **Delete**: Remove an item.
    

## Setting Up the Project

### Initialize a React Project

Over the years, developers are used to believe that create-react-app is the best way to create a blog application. Well using create-react-app didn’t turn out to be the best practice since React decided to depricate it. Now that, create-react-app has been depricated, we’'ll be using Vite to install the react application. Vite has always been the go-to tool for creating react applications following the best practices to set up react applications quickly. Hence, this how we would create our react application using Vite:

```bash
# Create a new React project with Vite
npm create vite@latest react-testing-demo --template react

# Move into the project directory
cd react-testing-demo

# Install dependencies
npm install

# Start the development server
npm run dev
```

### Install Dependencies

```bash
# install dependencies
npm install tailwindcss json-server axios react-query
```

### Configure TailwindCSS (Optional)

We all know CSS isn't everyone's cup of tea, even though people call it easy to learn and use for styling. But let's be honest I’ll just assume you're bad at it because I’m no expert either. So, we’ll be using Tailwind CSS instead. That said, you can skip Tailwind or even plain CSS altogether since this guide focuses more on testing than styling practices.

You can also follow the Tailwind documentation for this: https://tailwindcss.com/docs/installation/using-vite

```bash
# intialise TailwindCSS for this project
npx tailwindcss init
```

### Set Up `json-server` in `db.json`

```json
{
 "items": [
  { "id": 1, "name": "Item 1" },
  { "id": 2, "name": "Item 2" },
]
}
```

### Run the Mock API

```bash
npx json-server --watch db.json --port 3001
```

## Implementing CRUD Features

After setting up the project really fast, being lazy enough to skip to the code writing and testing part. We’ll now try and create CRUD features and test them using the best practices. Now, let’s build some CRUD features and test them using best practices. Since I’m lazy too, I’ll only create two CRUD features and skip to the testing, you’re free to add more if you feel like it!

Create

```javascript
import { useState } from 'react';
import axios from 'axios';

const AddItem = ({ onAdd }) => {
  const [name, setName] = useState('');
  
  const addItem = async () => {
    const res = await axios.post('http://localhost:3001/items', { name });
    onAdd(res.data);
  };
  
  return (
    <div>
      <input value={name} onChange={(e) => setName(e.target.value)} />
      <button onClick={addItem}>Add Item</button>
    </div>
  );
};
export default AddItem;
```

### Read

```javascript
import { useEffect, useState } from 'react';
import axios from 'axios';

const ItemList = () => {
  const [items, setItems] = useState([]);

  useEffect(() => {
    axios.get('http://localhost:3001/items').then((res) => setItems(res.data));
  }, []);

  return (
    <ul>
      {items.map((item) => (
        <li key={item.id}>{item.name}</li>
      ))}
    </ul>
  );
};
export default ItemList;
```

## Testing the Application

Now, we get to the main point of this entire blog - testing a React application in VS Code. We’ll start with the straightforward, go-to tools that developers have been using for ages. Then, we’ll dive into best practices for testing. And finally - saving the best for the last for a happy ending, we’ll see how to automate the entire testing process like a pro using Keploy’s AI-powered VS Code testing extension. Because why waste time and effort when you can let AI do the work for you?

### Unit Testing with Jest & React Testing Library

Now that we’re diving into testing, let’s start with the basics - unit testing. We’ll be using [**Jest** and **React Testing Library**,](https://keploy.io/blog/community/a-guide-to-testing-react-components-with-jest-and-react-testing-library) the two obvious choices for testing React applications. Jest is a powerful JavaScript testing framework, while React Testing Library helps us test components the way users interact with them. Together, they make a solid duo for writing meaningful tests.

But before we start writing tests, we need to install the required libraries.

#### Install Testing Libraries

Alright, time to install some dependencies - don’t worry, it’s just a one-liner. Run the following command in your terminal:

```bash
npm install --save-dev @testing-library/react jest
```

That’s it! Now we have everything we need to start testing. No complicated setups, no unnecessary configs - just pure testing goodness. Let’s move on and actually write some tests! 🚀

#### Write a Test for `AddItem.js`

Now that we’ve set everything up, it’s time to write some actual tests! Let’s start by testing our **AddItem** component. The goal? Ensure that when a user adds a new item, the correct function gets called.

Here’s the test:

```javascript
import { render, fireEvent } from '@testing-library/react';
import AddItem from './AddItem';

test('adds a new item', () => {
  const mockOnAdd = jest.fn();
  const { getByText, getByRole } = render(<AddItem onAdd={mockOnAdd} />);

  fireEvent.change(getByRole('textbox'), { target: { value: 'New Item' } });
  fireEvent.click(getByText('Add Item'));

  expect(mockOnAdd).toHaveBeenCalled();
});
```

### What’s Happening Here?

* First, we **render** the `AddItem` component while passing a **mock function** (`mockOnAdd`) to simulate the `onAdd` prop.
    
* Next, we use `fireEvent.change` to type **"New Item"** into the input field.
    
* Then, we simulate a click on the **"Add Item"** button.
    
* Finally, we **assert** that `mockOnAdd` was called, meaning the item was successfully added.
    

Simple, clean, and effective! Now, let’s move on to testing other features. 🚀

### End-to-End UI Testing with Playwright

Alright, time to take testing to the next level - **UI testing** with Playwright. Unlike unit tests, which focus on individual components, UI tests check if everything looks and works correctly in a real browser environment. Think of it as watching your app run automatically, clicking buttons, filling forms, and making sure it behaves as expected.

#### Install Playwright

First things first, let’s install Playwright:

```bash
npm install --save-dev @playwright/test
```

Next, set it up by running:

```bash
npx playwright install
```

This downloads the necessary browser binaries so Playwright can run tests across different browsers. Now that we’re all set, let’s write some UI tests and watch Playwright take control of our app like a pro! 🚀

#### Write a Playwright Test:

Now that Playwright is set up, let’s write a simple UI test to check if adding an item works as expected. Here’s the Playwright test:

```javascript
import { test, expect } from '@playwright/test';

test('add an item', async ({ page }) => {
  await page.goto('http://localhost:3000');
  await page.fill('input', 'New Item');
  await page.click('text=Add Item');
  expect(await page.textContent('ul')).toContain('New Item');
});
```

### End-to-End Testing with Keploy

Alright, let’s talk about **Keploy**, the real MVP when it comes to end-to-end testing automation. If you’re tired of writing and maintaining endless test cases, Keploy has got your back. It’s an **AI-powered, zero-code testing framework** that generates, runs, and maintains test cases automatically - because, let’s be honest, manually writing tests is for rookies.

Keploy takes a **record-replay** approach, meaning it captures API requests and responses while you’re running your app and automatically converts them into test cases. No need to manually write assertions, mocks, or dependencies - Keploy does it all for you.

### Why Use Keploy for End-to-End Testing? 🚀

✅ **Zero Manual Test Writing** – Keploy auto-generates test cases, so you can focus on development instead of writing and maintaining tests.

✅ **Record Once, Test Forever** – It captures API interactions while you run your app and replays them as test cases every time you need to validate your system.

✅ **Mocks & Dependencies? Handled!** – Keploy automatically mocks external APIs, databases, and dependencies, ensuring tests run in an isolated and predictable manner.

✅ **Integrates Seamlessly with Playwright & Jest** – While Playwright is great for UI testing and Jest for unit testing, Keploy ensures your **entire application** (backend + APIs + integrations) is tested comprehensively.

✅ **Saves Time & Effort** – No need to maintain outdated test cases. Keploy evolves with your application, making it perfect for **continuous testing and deployment**.

Now that we understand why Keploy is a **game-changer for E2E testing**, let’s install and set it up! 🚀

### Installing Keploy for End-to-End Testing 🛠️

Setting up Keploy is quick and easy. Follow the steps given below to integrate it into your project, you can also refer to [The Keploy Installation Guide](https://keploy.io/docs/server/installation/) for the same.

#### Step 1: Download and Install Keploy

Run the following command to install Keploy on your system:

```bash
 curl --silent -O -L https://keploy.io/install.sh && source install.sh
```

You should see something like this:

```bash
       ▓██▓▄
    ▓▓▓▓██▓█▓▄
     ████████▓▒
          ▀▓▓███▄      ▄▄   ▄               ▌
         ▄▌▌▓▓████▄    ██ ▓█▀  ▄▌▀▄  ▓▓▌▄   ▓█  ▄▌▓▓▌▄ ▌▌   ▓
       ▓█████████▌▓▓   ██▓█▄  ▓█▄▓▓ ▐█▌  ██ ▓█  █▌  ██  █▌ █▓
      ▓▓▓▓▀▀▀▀▓▓▓▓▓▓▌  ██  █▓  ▓▌▄▄ ▐█▓▄▓█▀ █▓█ ▀█▄▄█▀   █▓█
       ▓▌                           ▐█▌                   █▌
        ▓

Keploy CLI

Available Commands:
  example           Example to record and test via keploy
  config --generate generate the keploy configuration file
  record            record the keploy testcases from the API calls
  test              run the recorded testcases and execute assertions
  update            Update Keploy

Flags:
      --debug     Run in debug mode
  -h, --help      help for keploy
  -v, --version   version for keploy

Use "keploy [command] --help" for more information about a command.
```

🎉 Wohoo! You are all set to use Keploy.

### 🎬 Capturing Test Cases with Keploy

Recording API interactions is the key to Keploy’s automation magic. To start capturing API calls, simply run the following command in your terminal:

```bash
keploy record -c "CMD_TO_RUN_APP"
```

Replace `CMD_TO_RUN_APP` with the actual command that starts your application.

#### Example:

If you have a **Node.js** server running with `npm start`, you would use:

```bash
keploy record -c "npm start"
```

Once this command is executed, Keploy will capture all API interactions while you use the application. These interactions will be saved as **test cases**, which can later be replayed to detect regressions.

### 🏃 Running Test Cases (Regression Testing)

Once you’ve recorded test cases, you can **replay them** to check if your latest changes have introduced any regressions. To do this, run:

```bash
keploy test -c "CMD_TO_RUN_APP" --delay 10
```

The `--delay 10` ensures the app has enough time to initialize before running the tests.

#### Why is This Important?

* It **automatically detects breaking changes** in your APIs.
    
* You don’t have to manually write tests—[Keploy](http://keploy.io) replays the same API calls and **compares the responses** to detect discrepancies.
    
* If a test fails, you can quickly debug and fix issues before deploying changes.
    

**Pro Tip:** Explore the [Test Coverage Generation Guide](https://keploy.io/docs/server/sdk-installation/go/)  to see how Keploy integrates with your **unit testing library** for complete test coverage. Also, check out the [Keploy Running Guide](https://keploy.io/docs/running-keploy/configuration-file/) for customization options and advanced setup tips.

## Automated Testing in VS Code with Keploy Extension

Incorporating automated testing into your development workflow is essential for maintaining robust and reliable applications. The Keploy AI Testing Assistant for Visual Studio Code (VS Code) streamlines this process by leveraging artificial intelligence to generate and manage unit, integration, and API tests directly within your favorite IDE.

![Keploy VS Code AI Extension](https://cdn.hashnode.com/res/hashnode/image/upload/v1740425156030/647ce15a-1654-4e29-8723-075bee95c1e8.png align="center")

#### Key Features of the Keploy AI Extension

* **AI-Powered Test Generation**: Automatically create unit tests for languages like Python, JavaScript, TypeScript, Java, PHP, and Go, enhancing code coverage without manual effort.
    
* **Flake-Free Test Filtering**: Ensures reliability by executing tests multiple times to eliminate flaky results.
    
* **Coverage-Driven Refinement**: Focuses on meaningful tests by discarding those that don't improve code coverage.
    
* **Seamless Integration**: Works effortlessly with popular testing frameworks such as JUnit, TestNG, and pytest, fitting smoothly into your existing development workflow.
    

![Keploy Key Features](https://cdn.hashnode.com/res/hashnode/image/upload/v1740425673657/4bcee90a-9589-404f-8eac-091373912a8d.png align="center")

#### Installing the Keploy AI Extension in VS Code

1. **Open VS Code**: Launch your Visual Studio Code editor.
    
2. **Access the Extensions Marketplace**: Click on the Extensions icon in the sidebar or press `Ctrl+Shift+X` (`Cmd+Shift+X` on macOS).
    
3. **Search for Keploy**: In the search bar, type "Keploy" to locate the AI Testing Assistant extension.
    
4. **Install the Extension**: Click the "Install" button to add Keploy to your editor.
    
5. **Reload if Necessary**: Some installations may require reloading VS Code to activate the extension.
    

![Keploy VS Code AI Extension](https://cdn.hashnode.com/res/hashnode/image/upload/v1740425699790/5c49c6f8-b985-4e22-874b-2bc4bbad4c7a.png align="center")

#### Generating Tests with Keploy

1. **Open Your Project**: Load the project you wish to test in VS Code. Ensure the codebase is free of errors to facilitate seamless test generation.
    
2. **Generate Tests for a Function**:
    
    * Navigate to the desired function in your code.
        
    * Hover over the function name; a "Generate for this function" option will appear.
        
    * Click this option to create a unit test for the specific function.
        
3. **Generate Tests for a File**:
    
    * In the Explorer panel, locate the file you want to test.
        
    * Right-click the file and select "Generate Unit Tests" from the context menu.
        
    * Keploy will produce tests for all relevant functions within that file.
        
4. **Review and Edit Generated Tests**: Keploy generates test cases based on your code. It's advisable to review these tests to ensure they meet your requirements and make any necessary adjustments.
    
5. **Run Tests**: Utilize your preferred testing framework's commands within VS Code's terminal to execute the tests.
    

## Debugging and Optimization in VS Code

Debugging plays a crucial role in ensuring the smooth functionality of your React application. Thankfully, VS Code provides a built-in **Debugger** that helps track issues efficiently without relying on `console.log()` everywhere.

### Using VS Code Debugger

1. Open VS Code, go to **Run and Debug**.
    
2. Add a breakpoint in `AddItem.js`.
    
3. Click **Start Debugging** to inspect the code.
    

### Performance Optimization

* Use **React.memo** for component optimization.
    
* Use **lazy loading** with `React.lazy()`.
    
* Optimize API calls with **React Query**.
    

### Linting and Code Formatting

Maintaining clean, readable, and error-free code is essential for a scalable React project. That’s where **ESLint** and **Prettier** come into play.

#### Install ESLint and Prettier:

```bash
npm install --save-dev eslint prettier eslint-plugin-react-hooks
```

```json
{
  "extends": ["react-app", "plugin:react-hooks/recommended"]
}
```

## Deploying the Application

Now that our **CRUD React application** is fully built and tested, it's time to **deploy it** so the world can see it! 🎉

For frontend applications, the best hosting platforms include:

* **Vercel** (Recommended for React/Vite apps)
    
* **Netlify** (Great for static sites)
    
* **GitHub Pages** (For simple projects)
    
* **Firebase Hosting** (If using Firebase backend)
    

In this guide, we'll use **Vercel** since it’s **fast, free for personal use, and requires zero configuration** for React apps.

### Deploying to Vercel

Now that our React app is built and tested, it's time to deploy it to **Vercel**—one of the easiest and fastest ways to get your frontend live.

### Why Deploy to Vercel?

✅ **Zero Config** – Deploys Next.js, React, and other frontend frameworks seamlessly.  
✅ **Automatic Builds & Previews** – Get instant preview links for each commit.  
✅ **Free for Personal Use** – No need for server management, plus a generous free tier.

Go to The Vercel Website and login to your Vercel account.

### Deploying Your React App

* Select your existing Vercel project or create a new one.
    
* If creating a new one, you can directly import a githuub repository.
    
* Set up your project settings (or accept the defaults).
    
* Vercel will automatically detect your framework (**React/Vite** in this case).
    
* Click on Deploy, it will some time to get deployed.
    

Once deployment is complete, Vercel will give you a live URL like:

```bash
https://your-app.vercel.app
```

🎉 **Boom! Your app is live!**

## Conclusion

Thorough testing ensures a reliable and maintainable React application. [Using Jest,](https://keploy.io/blog/community/migrate-from-jest-to-vitest) Playwright, and Keploy allows for comprehensive unit, UI, and API testing. Debugging in VS Code enhances efficiency, and deploying with Vercel and GitHub Actions streamlines the development workflow. By integrating automated testing and CI/CD pipelines, developers can catch issues early and deliver high-quality applications with confidence.

* Testing improves application reliability and maintainability.
    
* Use **Jest** for unit tests, **Playwright** for UI tests, and **Keploy** for automated testing.
    
* Debugging in VS Code streamlines development.
    
* Deploying with Vercel and GitHub Actions ensures a smooth workflow.
    

## FAQs

### What are the best practices for testing React applications?

* Use Jest and React Testing Library for unit testing.
    
* Implement [Playwright for end-to-end UI testing](https://keploy.io/blog/community/playwright-vs-cypress-choosing-the-best-e2e-testing-framework).
    
* Automated testing with Keploy.
    
* Use mock functions to isolate component behavior.
    

### Why use automated testing instead of manual testing?

* Automated testing increases efficiency and reduces human error.
    
* It provides quick feedback and ensures code reliability.
    
* Continuous Integration (CI) becomes more effective with automation.
    

### How does Keploy compare to VS Code AI testing extensions?

* Keploy provides automated API testing with record and replay functionality.
    
* It eliminates the need for manual test case writing.
    
* VS Code AI extensions assist in code generation but may not provide full API automation.
    

### How can I optimize my test suite performance?

* Run tests in parallel to speed up execution.
    
* Use snapshot testing for UI components.
    
* Avoid unnecessary re-renders by optimizing React components.