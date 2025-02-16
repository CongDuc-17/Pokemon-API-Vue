<script setup>
import { ref, computed } from "vue";

import RenderPKM from "./components/renderPKM.vue";

const fetchAPI = async (url) => {
  const response = await fetch(url);
  const data = await response.json();
  return data;
}; //Hàm fetchAPI nhận một URL, thực hiện fetch dữ liệu từ API, chuyển đổi thành JSON và trả về dữ liệu.

let pokemons = [];
const offset = ref(0); //lưu số Pokémon đã được hiển thị.
const NUMBER_OF_RENDER = 6; //số Pokémon sẽ được tải thêm mỗi lần.
const filteredPokemons = ref([]); // danh sách Pokémon đã được lọc theo tìm kiếm.

const renderPokemons = computed(() =>
  filteredPokemons.value.slice(0, offset.value + NUMBER_OF_RENDER)
); //renderPokemons là một computed để lấy danh sách Pokémon hiện tại cần hiển thị dựa trên offset.

async function getPokemon() {
  const data = await fetchAPI("https://pokeapi.co/api/v2/pokemon/?offset=0&limit=898");
  pokemons = data.results;
  filteredPokemons.value = pokemons;
} //getPokemon gọi API lấy danh sách 898 Pokémon.
//Gán dữ liệu vào pokemons, sau đó cập nhật filteredPokemons để hiển thị danh sách đầy đủ ban đầu.
getPokemon();

function handleLoadmore() {
  offset.value += NUMBER_OF_RENDER;
} //Khi nhấn nút "Load More", offset tăng thêm NUMBER_OF_RENDER, giúp hiển thị thêm Pokémon.

function handleSearch(event) {
  filteredPokemons.value = pokemons.filter((pokemons) => {
    return pokemons.name.includes(event.target.value);
  });
  offset.value = 0;
} //Khi người dùng nhập vào ô tìm kiếm:
//filteredPokemons cập nhật theo kết quả tìm kiếm.
//offset được đặt về 0 để hiển thị lại từ đầu.
</script>

<template>
  <div class="container">
    <div class="title">
      <div class="title__main"><h2>Pokemon API</h2></div>
      <input
        class="title__input"
        type="text"
        placeholder="Search some Pokemon..."
        @input="handleSearch"
      />
    </div>

    <RenderPKM :pokemons="renderPokemons" />

    <button
      v-show="filteredPokemons.length > NUMBER_OF_RENDER"
      class="load-more"
      @click="handleLoadmore"
    >
      Load More
    </button>
  </div>
</template>
<style scoped>
#app {
  display: flex !important;
  justify-content: center;
}
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 100%;
}
.title {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.title__main {
  margin-block: 25px;
}
.title__input {
  width: 100%;
  padding: 20px;
  border: none;
  border-radius: 30px;
  outline: 1px solid #00000036;
  box-shadow: #64646f33 0 7px 29px;
  font-size: 16px;
  transition: all 0.2s ease;
  margin-bottom: 50px;
}
.load-more {
  margin-top: 50px;
  margin-bottom: 50px;
  cursor: pointer;
  padding: 20px 25px;
  border: none;
  border-radius: 10px;
  font-size: 16px;
  color: #fff;
  background-color: #ff4d4f;
}
</style>
