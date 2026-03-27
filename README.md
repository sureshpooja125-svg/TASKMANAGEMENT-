<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>FocusFlow - Task Manager</title>

  <!-- Google Font -->
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">

  <!-- Tailwind -->
  <script src="https://cdn.tailwindcss.com"></script>

  <style>
    body { font-family: 'Poppins', sans-serif; }
  </style>
</head>

<body class="bg-gradient-to-br from-indigo-100 to-blue-200 min-h-screen flex items-center justify-center">

<div class="bg-white w-full max-w-md p-6 rounded-2xl shadow-xl">

  <!-- Header -->
  <div class="flex justify-between items-center mb-4">
    <h1 class="text-2xl font-semibold text-gray-800">FocusFlow</h1>
    <span class="text-sm text-gray-400">Task Manager</span>
  </div>

  <!-- Input -->
  <div class="flex gap-2 mb-4">
    <input id="taskInput" type="text" placeholder="Enter a task..."
      class="flex-1 p-2 border rounded-lg outline-none focus:ring-2 focus:ring-indigo-400">
    <button onclick="addTask()"
      class="bg-indigo-500 hover:bg-indigo-600 text-white px-4 rounded-lg">
      +
    </button>
  </div>

  <!-- Filters -->
  <div class="flex justify-between mb-4 text-sm">
    <button onclick="filterTasks('all')" class="text-indigo-600">All</button>
    <button onclick="filterTasks('active')">Active</button>
    <button onclick="filterTasks('completed')">Completed</button>
  </div>

  <!-- Task List -->
  <ul id="taskList" class="space-y-2"></ul>

</div>

<script>
let tasks = JSON.parse(localStorage.getItem("tasks")) || [];
let currentFilter = "all";

function saveTasks() {
  localStorage.setItem("tasks", JSON.stringify(tasks));
}

function renderTasks() {
  const list = document.getElementById("taskList");
  list.innerHTML = "";

  let filtered = tasks.filter(task => {
    if(currentFilter === "active") return !task.completed;
    if(currentFilter === "completed") return task.completed;
    return true;
  });

  filtered.forEach((task, index) => {
    let li = document.createElement("li");
    li.className = "flex justify-between items-center bg-gray-100 p-3 rounded-lg";

    li.innerHTML = `
      <span onclick="toggleTask(${index})"
        class="cursor-pointer ${task.completed ? 'line-through text-gray-400' : ''}">
        ${task.text}
      </span>

      <div class="flex gap-2">
        <button onclick="editTask(${index})" class="text-blue-500">✏️</button>
        <button onclick="deleteTask(${index})" class="text-red-500">🗑</button>
      </div>
    `;

    list.appendChild(li);
  });
}

function addTask() {
  let input = document.getElementById("taskInput");

  if(input.value.trim() === "") return;

  tasks.push({ text: input.value, completed: false });
  input.value = "";
  saveTasks();
  renderTasks();
}

function deleteTask(index) {
  tasks.splice(index, 1);
  saveTasks();
  renderTasks();
}

function toggleTask(index) {
  tasks[index].completed = !tasks[index].completed;
  saveTasks();
  renderTasks();
}

function editTask(index) {
  let newTask = prompt("Edit task:", tasks[index].text);
  if(newTask !== null) {
    tasks[index].text = newTask;
    saveTasks();
    renderTasks();
  }
}

function filterTasks(type) {
  currentFilter = type;
  renderTasks();
}

renderTasks();
</script>

</body>
</html>
