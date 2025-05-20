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

- `npm run dev`

The app should be running at [http://localhost:3000](http://localhost:3000)

---

## Exercise 1: Describe Movie App Scenarios with Gherkin

In this exercise, you'll practice writing Cucumber (Gherkin) scenarios that describe the core functionality of the movies app. Focus on what the user wants to achieve, not how the UI looks or is implemented.

### Functional Actions to Try
- Browse the list of movies
- Search a movie
- View details of a movie
- Switch between dark and light mode
- (If available) Log in and log out

### Example Scenario: Browse the List of Movies

```gherkin
Feature: Browse Movies
  As a movie fan
  I want to see a list of available movies
  So that I can choose one to watch

  Scenario: Viewing the movies list
    Given the movies app is running
    When the user navigates to the homepage
    Then the user should see a list of movies
```

### Your Turn: Write a Scenario for Searching a Movie

Write a Gherkin scenario that describes how a user would search for a movie by title. Focus on the user's intent and the expected outcome, not the UI steps (e.g., avoid mentioning clicking buttons or entering text in fields directly).

#### Guidance for Writing Good Scenarios
- Use the structure: `Given` (initial context), `When` (action), `Then` (expected outcome)
- Keep steps high-level and focused on behavior, not UI details
- Make scenarios readable and meaningful to both technical and non-technical team members
- Example scenario ideas:
  - Searching for a movie that exists
  - Searching for a movie that does not exist
  - Searching with an empty query

#### Example Prompt for Students
> Write a scenario for searching for a movie called "Inception". What should happen if the movie exists? What if it does not?

#### More Guidance
- Avoid steps like "click the search button"; instead, use "the user searches for a movie by title"
- Use clear and concise language
- Each scenario should describe a single behavior or outcome

When you're done, share your scenarios with the group for feedback and discussion.

---

## Exercise 2: Your First Playwright Test

**Goal:** Write a Playwright test to verify that the homepage loads and displays a list of movies.

```gherkin
Feature: Movies Homepage

  Scenario: User visits the homepage
    Given the movies app is running
    When the user navigates to "http://localhost:3000"
    Then the user should see a list of movies
```

**Playwright Example:**
```js
import { test, expect } from '@playwright/test';

test('homepage displays list of movies', async ({ page }) => {
  await page.goto('http://localhost:3000');
  await expect(page.locator('.movie-list')).toBeVisible();
  await expect(page.locator('.movie-card')).toHaveCountGreaterThan(0);
});
```

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
