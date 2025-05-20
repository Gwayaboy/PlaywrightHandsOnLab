# Playwright Hands Lunch & Learn

Welcome to the Playwright Hands & Lunch Learn!  
In this hands-on session, you will learn the fundamentals of UI automation using [Playwright](https://playwright.dev/) by testing the [playwright-movies-app](https://github.com/debs-obrien/playwright-movies-app).

## Agenda

- Brief overview of Behaviour Driven Development (BDD)
  - [Gherkin Fundamentals](https://cucumber.io/docs/gherkin/reference/)
  - [Acceptance Testing](https://www.agilealliance.org/glossary/acceptance/)
- Introduction to Playwright
- Setting up the project
- Writing and running your first Playwright test
- Gherkin fundamentals & scenario writing
- Hands-on exercises with the movies app

---

## Prerequisites

- Node.js (v18+ recommended)

---

## Getting Started

To get started, follow the official instructions for the [playwright-movies-app](https://github.com/debs-obrien/playwright-movies-app):

### [Installation](https://github.com/debs-obrien/playwright-movies-app?tab=readme-ov-file#installation)

Clone the repository and install dependencies:

```bash
git clone https://github.com/debs-obrien/playwright-movies-app.git
cd playwright-movies-app
npm install
```

### [Running the App Locally](https://github.com/debs-obrien/playwright-movies-app?tab=readme-ov-file#running-the-app-locally)

Make sure port 3000 is available as the app needs to run on this port. Using a different port will result in errors because the movies loaded from the API use this port.

```bash
npm run dev
```

The web app should be now running locally at [http://localhost:3000](http://localhost:3000)

---

## Exercise 1: Describe Movie App Scenarios with Gherkin

In this exercise, you'll practice writing Cucumber (Gherkin) scenarios that describe the core functionality of the movies app. Focus on what the user wants to achieve, not how the UI looks or is implemented.

### Familiarise yourself with the movie web app
- Browse the list of movies
- Search a movie
- View details of a movie
- Switch between dark and light mode
- (If available) Log in and log out

### Your Turn: Write a Scenario for Searching a Movie

Write a Gherkin scenario that describes how a user would search for a movie by title. Focus on the user's intent and the expected outcome, not the UI steps (e.g., avoid mentioning clicking buttons or entering text in fields directly).

Example scenario ideas:
  - Searching for a movie that exists
  - Searching for a movie that does not exist
  - Searching with an empty query

When you're done, share your scenarios with the group for feedback and discussion.

<details>
<summary>✅ <strong>Guidance for Writing Good Scenarios</strong></summary>

- Use the structure: <code>Given</code> (initial context), <code>When</code> (action), <code>Then</code> (expected outcome)
- Keep steps high-level and focused on behavior, not UI details
- Make scenarios readable and meaningful to both technical and non-technical team members

❌ <strong>Additionally</strong>
- Avoid steps like "click the search button"; instead, use "the user searches for a movie by title"
- Use clear and concise language
- Each scenario should describe a single behavior or outcome

</details>


---

## Exercise 2: Your First Playwright Test

**Goal:** Write a Playwright test to verify that searching for a movie displays the correct results.

### Step 1: Implement Your Scenario with a Cucumber Framework

You can use a Cucumber framework to automate your Gherkin scenarios. Below are examples for both JavaScript/TypeScript and C# using [Reqnroll](https://docs.reqnroll.net/latest/quickstart/index.html).

#### JavaScript/TypeScript Example (using Cucumber.js)

1. Install dependencies:
   ```bash
   npm install --save-dev @cucumber/cucumber playwright
   ```
2. Create a step definition file (e.g., `features/step_definitions/movies.steps.js`):
   ```js
   const { Given, When, Then } = require('@cucumber/cucumber');
   const { chromium } = require('playwright');

   let browser, page;

   Given('the movies app is running', async function () {
     browser = await chromium.launch();
     page = await browser.newPage();
   });

   When('the user searches for {string}', async function (title) {
     await page.goto('http://localhost:3000');
     await page.fill("input[placeholder='Search movies']", title); // Adjust selector as needed
     await page.press("input[placeholder='Search movies']", 'Enter');
   });

   Then('the user should see results related to {string}', async function (title) {
     await page.waitForSelector('.movie-list');
     const results = await page.$$(".movie-card:has-text('" + title + "')");
     if (results.length === 0) throw new Error('No results found for ' + title);
     await browser.close();
   });
   ```
3. Run your tests with:
   ```bash
   npx cucumber-js
   ```

#### C# Example (using Reqnroll + Playwright)

1. Follow the [Reqnroll quickstart guide](https://docs.reqnroll.net/latest/quickstart/index.html) and [Microsoft.Playwright](https://playwright.dev/dotnet/) to set up your project.
2. Example step definitions:
   ```csharp
   using Reqnroll;
   using Microsoft.Playwright;
   using System.Threading.Tasks;

   [Binding]
   public class MovieSearchSteps
   {
       private IPage page;
       private IBrowser browser;

       [Given(@"the movies app is running")]
       public async Task GivenTheMoviesAppIsRunning()
       {
           var playwright = await Playwright.CreateAsync();
           browser = await playwright.Chromium.LaunchAsync(new BrowserTypeLaunchOptions { Headless = true });
           page = await browser.NewPageAsync();
       }

       [When(@"the user searches for "(.*)"")]
       public async Task WhenTheUserSearchesFor(string title)
       {
           await page.GotoAsync("http://localhost:3000");
           await page.FillAsync("input[placeholder='Search movies']", title); // Adjust selector as needed
           await page.PressAsync("input[placeholder='Search movies']", "Enter");
       }

       [Then(@"the user should see results related to "(.*)"")]
       public async Task ThenTheUserShouldSeeResultsRelatedTo(string title)
       {
           var results = await page.Locator($".movie-card:has-text('{title}')").CountAsync();
           if (results == 0) throw new Exception($"No results found for {title}");
           await browser.CloseAsync();
       }
   }
   ```
3. Run your tests using your test runner (e.g., Visual Studio Test Explorer).

---

## Exercise 3: Search Functionality

**Goal:** Test the search feature.

```gherkin
Feature: Movie Search

  Scenario: User searches for a movie
    Given the movies app is running
    When the user enters "Avengers" in the search bar
    And clicks the search button
    Then the user should see results related to "Avengers"
```

---

## Exercise 4: Movie Details

**Goal:** Test navigation to a movie details page.

```gherkin
Feature: Movie Details

  Scenario: User views details of a movie
    Given the movies app is running
    When the user clicks on the first movie in the list
    Then the user should see the movie's title, description, and cast
```

---

## Exercise 5: Theme Switching

**Goal:** Test switching between dark and light mode.

```gherkin
Feature: Theme Switching

  Scenario: User toggles between dark and light mode
    Given the movies app is running
    When the user clicks the theme toggle button
    Then the app should switch between dark and light themes
```

---

## Bonus: Authentication (if enabled)

**Goal:** Test login and logout functionality.

```gherkin
Feature: User Authentication

  Scenario: User logs in and logs out
    Given the movies app is running
    When the user logs in with valid credentials
    Then the user should see their profile
    When the user logs out
    Then the user should be redirected to the login page
```

---

## Resources

- [Playwright Documentation](https://playwright.dev/)
- [Gherkin Syntax](https://cucumber.io/docs/gherkin/)

---

Feel free to expand these exercises or add more scenarios as you explore the app!

### Example Scenario: Search for a Movie

You can implement the following scenario in different languages. Switch tabs below to see examples for JavaScript/TypeScript and C#.

<!-- tabs -->

#### JavaScript/TypeScript ([Cucumber.js](https://github.com/cucumber/cucumber-js))

```gherkin
Feature: Movie Search
  As a movie fan
  I want to search for a movie by title
  So that I can quickly find the movie I want to watch

  Scenario: Searching for a movie that exists
    Given the movies app is running
    When the user searches for "Inception"
    Then the user should see results related to "Inception"
```

#### C# ([Reqnroll](https://docs.reqnroll.net/latest/quickstart/index.html) + [Microsoft.Playwright](https://playwright.dev/dotnet/))

```gherkin
Feature: Movie Search
  As a movie fan
  I want to search for a movie by title
  So that I can quickly find the movie I want to watch

  Scenario: Searching for a movie that exists
    Given the movies app is running
    When the user searches for "Inception"
    Then the user should see results related to "Inception"
```

<!-- end tabs -->

#### C# Step Definitions Example (Playwright + Reqnroll)
```csharp
using Reqnroll;
using Microsoft.Playwright;
using System.Threading.Tasks;

[Binding]
public class MovieSearchSteps
{
    private IPage page;
    private IBrowser browser;

    [Given(@"the movies app is running")]
    public async Task GivenTheMoviesAppIsRunning()
    {
        var playwright = await Playwright.CreateAsync();
        browser = await playwright.Chromium.LaunchAsync(new BrowserTypeLaunchOptions { Headless = true });
        page = await browser.NewPageAsync();
    }

    [When(@"the user searches for "(.*)"")]
    public async Task WhenTheUserSearchesFor(string title)
    {
        await page.GotoAsync("http://localhost:3000");
        await page.FillAsync("input[placeholder='Search movies']", title); // Adjust selector as needed
        await page.PressAsync("input[placeholder='Search movies']", "Enter");
    }

    [Then(@"the user should see results related to "(.*)"")]
    public async Task ThenTheUserShouldSeeResultsRelatedTo(string title)
    {
        var results = await page.Locator($".movie-card:has-text('{title}')").CountAsync();
        if (results == 0) throw new Exception($"No results found for {title}");
        await browser.CloseAsync();
    }
}
```

---

### Bonus: Use GitHub Copilot Agents with Playwright MCP Server

You can leverage GitHub Copilot agents with a Playwright MCP server to generate accurate test code and selectors by interacting with your running app.

**Try this prompt with Copilot Agents:**
> "Use Playwright MCP to navigate to http://localhost:3000 and generate step definitions for searching for a movie called 'Inception'. Make sure the locators match the actual UI elements."

This will help you generate code that is tailored to your app's real DOM and selectors, improving test reliability.
