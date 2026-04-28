# project-02
Task 2


HTML Structure
<div class="container">

Main container that holds the entire To-Do app.

<div class="header">My Tasks</div>

Displays the title of the app.

<input type="text" id="taskInput">

Input field where the user types a new task.

<button onclick="addTask()">Add</button>

Button that calls the addTask() function when clicked.

<ul id="TaskList"></ul>

Empty list where tasks will be dynamically added.

<footer>WebNova Studio | Designed by Anmol</footer>

Footer section showing credits.

 Function 1: Add Task
function addTask()

This function runs when the Add button is clicked.

let input = document.getElementById("taskInput");

Gets the input field.

let taskText = input.value.trim();

Gets the user input and removes extra spaces.

if (taskText == "") {
    alert("Enter The Task");
    return;
}

Checks if the input is empty.
If empty → shows alert and stops execution.

let li = document.createElement("li");

Creates a new list item (<li>).

li.innerHTML = `
<span>${taskText}</span>
<button class="delete" onclick="deleteTask(this)">X</button>
`;

Adds:

Task text
Delete button

this refers to the clicked delete button.

document.getElementById("TaskList").appendChild(li);

Adds the new task to the list.

input.value = "";

Clears the input field after adding the task.

 Function 2: Delete Task
function deleteTask(btn)

Runs when the delete button is clicked.

let li = btn.parentElement;

Gets the <li> element (task item) that contains the button.

li.style.opacity = "0";
li.style.transform = "translateX(100px)";

Applies animation:

Fades out (opacity)
Moves right (translateX)
setTimeout(() => {
    li.remove();
}, 300);

Waits 300 milliseconds (animation time), then removes the task from the list.

 How the App Works
User types a task
Clicks Add
Task appears in the list
Click X to delete
Task fades out and disappears
