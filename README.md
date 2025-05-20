# Playwright Hands Lunch & Learn

Welcome to the Playwright Hands Lunch & Learn!  
In this hands-on session, you will learn the fundamentals of UI automation using [Playwright](https://playwright.dev/) by testing the [playwright-movies-app](https://github.com/debs-obrien/playwright-movies-app).

## Agenda

- Introduction to Playwright
- Setting up the project
- Writing and running your first Playwright test
- Gherkin fundamentals & scenario writing
- Hands-on exercises with the movies app

---

## Prerequisites

- Node.js (v18+ recommended)
- Git
- [playwright-movies-app](https://github.com/debs-obrien/playwright-movies-app) running locally at [http://localhost:3000](http://localhost:3000)

---

## Getting Started

1. **Clone this repository:**
   ```bash
   git clone https://github.com/your-org/Playwright-Hands-Lunch-Learn.git
   cd Playwright-Hands-Lunch-Learn
   ```

2. **Install dependencies:**
   ```bash
   npm install
   ```

3. **Start the movies app (in a separate terminal):**
   ```bash
   cd path/to/playwright-movies-app
   npm install
   npm run dev
   # App should be running at http://localhost:3000
   ```

---

## Exercise 1: Explore the Movies App

- Open [http://localhost:3000](http://localhost:3000)
- Try the following:
  - Browse the list of movies
  - Use the search bar to find a movie
  - Click on a movie to view its details
  - Switch between dark and light mode
  - (If available) Log in and log out

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
