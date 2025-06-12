# Playwright MCP + GitHub Copilot Demo Instructions

## Demo Overview
This demonstration showcases the integration of Playwright MCP (Model Context Protocol) with GitHub Copilot to enable intelligent web exploration and automated test creation. The demo follows the structure outlined in the [Playwright Hands-On Lab](https://github.com/Gwayaboy/PlaywrightHandsOnLab?tab=readme-ov-file#bonus-optional-use-github-copilot-agents-with-playwright-mcp-server).

---

## Demo Flow


### 1. [Install & Run Playwright MCP Server](https://github.com/Gwayaboy/PlaywrightHandsOnLab?tab=readme-ov-file#how-to-switch-to-agent-mode-and-install-playwright-mcp-server)4

#### Key Points to Explain:
- **MCP (Model Context Protocol)**: Enables AI assistants to interact with external tools and services
- **Playwright Integration**: Provides web browser automation capabilities to GitHub Copilot
- **Available Tools**: Navigation, element interaction, content extraction, screenshot capture


### 2. Web Exploration with Playwright MCP

**Demo Prompt for GitHub Copilot:**
```
Can you navigate to the main page at http://localhost:3000/ and explore the main functionality of the website. Please take a snapshot of the page and describe what you see.
```

**Deep Dive Exploration**

```
What are the main navigation elements on this page?
```

```
Can you identify all the interactive elements like buttons, forms, and links? Please describe their purposes.
```

```
Take a screenshot of the page and analyze the layout and design elements.
```

#### Expected Copilot Agent Actions:
- Navigate to the website
- Take page snapshots
- Identify UI elements
- Analyze page structure
- Describe functionality

#### Key Points to Highlight:
- Real-time web browsing through natural language
- Automatic element discovery
- Visual analysis capabilities
- No manual coding required for exploration

---

### 3. Authentication & Advanced Feature Discovery (10 minutes)

#### 3.1 Login Process

**Demo Prompt:**
```
Please log in with the test user credentials: username me@outlook.com and password 12345. Then explore what additional functionalities become available after authentication.
```

#### 3.2 Post-Authentication Exploration

**Follow-up Prompts:**
```
What new features or pages are now accessible after logging in?
```

```
Can you navigate through the authenticated user dashboard and describe the available options?
```

```
Take screenshots of the key authenticated pages and compare them with the pre-login experience.
```

#### Expected Feature to discover:
- Create my lists


### 4. Automated Scenario Creation 

#### 4.1 Generate Test Scenarios


#### 4.2 Expected Generated Features:
- Create my lists
- View my lists

#### Key Points to Demonstrate:
- AI-generated test scenarios based on actual exploration
- Comprehensive coverage of discovered functionality
- Natural language test descriptions
- Gherkin syntax compliance

---

### 5. Cucumber.js Implementation 

#### 5.1 Project Structure Setup

**Demo Prompt:**
```
Based on your exploration of the website, create comprehensive Cucumber feature files for the authenticated user journeys you discovered. Include both authenticated and non-authenticated scenarios.

Create a Cucumber.js project with Playwright using this structure:
project-root/
├── src/
│   ├── features/
│   │   └── *.feature
│   ├── step_definitions/
│   │   └── *.js
├── package.json
└── tsconfig.json

Please implement the step definitions for the scenarios you created earlier.
```

#### 5.2 Expected Project Files:
- `package.json` with dependencies
- Feature files in Gherkin syntax
- Step definition files with Playwright code
- Configuration files

#### 5.3 Run Tests

**Demo Commands:**
```bash
# Install dependencies
npm install

# Run all tests
npm test

# Run tests in headed mode (visible browser)
npm run test:headed

# Run specific scenarios
npm test -- --tags "@smoke"
```

#### Key Implementation Points:
- Playwright browser automation
- Cucumber.js integration
- Page Object Model patterns
- Async/await JavaScript syntax
- Test data management

---

### 6. Reqnroll/.NET Implementation (20 minutes)

#### 6.1 .NET Project Setup

**Demo Prompt:**
```
Now create an equivalent implementation using Reqnroll and Playwright in .NET. Use the same scenarios but implement them in C# following .NET best practices.
```

#### 6.2 Expected .NET Structure:
```
PlaywrightReqnrollDemo/
├── Features/
│   └── *.feature
├── StepDefinitions/
│   └── *.cs
├── Support/
│   └── *.cs
└── PlaywrightReqnrollDemo.csproj
```

#### 6.3 Run .NET Tests

**Demo Commands:**
```bash
# Navigate to .NET project
cd PlaywrightReqnrollDemo

# Restore packages
dotnet restore

# Build project
dotnet build

# Run tests
dotnet test

# Run with verbose output
dotnet test --logger:console;verbosity=detailed
```

#### Key Implementation Points:
- Reqnroll (SpecFlow successor) integration
- Microsoft.Playwright for .NET
- Dependency injection patterns
- MSTest framework usage
- C# async/await patterns

---

## Comparison Highlights

### Framework Comparison Table

| Aspect | Cucumber.js + Playwright | Reqnroll + Playwright |
|--------|--------------------------|------------------------|
| **Language** | JavaScript/TypeScript | C# |
| **Test Runner** | Cucumber.js | MSTest/NUnit/xUnit |
| **IDE Support** | VS Code, WebStorm | Visual Studio, VS Code |
| **Package Manager** | npm/yarn | NuGet |
| **Ecosystem** | Node.js ecosystem | .NET ecosystem |
| **Performance** | Good | Excellent |
| **Enterprise Support** | Community | Microsoft backed |

### Key Differences to Highlight:
- **Syntax**: JavaScript vs C# implementations
- **Tooling**: Different IDE experiences
- **Debugging**: Browser dev tools vs Visual Studio debugger
- **CI/CD**: Different pipeline configurations
- **Team Skills**: JavaScript vs .NET developer preferences

---

## Demo Tips & Best Practices

### Before Starting the Demo:
1. **Test Everything**: Run through the entire demo beforehand
2. **Prepare Fallbacks**: Have screenshots ready if live demo fails
3. **Check Dependencies**: Ensure all tools are installed and working
4. **Network**: Verify internet connectivity for package installations
5. **Backup Plan**: Have pre-built examples ready

### During the Demo:
1. **Explain as You Go**: Describe what Copilot is doing
2. **Show Thinking Process**: Demonstrate iterative prompting
3. **Handle Errors Gracefully**: Use mistakes as learning opportunities
4. **Engage Audience**: Ask about their current testing approaches
5. **Time Management**: Keep each section within allocated time

### Common Issues & Solutions:

#### MCP Connection Problems:
- **Issue**: Copilot can't connect to MCP server
- **Solution**: Restart VS Code and MCP server, check configuration

#### Browser Launch Failures:
- **Issue**: Playwright can't launch browser
- **Solution**: Run `npx playwright install` again, check system permissions

#### Port Conflicts:
- **Issue**: Application not accessible on localhost:3000
- **Solution**: Check if port is in use, restart application

#### Package Installation Errors:
- **Issue**: npm or dotnet restore fails
- **Solution**: Clear caches, check network connectivity, use alternative registries

---

## Audience Engagement Questions

### Opening Questions:
- "Who here is currently using automated testing?"
- "What's your biggest challenge with test maintenance?"
- "Have you tried AI-assisted coding before?"

### During Demo:
- "What do you think about AI discovering test scenarios?"
- "How does this compare to your current testing approach?"
- "Which implementation style appeals to your team more?"

### Closing Questions:
- "What use cases can you see for this in your projects?"
- "What concerns do you have about AI-generated tests?"
- "How might this change your testing strategy?"

---

## Follow-up Resources

### Documentation Links:
- [Playwright MCP Server](https://github.com/microsoft/playwright-mcp-server)
- [GitHub Copilot Documentation](https://docs.github.com/en/copilot)
- [Cucumber.js Documentation](https://cucumber.io/docs/cucumber/)
- [Reqnroll Documentation](https://docs.reqnroll.net/)
- [Playwright Documentation](https://playwright.dev/)

### Next Steps for Attendees:
1. Set up Playwright MCP in their environment
2. Try the exploration prompts on their applications
3. Generate test scenarios for existing projects
4. Implement automated tests using preferred framework
5. Integrate with existing CI/CD pipelines

### Advanced Topics for Future Sessions:
- Visual regression testing with AI
- Performance testing integration
- Cross-browser testing strategies
- Test data management with AI
- Accessibility testing automation

---

## Demo Checklist

### Pre-Demo (30 minutes before):
- [ ] Start test application on localhost:3000
- [ ] Launch Playwright MCP server
- [ ] Open VS Code with GitHub Copilot
- [ ] Verify MCP connection
- [ ] Test login credentials
- [ ] Prepare demo workspace
- [ ] Clear browser data/cache

### During Demo:
- [ ] Introduce the problem Playwright MCP solves
- [ ] Show installation and setup process
- [ ] Demonstrate web exploration capabilities
- [ ] Perform authentication and discovery
- [ ] Generate test scenarios automatically
- [ ] Implement Cucumber.js version
- [ ] Implement Reqnroll/.NET version
- [ ] Compare both approaches
- [ ] Address audience questions

### Post-Demo:
- [ ] Share demo repository link
- [ ] Provide follow-up resources
- [ ] Schedule follow-up sessions if needed
- [ ] Collect feedback for improvement

---

*Demo Duration: Approximately 90 minutes including Q&A*
