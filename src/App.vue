<template>
  <main>
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <!-- any change in the textarea is going to change the state newNote in the script and viceversa -->
        <!-- v-model.trim cuts all the white spacing and counts just the characters  -->
        <textarea v-model.trim="newNote" name="note" id="note" cols="30" rows="10"></textarea>
        <p class="error" v-if="errorMessage"> {{ errorMessage }}</p>
        <button @click="postNote">Post this note</button>
        <button class="cancel" @click="showModal = false">Cancel</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>My notes</h1>
        <button @click="showModal = true">Add note</button>
      </header>
      <div class="cards-container">
        <!-- use directives v-for (always use with a unique value :key="") to iterate over our array of notes and render them as html elements  -->
        <div v-for="note in notes" :key="note.id" class="card" :style="{backgroundColor: note.noteColor}">
          <p class="main-text">{{note.text}}</p>
          <p class="date">{{ note.date.toLocaleDateString() }}</p>
          <!-- <div class="delete-update-btn">
          <button class="updateNote">Update</button>
          <button class="deleteNote">Delete</button>
          </div> -->
        </div>
      </div>
    </div>
  </main>
</template>

<script setup>
  import {ref} from "vue";
  const errorMessage = ref("") //pass this error with a message in the if of postNote and on p below the textarea with: v-if="errorMessage"> {{ errorMessage }}
  const showModal = ref(false)    //pasar al template en div v-if="showModal", al boton Add @click="showModal = true" y al boton Cancel @click="showModal = false"
  // state binded to the textarea,captures the text area note
  const newNote = ref("")        //pasar al template en textarea v-model="newNote" para que capture el texto
  // create an array for the notes
  const notes = ref([])

  function getRandomColor() {
    let color = "hsl("+ Math.random() * 360 +",100%,75%)";
    return color
  }
  // create handler to add note
  const postNote = () => { //se lo pasaamos al overlay button Post note @click="postNote"
  if(newNote.value.length < 10) {
    errorMessage.value = "The note needs to be at least 10 characters long"//return early: if less than 10 characters, the app never goes to the push line below and throws an error message
    setTimeout(() => {
      errorMessage.value = ""
    }, 5000);
    return errorMessage.value
  }
  // push a new object with the properties
    notes.value.push({
      // access the value of newNote and push it as a property of the object
      id: Math.floor(Math.random()*1000000),
      // text is goin to be the value of the textarea captured into newNote
      text: newNote.value,
      date: new Date(),
      noteColor: getRandomColor()
    });
    //closes the overlay after publishing
    showModal.value = false,
    //clears the textarea after publishing
    newNote.value = "" 
  }

</script>

<style scoped>
  main {
    height: 100%;
    width: 100%;
    /* background-color: beige; */
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
    font-size: 60px;
    font-family:Arial, Helvetica, sans-serif;   
  }
  header button {
    border: none;
    padding: 10px;
    width: 7rem;
    height: 3rem;
    cursor: pointer;
    background-color: rgb(21, 20,20) ;
    border-radius: 20px;
    color: white;
    font-size: 20px;
    /* font-weight: bold; */
  }
  .card {
    width: 225px;
    height: 225px;
    background-color: rgb(255, 255, 88);
    padding: 10px;
    border-radius: 15px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    margin-right: 20px;
    margin-bottom: 20px;
  }
  .date {
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
    background-color: rgba(0,0,0,0.9);
    /* z-index: 10; */
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .modal {
    width: 750px;
    background-color: white;
    border-radius: 20px;
    padding: 25px;
    position: relative;
    display: flex;
    flex-direction: column;
  }
  .modal button {
    padding: 10px 20px;
    font-size: 20px;
    max-width: 100%;
    border: none;
    border-radius: 15px;
    color: white;
    background-color: rgb(25, 168, 66);
    cursor:pointer;
    margin: 7px;
  }
  .modal .cancel {
    background-color: rgb(209, 48, 48) ;
  }
  .error {
    color: red;
  }
  /* .delete-update-btn {
    display: flex;
    justify-content: space-around;
    align-items: center;

  }
  .deleteNote, .updateNote {
    border: none;
    border-radius: 6px;
    padding: 4px 7px;
    background-color: rgb(66, 66, 66);
    color: white;
    text-transform: uppercase;
    font-weight: bold;
  } */
</style>