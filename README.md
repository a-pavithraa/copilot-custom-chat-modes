# GitHub Copilot Custom Chat Modes and Instructions

This repository contains custom chat modes and instructions for GitHub Copilot to enhance your development experience in Visual Studio Code. Custom chat modes and repository instructions allow you to tailor Copilot's behavior to your specific needs and team workflow.

## Prerequisites

- Latest version of [Visual Studio Code](https://code.visualstudio.com/download)
- Access to [GitHub Copilot](https://github.com/github-copilot/signup)

## Custom Chat Modes

Custom chat modes in VS Code allow you to create tailored chat experiences for specific tasks. Each chat mode consists of instructions and tools that are applied automatically when you switch to that mode.

### Setting Up Custom Chat Modes

1. Create a `.github/chatmodes` directory in your workspace
2. Copy the desired `.chatmode.md` files from this repository to your workspace
3. Open the Chat view in VS Code (⌃⌘I)
4. Select your custom chat mode from the chat mode dropdown list

### Structure of Chat Mode Files

Chat mode files use the `.chatmode.md` suffix and contain:

1. Front Matter metadata:
```yaml
---
description: Description of what the chat mode does
tools: ['tool1', 'tool2', 'tool3']
model: Optional AI model specification
---
```

2. Body with chat mode instructions in Markdown format

### Available Modes

#### 1. Prompt Enhancer Chat Mode
A sophisticated system that automatically improves user prompts through:
- Context-aware enhancement
- Interactive questioning
- Few-shot learning integration

Key features:
- Automatic prompt enhancement with context addition
- Smart questioning for gathering critical information
- Pattern recognition and template building
- Adaptive inquiry based on domain and complexity

[Learn more about Prompt Enhancer Chat Mode](custom-chat-modes/prompt-enhancer.chatmode.md)

### 2. React 16 Redux Class Components Mode
A specialized mode for React 16 applications focusing on:
- Class-based components
- Redux state management
- Enzyme and Cypress testing
- 100% test coverage requirement

Key features:
- Strict architectural constraints for React 16
- Traditional Redux patterns with connect() HOC
- Comprehensive testing standards
- Performance optimization guidelines

[Learn more about React 16 Redux Mode](custom-chat-modes/react-16-legacy-mode.chatmode.md)

## Custom Instructions

### Java 17 Open Liberty Web Application
Detailed instructions for developing enterprise Java applications using:
- Java 17 (LTS)
- IBM Open Liberty server
- Gradle build tool
- Jakarta EE specifications

Key features:
- Modern Java 17 features utilization
- Open Liberty architecture best practices
- Jakarta EE implementation patterns
- Comprehensive project structure
- Performance optimization guidelines

[Learn more about Java 17 Open Liberty Instructions](custom-instructions/java17-openliberty.md)

## Usage

### Chat Modes
1. Load the desired chat mode file in your development environment
2. Follow the specific architectural constraints and patterns defined in the mode
3. Use the provided templates and examples for consistent implementation

### Custom Instructions
1. Reference the instruction files when working on specific technology stacks
2. Follow the defined project structure and coding standards
3. Implement the recommended best practices and patterns

## Contributing

Feel free to contribute by:
1. Adding new chat modes for different development scenarios
2. Enhancing existing modes with more patterns and examples
3. Adding custom instructions for other technology stacks
4. Improving documentation and examples

## License

## Repository Custom Instructions

Repository custom instructions help configure GitHub Copilot to generate responses tailored to your team's workflow and project requirements.

### Setting Up Custom Instructions

1. Create a `.github` directory in your repository root
2. Create a file named `copilot-instructions.md` inside the `.github` directory
3. Add your custom instructions in Markdown format

### Best Practices for Custom Instructions

1. Keep instructions short and self-contained
2. Focus on project-specific requirements
3. Avoid instructions that:
   - Reference external resources
   - Request specific response styles
   - Demand particular levels of detail

### Instructions Priority

When multiple instruction sets are available:
1. Personal instructions (highest priority)
2. Repository instructions
3. Organization instructions (lowest priority)

### Managing Instructions

Custom instructions are enabled by default but can be controlled:

1. For Copilot Chat:
   - Open the Chat panel
   - Click the options button
   - Select "Enable/Disable custom instructions"

2. For Copilot Code Review:
   - Go to repository Settings
   - Navigate to Copilot > Code review
   - Toggle "Use custom instructions when reviewing pull requests"


