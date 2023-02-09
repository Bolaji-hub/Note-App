<script setup>
import { ref } from "vue";

const showModel = ref(false);
const newNote = ref("");
const errorMessage = ref("")
const notes = ref([]);

function getRandomColor() {
  return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
}

const dateFormatter = Intl.DateTimeFormat('en-US', {
  dateStyle: 'medium'
})

const addNote = () => {
  if(newNote.value.length < 10) {
    return errorMessage.value = "10 characters or more is allowed"
  }
  notes.value.push({
    id: Math.floor(Math.random() * 1000000),
    text: newNote.value,
    date: new Date(),
    backgroundColor: getRandomColor(),
  });
  showModel.value = false;
  newNote.value = "";
  errorMessage.value = ""
};
</script>

<template>
  <main>
    <div v-if="showModel" class="overlay">
      <div class="model">
        <textarea v-model.trim="newNote">
 name="note" id="note" cols="30" rows="10"></textarea
        >
        <p v-if="errorMessage">{{ errorMessage }}</p>
        <button @click="addNote">Add Note</button>
        <button class="close" @click="showModel = false">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModel = true">+</button>
      </header>
      <div class="card-container">
        <div
          v-for="(note, i) in notes"
          :key="i"
          class="card"
          :style="{ backgroundColor: note.backgroundColor }"
        >
          <p class="main-text">{{ note.text }}</p>
          <p class="date">{{ dateFormatter.format(note.date) }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  height: 100vh;
  width: 100vw;
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
  font-size: 80px;
}

header button {
  border: none;
  cursor: pointer;
  padding: 12px;
  width: 50px;
  height: 50px;
  background-color: black;
  border-radius: 90%;
  color: whitesmoke;
  font-size: 20px;
}

.card {
  width: 225px;
  height: 225px;
  padding: 10px;
  color: black;
  border-radius: 15px;
  display: flex;
  background-color: orange;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
}

.date {
  font-size: 12px;
  font-weight: bold;
  color: black;
}

.card-container {
  display: flex;
  flex-wrap: wrap;
}

.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 10;
}
.model {
  width: 750px;
  border-radius: 10px;
  padding: 30px;
  background-color: whitesmoke;
  position: relative;
  display: flex;
  flex-direction: column;
}

.model button {
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  background-color: blueviolet;
  border: none;
  color: whitesmoke;
  cursor: pointer;
  margin-top: 15px;
}

.model .close {
  background-color: brown;
  margin-top: 9px;
}
.model p {
  color: rgba(255, 0, 0, 0.807);
}
</style>
