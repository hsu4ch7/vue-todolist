<script setup>
import { ref, onMounted, reactive, computed, onBeforeUnmount } from 'vue';
import { compileScript } from 'vue/compiler-sfc';

const todoTasks = reactive([])
const progressingTasks = reactive([])
const completedTasks = reactive([])
const inputTask = ref("")
const currentTime = ref(new Date())

// input field
const handleEnterKey = () => {
  console.log("Enter!");
  todoTasks.push(
    {
      "taskName": inputTask.value,
      "createdTime": currentTime.value.toLocaleDateString(),
      "dateObject": currentTime.value,
    }
  )
  inputTask.value = ""
  document.activeElement.blur();
  console.log(todoTasks)
}

const deleteTask = (index, taskArray) => {
  console.log("Delete");
  console.log(taskArray)
  taskArray.splice(index, 1)
  console.log(taskArray)
}

const doSpecificTask = (index) => {
  console.log("Doing Task: " + todoTasks[index].taskName);
  let doTask = todoTasks.splice(index, 1)[0]
  let startedTime = (
    currentTime.value.getFullYear() + "/" + 
    (currentTime.value.getMonth() + 1) + "/" + 
    currentTime.value.getDate() + " " + 
    currentTime.value.getHours() + ":" +
    currentTime.value.getMinutes() + ":" + 
    currentTime.value.getSeconds()
  )
  progressingTasks.push(
    {
      "taskName": doTask.taskName,
      "startedTime": startedTime,
      "startedDateObject": currentTime.value,
    }
  )
}

const completeTask = (index) => {
  let doneTask = progressingTasks.splice(index, 1)[0]
  let completedTime = (
    currentTime.value.getFullYear() + "/" + 
    (currentTime.value.getMonth() + 1) + "/" + 
    currentTime.value.getDate() + " " + 
    currentTime.value.getHours() + ":" +
    currentTime.value.getMinutes() + ":" + 
    currentTime.value.getSeconds()
  )
  console.log(doneTask.startedDateObject)
  console.log(doneTask)
  // let pastTime = (currentTime.value.getTime() - doneTask.completeDateObject.getTime()) / 1000;
  
  completedTasks.push(
    {
      "taskName": doneTask.taskName,
      "completedTime": completedTime,
      "startedDateObject": doneTask.startedDateObject,
      "completeDateObject": currentTime.value,
      // "pastTime": pastTime,
    }
  )
  console.log(completedTasks)
}


// const currentTime = () => {
//  let nowTime = new Date();
//  console.lof(nowTime); 

// }

// time handler
const updateCurrentTime = () => {
  currentTime.value = new Date();
  
};

const udpateTimeInterval = setInterval(updateCurrentTime, 1000);

onBeforeUnmount(() => {
  clearInterval(udpateTimeInterval)
})



</script>

<template>
  <div id="todoListBody">
    <div>
      <p>{{ currentTime.toLocaleTimeString() }}</p>
    </div>
    <div>
      <input id="taskInputBox" v-model="inputTask" @keyup.enter="handleEnterKey" placeholder="Input task">
    </div>
    <div id="taskBlock">
      <div id="todoDiv">
        <h3>
          Todo
        </h3>
        <table>
          <thead>
            <tr>
              <th>Do it</th>
              <th>TaskName</th>
              <th>CreateTime</th>
              <th>Operation</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(task, index) in todoTasks" :key="index">
              <td>
                <input type="checkbox" :id="'checkForTask' + Index" @change="doSpecificTask(index)">
              </td>
              <td>
                <label :for="'checkForTask' + Index">{{ task.taskName }}</label>
              </td>
              <td>
                <p>{{ currentTime.toLocaleDateString() }}</p>
              </td>
              <td>
                <button @click="deleteTask(index, todoTasks)">Delete</button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
      <div id="progressDiv">
        <h3>
          Progressing
        </h3>
        <table>
          <thead>
            <tr>
              <th>check</th>
              <th>Doing</th>
              <th>StartedTime</th>
              <th>Operation</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(task, index) in progressingTasks" :key="index">
              <td>
                <input type="checkbox" @change="completeTask(index)">
              </td>
              <td>
                <label>{{ task.taskName }}</label>
              </td>
              <td>
                <p>{{ task.startedTime }}</p>
              </td>
              <td>
                <button @click="deleteTask(index, progressingTasks)">Delete</button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
      <div id="completeDiv">
        <h3>
          Completed
        </h3>
        <table>
          <thead>
            <tr>
              <th>TaskName</th>
              <th>EndTime</th>
              <th>PastTime</th>
              <th>Operation</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(task, index) in completedTasks" :key="index">
              <td>
                <label>{{ task.taskName }}</label>
              </td>
              <td>
                <p>{{ task.completedTime }}</p>
              </td>
              <td>
                <p>{{ task.pastTime }}</p>
              </td>
              <td>
                <button @click="deleteTask(index, completedTasks)">Delete</button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>

</template>

<style scoped>
  #todoListBody{
    text-align: center;
  }
  #taskInputBox {
    padding: 10px;
    margin-bottom: 30px;
    width: 20%;
    border : solid .1px black;
    border-radius: 35px;
    background-color :  #ffffff;
    
    outline:none;
    
  }
  #todoDiv, #progressDiv, #completeDiv{
    padding-left: 15px;
    padding-right: 15px;
    padding-bottom: 40px;
    padding-top: 5px;

    background-color :  #fff;
    margin: 2%;

  }
  #taskBlock{
    display: flex;
  }
  #taskBlock{
    overflow:hidden;
    border-radius:10px 10px 0px 0px;
    box-shadow: 0 0 20px rgba(0, 0, 0, 0.35);
    min-height: 80vh;
  }

  table{
    font-family: 'Oswald', sans-serif;
    border-collapse:collapse;

  }

  th{

    color:#ffffff;
    width:25vw;
    height:75px;
  }

  td{
    background-color:#ffffff;
    width:25vw;
    height:50px;
    text-align:center;
  }

  tr{
    border-radius: 10%;
    background-color: #003098;
  }



</style>