# Flexmoney
Assignment
 Frontend (React):

jsx
// App.js
import React, { useState } from 'react';
import axios from 'axios';

const App = () => {
  const [formData, setFormData] = useState({
    name: '',
    age: '',
    batch: '',
  });

  const handleSubmit = async (e) => {
    e.preventDefault();
    try {
      // Make API call to backend
      const response = await axios.post('http://localhost:3001/api/enroll', formData);
      console.log(response.data); // Log the response from the backend
    } catch (error) {
      console.error('Error submitting form:', error);
    }
  };

  return (
    <div>
      <h1>Yoga Class Admission Form</h1>
      <form onSubmit={handleSubmit}>
        <label>Name:</label>
        <input
          type="text"
          value={formData.name}
          onChange={(e) => setFormData({ ...formData, name: e.target.value })}
        />
        <br />

        <label>Age:</label>
        <input
          type="number"
          value={formData.age}
          onChange={(e) => setFormData({ ...formData, age: e.target.value })}
        />
        <br />

        <label>Preferred Batch:</label>
        <select
          value={formData.batch}
          onChange={(e) => setFormData({ ...formData, batch: e.target.value })}
        >
          <option value="">Select Batch</option>
          <option value="6-7AM">6-7AM</option>
          <option value="7-8AM">7-8AM</option>
          <option value="8-9AM">8-9AM</option>
          <option value="5-6PM">5-6PM</option>
        </select>
        <br />

        <button type="submit">Submit</button>
      </form>
    </div>
  );
};

export default App;


### Backend (Node.js with Express):

javascript
// server.js
const express = require('express');
const bodyParser = require('body-parser');
const app = express();
const port = 3001;

// Middleware
app.use(bodyParser.json());

// Mock database (in-memory for simplicity)
const users = [];

// API endpoint for enrollment
app.post('/api/enroll', (req, res) => {
  const { name, age, batch } = req.body;

  // Basic validation
  if (!name || !age || !batch) {
    return res.status(400).json({ error: 'Name, age, and batch are required fields.' });
  }

  // Mock database insertion
  const user = { name, age, batch, enrollment_date: new Date() };
  users.push(user);

  // Mock payment function
  // Assume CompletePayment() returns a success message
  const paymentResponse = CompletePayment(user);

  res.json({ success: true, paymentResponse, user });
});

// Mock payment function
function CompletePayment(user) {
  // Mock payment logic here
  return `Payment successful for ${user.name}`;
}

app.listen(port, () => {
  console.log(`Server is running on http://localhost:${port}`);
});

//ENTITY-RELATIONSHIP DIAGRAM
User
- UserID (Primary Key)
- Name
- Age
- EnrollmentDate

Batch
- BatchID (Primary Key)
- BatchName
- StartTime
- EndTime

Enrollment
- EnrollmentID (Primary Key)
- UserID (Foreign Key referencing User)
- BatchID (Foreign Key referencing Batch)


In this ER diagram:

- The "User" entity represents individuals enrolling in yoga classes, with attributes such as UserID, Name, Age, and EnrollmentDate.
- The "Batch" entity represents the different batches available for yoga classes, with attributes including BatchID, BatchName, StartTime, and EndTime.
- The "Enrollment" relationship table connects users to batches, with EnrollmentID as the primary key and UserID and BatchID as foreign keys referencing the respective entities.
