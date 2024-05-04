#Create a file named server.js:
const express = require('express');
const app = express();
const PORT = process.env.PORT || 5000;

app.use(express.json());

let tasks = [];

// Routes
app.get('/api/tasks', (req, res) => {
  res.json(tasks);
});

app.post('/api/tasks', (req, res) => {
  const { title } = req.body;
  const newTask = { id: tasks.length + 1, title };
  tasks.push(newTask);
  res.status(201).json(newTask);
});

app.put('/api/tasks/:id', (req, res) => {
  const { id } = req.params;
  const { title } = req.body;
  const taskToUpdate = tasks.find(task => task.id === parseInt(id));
  if (!taskToUpdate) return res.status(404).json({ message: 'Task not found' });
  taskToUpdate.title = title;
  res.json(taskToUpdate);
});

app.delete('/api/tasks/:id', (req, res) => {
  const { id } = req.params;
  tasks = tasks.filter(task => task.id !== parseInt(id));
  res.json({ message: 'Task deleted' });
});

app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});







