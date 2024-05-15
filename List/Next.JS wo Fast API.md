If you're not using FastAPI and only using Next.js for both frontend and backend, you can still achieve the same functionality. Next.js supports server-side rendering and API routes, so you can create your API endpoints directly within your Next.js app.

Here's how you can modify the previous example to use Next.js for both frontend and backend:

1. **Create Next.js API routes**: Next.js allows you to create API routes by creating files inside the `pages/api` directory. These files export functions that handle API requests.

2. **Handle API requests**: In these API route files, you can handle requests like adding tasks, removing tasks, or fetching the list of tasks.

3. **Communicate between frontend and backend**: Your Next.js frontend can make requests to these API routes to perform CRUD (Create, Read, Update, Delete) operations on tasks.

Here's a simplified example of how you can implement this:

```javascript
// pages/api/tasks.js
let tasks = [];

export default function handler(req, res) {
  if (req.method === 'GET') {
    res.status(200).json({ tasks });
  } else if (req.method === 'POST') {
    const { task } = req.body;
    tasks.push(task);
    res.status(201).json({ message: 'Task added successfully' });
  } else if (req.method === 'DELETE') {
    const { task } = req.body;
    const index = tasks.indexOf(task);
    if (index !== -1) {
      tasks.splice(index, 1);
      res.status(200).json({ message: 'Task removed successfully' });
    } else {
      res.status(404).json({ message: 'Task not found' });
    }
  } else {
    res.setHeader('Allow', ['GET', 'POST', 'DELETE']);
    res.status(405).end(`Method ${req.method} Not Allowed`);
  }
}
```

In your Next.js frontend components, you can use `fetch` or Axios to make requests to these API routes:

```javascript
import { useState, useEffect } from 'react';

export default function Home() {
  const [tasks, setTasks] = useState([]);
  const [newTask, setNewTask] = useState('');

  useEffect(() => {
    fetch('/api/tasks')
      .then((res) => res.json())
      .then((data) => setTasks(data.tasks))
      .catch((error) => console.error('Error fetching tasks:', error));
  }, []);

  const addTask = () => {
    fetch('/api/tasks', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ task: newTask }),
    })
      .then((res) => {
        if (res.ok) {
          setTasks([...tasks, newTask]);
          setNewTask('');
        } else {
          throw new Error('Failed to add task');
        }
      })
      .catch((error) => console.error('Error adding task:', error));
  };

  const removeTask = (task) => {
    fetch('/api/tasks', {
      method: 'DELETE',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ task }),
    })
      .then((res) => {
        if (res.ok) {
          setTasks(tasks.filter((t) => t !== task));
        } else {
          throw new Error('Failed to remove task');
        }
      })
      .catch((error) => console.error('Error removing task:', error));
  };

  return (
    <div>
      <h1>To-Do List</h1>
      <input
        type="text"
        value={newTask}
        onChange={(e) => setNewTask(e.target.value)}
      />
      <button onClick={addTask}>Add Task</button>
      <ul>
        {tasks.map((task, index) => (
          <li key={index}>
            {task}
            <button onClick={() => removeTask(task)}>Remove</button>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

This setup eliminates the need for a separate backend server like FastAPI. Instead, you handle API requests directly within your Next.js app using API routes. This can simplify the setup, especially for smaller projects, and reduce the complexity of managing multiple servers.
