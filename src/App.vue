<script setup>
  import { ref, nextTick } from "vue";
  const showModel = ref(false)
  const newMemo = ref("")
  const memos = ref([])
  const errorMessage = ref("")
  const memoInput = ref("");

  function getRandomColor() {
    return "hsl(" + Math.random() * 360 +", 100%, 75%)";

  }

  const addMemo = () => {
    if(newMemo.value.length < 5){
      return errorMessage.value = "Memo needs to have atleast 5 characters"
    }
    memos.value.push({
      id: Math.floor(Math.random() * 1000000),
      text: newMemo.value,
      date: new Date(),
      backgroundColor: getRandomColor()
    });
    showModel.value = false;
    newMemo.value = "";
    errorMessage.value = ""
  }


  const openMemo = () => {
      showModel.value = true;
      nextTick(() => {
        memoInput.value.focus();
      });
    };

  const closeMemo = () => {
    showModel.value = false;
    newMemo.value = "";
  }

</script>
<template>
  <main>
    <div v-if="showModel" class="overlay">
      <div class="model">
        <textarea v-model.trim="newMemo" ref="memoInput" name="memo" id="memo" cols="30" rows="10"></textarea>
        <p class="error">{{  errorMessage }}</p>
        <button @click="addMemo">Add Memo</button>
        <button class="close" @click="closeMemo">Close</button>
      </div>
    </div>
    <div class = "container">
      <header>
        <h1>Memo</h1>
        <button @click="openMemo">+</button>
      </header>
      <div class="cards-container">
        <div
         v-for="memo in memos" 
         :key="memo.id"
         class="card" 
         :style="{backgroundColor: memo.backgroundColor}"
         >
          <p class="main-text">{{ memo.text }}</p>
          <p class="date">{{ memo.date.toLocaleDateString("en-US") }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
  main {
    height: 100vh;
    width: 100vw;
    background-color: rgb(247, 224, 242);
  }
  
  .container {
    max-width: 1000px;
    padding: 10px;
    margin: 0 auto;
  }

  header {
    display: flex;
    justify-content:space-between;
    align-items: center;
    margin-bottom: 20px;
  }

  h1 {
    font-weight: 500;
    font-size: 75px;
    color: rgb(27, 231, 231);
  }

  header button {
    border:none;
    padding: 10px;
    width: 50px;
    height: 50px;
    cursor: pointer;
    background-color: rgb(27, 231, 231);
    color: rgb(253, 253, 253);
    font-size: 20px;
    border-radius: 100%;
  }

  .card {
    width: 225px;
    height: 225px;
    background-color:floralwhite;
    padding: 10px;
    border-radius: 10%;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    margin-right: 20px;
    margin-bottom: 20px;
  }

  .date {
    font-size: 12.5px;
    font-weight: 500;
  }

  .cards-container {
    display: flex;
    flex-wrap: wrap;
  }

  .overlay {
    position: absolute;
    width: 100%;
    height: 100%;
    background-color:rgba(0,0,0,0.47) ;
    z-index: 10;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .model {
    width: 750px;
    background-color: white;
    border-radius: 10px;
    padding: 30px;
    position: relative;
    display: flex;
    flex-direction: column;
  }

  .model button {
    padding: 10px 20px;
    font-size: 20px;
    width: 100%;
    background-color: rgb(182, 113, 246);
    border:none;
    color:white;
    cursor: pointer;
    margin-top: 15px;
  }
  
  .model button:hover {
    background-color: rgb(144, 70, 213);
  }

  .model .close {
    background-color: rgb(235, 119, 119);
    margin-top: 7px;
  }

  .model .close:hover {
    background-color: rgb(228, 94, 94);
  }

  .error {
    color: red;
  }
</style>