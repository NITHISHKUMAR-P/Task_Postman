# Task_Postman
## Task Controller.js:
```js
let tasks = [];
let currentId = 1;

// Create Task
const createTask = (req, res) => {
    const { name, completed = false } = req.body;
    const newTask = { id: currentId++, name, completed };
    tasks.push(newTask);
    res.status(201).json(newTask);
};

// Get All Tasks
const getAllTasks = (req, res) => {
    res.json(tasks);
};

// Get Task by ID
const getTaskById = (req, res) => {
    const task = tasks.find(t => t.id === parseInt(req.params.id));
    if (task) {
        res.json(task);
    } else {
        res.status(404).json({ message: 'Task not found' });
    }
};

// Update Task
const updateTask = (req, res) => {
    const task = tasks.find(t => t.id === parseInt(req.params.id));
    if (task) {
        const { name, completed } = req.body;
        task.name = name !== undefined ? name : task.name;
        task.completed = completed !== undefined ? completed : task.completed;
        res.json(task);
    } else {
        res.status(404).json({ message: 'Task not found' });
    }
};

// Delete Task
const deleteTask = (req, res) => {
    const taskIndex = tasks.findIndex(t => t.id === parseInt(req.params.id));
    if (taskIndex > -1) {
        tasks.splice(taskIndex, 1);
        res.json({ message: 'Task deleted successfully' });
    } else {
        res.status(404).json({ message: 'Task not found' });
    }
};

module.exports = {
    createTask,
    getAllTasks,
    getTaskById,
    updateTask,
    deleteTask
};
```

## Task Router:
```js
const express = require('express');
const router = express.Router();
const taskController = require('../controller/Taskcontroller');

router.post('/', taskController.createTask);
router.get('/', taskController.getAllTasks);
router.get('/:id', taskController.getTaskById);
router.put('/:id', taskController.updateTask);
router.delete('/:id', taskController.deleteTask);

module.exports = router;
```

### Task Server
```js
const express = require('express');
const bodyParser = require('body-parser');
const taskRoutes = require('./routes/Taskroutes');

const app = express();
const PORT = 3000;

// Middleware
app.use(bodyParser.json());

// Routes
app.use('/api/tasks', taskRoutes);

// Handle root URL
app.get('/', (req, res) => {
    res.send('Welcome to the Task API');
});

app.listen(PORT, () => {
    console.log('Server is running on http://localhost:3000');
});
```
## Output:
### GET
![get](https://github.com/NITHISHKUMAR-P/Task_Postman/assets/93427017/7a132012-354b-458b-a3a9-97b0a4a16daa)

### POST
![postn](https://github.com/NITHISHKUMAR-P/Task_Postman/assets/93427017/9af765ba-4a72-44cb-bdbd-97832306fa67)

### UPDATE
![image](https://github.com/NITHISHKUMAR-P/Task_Postman/assets/93427017/166bf9a6-fdbb-47c5-b87c-e5b360c75fce)


### DELETE
![updt](https://github.com/NITHISHKUMAR-P/Task_Postman/assets/93427017/aaf43ec1-4a98-4805-9d5f-6fcb612c6dd0)


