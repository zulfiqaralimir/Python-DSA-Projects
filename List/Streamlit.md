To display your simple to-do list manager project on Streamlit, you can create a Streamlit web application. Streamlit is a great tool for creating interactive web applications with Python. Here's how you can adapt your code for Streamlit:

First, ensure you have Streamlit installed. You can install it via pip:

```bash
pip install streamlit
```

Now, create a Python file, let's call it `todo_list_app.py`, and put your `ToDoList` class and example usage inside a function. Then, use Streamlit to create a web interface for your to-do list manager:

```python
import streamlit as st

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

    def display_tasks(self):
        if self.tasks:
            st.write("Tasks:")
            for idx, task in enumerate(self.tasks, start=1):
                st.write(f"{idx}. {task}")
        else:
            st.write("No tasks!")

def main():
    # Create an instance of ToDoList
    todo_list = ToDoList()

    st.title("Simple To-Do List Manager")

    # Add task section
    st.header("Add Task")
    new_task = st.text_input("Enter a new task:")
    if st.button("Add Task"):
        todo_list.add_task(new_task)
        st.success("Task added successfully!")

    # Remove task section
    st.header("Remove Task")
    task_to_remove = st.selectbox("Select task to remove:", todo_list.tasks, 0)
    if st.button("Remove Task"):
        todo_list.remove_task(task_to_remove)
        st.success("Task removed successfully!")

    # Display tasks
    st.header("Tasks")
    todo_list.display_tasks()

if __name__ == "__main__":
    main()
```

Save this code in `todo_list_app.py`. Now, to run your Streamlit app, open your terminal, navigate to the directory containing `todo_list_app.py`, and run:

```bash
streamlit run todo_list_app.py
```

This will start a local web server and open your browser with the Streamlit app displaying your to-do list manager. You can add tasks, remove tasks, and see the updated list in real-time.
