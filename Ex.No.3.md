# Ex03 To-Do List using JavaScript
## Date:24.10.20205

## AIM
To create a To-do Application with all features using JavaScript.

## ALGORITHM
### STEP 1
Build the HTML structure (index.html).

### STEP 2
Style the App (style.css).

### STEP 3
Plan the features the To-Do App should have.

### STEP 4
Create a To-do application using Javascript.

### STEP 5
Add functionalities.

### STEP 6
Test the App.

### STEP 7
Open the HTML file in a browser to check layout and functionality.

### STEP 8
Fix styling issues and refine content placement.

### STEP 9
Deploy the website.

### STEP 10
Upload to GitHub Pages for free hosting.

## PROGRAM
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>To-Do List</title>
    <link rel="stylesheet" href="style.css">

</head>
<body>
    <div class="container">
        <h2>To-Do List</h2>
        <div class="input-container">
            <input type="text" id="taskInput" placeholder="Add a task...">
            <button class="add-btn" onclick="addTask()">Add</button>
        </div>
        <ul id="taskList"></ul>
    </div>
    <div class="footer">&copy; 2025 STARBIYA S 212223040208. All Rights Reserved.</div>
    <script src="script.js"></script>
</body>
</html>
```
style.css
```
body {
    font-family: 'Poppins', sans-serif;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background: linear-gradient(to right, #74ebd5, #acb6e5);
    color: #333;
}
.container {
    background: white;
    padding: 20px;
    border-radius: 15px;
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
    width: 350px;
    text-align: center;
}
h2 {
    font-size: 22px;
    color: #333;
    font-weight: 600;
}
.input-container {
    display: flex;
    gap: 8px;
}
input {
    flex: 1;
    padding: 8px;
    border: 2px solid #74ebd5;
    border-radius: 5px;
    font-size: 14px;
}
.add-btn {
    padding: 8px 12px;
    background-color: #4caf50;
    color: white;
    border: none;
    cursor: pointer;
    border-radius: 5px;
    transition: 0.3s;
}
.add-btn:hover {
    background-color: #45a049;
}
.remove-btn {
    padding: 6px;
    background-color: #e74c3c;
    color: white;
    border: none;
    cursor: pointer;
    border-radius: 5px;
    transition: 0.3s;
}
.remove-btn:hover {
    background-color: #c0392b;
}
ul {
    list-style: none;
    padding: 0;
    margin-top: 10px;
}
li {
    padding: 10px;
    background: #f1f1f1;
    margin-top: 8px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    border-radius: 5px;
    font-size: 16px;
    transition: 0.3s;
}
li:hover {
    background: #e0e0e0;
}
.completed {
    text-decoration: line-through;
    color: grey;
}
.footer {
    margin-top: 20px;
    text-align: center;
    font-size: 14px;
    color: #fff;
}
```
script.js
```
let taskCount = 0;

        function addTask() {
            let taskInput = document.getElementById("taskInput");
            let taskText = taskInput.value.trim();
            if (taskText === "") return;

            taskCount++;
            let li = document.createElement("li");
            let span = document.createElement("span");
            span.textContent = `${taskCount}. ${taskText}`;
            let button = document.createElement("button");
            button.textContent = "X";
            button.classList.add("remove-btn");
            button.onclick = function() { removeTask(this); };
            
            li.appendChild(span);
            li.appendChild(button);
            li.addEventListener("click", function(event) {
                if (event.target !== button) {
                    this.classList.toggle("completed");
                }
            });

            document.getElementById("taskList").appendChild(li);
            taskInput.value = "";
        }

        function removeTask(btn) {
            btn.parentElement.remove();
            updateTaskNumbers();
        }

        function updateTaskNumbers() {
            let tasks = document.querySelectorAll("#taskList li span");
            taskCount = 0;
            tasks.forEach((task) => {
                taskCount++;
                task.textContent = `${taskCount}. ${task.textContent.replace(/^\d+\. /, '')}`;
            });
        }
```
## OUTPUT
<img width="1846" height="968" alt="500234874-9af22a5c-bb54-406c-93d7-96753091d6de" src="https://github.com/user-attachments/assets/8ff6b1ff-1b02-45f7-8f5c-5639a5392fa6" />



## RESULT
The program for creating To-do list using JavaScript is executed successfully.
