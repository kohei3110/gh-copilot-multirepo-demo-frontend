# Hands-On Tutorial: Building a TODO App Frontend with Copilot Orchestra

This hands-on guide walks you through building a TODO application frontend using GitHub Copilot Orchestra. You'll learn how to leverage multiple AI agents to streamline your development workflow with React and TypeScript.

## 🎯 Learning Objectives

By completing this tutorial, you will:
- Set up a dev container environment with necessary tools
- Understand the Copilot Orchestra workflow
- Build a modern React UI with TypeScript
- Integrate with a REST API backend
- Implement TODO management features
- Add responsive design and user interactions
- Write tests and ensure code quality
- Create pull requests using AI-assisted workflows

## 📚 Prerequisites

Before starting, ensure you have:
- Basic knowledge of React and TypeScript
- Basic understanding of REST APIs
- Docker Desktop installed
- VS Code with Dev Containers extension
- GitHub Copilot subscription
- GitHub account

## 🏁 Part 1: Environment Setup

### Step 1.1: Open the Project in Dev Container

1. Open this project in VS Code
2. When prompted "Reopen in Container", click **Reopen in Container**
   - Or press `F1` and select `Dev Containers: Reopen in Container`
3. Wait for the container to build (first time takes ~2-3 minutes)
4. Once complete, you'll have a fully configured environment with:
   - Node.js 20
   - Git CLI
   - GitHub CLI (gh)
   - GitHub Copilot
   - Web Search for Copilot extension
   - ESLint & Prettier

### Step 1.2: Verify Your Environment

Open the integrated terminal and run:

```bash
# Check Node.js version
node --version  # Should show v20.x.x

# Check Git
git --version

# Check GitHub CLI
gh --version

# Check npm packages are installed
npm list
```

### Step 1.3: Authenticate GitHub CLI

```bash
gh auth login
```

Follow the prompts to authenticate with your GitHub account.

## 🤖 Part 2: Understanding Copilot Orchestra

Copilot Orchestra coordinates multiple specialized AI agents:

```
User Request
    ↓
[Orchestrator Agent] ← You are here
    ↓
    ├─→ [Issue Agent] ────→ Creates GitHub Issue
    ↓
    ├─→ [Plan Agent] ─────→ Designs Implementation Plan
    ↓
    ├─→ [Impl Agent] ─────→ Writes Code
    ↓
    ├─→ [Review Agent] ───→ Reviews & Improves Code
    ↓
    └─→ [PR Agent] ───────→ Creates Pull Request
```

### Key Concepts

1. **Orchestrator Agent**: Manages the overall workflow
2. **Issue Agent**: Understands requirements and creates detailed issues
3. **Plan Agent**: Breaks down tasks into actionable steps
4. **Implementation Agent**: Writes code following the plan
5. **Review Agent**: Ensures code quality and best practices
6. **PR Agent**: Creates comprehensive pull requests

## 🔨 Part 3: Building the TODO Frontend

### Step 3.1: Explore the Existing Code Structure

Let's first understand what we have:

```bash
# View the current structure
tree -L 2 src/
```

You'll see:
- `App.tsx` - Main application component
- `main.tsx` - Application entry point
- `assets/` - Images and static resources
- `components/` - React components (to be created)

### Step 3.2: Start the Development Server

```bash
npm run dev
```

The dev server should start on `http://localhost:5173`. Your browser should automatically open, or you can navigate to the URL manually.

### Step 3.3: Understanding the Vite Setup

This project uses Vite for:
- ⚡️ Lightning-fast HMR (Hot Module Replacement)
- 📦 Optimized production builds
- 🎨 Built-in TypeScript support
- 🔧 Simple configuration

### Step 3.4: Project Structure Overview

```
gh-copilot-multirepo-demo-frontend/
├── src/
│   ├── App.tsx              # Main app component
│   ├── main.tsx             # Entry point
│   ├── components/          # React components
│   │   ├── TodoList.tsx     # Todo list component
│   │   ├── TodoItem.tsx     # Individual todo item
│   │   └── TodoForm.tsx     # Form for adding todos
│   ├── hooks/               # Custom React hooks
│   ├── services/            # API communication
│   ├── types/               # TypeScript definitions
│   └── utils/               # Helper functions
├── public/                  # Static assets
└── index.html              # HTML template
```

## 🎨 Part 4: Building the UI Components

### Step 4.1: Understanding Component Architecture

The TODO app will consist of these main components:

1. **App.tsx** - Root component, manages state and layout
2. **TodoList.tsx** - Displays list of todos
3. **TodoItem.tsx** - Individual todo item with actions
4. **TodoForm.tsx** - Form for creating new todos

### Step 4.2: Connecting to the Backend

The frontend will communicate with the backend API:

| Frontend Action | Backend Endpoint | Method |
|----------------|------------------|--------|
| Load todos | `/api/todos` | GET |
| Create todo | `/api/todos` | POST |
| Update todo | `/api/todos/:id` | PUT |
| Delete todo | `/api/todos/:id` | DELETE |

### Step 4.3: State Management Strategy

This project uses React's built-in state management:
- `useState` for local component state
- `useEffect` for side effects (API calls)
- Custom hooks for reusable logic

## 🧪 Part 5: Working with Copilot

### Step 5.1: Using Copilot Chat

1. Open Copilot Chat (`Ctrl+Cmd+I` on Mac, `Ctrl+Shift+I` on Windows/Linux)
2. Try asking:
   ```
   @workspace How do I create a new React component for displaying a todo item?
   ```

3. Copilot will analyze your workspace and provide context-aware suggestions

### Step 5.2: Inline Suggestions

1. Open `src/App.tsx`
2. Start typing a comment: `// Create a function to fetch todos from the API`
3. Press `Tab` to accept Copilot's suggestion or use `Delegate to agent`

### Step 5.3: Delegate Tasks to the Orchestrator Agent

1. Open Copilot Chat and select `orchestrator`

2. Enter: `I want to implement a component that displays todos in a grid layout with filtering by status`

3. Verify that Todos are created like the following:

- Create a GitHub issue with the issue agent
- Create an implementation plan with the plan agent
- Implement with the impl agent
- Perform code review with the review agent

**Please note that the wording may vary. It's OK if you can confirm that the orchestrator agent is distributing tasks to each agent.**

## 🎯 Practice Exercises

### Exercise 1: Add Todo Priority
Implement a priority system (High, Medium, Low) for todos with color coding.

### Exercise 2: Implement Search Functionality
Add a search bar to filter todos by title or description.

### Exercise 3: Add Dark Mode
Implement a dark mode toggle with theme persistence in localStorage.

### Exercise 4: Create a Dashboard
Build a dashboard showing statistics (total todos, completed, pending, etc.).

### Exercise 5: Add Animations
Use CSS transitions or libraries like Framer Motion to add smooth animations.

## 🎨 Styling Best Practices

### Using CSS Modules
```tsx
import styles from './TodoItem.module.css'

function TodoItem() {
  return <div className={styles.todoItem}>...</div>
}
```

### Responsive Design
```css
/* Mobile first approach */
.container {
  padding: 1rem;
}

@media (min-width: 768px) {
  .container {
    padding: 2rem;
  }
}
```

## 🧪 Testing Your Components

### Step 6.1: Writing Component Tests

```bash
# Install testing dependencies
npm install --save-dev @testing-library/react @testing-library/jest-dom vitest
```

### Step 6.2: Example Test

```tsx
import { render, screen } from '@testing-library/react'
import TodoItem from './TodoItem'

test('renders todo item', () => {
  render(<TodoItem title="Test Todo" completed={false} />)
  expect(screen.getByText('Test Todo')).toBeInTheDocument()
})
```

## 🐛 Troubleshooting

### Container Won't Build
- Check Docker is running
- Try rebuilding: `Dev Containers: Rebuild Container`

### Copilot Not Working
- Verify your subscription is active
- Check extension is enabled
- Try reloading VS Code

### API Connection Issues
- Ensure backend server is running
- Check the API URL in your configuration
- Verify CORS settings on the backend

### Vite Build Errors
- Clear node_modules and reinstall: `rm -rf node_modules && npm install`
- Check for TypeScript errors: `npm run build`

## 📚 Additional Resources

- [React Documentation](https://react.dev/)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [Vite Guide](https://vite.dev/guide/)
- [GitHub Copilot Documentation](https://docs.github.com/copilot)
- [copilot-orchestra](https://github.com/ShepAlderson/copilot-orchestra)

## 🚀 Next Steps

After completing this tutorial, consider:
1. Adding user authentication
2. Implementing real-time updates with WebSockets
3. Adding offline support with Service Workers
4. Deploying to Azure Static Web Apps or similar platforms
5. Integrating with the Web Push notification feature from the backend