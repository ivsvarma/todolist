# Todo List Application

## Description

This is a simple Todo List web application built using Flask. The application allows users to add, edit, and delete tasks. Users can also mark tasks as done or not done.

## Features

- Add new tasks
- Edit existing tasks
- Delete tasks
- Mark tasks as done or not done
- Simple and clean user interface

## Technologies Used

- **Flask**: Web framework for Python
- **HTML/CSS**: For the front-end
- **Jinja2**: Templating engine for Flask

## Installation

1. **Clone the Repository**:
    ```sh
    git clone https://github.com/your-username/todo-list-flask.git
    cd todo-list-flask
    ```

2. **Create and Activate a Virtual Environment**:
    ```sh
    python -m venv venv
    source venv/bin/activate   # On Windows, use `venv\Scripts\activate`
    ```

3. **Install Dependencies**:
    ```sh
    pip install Flask
    ```

4. **Run the Application**:
    ```sh
    python app.py
    ```

5. **Open Your Browser**:
    - Navigate to `http://127.0.0.1:5000/` to view the application.

## Usage

- **Add a New Task**:
    - Enter the task in the input field and click the "Add" button.
- **Edit a Task**:
    - Click the "edit" link next to the task you want to edit.
    - Update the task in the input field and click "Submit".
- **Delete a Task**:
    - Click the "delete" link next to the task you want to remove.
- **Mark a Task as Done/Not Done**:
    - Click the checkbox next to the task to mark it as done or not done.

## Code Structure

- `app.py`: Main application file containing the Flask routes and logic.
- `templates/`: Folder containing HTML templates.
  - `index.html`: Template for the main page displaying the todo list.
  - `edit.html`: Template for editing a task.
- `static/`: Folder for static files (e.g., CSS).

## Example Code

### `app.py`

```python
from flask import Flask, render_template, request, redirect, url_for

app = Flask(__name__, template_folder='templates')

todos = []

@app.route('/')
def index():
    return render_template('index.html', todos=todos)

@app.route('/add', methods=['POST'])
def add():
    todo = request.form['todo']
    todos.append({'task': todo, 'done': False})
    return redirect(url_for('index'))

@app.route('/edit/<int:index>', methods=['GET', 'POST'])
def edit(index):
    todo = todos[index]
    if request.method == 'POST':
        todo['task'] = request.form['todo']
        return redirect(url_for('index'))
    else:
        return render_template('edit.html', todo=todo, index=index)

@app.route('/check/<int:index>')
def check(index):
    todos[index]['done'] = not todos[index]['done']
    return redirect(url_for('index'))

@app.route('/delete/<int:index>')
def delete(index):
    del todos[index]
    return redirect(url_for('index'))

if __name__ == '__main__':
    app.run(debug=True)
