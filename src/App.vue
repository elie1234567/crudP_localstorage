<template>
  <div class="container">
    <h1>CRUD Vue.js</h1>
    <label for="search">Recherche par nom et ID:</label>
    <input type="text" id="search" v-model="searchTerm" @input="filterRows" style=border-color:rouge; width:50px;>
    
    <form @submit.prevent="addData">
      <label for="name">Name:</label>
      <input type="text" id="name" v-model="name" required>
      <label for="email">Email:</label>
      <input type="email" id="email" v-model="email" required>
      <label for="devise">Devise:</label>
      <input type="text" id="devise" v-model="devise" required>
      <button type="submit">Add</button>
    </form>
    
    <table>
      <thead>
        <tr>
          <th>ID</th>
          <th>Name</th>
          <th>Email</th>
          <th>Devise</th>
          <th>Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in filteredData" :key="item.id">
          <td>{{ item.id }}</td>
          <td contenteditable="true" @input="updateData(item.id, 'name', $event.target.innerText)">{{ item.name }}</td>
          <td contenteditable="true" @input="updateData(item.id, 'email', $event.target.innerText)">{{ item.email }}</td>
          <td contenteditable="true" @input="updateData(item.id, 'devise', $event.target.innerText)">{{ item.devise }}</td>
          <td>
            <button @click="deleteData(item.id)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import  src from './data.json';
export default {
  data() {
    return {
      data: [],
      searchTerm: '',
      name: '',
      email: '',
      devise: '',
      lastGeneratedId: 47,
    };
  },
  computed: {
    filteredData() {
      return this.data.filter(item => {
        return (
          item.name.toLowerCase().includes(this.searchTerm.toLowerCase()) ||
          item.id.toString().includes(this.searchTerm.toLowerCase())
        );
      });
    },
  },
  methods: {
    generateId() {
      this.lastGeneratedId++;
      return this.lastGeneratedId;
    },
    addData() {
      const id = this.generateId();
      this.data.push({ id, name: this.name, email: this.email, devise: this.devise });
      this.updateLocalStorage();
      this.name = '';
      this.email = '';
      this.devise = '';
    },
    updateData(id, key, value) {
      const index = this.data.findIndex(item => item.id === id);
      this.data[index][key] = value;
      this.updateLocalStorage();
    },
    deleteData(id) {
      this.data = this.data.filter(item => item.id !== id);
      this.updateLocalStorage();
    },
    updateLocalStorage() {
      localStorage.setItem('crudData', JSON.stringify(this.data));
    },
    populateTable() {
      const localData = localStorage.getItem('crudData');
      this.data = localData ? JSON.parse(localData) : jsonData || [];
    },
  },
  mounted() {
    this.populateTable();
  },
};
</script>


<style>
body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
   background-color: #272727;
     color: #191818;
}
.container {
    max-width: 800px;
    margin: 20px auto;
    padding: 20px;
    background-color: #fff;
    border-radius: 5px;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}
h1 {
    text-align: center;
}
form {
    margin-bottom: 20px;
}
label {
    display: block;
    margin-bottom: 5px;
}
input[type="text"],
input[type="email"] {
    width: 100%;
    padding: 8px;
    margin-bottom: 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
}
button {
    padding: 8px 16px;
    background-color: #007bff;
    color: black;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}
button:hover {
    background-color: #0056b3;
}
table {
    width: 100%;
    border-collapse: collapse;
}
table, th, td {
    border: 1px solid #191818;
    padding: 8px;
}
th {
    background-color: #514f4f;
    text-align: left;
}

</style>
