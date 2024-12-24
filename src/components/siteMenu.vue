<template>
  <nav class="menu">
    <ul class="menu-list">
      <li><router-link to="/">Главная</router-link></li>
      <li><router-link to="/about">О нас</router-link></li>
      <li><router-link to="/contact">Заказать</router-link></li>
    </ul>
  </nav>
</template>

<script>
export default {
  name: 'SiteMenu',
};
</script>

<style scoped>
/* Стили для menu */
.menu {
  display: flex;
  justify-content: center;
  align-items: center; /* Центрирует элементы по вертикали */
  background-color: #333; /* Темный фон для меню */
  width: 100%; /* Полная ширина страницы */
  height: 70px; /* Увеличенная высота полоски меню */
  top: 0; /* Расположение сверху */
  left: 0; /* Начало от левого края */
  z-index: 1000; /* Поверх других элементов */
}

.menu-list {
  list-style: none;
  display: flex;
  padding: 0;
  margin: 0;
}

.menu-list li {
  margin: 0 30px; /* Увеличиваем расстояние между пунктами меню */
  color:white;
}

.menu-list li a {
  color: white; /* Белый цвет текста */
  text-decoration: none;
  font-size: 18px; /* Размер шрифта */
  padding: 10px 20px; /* Отступы внутри ссылок */
  display: inline-block;
}

/* При наведении на ссылку */
.menu-list li a:hover {
  background-color: #444; /* Изменение фона при наведении */
  color: white;
  text-decoration: underline;
}
</style>
