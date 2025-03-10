<template>
    <div class="container d-flex justify-content-center align-items-center vh-100">
      <div class="form-container">
        <h2 class="text-center">User Registration</h2>
  
        <transition name="slide-fade" mode="out-in">
          <form v-if="!submitted" @submit.prevent="submitForm" key="form">
            <div class="mb-3">
              <label class="form-label">Name</label>
              <input type="text" class="form-control" v-model="user.name" />
              <small v-if="errors.name" class="text-danger">{{ errors.name }}</small>
            </div>
  
            <div class="mb-3">
              <label class="form-label">Email</label>
              <input type="email" class="form-control" v-model="user.email" />
              <small v-if="errors.email" class="text-danger">{{ errors.email }}</small>
            </div>
  
            <div class="mb-3">
              <label class="form-label">Password</label>
              <input type="password" class="form-control" v-model="user.password" />
              <small v-if="errors.password" class="text-danger">{{ errors.password }}</small>
            </div>
  
            <div class="mb-3">
              <label class="form-label">Age</label>
              <input type="number" class="form-control" v-model="user.age" />
              <small v-if="errors.age" class="text-danger">{{ errors.age }}</small>
            </div>
  
            <button type="submit" class="btn btn-primary w-100">Register</button>
          </form>
        </transition>
  
        <div v-if="loading" class="text-center mt-3">
          <div class="spinner-border text-primary" role="status">
            <span class="visually-hidden">Loading...</span>
          </div>
        </div>
  
        <transition name="fade">
          <div v-if="users.length > 0 && submitted" class="mt-4 user-list" key="user-list">
            <h3 class="text-center">Registered Users</h3>
            <ul class="list-group">
              <li v-for="user in users" :key="user.id" class="list-group-item">
                {{ user.name }} ({{ user.email }})
              </li>
            </ul>
          </div>
        </transition>
      </div>
    </div>
  </template>
  
  <style>
  
  .container {
    height: 100vh; 
    display: flex;
    justify-content: center;
    align-items: center;
  }
  
 
  .form-container {
    background: #ffffff;
    padding: 25px;
    border-radius: 10px;
    box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1);
    width: 100%;
    max-width: 400px;
  }
  

  .slide-fade-enter-active, .slide-fade-leave-active {
    transition: all 0.5s ease-in-out;
  }
  .slide-fade-enter, .slide-fade-leave-to {
    transform: translateY(-20px);
    opacity: 0;
  }
  

  .fade-enter-active, .fade-leave-active {
    transition: opacity 0.5s ease-in-out;
  }
  .fade-enter, .fade-leave-to {
    opacity: 0;
  }
  </style>
  
  
  <script>
  import axios from "axios";
  
  export default {
    data() {
      return {
        user: { name: "", email: "", password: "", age: "" },
        errors: {},
        users: [],
        submitted: false,
        loading: false, 
      };
    },
    methods: {
      validateForm() {
        this.errors = {};
        if (this.user.name.length < 3) this.errors.name = "Name must be at least 3 characters.";
        if (!/\S+@\S+\.\S+/.test(this.user.email)) this.errors.email = "Invalid email format.";
        if (this.user.password.length < 6) this.errors.password = "Password must be at least 6 characters.";
        if (!this.user.age || this.user.age <= 18) this.errors.age = "Age must be a number greater than 18.";
  
        return Object.keys(this.errors).length === 0;
      },
      async submitForm() {
        if (!this.validateForm()) return;
  
        try {
          await axios.post("https://jsonplaceholder.typicode.com/users", this.user);
          alert("Registration successful! Fetching users...");
  
          this.submitted = true; 
          this.loading = true; 
  
          setTimeout(() => {
            this.fetchUsers();
          }, 1000); 
        } catch (error) {
          console.error("Error submitting form:", error);
        }
      },
      async fetchUsers() {
        try {
          const response = await axios.get("https://jsonplaceholder.typicode.com/users");
          this.users = response.data;
          this.loading = false; 
        } catch (error) {
          console.error("Error fetching users:", error);
        }
      },
    },
    created() {
      this.fetchUsers();
    },
  };
  </script>
  
  <style>
 
  .slide-fade-enter-active, .slide-fade-leave-active {
    transition: all 0.5s ease-in-out;
  }
  .slide-fade-enter, .slide-fade-leave-to {
    transform: translateY(-20px);
    opacity: 0;
  }
  

  .fade-enter-active, .fade-leave-active {
    transition: opacity 0.5s ease-in-out;
  }
  .fade-enter, .fade-leave-to {
    opacity: 0;
  }
  </style>