<template>
    <div>
      <h2>Sign Up Form</h2>
      <form @submit.prevent="handleSubmit">
        <div>
          <label for="name">Name:</label>
          <input
            type="text"
            v-model="formData.name"
            id="name"
            @input="validateName"
          />
          <span v-if="nameError" style="color: red; display: block;">{{ nameError }}</span>
        </div>
  
        <div>
          <label for="email">Email:</label>
          <input
            type="email"
            v-model="formData.email"
            id="email"
            data=""
            @input="validateEmail"
          />
          <span v-if="emailError" style="color: red; display: block;">{{ emailError }}</span>
        </div>
  
        <div>
          <button type="submit" :disabled="nameError || emailError">Submit</button>
        </div>
      </form>
  
      <div v-if="submitted">
        <h3>Form Submitted</h3>
        <p><strong>Name:</strong> {{ formData.name }}</p>
        <p><strong>Email:</strong> {{ formData.email }}</p>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        formData: {
          name: '',
          email: '',
        },
        nameError: '',
        emailError: '',
        submitted: false,
        isMobile: true,
      };
    },
    methods: {
      validateName() {
        const nameRegex = /^[A-Za-z\s]+$/;
        if (!this.formData.name) {
          this.nameError = 'Name is required';
        } else if (!nameRegex.test(this.formData.name)) {
          this.nameError = 'Name can only contain alphabets.';
        } else {
          this.nameError = '';
        }

        let john = {
            name: 'john',
            location: 'jakarta',
        }

        let lucy = {...john};
        lucy.name = 'lucy';
        console.log(lucy.name);
        console.log(john.name);
      },
      validateEmail() {
        const emailRegex =
          /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/;
        if (!this.formData.email) {
          this.emailError = 'Email is required';
        } else if (!emailRegex.test(this.formData.email)) {
          this.emailError = 'Please enter a valid email address';
        } else {
          this.emailError = '';
        }
      },
      handleSubmit() {
        this.validateName();
        this.validateEmail();
  
        if (!this.nameError && !this.emailError) {
          this.submitted = true;
        } else {
          alert('Please correct the errors in the form');
        }
      },
    },
  };
  </script>
  
  <style scoped>
  div {
    margin-bottom: 10px;
  }
  label {
    margin-right: 10px;
  }
  input {
    padding: 5px;
    border: 1px solid #ccc;
  }
  button {
    padding: 10px;
    background-color: #4CAF50;
    color: white;
    border: none;
    cursor: pointer;
  }
  button:disabled {
    background-color: #ccc;
    cursor: not-allowed;
  }
  span {
    font-size: 12px;
  }
  </style>  