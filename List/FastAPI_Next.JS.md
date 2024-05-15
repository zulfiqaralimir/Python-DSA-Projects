Sure, you can create a frontend interface using Next.js to interact with the FastAPI backend. Here's a basic example of how you can set it up:

First, ensure you have Node.js and npm (Node Package Manager) installed on your machine.

1. Create a new Next.js app:

```bash
npx create-next-app todo-list-frontend
```

2. Navigate into the created directory:

```bash
cd todo-list-frontend
```

3. Install Axios for making HTTP requests:

```bash
npm install axios
```

4. Create a new component for your to-do list:

Inside the `pages` directory, create a new file named `index.js`:

```jsx
// pages/index.js
import { useState } from 'react';
import axios from 'axios';

export default function Home() {
  const [tasks, setTasks] = useState([]);
  const [newTask, setNewTask] = useState('');

  const fetchTasks = async () => {
    try {
      const response = await axios.get('http://localhost:8000/tasks/');
      setTasks(response.data.tasks);
    } catch (error) {
      console.error('Error fetching tasks:', error);
    }
  };

  const addTask = async () => {
    try {
      await axios.post('http://localhost:8000/tasks/', { task: newTask });
      fetchTasks();
      setNewTask('');
    } catch (error) {
      console.error('Error adding task:', error);
    }
  };

  const removeTask = async (task) => {
    try {
      await axios.delete('http://localhost:8000/tasks/', { data: { task } });
      fetchTasks();
    } catch (error) {
      console.error('Error removing task:', error);
    }
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

5. Run the Next.js app:

```bash
npm run dev
```

This will start the Next.js development server. Now, you should have a basic frontend interface where you can add tasks, remove tasks, and see the list of tasks. The frontend communicates with the FastAPI backend to perform these actions. Make sure your FastAPI server is running on `http://localhost:8000`. Adjust the URLs accordingly if your backend is running on a different port or host.
