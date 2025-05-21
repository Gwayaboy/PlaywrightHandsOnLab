# Playwright Hands-on Lunch & Learn

Welcome to the Playwright Hands-on & Lunch Learn!  
In this hands-on session, you will learn the fundamentals of UI automation using [Playwright](https://playwright.dev/) by testing the [playwright-movies-app](https://github.com/debs-obrien/playwright-movies-app).

## Agenda

- Brief overview of Behaviour Driven Development (BDD)
  - [Gherkin Fundamentals](https://cucumber.io/docs/gherkin/reference/)
  - [Acceptance Testing](https://www.agilealliance.org/glossary/acceptance/)
- Setting up the System under test (SUT) - a Movie web app
- Exercise 1: Gherkin fundamentals & scenario writing
- Exercise 2: Writing and running your first BDD Playwright test
- Exercise 3: API Testing





---

## Prerequisites

- Node.js (v18+ recommended)
- VS Code

---

## Getting Started

To get started, follow the official instructions for the [playwright-movies-app](https://github.com/debs-obrien/playwright-movies-app?tab=readme-ov-file#installation):

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
       Given I am on web movies app landing page
       When I search for "Sonic the Hedgehog 3"
       Then I should see results related to "Sonic the Hedgehog 3"
   ```

#### JavaScript/TypeScript Example (using Cucumber.js)

1. Create a new folder for your test project
2. Install cucumber dependencies:
   ```bash
   npm install --save-dev @cucumber/cucumber playwright
   ```
3. Create a `.feature` file to store your Feature and scenario (e.g., `features/movie-search.feature`). 

4. Create a step definition file (e.g., <code>features/steps/movies-search.js</code>):
5. Implement the scenarios steps in your `features/movie-search.feature`
    <details>
      <summary>Reveal sample code (if you're stuck)</summary>

      ```js
      const { Given, When, Then } = require('@cucumber/cucumber');
      const { chromium } = require('playwright');

      let browser, page;

      Given('I am on web movies app landing page', async function () {
        browser = await chromium.launch();
        page = await browser.newPage();
        await page.goto('http://localhost:3000');
      });

      When('I search for {string}', async function (title) {       
        await page.getByRole('search').click();
        var searchBox = await page.getByRole('textbox', { name: 'Search Input' });
        searchBox.fill(title);
        searchBox.press('Enter');
      });

      Then('I should see results related to {string}', async function (title) {
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

1. Follow the [Reqnroll quickstart guide](https://docs.reqnroll.net/latest/quickstart/index.html) and [Microsoft.Playwright](https://playwright.dev/dotnet/) to set up your project.
2. Create a new solution/folder for your test project
3. Create a `.feature` file to store your Feature and scenario (e.g., `MovieSearch.feature`). Example content:   
4. <details>
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

       [Given(@"I am on web movies app landing page")]
       public async Task GivenIamOnTheWebMoviesAppLandingPage()
       {
           var playwright = await Playwright.CreateAsync();
           browser = await playwright.Chromium.LaunchAsync(new BrowserTypeLaunchOptions { Headless = true });
           page = await browser.NewPageAsync();
           await page.GotoAsync("http://localhost:3000");
           
       }

       [When(@"I search for "(.*)"")]
       public async Task WhenISearchFor(string title)
       {
           await page.GetByRole('search').click();
           var searchBox = await page.getByRole('textbox', { name: 'Search Input' });
           await searchBox.FillAsync(title); // Adjust selector as needed
           await searchBox.PressAsync("Enter");
       }

       [Then(@"I should see results related to "(.*)"")]
       public async Task ThenIShouldSeeResultsRelatedTo(string title)
       {
           var results = await page.Locator($".movie-card:has-text('{title}')").CountAsync();
           if (results == 0) throw new Exception($"No results found for {title}");
           await browser.CloseAsync();
       }
   }
   ```
   </details>
5. Run your tests with.
    ```bash
    dotnet test
    ```

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
    using Microsoft.Playwright.NUnit;
    using NUnit.Framework;
    using System.Threading.Tasks;

    public class ApiSearchMovieTests : PageTest
    {
        [Test]
        public async Task SearchMovieApiReturnsCorrectResults()
        {
            var request = await Playwright.APIRequest.NewContextAsync();
            var response = await request.GetAsync("https://api.themoviedb.org/3/search/movie?query=Twisters");
            Assert.That(response.Ok, Is.True);
            var json = await response.JsonAsync();
            var results = json?.GetProperty("results");
            Assert.That(results.ToString(), Does.Contain("Twisters"));
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
- Compare your implementation to the examples in `api.spec.ts` if you get stuck.

---
