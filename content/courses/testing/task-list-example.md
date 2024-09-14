---
modified: 2024-09-14T10:37:18-06:00
---
## Building a To-Do List Application in React Using Test-Driven Development with Vitest

**Table of Contents**

1. [Introduction](#introduction)
2. [Prerequisites](#prerequisites)
3. [Project Setup](#project-setup)
4. [Understanding Test-Driven Development](#understanding-test-driven-development)
5. [Setting Up React with Vitest](#setting-up-react-with-vitest)
6. [Designing the To-Do List Application](#designing-the-to-do-list-application)
7. [Implementing the Application with TDD](#implementing-the-application-with-tdd)
   - [1. Creating the ToDo Component](#1-creating-the-todo-component)
   - [2. Fetching To-Dos from an API](#2-fetching-to-dos-from-an-api)
   - [3. Adding a New To-Do](#3-adding-a-new-to-do)
   - [4. Marking a To-Do as Completed](#4-marking-a-to-do-as-completed)
   - [5. Deleting a To-Do](#5-deleting-a-to-do)
8. [Mocking the API with MSW](#mocking-the-api-with-msw)
9. [Running the Tests](#running-the-tests)
10. [Conclusion](#conclusion)
11. [Additional Exercises](#additional-exercises)

---

### Introduction

In this guide, we'll build a **To-Do List** application using **Test-Driven Development (TDD)** with **React** and **Vitest**, a modern JavaScript testing framework. The application will interact with an API to fetch, add, update, and delete to-do items. We'll mock the API using **Mock Service Worker (MSW)** to simulate server responses during testing.

**Objectives:**

- Learn how to apply TDD in a React application.
- Understand how to write unit and integration tests with Vitest and React Testing Library.
- Use MSW to mock API interactions in tests.
- Develop a To-Do List application with features like fetching tasks, adding new tasks, marking tasks as completed, and deleting tasks.

### Prerequisites

- Basic knowledge of JavaScript (ES6+ syntax) and React.
- Familiarity with Node.js and npm.
- Understanding of unit testing concepts.
- Node.js and npm installed on your machine.

### Project Setup

1. **Create a New React App Using Vite**

   We'll use **Vite** to bootstrap our React application quickly.

   ```bash
   npm create vite@latest todo-app-tdd -- --template react
   cd todo-app-tdd
   ```

2. **Install Dependencies**

   ```bash
   npm install
   ```

3. **Install Vitest and Related Packages**

   ```bash
   npm install --save-dev vitest @testing-library/react @testing-library/jest-dom jsdom msw
   ```

   - `vitest`: Testing framework.
   - `@testing-library/react`: Utilities for testing React components.
   - `@testing-library/jest-dom`: Custom Jest matchers for the DOM.
   - `jsdom`: Simulates a browser environment for Node.js.
   - `msw`: Mock Service Worker for API mocking.

4. **Project Structure**

   ```ts
   todo-app-tdd/
   ├── node_modules/
   ├── public/
   ├── src/
   │   ├── components/
   │   ├── App.jsx
   │   ├── main.jsx
   │   ├── index.css
   │   ├── mocks/
   │   └── tests/
   ├── tests/
   ├── package.json
   ├── vite.config.js
   └── vitest.config.js
   ```

### Understanding Test-Driven Development

**Test-Driven Development (TDD)** is a software development approach where you:

1. **Write a Test**: Write a test for the next bit of functionality.
2. **Run the Test and See It Fail**: Ensures the test detects the absence of functionality.
3. **Write the Minimal Code to Pass the Test**: Implement just enough code to make the test pass.
4. **Refactor**: Improve the code while keeping the tests passing.
5. **Repeat**: Continue with the next functionality.

This cycle is often referred to as **Red-Green-Refactor**.

### Setting Up React with Vitest

1. **Configure Vitest**

   Create a `vitest.config.js` file in the root directory:

   ```javascript
   // vitest.config.js
   import { defineConfig } from 'vitest/config';
   import react from '@vitejs/plugin-react';

   export default defineConfig({
     plugins: [react()],
     test: {
       globals: true,
       environment: 'jsdom',
       setupFiles: './tests/setupTests.js',
     },
   });
   ```

2. **Create a Test Setup File**

   Create `tests/setupTests.js` to set up testing utilities:

   ```javascript
   // tests/setupTests.js
   import '@testing-library/jest-dom';
   ```

3. **Update `package.json` Scripts**

   ```json
   {
     "scripts": {
       "dev": "vite",
       "build": "vite build",
       "preview": "vite preview",
       "test": "vitest",
       "test:watch": "vitest --watch",
       "coverage": "vitest run --coverage"
     }
   }
   ```

### Designing the To-Do List Application

**Features:**

- Display a list of to-do items fetched from an API.
- Add a new to-do item.
- Mark a to-do item as completed.
- Delete a to-do item.

**API Endpoints:**

- `GET /api/todos`: Fetch all to-dos.
- `POST /api/todos`: Add a new to-do.
- `PUT /api/todos/:id`: Update a to-do.
- `DELETE /api/todos/:id`: Delete a to-do.

### Implementing the Application with TDD

#### 1. Creating the `ToDoList` Component

##### Step 1: Write the Test (Red)

Create `src/tests/ToDoList.test.jsx`:

```jsx
// src/tests/ToDoList.test.jsx
import { render, screen, waitFor } from '@testing-library/react';
import { rest } from 'msw';
import { setupServer } from 'msw/node';
import { ToDoList } from '../components/ToDoList';

const mockTodos = [
  { id: 1, title: 'Buy groceries', completed: false },
  { id: 2, title: 'Walk the dog', completed: true },
];

const server = setupServer(
  rest.get('/api/todos', (req, res, ctx) => {
    return res(ctx.json(mockTodos));
  })
);

beforeAll(() => server.listen());
afterEach(() => server.resetHandlers());
afterAll(() => server.close());

test('renders to-do items fetched from API', async () => {
  render(<ToDoList />);

  expect(screen.getByText(/loading/i)).toBeInTheDocument();

  const items = await screen.findAllByRole('listitem');
  expect(items).toHaveLength(2);
  expect(screen.getByText('Buy groceries')).toBeInTheDocument();
  expect(screen.getByText('Walk the dog')).toBeInTheDocument();
});
```

**Explanation:**

- We set up an MSW server to mock the API response.
- The test checks that the loading indicator appears.
- It waits for the to-do items to be rendered.

##### Step 2: Run the Test and See It Fail

Run the test:

```bash
npm run test
```

The test fails because `ToDoList` component doesn't exist.

##### Step 3: Write Minimal Code to Pass the Test (Green)

Create `src/components/ToDoList.jsx`:

```jsx
// src/components/ToDoList.jsx
import React, { useEffect, useState } from 'react';

export function ToDoList() {
  const [todos, setTodos] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch('/api/todos')
      .then((res) => res.json())
      .then((data) => {
        setTodos(data);
        setLoading(false);
      });
  }, []);

  if (loading) return <div>Loading…</div>;

  return (
    <ul>
      {todos.map((todo) => (
        <li key={todo.id}>
          {todo.title} {todo.completed ? '(Completed)' : ''}
        </li>
      ))}
    </ul>
  );
}
```

##### Step 4: Run the Test Again

Run the test:

```bash
npm run test
```

The test should pass.

##### Step 5: Refactor (if necessary)

No immediate refactoring needed.

#### 2. Fetching To-Dos From an API

We've already implemented fetching to-dos in the previous step.

#### 3. Adding a New To-Do

##### Step 1: Write the Test (Red)

Update `src/tests/ToDoList.test.jsx`:

```jsx
import userEvent from '@testing-library/user-event';

// … previous imports and setup …

test('adds a new to-do item', async () => {
  render(<ToDoList />);

  const input = screen.getByPlaceholderText('Add new to-do');
  const addButton = screen.getByRole('button', { name: /add/i });

  await userEvent.type(input, 'Learn TDD');
  await userEvent.click(addButton);

  // Mock the POST request
  server.use(
    rest.post('/api/todos', (req, res, ctx) => {
      return res(ctx.status(201), ctx.json({ id: 3, title: 'Learn TDD', completed: false }));
    })
  );

  // Mock the updated GET request
  server.use(
    rest.get('/api/todos', (req, res, ctx) => {
      return res(ctx.json([…mockTodos, { id: 3, title: 'Learn TDD', completed: false }]));
    })
  );

  const newItem = await screen.findByText('Learn TDD');
  expect(newItem).toBeInTheDocument();
});
```

**Explanation:**

- We simulate typing into the input and clicking the add button.
- We mock the POST request and the subsequent GET request to include the new item.

##### Step 2: Run the Test and See It Fail

Run the test:

```bash
npm run test
```

The test fails because the input and button don't exist.

##### Step 3: Write Minimal Code to Pass the Test (Green)

Update `src/components/ToDoList.jsx`:

```jsx
import React, { useEffect, useState } from 'react';

export function ToDoList() {
  const [todos, setTodos] = useState([]);
  const [loading, setLoading] = useState(true);
  const [newTodo, setNewTodo] = useState('');

  useEffect(() => {
    fetchTodos();
  }, []);

  function fetchTodos() {
    fetch('/api/todos')
      .then((res) => res.json())
      .then((data) => {
        setTodos(data);
        setLoading(false);
      });
  }

  function addTodo() {
    fetch('/api/todos', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ title: newTodo }),
    }).then(() => {
      setNewTodo('');
      fetchTodos();
    });
  }

  if (loading) return <div>Loading…</div>;

  return (
    <div>
      <input
        placeholder="Add new to-do"
        value={newTodo}
        onChange={(e) => setNewTodo(e.target.value)}
      />
      <button onClick={addTodo}>Add</button>

      <ul>
        {todos.map((todo) => (
          <li key={todo.id}>
            {todo.title} {todo.completed ? '(Completed)' : ''}
          </li>
        ))}
      </ul>
    </div>
  );
}
```

##### Step 4: Run the Test Again

Run the test:

```bash
npm run test
```

The test should pass.

##### Step 5: Refactor (if necessary)

Consider error handling and optimizing state updates.

#### 4. Marking a To-Do as Completed

##### Step 1: Write the Test (Red)

Update `src/tests/ToDoList.test.jsx`:

```jsx
test('marks a to-do item as completed', async () => {
  render(<ToDoList />);

  const itemCheckbox = await screen.findByRole('checkbox', { name: 'Buy groceries' });
  expect(itemCheckbox).not.toBeChecked();

  // Mock the PUT request
  server.use(
    rest.put('/api/todos/1', (req, res, ctx) => {
      return res(ctx.json({ id: 1, title: 'Buy groceries', completed: true }));
    })
  );

  await userEvent.click(itemCheckbox);

  expect(itemCheckbox).toBeChecked();
});
```

**Explanation:**

- We find the checkbox associated with the to-do item.
- We mock the PUT request to update the to-do item.
- We simulate clicking the checkbox and assert that it's checked.

##### Step 2: Run the Test and See It Fail

Run the test:

```bash
npm run test
```

The test fails because the checkbox doesn't exist.

##### Step 3: Write Minimal Code to Pass the Test (Green)

Update `src/components/ToDoList.jsx`:

```jsx
function toggleComplete(todo) {
  fetch(`/api/todos/${todo.id}`, {
    method: 'PUT',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ …todo, completed: !todo.completed }),
  }).then(() => {
    fetchTodos();
  });
}

// … in the return statement …

<ul>
  {todos.map((todo) => (
    <li key={todo.id}>
      <label>
        <input
          type="checkbox"
          checked={todo.completed}
          onChange={() => toggleComplete(todo)}
          aria-label={todo.title}
        />
        {todo.title}
      </label>
    </li>
  ))}
</ul>
```

##### Step 4: Run the Test Again

Run the test:

```bash
npm run test
```

The test should pass.

##### Step 5: Refactor (if necessary)

Optimize `fetchTodos` to avoid unnecessary network calls.

#### 5. Deleting a To-Do

##### Step 1: Write the Test (Red)

Update `src/tests/ToDoList.test.jsx`:

```jsx
test('deletes a to-do item', async () => {
  render(<ToDoList />);

  const deleteButton = await screen.findByRole('button', { name: 'Delete Buy groceries' });

  // Mock the DELETE request
  server.use(
    rest.delete('/api/todos/1', (req, res, ctx) => {
      return res(ctx.status(200));
    })
  );

  // Mock the updated GET request
  server.use(
    rest.get('/api/todos', (req, res, ctx) => {
      return res(ctx.json(mockTodos.filter((todo) => todo.id !== 1)));
    })
  );

  await userEvent.click(deleteButton);

  expect(screen.queryByText('Buy groceries')).not.toBeInTheDocument();
});
```

**Explanation:**

- We simulate clicking the delete button.
- We mock the DELETE request and update the GET response.
- We assert that the to-do item is no longer in the document.

##### Step 2: Run the Test and See It Fail

Run the test:

```bash
npm run test
```

The test fails because the delete button doesn't exist.

##### Step 3: Write Minimal Code to Pass the Test (Green)

Update `src/components/ToDoList.jsx`:

```jsx
function deleteTodo(id) {
  fetch(`/api/todos/${id}`, {
    method: 'DELETE',
  }).then(() => {
    fetchTodos();
  });
}

// … in the return statement …

<ul>
  {todos.map((todo) => (
    <li key={todo.id}>
      <label>
        <input
          type="checkbox"
          checked={todo.completed}
          onChange={() => toggleComplete(todo)}
          aria-label={todo.title}
        />
        {todo.title}
      </label>
      <button onClick={() => deleteTodo(todo.id)} aria-label={`Delete ${todo.title}`}>
        Delete
      </button>
    </li>
  ))}
</ul>
```

##### Step 4: Run the Test Again

Run the test:

```bash
npm run test
```

The test should pass.

##### Step 5: Refactor (if necessary)

Consider updating the state locally instead of re-fetching the entire list.

### Mocking the API with MSW

We've used **MSW** to mock API responses in our tests. Here's a brief explanation of how it works.

1. **Set Up MSW Handlers**

   Create `src/mocks/handlers.js`:

   ```javascript
   // src/mocks/handlers.js
   import { rest } from 'msw';

   const mockTodos = [
     { id: 1, title: 'Buy groceries', completed: false },
     { id: 2, title: 'Walk the dog', completed: true },
   ];

   export const handlers = [
     rest.get('/api/todos', (req, res, ctx) => {
       return res(ctx.json(mockTodos));
     }),

     rest.post('/api/todos', (req, res, ctx) => {
       const newTodo = { id: Date.now(), …req.body, completed: false };
       mockTodos.push(newTodo);
       return res(ctx.status(201), ctx.json(newTodo));
     }),

     rest.put('/api/todos/:id', (req, res, ctx) => {
       const { id } = req.params;
       const index = mockTodos.findIndex((todo) => todo.id === parseInt(id));
       mockTodos[index] = { …mockTodos[index], …req.body };
       return res(ctx.json(mockTodos[index]));
     }),

     rest.delete('/api/todos/:id', (req, res, ctx) => {
       const { id } = req.params;
       const index = mockTodos.findIndex((todo) => todo.id === parseInt(id));
       mockTodos.splice(index, 1);
       return res(ctx.status(200));
     }),
   ];
   ```

2. **Set Up the MSW Server**

   Create `src/mocks/server.js`:

   ```javascript
   // src/mocks/server.js
   import { setupServer } from 'msw/node';
   import { handlers } from './handlers';

   export const server = setupServer(…handlers);
   ```

3. **Update the Test Setup File**

   Update `tests/setupTests.js`:

   ```javascript
   // tests/setupTests.js
   import '@testing-library/jest-dom';
   import { server } from '../src/mocks/server';

   beforeAll(() => server.listen({ onUnhandledRequest: 'error' }));
   afterEach(() => server.resetHandlers());
   afterAll(() => server.close());
   ```

4. **Use the Handlers in Tests**

   In your tests, you can now use the handlers or override them as needed.

**Benefits of Using MSW:**

- Mocks API requests at the network level.
- Works both in tests and during development.
- Provides realistic API interactions.

### Running the Tests

Run all tests using:

```bash
npm run test
```

Vitest will execute all tests in the `tests/` directory and report the results.

### Conclusion

By following Test-Driven Development principles, we've built a functional To-Do List application in React. We've written unit and integration tests using Vitest and React Testing Library, ensuring our components behave as expected. Using MSW, we've effectively mocked API interactions, allowing us to test components that rely on external data without actual network requests.

**Key Takeaways:**

- **TDD Workflow:** Writing tests first helps define expected behavior and leads to better-designed code.
- **Vitest and React Testing Library:** Provide powerful tools for testing React components.
- **MSW for API Mocking:** Allows for realistic and maintainable API mocking in tests.

### Additional Exercises

To further enhance your To-Do List application and testing skills, consider implementing the following features:

1. **Edit To-Do Items**

   - Allow users to edit the title of a to-do item.
   - Write tests to ensure the edit functionality works correctly.

2. **Error Handling**

   - Implement error handling for API failures.
   - Write tests to simulate API errors and verify that the application responds appropriately.

3. **Optimistic Updates**

   - Update the UI immediately after user actions before the server confirms.
   - Write tests to ensure the UI remains consistent with the server state.

4. **Pagination**

   - Implement pagination or infinite scrolling for the to-do list.
   - Write tests to ensure items are loaded correctly as the user navigates.

5. **Filter and Search**

   - Add functionality to filter to-dos by status (completed, active).
   - Implement a search feature to find to-dos by title.
   - Write tests to verify filtering and searching.

6. **User Authentication**

   - Implement user authentication to manage to-dos for different users.
   - Mock authentication in tests and verify access control.

7. **Accessibility Improvements**

   - Ensure the application meets accessibility standards (ARIA roles, keyboard navigation).
   - Write tests to verify accessibility features.

8. **Deployment**

   - Deploy the application to a hosting service (e.g., Netlify, Vercel).
   - Ensure the application works in production and consider setting up end-to-end tests.

---

By extending the application and writing tests for new features, you'll deepen your understanding of TDD and improve your testing proficiency in a React environment.

Happy coding and testing!
