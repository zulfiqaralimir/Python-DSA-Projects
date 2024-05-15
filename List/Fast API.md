Yes, you can definitely use FastAPI for creating a web API for your to-do list manager project. FastAPI is a modern, fast (high-performance), web framework for building APIs with Python 3.7+ based on standard Python type hints.

Here's how you can adapt your to-do list manager to work with FastAPI:

First, ensure you have FastAPI and Uvicorn (ASGI server) installed. You can install them via pip:

```bash
pip install fastapi uvicorn
```

Now, create a Python file, let's call it `todo_list_api.py`, and put your `ToDoList` class inside it. Then, use FastAPI to create a web API for your to-do list manager:

```python
from fastapi import FastAPI
from typing import List

# Define ToDoList class
class ToDoList:
    def __init__(self):
        self.tasks = []

    def add_task(self, task):
        self.tasks.append(task)

    def remove_task(self, task):
        if task in self.tasks:
            self.tasks.remove(task)
        else:
            print("Task not found!")

    def get_tasks(self):
        return self.tasks

# Create an instance of FastAPI
app = FastAPI()

# Create an instance of ToDoList
todo_list = ToDoList()

# Define routes
@app.post("/tasks/")
def add_task(task: str):
    todo_list.add_task(task)
    return {"message": "Task added successfully"}

@app.delete("/tasks/")
def remove_task(task: str):
    todo_list.remove_task(task)
    return {"message": "Task removed successfully"}

@app.get("/tasks/")
def get_tasks():
    return {"tasks": todo_list.get_tasks()}
```

Save this code in `todo_list_api.py`. Now, to run your FastAPI server, open your terminal, navigate to the directory containing `todo_list_api.py`, and run:

```bash
uvicorn todo_list_api:app --reload
```

This will start the FastAPI server with automatic reloading enabled. You can now interact with your to-do list manager through the defined API endpoints (`/tasks/`).

For example, you can use tools like `curl` or Postman to make HTTP requests to add tasks, remove tasks, and get the list of tasks. Alternatively, you can create a frontend interface to interact with the API using JavaScript frameworks like React or Vue.js.
