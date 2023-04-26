<template>
  <div id="app">
    <p><strong>Add a new contact:</strong></p>
    <div class="card">
      <label>First Name: <input type="text" v-model="userFirstname" /></label>
      <span v-if="!validFirstname" class="error">First name is required</span> <br />
      <label>Last Name: <input type="text" v-model="userLastname" /></label>
      <span v-if="!validLastname" class="error">Last name is required</span> <br />
      <label>Phone: <input type="number" v-model="userPhone" /></label>
      <span v-if="!validPhone" class="error">Please enter a valid phone number with at least 10 digits</span> <br />
      <button @click="addContact" :disabled="!formValid">Add New Contact</button>
    </div>
    <table v-if="items.length > 0" id="phoneBookItems" class="informationTable">
      <thead>
        <tr>
          <th>First name</th>
          <th>Last name</th>
          <th>Phone</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in sortedItems" :key="index" class="fade-in">
          <td>{{ item.firstName }}</td>
          <td>{{ item.lastName }}</td>
          <td>{{ item.phone }}</td>
        </tr>
      </tbody>
    </table>
    <p v-if="items.length === 0">No contacts found.</p>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      userFirstname: "",
      userLastname: "",
      userPhone: null,
      items: [],
      validFirstname: true,
      validLastname: true,
      validPhone: true,
      showAddMessage: false,
    };
  },
  computed: {
    sortedItems() {
      const itemsCopy = [...this.items];
      return itemsCopy.sort((a, b) => a.lastName.localeCompare(b.lastName));
    },
    formValid() {
      return this.validFirstname && this.validLastname && this.validPhone;
    },
  },
  watch: {
    userPhone(value) {
      // Validar entrada do usuário no campo de telefone
      const phoneRegex = /^\d{10,}$/;
      this.validPhone = phoneRegex.test(value);
    },
  },
  methods: {
    addContact() {
      const item = {
        firstName: this.userFirstname,
        lastName: this.userLastname,
        phone: this.userPhone,
      };
      this.items.push(item);
      this.clearFields();
      this.showAddMessage = true;
      setTimeout(() => {
        this.showAddMessage = false;
      }, 2000);
    },
    clearFields() {
      this.userFirstname = "";
      this.userLastname = "";
      this.userPhone = null;
      this.validPhone = true;
    },
  },
};
</script>

<style>
#app {
  max-width: 600px;
  margin: 0 auto;
  font-family: Arial, sans-serif;
}

.card {
  width: 250px;
  padding: 20px;
  border-radius: 3px;
  border: 1px solid #ccc;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
}

label {
  display: inline-block;
  margin-bottom: 10px;
}



input {
  width: 200px;
  height: 30px;
  padding: 5px;
  border-radius: 3px;
  border: 1px solid #ccc;
  box-shadow: inset 0 1px 3px #ddd;
}

button {
  display: block;
  margin-top: 20px;
  background-color: #4caf50;
  color: white;
  border-radius: 3px;
  border: none;
  padding: 10px 15px;
  font-size: 16px;
  cursor: pointer;
}

table {
  border-collapse: collapse;
  margin-top: 20px;
}

th,
td {
  text-align: left;
  padding: 8px;
  border-bottom: 1px solid #ddd;
}

th {
  background-color: #4caf50;
  color: white;
}

.error {
  display: block;
  color: red;
  font-size: 14px;
  margin-top: 5px;
}

.fade-in {
  animation: fadeIn 1s;
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }

  to {
    opacity: 1;
  }
}
</style>
