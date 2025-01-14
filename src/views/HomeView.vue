<script setup>
import axios from "axios";
import { ref } from "vue";

const data = ref([]); 

const title= ref('');
const auther= ref('');
const edittitle= ref('');
const editauther= ref('');

async function getData() {
  try {
    const response = await axios.get("http://localhost:5000/api/books");
    data.value = response.data;
    console.log(response.data);
    
  } catch (error) {
    console.error("Xatolik yuz berdi:", error);
  }
}

getData();

async function addData() {
  if (title.value && auther.value) {
    const newBook = { title: title.value, author: auther.value };
    try {
      await axios.post("http://localhost:5000/api/books", newBook);
      getData(); // yangi ma'lumotlarni olish uchun
      title.value = ''; // inputni tozalash
      auther.value = ''; // inputni tozalash
    } catch (error) {
      console.error("Xatolik yuz berdi:", error);
    }
  }
}
async function deleteData(id) {
  try {
    await axios.delete(`http://localhost:5000/api/books/${id}`);
    getData(); 
  } catch (error) {
    console.error("Xatolik yuz berdi:", error);
  }
}
async function getIDData(id) {
  try {
    const res= await axios.get(`http://localhost:5000/api/books/${id}`);
    console.log(res.data);
    edittitle.value=res.data.title;
    editauther.value=res.data.author;
  } catch (error) {
    console.error("Xatolik yuz berdi:", error);
  }
}

</script>

<template>
<section>
  <div class="container mx-auto my-0 p-3 flex flex-col gap-2 items-center justify-center">
     <div class="bg-white p-6 rounded-lg shadow-md w-full max-w-md">
    <h1 class="text-2xl font-bold text-center text-gray-800 mb-4">Books</h1>
    
    <div class="grid gap-2">
    <span class="grid grid-cols-1 md:grid-cols-2 gap-2">
      <input
        id="todo-input"
        type="text"
        v-model="title"
        placeholder="Title"
        class="flex-grow border border-gray-300 rounded-lg px-4 py-2 focus:outline-none focus:ring focus:ring-blue-300"
      />
      <input
        id="todo-input"
        type="text"
        placeholder="Author"
        v-model="auther"
        class="flex-grow border border-gray-300 rounded-lg px-4 py-2 focus:outline-none focus:ring focus:ring-blue-300"
      />
    </span>
      <button
      @click="addData"
        class="bg-blue-500 text-white px-4 py-2 rounded-lg hover:bg-blue-600"
      >
        Qo'shish
      </button>
    </div>
  </div>
    <div class="grid grid-cols-2 md:grid-cols-4 gap-2">
      <div v-for="(item,itemKey) in data" class='shadow-lg rounded-md flex flex-col'>
        <img src="https://cdn.culture.ru/images/a7d5c2ba-87af-51f8-9305-034fa71dd4a1" @click="getIDData(item.id)" alt="Book" class="rounded-t-md">
        <span class="p-2 rounded-md ">
        <h2 class="text-xl font-bold text-gray-800 mt-4">{{item.title}}</h2>
        <p class="text-gray-600">{{item.author }}</p>
        </span>
        <span class="p-2 rounded-md grid grid-cols-2 gap-2">
        <button type="" @click="deleteData(item.id)" class="bg-red-500 rounded-md text-white cursor-pointer transition-all duration-200 hover:scale-95">Delete</button>
        <button type="" class="bg-orange-500 rounded-md text-white cursor-pointer transition-all duration-200 hover:scale-95">Edit</button>
        </span>
      </div>
    </div>
  </div>
</section>
</template>