# Playwright Hands-on Lunch & Learn

Welcome to the Playwright Hands-on & Lunch Learn!  

In this hands-on session, you will learn the fundamentals of UI automation using [Playwright](https://playwright.dev/) by testing the [playwright-movies-app](https://github.com/debs-obrien/playwright-movies-app).

Enjoy!

## Agenda

1. **[Brief overview of Behaviour Driven Development (BDD)](https://microsofteur-my.sharepoint.com/:p:/g/personal/frtheola_microsoft_com/ERZ-s8_OCkZFl5w79vhjCv8BEOTfA5lImxfljcyZFDDrYQ?e=ojKdLj)**
    - [Gherkin Fundamentals](https://cucumber.io/docs/gherkin/reference/)
    - [Acceptance Testing](https://www.agilealliance.org/glossary/acceptance/)
2. **[Setting up the System under test (SUT) - a Movie web app](?tab=readme-ov-file#prerequisites)**
3. **[Exercise 1: Gherkin fundamentals & scenario writing](?tab=readme-ov-file#exercise-1-describe-movie-app-scenarios-with-gherkin)**
4. **[Exercise 2: Writing and running your first BDD Playwright test](?tab=readme-ov-file#exercise-2-implement-your-bdd-playwright-test)**
    - [Bonus: Vibe coding - Use GitHub Copilot agentic feature & Playwright MCP to generate BDD tests](?tab=readme-ov-file#bonus-optional-use-github-copilot-agents-with-playwright-mcp-server)
5. **[Exercise 3: API Testing](?tab=readme-ov-file#exercise-3-api-testing-with-playwright)**
    - [Bonus : Implement more API tests](?tab=readme-ov-file#bonus-more-api-test-ideas)
6. **[Advanced Bonus Exercices](?tab=readme-ov-file#bonus-advanced-exercises)**
    - Exercise 4: Snapshot Testing with Playwright
    - Exercise 5: Mock API Responses in Playwright
7. **[Feedback](https://forms.office.com/r/CDmayftBvF)** : How was it?  What other topics would you like to explore?
  
    [![alt text](assets/FeedbackQR.png)](https://forms.office.com/r/CDmayftBvF)


---

## Prerequisites

- [Git for Windows](https://gitforwindows.org/)
- [Node.js/npm (v18+ recommended)](https://nodejs.org/en)
- [VS Code](https://code.visualstudio.com/Download)


---

## Getting Started

To get started, follow the official installation instructions from the [playwright-movies-app](https://github.com/debs-obrien/playwright-movies-app?tab=readme-ov-file#installation) (also below):

Open a bash, command or powershell prompt:

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
- Log in and log out with test user name `me@outlook.com` and password `12345`

### Write a Scenario for Searching a Movie

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

## Exercise 2: Implement your BDD Playwright Test

**Goal:** Write a Playwright test to verify that searching for a movie displays the correct results.

### Step 1: Implement Your Scenario with a Cucumber Framework

You can use a Cucumber framework to automate your Gherkin scenarios. Below are examples for both JavaScript/TypeScript and C# using [Reqnroll](https://docs.reqnroll.net/latest/quickstart/index.html).

An example ```Movie Search``` Feature and ```Searching for a movie that exists``` scenario:

 ```gherkin
   Feature: Movie Search
     As a movie fan
     I want to search for a movie by title
     So that I can quickly find the movie I want to watch

     Scenario: Searching for a movie that exists
       Given the user is on the web movies app landing page
       When the user searches for "Sonic the Hedgehog 3"
       Then the user should see results related to "Sonic the Hedgehog 3"
   ```

#### JavaScript/TypeScript Example (using Cucumber.js)

1. Create a new folder for your test project
2. Install cucumber dependencies:
   ```bash
   npm install --save-dev @cucumber/cucumber playwright
   ```
3. Create a `.feature` file to store your Feature and scenario (e.g., `features/movie-search.feature`). 

4. Create a step definition file (e.g., <code>steps/movies-search-steps.js</code>).
  A typical cucumber js project structure looks like:
    ```
    project-root/
    ├── src/
    │   ├── features/
    │   │   └── *.feature
    |   |   ├── steps/
    │   │       └── *.js
    ├── package.json
    ├── tsconfig.json
    ```

5. Implement the scenarios steps in your `steps/movies-search-steps.js`
    <details>
      <summary>Reveal sample code (if you're stuck)</summary>

      ```js
      const { Given, When, Then } = require('@cucumber/cucumber');
      const { chromium } = require('playwright');

      let browser, page;

      Given('The is on the web movies app landing page', async function () {
        browser = await chromium.launch();
        page = await browser.newPage();
        await page.goto('http://localhost:3000');
      });

      When('The user searches for {string}', async function (title) {       
        await page.getByRole('search').click();
        var searchBox = await page.getByRole('textbox', { name: 'Search Input' });
        searchBox.fill(title);
        searchBox.press('Enter');
      });

      Then('The user should see results related to {string}', async function (title) {
        await page.waitForSelector('.movie-list');
        const results = await page.$$(".movie-card:has-text('" + title + "')");
        if (results.length === 0) throw new Error('No results found for ' + title);
        await browser.close();
      });
      ```
    
  </details>
  
  6. Run your tests with:
      ```bash
      npx cucumber-js
      ```

#### C# Example (using Reqnroll + Playwright)

1. Follow the [Reqnroll/Playwright quickstart guide](https://blog.testery.io/reqnroll-and-playwright-in-net/) to set up your project.
2. Create a new solution/folder for your test project
3. Create a `.feature` file to store your Feature and movie search scenario (e.g., `MovieSearch.feature`). 
4. Create a step definition file (e.g., <code>Steps/MovieSearchSteps.cs</code>).
   <details>
   <summary>Click to reveal a sample step definition class (if you're stuck)</summary>
   <p>
   Example step definitions:
   </p>

   ```csharp
   using Reqnroll;
   using Microsoft.Playwright;
   using System.Threading.Tasks;

   [Binding]
   public class MovieSearchSteps
   {
       private IPage page;
       private IBrowser browser;

       [Given(@"The use is on the web movies app landing page")]
       public async Task GivenIamOnTheWebMoviesAppLandingPage()
       {
           var playwright = await Playwright.CreateAsync();
           browser = await playwright.Chromium.LaunchAsync(new BrowserTypeLaunchOptions { Headless = true });
           page = await browser.NewPageAsync();
           await page.GotoAsync("http://localhost:3000");
           
       }

       [When(@"The user searches for "(.*)"")]
       public async Task WhenISearchFor(string title)
       {
           await page.GetByRole('search').click();
           var searchBox = await page.getByRole('textbox', { name: 'Search Input' });
           await searchBox.FillAsync(title); // Adjust selector as needed
           await searchBox.PressAsync("Enter");
       }

       [Then(@"The user should see results related to "(.*)"")]
       public async Task ThenIShouldSeeResultsRelatedTo(string title)
       {
           var results = await page.Locator($".movie-card:has-text('{title}')").CountAsync();
           if (results == 0) throw new Exception($"No results found for {title}");
           await browser.CloseAsync();
       }
   }
   ```
   </details>
6. Run your tests with.
    ```bash
    dotnet test
    ```
---
### Bonus (optional): Use GitHub Copilot Agents with Playwright MCP Server

You can leverage GitHub Copilot agents with a Playwright MCP server to generate accurate test code and selectors by interacting with your running app.

#### How to Switch to Agent Mode and Install Playwright MCP Server

1. **Install Playwright MCP Server**
   - You can install the Playwright MCP extension directly from the VS Code Marketplace (Click on the relevant button below):
  
     [<img src="https://img.shields.io/badge/VS_Code-VS_Code?style=flat-square&label=Install%20Server&color=0098FF" alt="Install in VS Code">](https://insiders.vscode.dev/redirect?url=vscode%3Amcp%2Finstall%3F%257B%2522name%2522%253A%2522playwright%2522%252C%2522command%2522%253A%2522npx%2522%252C%2522args%2522%253A%255B%2522%2540playwright%252Fmcp%2540latest%2522%255D%257D) [<img alt="Install in VS Code Insiders" src="https://img.shields.io/badge/VS_Code_Insiders-VS_Code_Insiders?style=flat-square&label=Install%20Server&color=24bfa5">](https://insiders.vscode.dev/redirect?url=vscode-insiders%3Amcp%2Finstall%3F%257B%2522name%2522%253A%2522playwright%2522%252C%2522command%2522%253A%2522npx%2522%252C%2522args%2522%253A%255B%2522%2540playwright%252Fmcp%2540latest%2522%255D%257D)
 
   - For more details, see the [Playwright MCP GitHub repo](https://github.com/microsoft/playwright-mcp).

2. **Start the Playwright MCP Server**
   - Open the Command Palette (`Ctrl+Shift+P`), search for `Playwright MCP: Start Server`, and start it.

3. **Switch to Agent Mode in Copilot**
   - In VS Code, open the Copilot Chat sidebar.
   - Select Agent Mode from the drop-down to enable Agent Mode.

    ![alt text](assets/AgentMode.png)
   - Click on the tools icon to validate Playwright MCP tools are available

    ![alt text](assets/MCP_Tools.png)

4. **Prompt Copilot Agent for Accurate Locators**
   - Provide the feature file as reference
   - Make sure your [app is running locally](/?tab=readme-ov-file#running-the-app-locall) at [http://localhost:3000](http://localhost:3000)
   - Create a `test_prompt.md` file to your solutions under `.github/prompts` and copy/paste the following instructions:
     ```
      - You are a Playwright test generator.
      - You are given a scenario, and your task is to generate a Playwright test.
      - Do not generate test code based on the scenario alone.
      - Do run steps one by one using the tools provided by Playwright.
      - Only after all steps are completed, emit a Playwright TypeScript file.
      - Save the generated test file in the tests directory.
      - Execute the test file and iterate until the test passes.
        ```
   - Try this prompt:

     ```
     Based on the feature file, implement the scenario using  cucumber.js and playwright. 
     Using Playwright MCP server tools, navigate to http://localhost:3000 and generate step definitions for searching for a movie called 'Sonic The HedheHog 3'.
     Make sure the locators match the actual UI elements.
     Respect cucumber js folder structure to make sure features, step definition are discoverable and executable
     Execute and verify all test passes.     
     ```

    This will help you generate code that is tailored to your app's real DOM and selectors, improving test reliability.
---

## Exercise 3: API Testing with Playwright

**Goal:** Write a Playwright API test to verify that searching for a movie via the API returns the correct results.

### Step 1: Implement Your API Test

You can use Playwright’s API testing capabilities to directly test your backend endpoints. Below are examples for both JavaScript/TypeScript and C# (.NET).

#### JavaScript/TypeScript Example (using Playwright Test)

1. Create a new test file (e.g., `tests/api/search-movie.spec.ts`).
2. Use Playwright’s `request` fixture to send API requests.
3. Try to implement the test yourself first! If you get stuck, reveal the sample code below:

    <details>
    <summary>Reveal sample code (if you're stuck)</summary>

    ```typescript
    import { test, expect } from '@playwright/test';

    test('search movie API returns correct results', async ({ request }) => {
      const response = await request.get('https://api.themoviedb.org/3/search/movie', {
        params: { query: 'Twisters' },
        // Add authorization headers if required
      });
      await expect(response).toBeOK();
      const json = await response.json();
      expect(json.results).toEqual(
        expect.arrayContaining([
          expect.objectContaining({
            title: 'Twisters',
          }),
        ])
      );
    });
    ```
</details>

4. Run your test with:
   ```bash
   npx playwright test tests/api/search-movie.spec.ts
   ```

#### C# Example (using Playwright .NET)

1. Create a new test class (e.g., `ApiSearchMovieTests.cs`).
2. Use Playwright’s APIRequestContext to send API requests.
3. Try to implement the test yourself first! If you get stuck, reveal the sample code below:

    <details>
    <summary>Reveal sample code (if you're stuck)</summary>

    ```csharp
    using Microsoft.Playwright;
    using NUnit.Framework;
    using System.Text.Json;
    using System.Threading.Tasks;

    public class ApiSearchMovieTests
    {
        private IAPIRequestContext _request;

        [SetUp]
        public async Task SetUp()
        {
            var playwright = await Playwright.CreateAsync();
            _request = await playwright.APIRequest.NewContextAsync();
        }

        [TearDown]
        public async Task TearDown()
        {
            await _request.DisposeAsync();
        }

        [Test]
        public async Task SearchMovieApiReturnsCorrectResults()
        {
            var response = await _request.GetAsync("https://api.themoviedb.org/3/search/movie?query=Twisters");
            var body = await response.TextAsync();
            var json = JsonDocument.Parse(body);
            var results = json.RootElement.GetProperty("results").ToString();

            Assert.That(response.Ok && results.Contains("Twisters"),
                $"Expected response to be OK and results to contain 'Twisters'.\nStatus: {response.Status}\nBody: {body}");
        }
    }
    ```
    </details>

4. Run your test with:
   ```bash
   dotnet test
   ```

---

### Bonus: More API Test Ideas

Try implementing additional API tests based on the scenarios in the app. Here are some ideas (see `tests/logged-out/api.spec.ts` for inspiration):

- Test that the popular movies endpoint returns movies sorted by popularity (descending).
- Test that the top rated movies endpoint returns movies sorted by vote average (descending).
- Test that the upcoming movies endpoint returns movies sorted by release date (descending).
- Test that the action genre filter only returns action movies.
- Test that the movie credits endpoint returns expected cast and crew for a given movie.
- Test that sorting by different fields (popularity, vote average, original title, release date) works as expected.
- Test that the configuration endpoint returns the expected structure.

Challenge yourself to:
- Write the test without looking at the sample code first.
- Compare your implementation to the examples in [`api.spec.ts`](https://github.com/debs-obrien/playwright-movies-app/blob/main/tests/logged-out/api.spec.ts) if you get stuck.

---

## Bonus Advanced Exercises

<details>
<summary><strong>Exercise 4: Visual Regression Testing with Playwright (ARIA Snapshots)</strong></summary>

**Goal:** Learn how to use Playwright's ARIA snapshot feature to catch unexpected accessibility and UI changes.

1. Create a new test file (e.g., `tests/visual/movie-aria-snapshot.spec.ts`).
2. Write a Playwright test that navigates to a movie details page and takes an ARIA snapshot:
    
      <details>
      <summary>Reveal sample code (if you're stuck)</summary>
      
      ```typescript
      import { test, expect } from '@playwright/test';

      test('movie page ARIA snapshot', async ({ page }) => {
        await page.goto('http://localhost:3000/movie?id=1079091&page=1');
        await expect(page.getByRole('main')).toMatchAriaSnapshot(`
          - main:
            - heading "It Ends With Us" [level=1]
            - heading "We break the pattern or the pattern breaks us." [level=2]
            - text: ★ ★ ★ ★ ★ ★
            - paragraph: "7.173"
            - text: English / 131 min. / 2024
            # ...add more ARIA structure as needed for your test
        `);
      });
      ```

      </details>

3. Run your test with:
   ```bash
   npx playwright test tests/visual/movie-aria-snapshot.spec.ts
   ```
4. Try making a small UI or accessibility change and rerun the test to see how Playwright detects ARIA tree differences.


✅ <strong>Tips for ARIA Snapshot Testing</strong>

- Use `.toMatchAriaSnapshot()` to compare the accessibility tree, not just the visual layout.
- [Snapshopt testing with playwright](https://playwright.dev/docs/aria-snapshots)
- Focus on headings, roles, and key text for robust accessibility checks.
- Refer to the [movie-aria-snapshots.spec.ts](https://github.com/debs-obrien/playwright-movies-app/blob/main/tests/logged-out/movie-aria-snapshots.spec.ts) for a full example if you need help, but use it only as a last resort to maximize your learning.



</details>

<details>
<summary><strong>Exercise 5: Network Interception & Mocking</strong></summary>

**Goal:** Practice intercepting and mocking network requests to test UI behavior under different backend conditions.

1. Create a new test file (e.g., `tests/network/movie-mock-api.spec.ts`).
2. Write a test that intercepts the movie details API and returns a custom response:
    <details>
    <summary>Reveal sample code (if you're stuck)</summary>
    
    ```typescript
    import { test, expect } from '@playwright/test';

    test('movie details page with mocked API', async ({ page }) => {
      await page.route('**/api/movie*', async route => {
        await route.fulfill({
          status: 200,
          contentType: 'application/json',
          body: JSON.stringify({
            title: 'Mocked Movie',
            overview: 'This is a mocked movie overview.',
            // ...add other required fields
          }),
        });
      });
      await page.goto('http://localhost:3000/movie?id=1079091&page=1');
      await expect(page.getByRole('heading', { name: 'Mocked Movie' })).toBeVisible();
    });
    ```
    <details>
3. Try mocking error responses (e.g., 500 or 404) and verify the app displays error messages as expected.

✅ <strong>Tips for Network Mocking</strong>

- Use `page.route()` to intercept and modify network requests.
- Test edge cases: empty results, slow responses, and error codes.
- Combine with visual or accessibility assertions for robust tests.


</details>

---
Please provide us [feedback](https://forms.office.com/r/CDmayftBvF) : 
- How was it?  
- What other topics would you like to explore?
  
   [![alt text](assets/FeedbackQR.png)](https://forms.office.com/r/CDmayftBvF)
