
// src/App.js (simplified React component)
import React, { useState, useEffect } from 'react';

function App() {
  const [data, setData] = useState([]);

  useEffect(() => {
    fetch('/api/items') // Fetch data from backend API
      .then(response => response.json())
      .then(data => setData(data))
      .catch(error => console.error('Error fetching data:', error));
  }, []);

  return (
    <div className="App">
      <h1>MERN Stack Application</h1>
      <ul>
        {data.map(item => (
          <li key={item._id}>{item.name}</li>
        ))}
      </ul>
    </div>
  );
}

export default App;

// server.js (simplified Express.js server)
const express = require('express');
const mongoose = require('mongoose');
const app = express();
const PORT = process.env.PORT || 5000;

app.use(express.json()); // Enable JSON body parsing

// Connect to MongoDB
mongoose.connect('mongodb://localhost:27017/mern_app_db')
  .then(() => console.log('MongoDB connected'))
  .catch(err => console.error('MongoDB connection error:', err));

// Define a simple Mongoose schema and model
const ItemSchema = new mongoose.Schema({
  name: String,
});
const Item = mongoose.model('Item', ItemSchema);

// API endpoint to get items
app.get('/api/items', async (req, res) => {
  try {
    const items = await Item.find();
    res.json(items);
  } catch (err) {
    res.status(500).json({ message: err.message });
  }
});

app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
