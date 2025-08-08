<template>
  <div class="form-container">
    <h2>Simple Front-End Form</h2>
    <form @submit.prevent="handleSubmit" novalidate>
      <div class="form-group">
        <label for="name">Name *</label>
        <input
          id="name"
          v-model.trim="form.name"
          type="text"
          :class="{ invalid: errors.name }"
          placeholder="Enter your name"
          required
          minlength="3"
        />
        <span v-if="errors.name" class="error">{{ errors.name }}</span>
      </div>

      <div class="form-group">
        <label for="email">Email *</label>
        <input
          id="email"
          v-model.trim="form.email"
          type="email"
          :class="{ invalid: errors.email }"
          placeholder="Enter your email"
          required
        />
        <span v-if="errors.email" class="error">{{ errors.email }}</span>
      </div>

      <div class="form-group">
        <label for="age">Age (optional)</label>
        <input
          id="age"
          v-model.number="form.age"
          type="number"
          min="1"
          max="120"
          placeholder="Enter your age"
        />
        <span v-if="errors.age" class="error">{{ errors.age }}</span>
      </div>

      <button type="submit">Submit</button>
    </form>

    <div v-if="submitted" class="result">
      <h3>Form Submitted Successfully!</h3>
      <p><strong>Name:</strong> {{ form.name }}</p>
      <p><strong>Email:</strong> {{ form.email }}</p>
      <p v-if="form.age !== null"><strong>Age:</strong> {{ form.age }}</p>
    </div>
  </div>
</template>

<script setup>
import { reactive, ref } from 'vue';

const form = reactive({
  name: '',
  email: '',
  age: null,
});

const errors = reactive({
  name: '',
  email: '',
  age: '',
});

const submitted = ref(false);

function validateEmail(email) {
  // Simple email regex
  const re = /\S+@\S+\.\S+/;
  return re.test(email);
}

function validate() {
  errors.name = '';
  errors.email = '';
  errors.age = '';

  let valid = true;

  if (!form.name || form.name.length < 3) {
    errors.name = 'Name must be at least 3 characters.';
    valid = false;
  }

  if (!form.email || !validateEmail(form.email)) {
    errors.email = 'Please enter a valid email address.';
    valid = false;
  }

  if (form.age !== null) {
    if (isNaN(form.age) || form.age < 1 || form.age > 120) {
      errors.age = 'Age must be a number between 1 and 120.';
      valid = false;
    }
  }

  return valid;
}

function handleSubmit() {
  if (validate()) {
    submitted.value = true;
  } else {
    submitted.value = false;
  }
}
</script>

<style scoped>
.form-container {
  max-width: 400px;
  margin: 2rem auto;
  padding: 1rem 1.5rem;
  background: #f9fafb;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgb(0 0 0 / 0.1);
  font-family: Arial, sans-serif;
}

h2 {
  text-align: center;
  margin-bottom: 1rem;
  color: #4f46e5;
}

.form-group {
  margin-bottom: 1rem;
  display: flex;
  flex-direction: column;
}

label {
  margin-bottom: 0.25rem;
  font-weight: 600;
  color: #374151;
}

input {
  padding: 0.5rem;
  font-size: 1rem;
  border: 2px solid #d1d5db;
  border-radius: 4px;
  transition: border-color 0.3s ease;
}

input:focus {
  outline: none;
  border-color: #4f46e5;
  box-shadow: 0 0 5px #a78bfa;
}

input.invalid {
  border-color: #ef4444;
}

.error {
  margin-top: 0.25rem;
  color: #ef4444;
  font-size: 0.875rem;
}

button {
  width: 100%;
  background-color: #4f46e5;
  color: white;
  font-weight: 700;
  padding: 0.75rem;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #4338ca;
}

.result {
  margin-top: 1.5rem;
  padding: 1rem;
  border: 2px solid #4f46e5;
  border-radius: 6px;
  background-color: #e0e7ff;
  color: #3730a3;
}
</style>
