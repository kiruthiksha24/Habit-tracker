<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Habit Tracker - Spreadsheet</title>

    <!-- CSS -->
    <style>
        body {
            font-family: Arial, sans-serif;
            background: #eef2f7;
            padding: 30px;
        }

        h2 {
            text-align: center;
            margin-bottom: 20px;
        }

        .container {
            max-width: 700px;
            margin: auto;
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 6px 15px rgba(0,0,0,0.1);
        }

        .input-row {
            display: flex;
            gap: 10px;
            margin-bottom: 15px;
        }

        input {
            flex: 1;
            padding: 10px;
            border-radius: 6px;
            border: 1px solid #ccc;
        }

        button {
            padding: 10px 15px;
            border: none;
            background: #4CAF50;
            color: white;
            border-radius: 6px;
            cursor: pointer;
        }

        button:hover {
            background: #43a047;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 10px;
        }

        th, td {
            border: 1px solid #ccc;
            padding: 10px;
            text-align: center;
        }

        th {
            background: #f1f5f9;
        }

        .done {
            color: green;
            font-weight: bold;
        }

        .not-done {
            color: red;
            font-weight: bold;
        }

        .delete-btn {
            background: #e53935;
            padding: 6px 10px;
            border-radius: 4px;
        }

        .delete-btn:hover {
            background: #c62828;
        }
    </style>
</head>

<body>

<div class="container">
    <h2>Habit Tracker (Spreadsheet View)</h2>

    <div class="input-row">
        <input type="text" id="habitInput" placeholder="Enter habit name">
        <button onclick="addHabit()">Add Habit</button>
    </div>

    <table>
        <thead>
            <tr>
                <th>S.No</th>
                <th>Habit Name</th>
                <th>Status</th>
                <th>Mark</th>
                <th>Delete</th>
            </tr>
        </thead>
        <tbody id="habitTable">
        </tbody>
    </table>
</div>

<!-- JavaScript -->
<script>
    let habits = JSON.parse(localStorage.getItem("habits")) || [];

    function addHabit() {
        let input = document.getElementById("habitInput");
        let habitName = input.value.trim();

        if (habitName === "") {
            alert("Please enter a habit");
            return;
        }

        habits.push({ name: habitName, done: false });
        input.value = "";
        saveAndRender();
    }

    function toggleStatus(index) {
        habits[index].done = !habits[index].done;
        saveAndRender();
    }

    function deleteHabit(index) {
        habits.splice(index, 1);
        saveAndRender();
    }

    function saveAndRender() {
        localStorage.setItem("habits", JSON.stringify(habits));
        renderTable();
    }

    function renderTable() {
        let table = document.getElementById("habitTable");
        table.innerHTML = "";

        habits.forEach((habit, index) => {
            let row = document.createElement("tr");

            row.innerHTML = `
                <td>${index + 1}</td>
                <td>${habit.name}</td>
                <td class="${habit.done ? 'done' : 'not-done'}">
                    ${habit.done ? 'Done' : 'Not Done'}
                </td>
                <td>
                    <button onclick="toggleStatus(${index})">
                        ${habit.done ? 'Undo' : 'Done'}
                    </button>
                </td>
                <td>
                    <button class="delete-btn" onclick="deleteHabit(${index})">
                        Delete
                    </button>
                </td>
            `;

            table.appendChild(row);
        });
    }

    renderTable();
</script>

</body>
</html>
