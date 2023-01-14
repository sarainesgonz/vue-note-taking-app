<template>
  <main>
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <!-- any change in the textarea is going to change the state newNote in the script and viceversa -->
        <textarea v-model="newNote" name="note" id="note" cols="30" rows="10"></textarea>
        <button>Post note</button>
        <button class="close" @click="showModal = false">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>My notes</h1>
        <button @click="showModal = true">Add note</button>
      </header>
      <div class="cards-container">
        <div class="card">
          <p class="main-text">Lorem ipsum dolor sit amet consectetur adipisicing elit. Consequuntur dolor nihil sint quisquam rem suscipit optio magni asperiores corporis ipsa.</p>
          <p class="date">12/01/2023</p>
        </div>
        <div class="card">
          <p class="main-text">Lorem ipsum dolor sit amet consectetur adipisicing elit. Consequuntur dolor nihil sint quisquam rem suscipit optio magni asperiores corporis ipsa.</p>
          <p class="date">12/01/2023</p>
        </div>
      </div>
    </div>
  </main>
</template>

<script setup>
  import {ref} from "vue";

  const showModal = ref(false)    //pasar al template en div v-if="showModal", al boton Add @click="showModal = true" y al boton Close @click="showModal = false"
  // state binded to the textarea
  const newNote = ref("")        //pasar al template en textarea v-model="newNote" 
  // create an array for the notes
  const notes = ref([])
  // create handler to add note
  const addNote = () => {
    // push a new object with theproperties
    notes.value.push({
      // access the value of newNote and push it as a property of the object
      text:newNote.value
    })
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
    background-color: rgba(0,0,0,0.5);
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
    background-color: black;
    cursor:pointer;
    margin: 7px;
  }
  .modal .close {
    background-color: rgb(209, 48, 48) ;
  }
</style>