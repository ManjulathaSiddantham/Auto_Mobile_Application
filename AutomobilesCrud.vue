<template>
    <div>
      <div class="header">
        <h2 class="title">Auto Companion</h2>
      </div>
      <form @submit.prevent="saveData" class="form">
        <div class="form-group">
          <label class="label">Car Name</label>
          <input type="text" v-model="cars.CarName" class="form-control" placeholder="Car name">
        </div>
        <div class="form-group">
          <label class="label">Car Colour</label>
          <input type="text" v-model="cars.Colour" class="form-control" placeholder="Car color">
        </div>
        <div class="form-group">
          <label class="label">Car Price</label>
          <input type="text" v-model="cars.Price" class="form-control" placeholder="Car price">
        </div>
        <button type="submit" class="btn primary-btn">{{ editing ? 'Update' : 'Save' }}</button>
      </form>
      <div class="table-container">
        <h2 class="title">Cars Information</h2>
        <table class="table">
          <thead>
            <tr>
              <th scope="col">Car Name</th>
              <th scope="col">Car Colour</th>
              <th scope="col">Car Price</th>
              <th scope="col">Option</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="car in result" :key="car._id">
              <td>{{ car.CarName }}</td>
              <td>{{ car.Colour }}</td>
              <td>{{ car.Price }}</td>
              <td>
                <button type="button" class="btn edit-btn" @click="edit(car)">Edit</button>
                <button type="button" class="btn delete-btn" @click="remove(car._id)">Delete</button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    name: 'AutomobilesCrud',
    data() {
      return {
        result: [],
        cars: {
          CarName:'',
          Colour: '',
          Price: ''
        },
        editing: false
      };
    },
    created() {
      this.CarsLoad();
    },
    methods: {
      async CarsLoad() {
        try {
          const response = await axios.get("http://localhost:3000/user/getAll");
          this.result = response.data.data;
        } catch (error) {
          console.error("Error loading cars:", error);
        }
        this.editing = false;
      },
  
      async saveData() {
        try {
          if (this.editing) {
            const editrecords = `http://localhost:3000/user/update/${this.cars._id}`;
            const response = await axios.patch(editrecords, this.cars);
            console.log("Data updated:", response.data);
          } else {
            const response = await axios.post("http://localhost:3000/user/create", this.cars);
            console.log("Data saved:", response.data);
          }
          this.cars = {
            CarName: '',
            Colour: '',
            Price: ''
          };
          this.CarsLoad();
        } catch (error) {
          console.error("Error saving/updating data:", error);
        }
      },
  
      edit(car) {
        this.cars = { ...car };
        this.editing = true;
      },
  
      async remove(carId) {
        try {
          const url = `http://localhost:3000/user/delete/${carId}`;
          await axios.delete(url);
          alert("Data deleted");
          this.CarsLoad();
        } catch (error) {
          console.error("Error deleting data:", error);
        }
      }
    }
  };
  </script>
  
  <style scoped>
  .body {
    font-family: 'Arial', sans-serif;
    background-color: #f4f4f4;
    margin: 0;
  }
  
  .header {
    background-color: #5602ff;
    padding: 20px;
    text-align: center;
  }
  
  .title {
    font-size: 24px;
    color: white;
    margin-bottom: 20px;
  }
  
  .form {
    max-width: 600px;
    margin: 0 auto;
    background-color: white;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 0 10px rgba(5, 171, 52, 0.636);
  }
  
  .form-group {
    margin-bottom: 15px;
  }
  
  .label {
    display: block;
    font-weight: bold;
    color: #ef0606;
    margin-bottom: 5px;
  }
  
  .form-control {
    width: 100%;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
    font-size: 16px;
  }
  
  .btn {
    padding: 10px 15px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    font-size: 16px;
  }
  
  .primary-btn {
    background-color: #00ff08;
    color: #111;
  }
  
  .edit-btn {
    background-color: #13f1e2;
    color: #111;
  }
  
  .delete-btn {
    background-color: #f22b2b;
    color: #111;
  }
  
  .table-container {
    max-width: 800px;
    margin: 20px auto;
    background-color: white;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  }
  
  .table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 20px;
  }
  
  th,
  td {
    border: 1px solid #ccc;
    padding: 10px;
    text-align: left;
  }
  
  th {
    background-color: #5602ff;
    color: white;
  }
  </style>
  