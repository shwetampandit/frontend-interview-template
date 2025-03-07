<template>
    <div class="pagination">
      <button @click="prevPage" :disabled="currentPage === 1">Prev</button>
      <span>Page {{ currentPage }} of {{ totalPages }}</span>
      <button @click="nextPage" :disabled="currentPage === totalPages">Next</button>
    </div>
  </template>
  
  <script setup>
  import { defineProps, defineEmits } from "vue";
  
  const props = defineProps({
    currentPage: Number,
    totalPages: Number,
  });
  const emit = defineEmits(["update:page"]);
  
  const prevPage = () => {
    if (props.currentPage > 1) emit("update:page", props.currentPage - 1);
  };
  
  const nextPage = () => {
    if (props.currentPage < props.totalPages) emit("update:page", props.currentPage + 1);
  };
  </script>
  
  <style scoped>
  .pagination {
    display: flex;
    justify-content: center;
    gap: 10px;
    margin-top: 20px;
  }
  button {
    padding: 5px 10px;
    cursor: pointer;
  }
  button:disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }
  </style>
  