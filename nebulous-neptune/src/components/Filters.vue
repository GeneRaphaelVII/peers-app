<template>
  <div class="filter-bar">
    <button class="filter-btn active">ALL</button>
    <button class="filter-btn">MON</button>
    <button class="filter-btn">TUE</button>
    <button class="filter-btn">WED</button>
    <button class="filter-btn">THU</button>
    <button class="filter-btn">FRI</button>
    <button class="filter-btn">SAT</button>
    <button class="filter-btn">SUN</button>
  </div>
</template>

<script setup>
</script>

<style scoped>
.filter-bar {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  margin: 1rem 0;
}

.filter-btn {
  padding: 0.4rem 0.8rem;
  border: 1px solid #ccc;
  border-radius: 4px;
  background: #fff;
  font-family: OpenSans;
  font-size: 20px;
  font-weight: bold;
  font-stretch: normal;
  font-style: normal;
  line-height: 1.5;
  letter-spacing: -0.1px;
  color: #0070f3; 
  cursor: pointer;
}

.filter-btn.active {
  background: #0070f3;
  color: white;
  border-color: #0070f3;
  font-family: OpenSans;
  font-size: 20px;
  font-weight: bold;
  font-stretch: normal;
  font-style: normal;
  line-height: 1.5;
  letter-spacing: -0.1px;
}
</style>
