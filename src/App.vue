<template>
  <div class="app">
    <h1>Student List</h1>
    
    <!-- Táblázat megjelenítése az API-ból származó adatokkal -->
    <table v-if="students.length">
      <thead>
        <tr>
          <th>Student ID</th>
          <th>First Name</th>
          <th>Last Name</th>
          <th>Gender</th>
          <th>Email</th>
          <th>Phone Number</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="student in students" :key="student.studentId">
          <td>{{ student.studentId }}</td>
          <td>{{ student.firstName }}</td>
          <td>{{ student.lastName }}</td>
          <td>{{ student.gender }}</td>
          <td>{{ student.email }}</td>
          <td>{{ student.phoneNumber }}</td>
        </tr>
      </tbody>
    </table>

    <!-- Hozzáadási űrlap -->
    <h2>Add New Student</h2>
    <form @submit.prevent="addStudent">
      <div>
        <label for="firstName">First Name</label>
        <input type="text" v-model="newStudent.firstName" id="firstName" required />
      </div>
      <div>
        <label for="lastName">Last Name</label>
        <input type="text" v-model="newStudent.lastName" id="lastName" required />
      </div>
      <div>
        <label for="gender">Gender</label>
        <select v-model="newStudent.gender" id="gender" required>
          <option value="Male">Male</option>
          <option value="Female">Female</option>
        </select>
      </div>
      <div>
        <label for="email">Email</label>
        <input type="email" v-model="newStudent.email" id="email" required />
      </div>
      <div>
        <label for="phoneNumber">Phone Number</label>
        <input type="text" v-model="newStudent.phoneNumber" id="phoneNumber" required />
      </div>
      <button type="submit">Add Student</button>
    </form>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';
import axios from 'axios';

export default {
  name: 'App',
  setup() {
    const students = ref([]);
    const newStudent = ref({
      firstName: '',
      lastName: '',
      gender: 'Male',
      email: '',
      phoneNumber: ''
    });

    // API adat lekérése
    const fetchStudents = async () => {
      try {
        const response = await axios.get('https://localhost:7048/api/Student');
        students.value = response.data;
      } catch (error) {
        console.error('Error fetching data:', error);
      }
    };

    // Új hallgató hozzáadása
    const addStudent = async () => {
      try {
        const response = await axios.post('https://localhost:7048/api/Student', newStudent.value);
        // Hozzáadás után frissítjük az adatokat
        students.value.push(response.data);
        // Űrlap nullázása
        newStudent.value = { firstName: '', lastName: '', gender: 'Male', email: '', phoneNumber: '' };
      } catch (error) {
        console.error('Error adding student:', error);
      }
    };

    // Adatok betöltése, amikor a komponens inicializálódik
    onMounted(fetchStudents);

    return {
      students,
      newStudent,
      addStudent
    };
  }
};
</script>

<style scoped>
table {
  width: 100%;
  border-collapse: collapse;
  margin-bottom: 20px;
}

th, td {
  padding: 8px;
  border: 1px solid #ddd;
}

form div {
  margin-bottom: 10px;
}

label {
  display: block;
}

input, select {
  width: 100%;
  padding: 8px;
  margin-top: 4px;
}

button {
  padding: 10px 15px;
  background-color: #4CAF50;
  color: white;
  border: none;
  cursor: pointer;
}

button:hover {
  background-color: #45a049;
}
</style>