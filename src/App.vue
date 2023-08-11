<script setup>
import { ref } from "vue";

const showModal = ref(false);
const newNote = ref("");
const errorMessage = ref("");
const notes = ref([]);

const addNote = () =>
{
  if (newNote.value.length < 10)
  {
    return (errorMessage.value = "Note needs to be at least 10 characters");
  }
  notes.value.push({
    id: Math.floor(Math.random() * 100),
    text: newNote.value,
    color: getRandomLightColor(),
    date: new Date().toLocaleDateString("ru-KZ"),
  });
  showModal.value = false;
  newNote.value = "";
  errorMessage.value = "";
};

function getRandomLightColor()
{
  return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
}
</script>

<template>
  <main>
    <div v-if=" showModal " class="overlay">
      <div class="modal">
        <textarea v-model.trim=" newNote " name="note" id="note" cols="30" rows="10"></textarea>
        <p class="errorMessage" v-if=" errorMessage ">{{ errorMessage }}</p>
        <button class="addNote" @click=" addNote "> Add note</button>
        <button class="close" @click="showModal = false">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal = true">+</button>
      </header>
      <div class="cards-container">
        <div :key=" note.id " v-for="   note    in    notes   " class="card" :style=" { backgroundColor: note.color } ">
          <p class="main-text">{{ note.text }}</p>
          <div class="date">{{ note.date }}</div>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
.container {
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;
}

h1 {
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 75px;
}

.card {
  width: 225px;
  height: 225px;
  background-color: rgb(237, 182, 44);
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
}

.main-text {
  line-height: 1.25;
  font-size: 17.5px;
}

.date {
  font-size: 12.5px;
  margin-top: auto;
  font-weight: bold;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

header button {
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  background-color: rgb(21, 20, 20);
  border-radius: 1000px;
  color: white;
  font-size: 20px;
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
  transform: translate(-50%, -50%);
  top: 50%;
  left: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 10;
}

main {
  height: 100vh;
  width: 100vw;
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

.close {
  background-color: rgb(212, 11, 11);
}

.addNote {
  background-color: blueviolet;
}

.modal button {
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  border: none;
  color: white;
  cursor: pointer;
  margin-top: 15px;
}

.errorMessage {
  margin-left: auto;
  font-size: 20px;
  z-index: 100000;
  cursor: pointer;
  color: rgb(212, 11, 11);
}


textarea {
  width: 100%;
  height: 200px;
  padding: 5px;
  font-size: 20px;
}
</style>
