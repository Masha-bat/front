<template>
  <header class="header">
    <!-- Логотип по центру -->
    <img src="@/assets/logo.png" alt="Логотип" class="logo" />
  </header>
</template>

<script>
export default {
  name: 'SiteHeader',
};
</script>

<style scoped>
/* Стили для header */
.header {
  display: flex;
  justify-content: center;
  padding: 20px;
}

.logo {
  height: 50px; /* Настроить размер логотипа */
}
</style>
