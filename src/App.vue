<template>
  <h1>Personal Task List</h1>
  <form @submit.prevent="addtodo()">
    <label>New task</label>
    <input
      v-model="newtodo"
      name="newtodo"
      autocomplete="off"
    >
    <button>Add</button>
  </form>
  <h2>Task List</h2>
  <ul>
    <li
      v-for="(item, index) in tasks"
      :key="index"
    >
      <span 
        :class="{done:item.done}"
        @click="donetodo(item)"
      >
        {{ item.content }}
      </span>
      <button @click="removetodo(index)">Remove</button>
    </li>
  </ul>
</template>
<script>
  import { ref } from 'vue'
  export default {
    name : "App",
    setup() {
      const newtodo = ref("");
      const defaultdata = [{
        done : true,
        content : "Bangun Tidur"
      }];
      const tasksdata = JSON.parse(localStorage.getItem("tasks")) ||defaultdata;
      const tasks = ref(tasksdata);
      function addtodo() {
        if (newtodo.value) {
          tasks.value.push({
            done : false,
            content : newtodo.value
          });

          newtodo.value = "";
        }
        savedata();
      }
        function savedata() {
          const storagedata = JSON.stringify(tasks.value);
          localStorage.setItem("tasks", storagedata);
        }
        function donetodo(todo) {
          todo.done = !todo.done;
          savedata();
        }
        function removetodo(index) {
          tasks.value.splice(index,1);
          savedata();
        }
        return {
          tasks,
          newtodo,
          addtodo,
          savedata,
          removetodo,
          donetodo
        }
      }
    }
</script>
<style lang="scss">
  $border: 2px solid
	rgba(
		$color: black,
		$alpha: 0.35,
	);
$size1: 6px;
$size2: 12px;
$size3: 18px;
$size4: 24px;
$size5: 48px;
$backgroundColor: AliceBlue;
$textColor: black;
$primaryColor: #a0a4d9;
$secondTextColor: #1f2023;
body {
	margin: 0;
	padding: 0;
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	background-color: $backgroundColor;
	color: $textColor;
	#app {
		max-width: 600px;
		margin-left: auto;
		margin-right: auto;
		padding: 20px;
		h1 {
			font-weight: bold;
			font-size: 28px;
			text-align: center;
		}
		form {
			display: flex;
			flex-direction: column;
			width: 100%;
			label {
				font-size: 14px;
				font-weight: bold;
			}
			input,
			button {
				height: $size5;
				box-shadow: none;
				outline: none;
				padding-left: $size2;
				padding-right: $size2;
				border-radius: $size1;
				font-size: 18px;
				margin-top: $size1;
				margin-bottom: $size2;
			}
			input {
				background-color: transparent;
				border: $border;
				color: inherit;
			}
		}
    button {
			cursor: pointer;
			background-color: $primaryColor;
			border: 1px solid $primaryColor;
			color: $secondTextColor;
			font-weight: bold;
			outline: none;
			border-radius: $size1;
		}
		h2 {
			font-size: 22px;
			border-bottom: $border;
			padding-bottom: $size1;
		}
		ul {
			padding: 10px;
			li {
				display: flex;
				justify-content: space-between;
				align-items: center;
				border: $border;
				padding: $size2 $size4;
				border-radius: $size1;
				margin-bottom: $size2;
				span {
					cursor: pointer;
				}
				.done {
					text-decoration: line-through;
				}
				button {
					font-size: $size2;
					padding: $size1;
				}
			}
		}
		h4 {
			text-align: center;
			opacity: 0.5;
			margin: 0;
		}
	}
}
</style>