<template>
  <div class="w-full px-24 py-8">
    <div class="flex items-center justify-between mb-6">
      <img class="w-40" src="https://cdn.jobs.makesense.org/projects/6215/project/1679153560289lfe4ob8i.png" />
      <button @click="showModal('add-task-modal')"
        class="flex items-center justify-center px-4 py-2 text-sm font-medium text-white rounded-lg bg-green-700 hover:bg-green-800 focus:ring-4 focus:ring-green-400 focus:outline-none">
        <svg fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg" aria-hidden="true"
          class="h-3.5 w-3.5 mr-2">
          <path clip-rule="evenodd" fill-rule="evenodd"
            d="M10 3a1 1 0 011 1v5h5a1 1 0 110 2h-5v5a1 1 0 11-2 0v-5H4a1 1 0 110-2h5V4a1 1 0 011-1z"></path>
        </svg>
        Add new task
      </button>
    </div>
    <div class="w-full grid grid-cols-2 gap-12">
      <div class="rounded-[20px] p-6 bg-[#dbeafe]">
        <div class="flex items-center justify-between">
          <h3 class="font-semibold text-xl text-black">To do</h3>
          <h3 class="font-semibold text-lg text-black">{{ filteredTasks('incomplete').length }}</h3>
        </div>
        <transition-group name="fade" tag="div">
          <div v-for="task in filteredTasks('incomplete')" :key="task.id" class="bg-white rounded-[10px] p-4 my-4">
            <h5 class="font-semibold text-black text-lg">{{ task.title }}</h5>
            <hr class="h-px my-4 bg-gray-200 border-0">
            <div class="flex items-center justify-between">
              <button @click="makeTaskDone(task.id)" type="button"
                class="text-sm font-medium text-green-500 focus:outline-none border-0 hover:text-green-600 focus:z-10 focus:ring-0">
                Done
              </button>
              <button @click="deleteTask(task.id)" type="button"
                class="text-sm font-medium text-red-600 focus:outline-none border-0 hover:text-red-700 focus:z-10 focus:ring-0">
                Delete
              </button>
            </div>
          </div>
        </transition-group>
      </div>
      <div class="rounded-[20px] p-6 bg-[#dcfce7]">
        <div class="flex items-center justify-between">
          <h3 class="font-semibold text-xl text-black">Completed</h3>
          <h3 class="font-semibold text-lg text-black">{{ filteredTasks('completed').length }}</h3>
        </div>
        <transition-group name="fade" tag="div">
          <div v-for="task in filteredTasks('completed')" :key="task.id" class="bg-white rounded-[10px] p-4 my-4">
            <h5 class="font-semibold text-black text-lg">{{ task.title }}</h5>
            <hr class="h-px my-4 bg-gray-200 border-0">
            <div class="flex items-center justify-end">
              <button @click="deleteTask(task.id)" type="button"
                class="text-sm font-medium text-red-600 focus:outline-none border-0 hover:text-red-700 focus:z-10 focus:ring-0">Delete</button>
            </div>
          </div>
        </transition-group>
      </div>
    </div>

    <div id="add-task-modal" tabindex="-1" class="hidden">
      <div
        class="fixed top-0 left-0 right-0 z-50 w-full bg-black bg-opacity-30 p-4 overflow-x-hidden overflow-y-auto md:inset-0 md:h-full flex items-center justify-center">
        <div class="relative p-4 w-full max-w-2xl h-full md:h-auto">
          <div class="relative p-4 bg-white rounded-lg shadow sm:p-5">
            <div class="flex justify-between items-center pb-4 mb-4 rounded-t border-b sm:mb-5">
              <h3 class="text-lg font-semibold text-gray-900">
                Add new task
              </h3>
              <button type="button" @click="hideModal('add-task-modal')"
                class="text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm p-1.5 ml-auto inline-flex items-center">
                <svg aria-hidden="true" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"
                  class="w-5 h-5">
                  <path fill-rule="evenodd"
                    d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z"
                    clip-rule="evenodd"></path>
                </svg> <span class="sr-only">Close modal</span>
              </button>
            </div>
            <form @submit.prevent="addTask">
              <div class="mb-5 "><label for="userId" class="block mb-2 text-sm font-medium text-gray-900">User ID</label>
                <input v-model="newTask.userId" type="number" name="userId" id="userId" placeholder="Ex: 1" required="required"
                  class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-green-600 focus:border-green-600 block w-full px-2.5 py-2">
              </div>
              <div class="mb-5 "><label for="title" class="block mb-2 text-sm font-medium text-gray-900">Title</label>
                <input v-model="newTask.title" type="text" name="title" id="title" placeholder="Ex: Creating a web application"
                  required="required"
                  class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-green-600 focus:border-green-600 block w-full px-2.5 py-2">
              </div>
              <div class="grid gap-4 sm:grid-cols-2">
                <button type="submit"
                  class="flex items-center justify-center px-4 py-2 text-sm font-medium text-white rounded-lg bg-green-700 hover:bg-green-800 focus:ring-4 focus:ring-green-300 focus:outline-none">
                  <svg fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg" aria-hidden="true"
                    class="h-3.5 w-3.5 mr-2">
                    <path clip-rule="evenodd" fill-rule="evenodd"
                      d="M10 3a1 1 0 011 1v5h5a1 1 0 110 2h-5v5a1 1 0 11-2 0v-5H4a1 1 0 110-2h5V4a1 1 0 011-1z"></path>
                  </svg>
                  Add task
                </button>
                <button @click="hideModal('add-task-modal')" type="button"
                  class="text-gray-500 bg-white hover:bg-gray-100 focus:ring-4 focus:outline-none focus:ring-green-300 rounded-lg border border-gray-200 text-sm font-medium px-5 py-2 hover:text-gray-900 focus:z-10">Cancel</button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>

    <MyToast v-if="showToast" :content="toastContent" :type="toastType" @dismiss="dismissToast" />

  </div>
</template>

<script>
import { ref, onMounted, computed } from 'vue';
import MyToast from './MyToast.vue';

export default {
  name: 'TasksList',
  components: {
    MyToast,
  },
  setup() {
    const tasks = ref([]);
    const showToast = ref(false);
    const toastContent = ref('');
    const toastType = ref('');
    const newTask = ref({
      userId: '',
      title: '',
    });

    onMounted(async () => {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/todos');
        const data = await response.json();
        tasks.value = data;
      } catch (error) {
        toastContent.value = error;
        toastType.value = 'danger';
        showToast.value = true;
      }
    });

    const filteredTasks = computed(() => {
      return (status) => {
        if (status === 'incomplete') {
          return tasks.value.filter(task => !task.completed);
        } else if (status === 'completed') {
          return tasks.value.filter(task => task.completed);
        }
      };
    });

    const deleteTask = (taskId) => {
      const index = tasks.value.findIndex(task => task.id === taskId);
      if (index !== -1) {
        tasks.value.splice(index, 1);

        toastContent.value = `Task with ID ${taskId} has been deleted.`;
        toastType.value = 'success';
        showToast.value = true;
      }
    };

    const addTask = () => {
      const existingIds = tasks.value.map(task => task.id);
      let newTaskId = 1;
      while (existingIds.includes(newTaskId)) {
        newTaskId++;
      }

      const task = {
        userId: newTask.value.userId,
        id: newTaskId,
        title: newTask.value.title,
        completed: false,
      };

      tasks.value.unshift(task);

      newTask.value.userId = '';
      newTask.value.title = '';

      hideModal('add-task-modal');

      toastContent.value = 'New task has been added.';
      toastType.value = 'success';
      showToast.value = true;
    };

    const makeTaskDone = (taskId) => {
      const task = tasks.value.find((t) => t.id === taskId);
      if (task) {
        task.completed = true;
        toastContent.value = `Task with ID ${taskId} has been marked as done.`;
        toastType.value = 'success';
        showToast.value = true;
      }
    };

    const showModal = (modalId) => {
      document.getElementById(modalId).style.display = 'block';
    };

    const hideModal = (modalId) => {
      document.getElementById(modalId).style.display = 'none';
    };

    const dismissToast = () => {
      showToast.value = false;
    };

    return {
      tasks,
      filteredTasks,
      deleteTask,
      showToast,
      toastContent,
      toastType,
      dismissToast,
      showModal,
      hideModal,
      newTask,
      addTask,
      makeTaskDone
    };
  },
};
</script>

<style>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>