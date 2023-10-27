# Todolist
<!DOCTYPE html>
<html>
<head>
    <title>Simple To-Do List</title>
    <style>
        body {
            font-family: 'Times New Roman', Times, serif, sans-serif;
        }

        #container {
            max-width: 500px;
            margin: 0 auto;
        }

        #taskList {
            list-style: none;
            padding: 0;
        }

        #taskList li {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }

        #taskList button {
            background-color: #077b17b6;
            color: #130c01;
            border: none;
            padding: 5px 10px;
            cursor: pointer;
        }

        #taskList input[type="text"] {
            width: 70%;
            padding: 5px;
        }
    </style>
</head>
<body>
    <div id="container">
        <h1>Simple To-Do List</h1>
        <input type="text" id="taskInput" placeholder="Add a new task">
        <button id="addTask">Add Task</button>
        <ul id="taskList"></ul>
    </div>

    <script>
        document.addEventListener("DOMContentLoaded", function() {
            const taskInput = document.getElementById("taskInput");
            const addTaskButton = document.getElementById("addTask");
            const taskList = document.getElementById("taskList");

            // Load tasks from local storage
            const tasks = JSON.parse(localStorage.getItem("tasks")) || [];

            // Function to render tasks
            function renderTasks() {
                taskList.innerHTML = "";
                tasks.forEach((task, index) => {
                    const listItem = document.createElement("li");
                    listItem.innerHTML = `
                        <input type="text" value="${task}" readonly>
                        <button onclick="editTask(${index})">Edit</button>
                        <button onclick="deleteTask(${index})">Delete</button>
                    `;
                    taskList.appendChild(listItem);
                });
            }

            renderTasks();

            // Function to add a task
            addTaskButton.addEventListener("click", function() {
                const newTask = taskInput.value.trim();
                if (newTask !== "") {
                    tasks.push(newTask);
                    localStorage.setItem("tasks", JSON.stringify(tasks));
                    taskInput.value = "";
                    renderTasks();
                }
            });

            // Function to edit a task
            function editTask(index) {
                const listItem = taskList.children[index];
                const inputField = listItem.querySelector("input");
                inputField.removeAttribute("readonly");
                inputField.addEventListener("blur", function() {
                    tasks[index] = inputField.value;
                    inputField.setAttribute("readonly", true);
                    localStorage.setItem("tasks", JSON.stringify(tasks));
                });
            }

            window.editTask = editTask;

            // Function to delete a task
            function deleteTask(index) {
                tasks.splice(index, 1);
                localStorage.setItem("tasks", JSON.stringify(tasks));
                renderTasks();
            }

            window.deleteTask = deleteTask;
        });
    </script>
</body>
</html>
