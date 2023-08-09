<script setup>
import {ref} from "vue";

// 1. By default, the modal is false. Everytime I click on the button, I change the state to "true"
const showModal = ref(false)

// Data binded with the text area
const newNote = ref("");

// To store the new notes
const notes = ref([]);

const errorMessage = ref("");

function getRandomColor() {
  var letters = '0123456789ABCDEF';
  var color = '#';
  for (var i = 0; i < 6; i++) {
    color += letters[Math.floor(Math.random() * 16)];
  }
  return color;
}

// Everytime we write something on the text area, we will push the value inside the array notes. We also generate an ID, a text, date, and a background color
const addNote = () => {
  if(newNote.value.length < 10) {
    return errorMessage.value = "Note needs to be more than 10 characters"
  }
  notes.value.push({
    // Generate randomly an ID
    id: Math.floor(Math.random() * 1000000),
    // two-way binding
    text: newNote.value,
    date: new Date(),
    backgroundColor: getRandomColor()
  });
  // To close the modal as soon as the note is entered:
  showModal.value = false;
  // To erase the value of the note before.
  newNote.value = "";
  // "" = faksy  value by default. We use that to conditionnaly render our message.
  errorMessage.value = "";
};

</script>

<template>
  <main>
<!-- Directive to say that if showModal is true, render the class overlay-->
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <!-- Thaks to the v-model (two ways binding), the text-area and the newNte will be the same, no matter what. Everytime we change something to the
        DOM, it will change up there-->
        <textarea v-model.trim="newNote" name="note" id="note" cols="30" rows="10"></textarea>
        <!-- If error message is falsy, this is not going to be rendered -->
        <p v-if="errorMessage"> {{ errorMessage }}</p>
        <button @click="addNote">Add Note</button>
        <!-- If on click the showModal is false, close it -->
        <button class="close" @click="showModal = false">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal = true">+</button>
      </header>
      <div class="cards-container">
        <!-- We use v-for to render a new card for each new object -->
        <!-- :style because javascript -->
        <!-- we also need a key because if we remove one card, on update it will remove all the html element and render all over the array again.
          If we give one unique key, Vue will be smart enough to remove only the concerned html element. -->
        <div v-for="note in notes" :key="note.id" class="card" :style="{ backgroundColor: note.backgroundColor }">
          <p class="main-text">{{ note.text }}</p>
          <p class="date">{{ note.date.toLocaleDateString("en-US") }}</p>
        </div>
      </div>
    </div>
  </main>
</template>


<style scoped>
main {
  height: 100vh;
  width: 100vh;
}

.container {
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

h1 {
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 75px;
}

header button {
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  background-color: salmon;
  color: white;
  cursor: pointer;
  border-radius: 100px;
  font-size: 20px;
}

.card {
width: 225px;
height: 225px;
background-color: aquamarine;
border-radius: 15px;
padding: 15px;
display: flex;
flex-direction: column;
justify-content: space-between;
margin-right: 20px;
margin-bottom: 20px;
}

.date {
  font-size: 12.5px;
  font-weight: bold;
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
}

.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.77);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal {
  width: 750px;
  background-color: white;
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
}

.modal button {
  background-color: darksalmon;
  border-style: none;
  padding: 10px;
  color: white;
  font-weight: bold;
  margin-top: 10px
}

textarea {
  border-style: dotted;
  border-color: darksalmon;
}

.modal .close {
margin-top: 7px;
background-color: rgb(234, 69, 14);
}

.modal p {
  color:  rgb(234, 69, 14);
}
</style>
