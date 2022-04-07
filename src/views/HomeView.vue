/* global Vue, axios */
<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Contacts",
      contacts: [],
      newContact: {},
      currentContact: {},
    };
  },
  created: function () {
    this.indexContacts();
  },

  methods: {
    indexContacts: function () {
      axios.get("/contacts").then((response) => {
        console.log(response.data);
        this.contacts = response.data;
      });
    },
    createContact: function () {
      var params = this.newContact;
      axios.post("/contacts", params).then((response) => {
        console.log("New Contact Created");
        console.log(response.data);
        this.contacts.push(response.data);
      });
    },
    showContact(contact) {
      this.currentContact = contact;
      document.querySelector("#contact-details").showModal();
    },
    updateContact() {
      axios.patch(`/contacts/${this.currentContact.id}`,
      this.currentContact).then((response) => {
        console.log("Contact Updated", response);

      };
    };
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h2>New Contact</h2>
    First Name
    <input type="text" v-model="newContact.first_name" />
    Last Name
    <input type="text" v-model="newContact.last_name" />
    Email
    <input type="text" v-model="newContact.email" />
    Phone Number
    <input type="text" v-model="newContact.phone_number" />
    <!-- Image
    <input type="text" v-model="newContact.image" /> -->
    <button v-on:click="createContact()">Create New Contact</button>
    <div v-for="contact in contacts" v-bind:key="contact.id">
      <h2>{{ contact.first_name }}</h2>
      <h2>{{ contact.last_name }}</h2>
      <h2>{{ contact.email }}</h2>
      <h2>{{ contact.phone_number }}</h2>
      <button v-on:click="showContact(contact)">More Info</button>
    </div>
  </div>
  <dialog id="contact-details">
    <h1>Contact Info</h1>
    <p>First Name: {{ currentContact.first_name }}</p>
    <p>Last Name: {{ currentContact.last_name }}</p>
    <p>Email : {{ currentContact.email }}</p>
    <p>Phone Number: {{ currentContact.phone_number }}</p>
  </dialog>
</template>

<style></style>
