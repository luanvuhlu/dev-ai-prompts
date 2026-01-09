# Developer AI Prompts
A collection of prompts designed to assist developers in various coding tasks using AI.

## Usage

Each prompt is structured to provide clear instructions for specific coding tasks. Users can copy and paste these prompts into their AI tools to generate code, documentation, or tests as needed.

### Sammple Input Form for Unit Test Request

The prompt itself is structured to gather necessary information for generating unit tests. You can just link the file you want to write tests.
Additionally, can provide more context or specific scope.
**Example:**  
Write unit tests for UserService class that handles user creation and validation.
Business rules:
- Username and email are required fields.
- Email must be unique.
- On successful creation, a welcome email is sent via EmailService.
Language: Java
Test framework: JUnit 5, Mockito, Spring Boot Test
Focus: Need complete coverage including edge cases, security and validation rules.

### Code Review Prompt

*Note*: To review a PR or a branch, can use AI agent to collect the code diff or changes, no need to paste entire code into prompt.