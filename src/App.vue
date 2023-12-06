<template>
  <div class="bg-gray-100 min-h-screen flex flex-col p-6">
    <h1 class="text-4xl font-semibold mb-6">To-Do List</h1>
    <div class="bg-white p-6 rounded shadow-lg">
      <form @submit.prevent="addItem" class="mb-4">
        <input
          v-model="newItem.title"
          placeholder="Add new item"
          class="border-2 border-gray-300 p-2 w-full mb-2 rounded focus:outline-none focus:border-gray-400"
        />
        <input
          v-model="newItem.description"
          placeholder="Add description"
          class="border-2 border-gray-300 p-2 w-full rounded focus:outline-none focus:border-gray-400"
        />
        <button
          type="submit"
          class="bg-blue-500 text-white mt-2 px-4 py-2 rounded w-full shadow-lg focus:outline-none"
        >
          Add
        </button>
      </form>
      <ul>
        <li
          v-for="(item, index) in items"
          :key="index"
          class="p-4 rounded mb-4 bg-gray-200 shadow-lg flex justify-between items-center"
        >
          <div v-if="!item.edit" class="text-xl font-semibold">{{ item.title }}</div>
          <input
            v-else
            v-model="item.title"
            class="border-2 border-gray-300 p-2 w-full mb-2 rounded focus:outline-none focus:border-gray-400"
          />
          <div class="flex">
            <button
              v-if="!item.edit"
              @click="toggleEdit(index)"
              class="bg-yellow-500 text-white px-3 py-1 rounded mr-1"
            >
              Edit
            </button>
            <button
              v-else
              @click="toggleEdit(index)"
              class="bg-green-500 text-white px-3 py-1 rounded mr-1"
            >
              Save
            </button>
            <button
              v-if="!item.edit"
              @click="viewItem(item)"
              class="bg-blue-500 text-white px-3 py-1 rounded mr-1"
            >
              View
            </button>
            <button @click="removeItem(index)" class="bg-red-500 text-white px-3 py-1 rounded">
              Remove
            </button>
          </div>
        </li>
      </ul>
    </div>
    <div
      v-if="isModalOpen"
      @click.self="isModalOpen = false"
      class="fixed inset-0 flex items-center justify-center z-10 p-6 bg-black bg-opacity-50"
    >
      <div class="bg-white rounded p-8 shadow-lg">
        <h2 class="text-2xl font-semibold mb-4">{{ currentItem.title }}</h2>
        <p>{{ currentItem.description }}</p>
        <button
          @click="isModalOpen = false"
          class="bg-blue-500 text-white mt-6 px-4 py-2 rounded shadow-lg focus:outline-none"
        >
          Close
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      items: [
        {
          title: 'Buy milk',
          description: 'Purchase a gallon of milk from the grocery store.',
          edit: false
        },
        {
          title: 'Finish project',
          description: 'Complete work on the ongoing project before the deadline.',
          edit: false
        },
        {
          title: 'Call mom',
          description: 'Give mom a call and check in with her.',
          edit: false
        }
      ],
      newItem: {
        title: '',
        description: ''
      },
      currentItem: {
        title: '',
        description: ''
      },
      isModalOpen: false
    }
  },
  methods: {
    addItem() {
      if (this.newItem.title.trim() !== '') {
        this.items.push({
          title: this.newItem.title.trim(),
          description: this.newItem.description.trim(),
          edit: false
        })
        this.newItem.title = ''
        this.newItem.description = ''
      }
    },
    removeItem(index) {
      this.items.splice(index, 1)
    },
    toggleEdit(index) {
      this.items[index].edit = !this.items[index].edit
    },
    viewItem(item) {
      this.currentItem = item
      this.isModalOpen = true
    }
  }
}
</script>
