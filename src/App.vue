<template>
  <div id="app">
    <div class="main-content" v-if="!isModalOpen && !isLoginModalOpen">
      <site-header />
      <site-menu />
      <div class="content">
        <div class="filters">
          <!-- Фильтр по категории -->
          <select v-model="selectedCategory" @change="filterDishes">
            <option value="">Все категории</option>
            <option value="супы">Супы</option>
            <option value="салаты">Салаты</option>
            <option value="пицца">Пицца</option>
          </select>

          <!-- Поиск по названию и описанию -->
          <input
            type="text"
            v-model="searchQuery"
            @input="filterDishes"
            placeholder="Поиск по названию и описанию"
          />
        </div>

        <div class="view-toggle">
          <button @click="toggleView">
            {{ isTableView ? "Показать карточки" : "Показать таблицу" }}
          </button>
        </div>

        <div class="sort-button">
          <!-- Кнопка сортировки -->
          <button @click="sortByPrice">
            Сортировать по цене: {{ isPriceAscending ? 'по возрастанию' : 'по убыванию' }}
          </button>
        </div>

        <div v-if="isTableView">
          <!-- Табличное представление -->
          <table>
            <thead>
              <tr>
                <th @click="sortBy('name')">Блюдо</th>
                <th @click="sortBy('price')">Цена</th>
                <th @click="sortBy('description')">Описание</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="dish in filteredDishes" :key="dish.id">
                <td>{{ dish.name }}</td>
                <td>{{ dish.price }} ₽</td>
                <td>{{ dish.description }}</td>
              </tr>
            </tbody>
          </table>
        </div>
        <div v-else>
          <!-- Карточное представление -->
          <div class="card-container">
            <div v-for="dish in filteredDishes" :key="dish.id" class="card">
              <h3>{{ dish.name }}</h3>
              <p><strong>Цена:</strong> {{ dish.price }} ₽</p>
              <p>{{ dish.description }}</p>
            </div>
          </div>
        </div>
      </div>
      <site-footer />
    </div>

    <!-- Первичное окно -->
    <div v-if="isModalOpen" class="modal">
      <div class="modal-content">
        <h2>Выберите роль</h2>
        <button @click="enterAsGuest">Гость</button>
        <button @click="showLogin">Сотрудник</button>
      </div>
    </div>

    <!-- Окно авторизации -->
    <div v-if="isLoginModalOpen" class="modal">
      <div class="modal-content">
        <h2>Авторизация</h2>
        <form @submit.prevent="handleLogin">
          <div>
            <label for="username">Логин:</label>
            <input type="text" id="username" v-model="username" required />
          </div>
          <div>
            <label for="password">Пароль:</label>
            <input type="password" id="password" v-model="password" required />
          </div>
          <button type="submit">Войти</button>
        </form>
        <button @click="closeLoginModal">Назад</button>
      </div>
    </div>
  </div>
</template>

<script>
import SiteHeader from './components/siteHeader.vue';
import SiteFooter from './components/siteFooter.vue';
import SiteMenu from './components/siteMenu.vue';

export default {
  components: {
    SiteHeader,
    SiteFooter,
    SiteMenu,
  },
  data() {
    return {
      isModalOpen: true, // Первичное окно
      isLoginModalOpen: false, // Окно авторизации
      username: '',
      password: '',
      isTableView: true, // Отображение: таблица или карточки
      isPriceAscending: true, // Направление сортировки по цене
      searchQuery: '', // Строка поиска
      selectedCategory: '', // Выбранная категория
      dishes: [
        { id: 1, name: "Борщ", price: 250, description: "Традиционный суп с мясом и свёклой.", category: "супы" },
        { id: 2, name: "Цезарь", price: 300, description: "Салат с курицей, сыром и сухариками.", category: "салаты" },
        { id: 3, name: "Пицца Маргарита", price: 450, description: "Пицца с томатным соусом и сыром.", category: "пицца" },
        { id: 4, name: "Суп-пюре из грибов", price: 200, description: "Нежный суп с грибами.", category: "супы" },
        { id: 5, name: "Пепперони", price: 500, description: "Пицца с колбасой пепперони.", category: "пицца" },
        { id: 6, name: "Оливье", price: 350, description: "Салат с картошкой, яйцом и колбасой.", category: "салаты" },
      ],
      sortKey: '', // Поле для сортировки
      sortDirection: 1, // Направление сортировки
    };
  },
  computed: {
    filteredDishes() {
      return this.dishes
        .filter((dish) => {
          const matchesCategory = this.selectedCategory
            ? dish.category === this.selectedCategory
            : true;
          const matchesSearchQuery =
            dish.name.toLowerCase().includes(this.searchQuery.toLowerCase()) ||
            dish.description.toLowerCase().includes(this.searchQuery.toLowerCase());
          return matchesCategory && matchesSearchQuery;
        })
        .sort((a, b) => {
          if (this.sortKey) {
            const aValue = a[this.sortKey];
            const bValue = b[this.sortKey];
            if (aValue < bValue) return -1 * this.sortDirection;
            if (aValue > bValue) return 1 * this.sortDirection;
            return 0;
          } else {
            // Сортировка по цене
            if (this.isPriceAscending) {
              return a.price - b.price;
            } else {
              return b.price - a.price;
            }
          }
        });
    },
  },
  methods: {
    enterAsGuest() {
      this.isModalOpen = false;
    },
    showLogin() {
      this.isModalOpen = false;
      this.isLoginModalOpen = true;
    },
    handleLogin() {
      const validUsername = 'admin';
      const validPassword = '1234';

      if (this.username === validUsername && this.password === validPassword) {
        alert('Добро пожаловать!');
        this.isLoginModalOpen = false;
      } else {
        alert('Неверный логин или пароль');
      }
    },
    closeLoginModal() {
      this.isModalOpen = true;
      this.isLoginModalOpen = false;
    },
    toggleView() {
      this.isTableView = !this.isTableView;
    },
    sortByPrice() {
      this.isPriceAscending = !this.isPriceAscending; // Переключение порядка сортировки
    },
    sortBy(key) {
      if (this.sortKey === key) {
        this.sortDirection *= -1; // Меняем направление сортировки
      } else {
        this.sortKey = key;
        this.sortDirection = 1; // Сортировка по возрастанию
      }
    },
    filterDishes() {
      // Фильтрация блюд уже происходит в вычисляемом свойстве filteredDishes
    },
  },
};
</script>

<style>
/* Основной стиль для страницы */
#app {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.main-content {
  display: flex;
  flex-direction: column;
  flex: 1;
}

.content {
  flex: 1;
  padding: 20px;
}

.filters {
  display: flex;
  gap: 10px;
  justify-content: space-between;
  margin-bottom: 20px;
}

.filters input,
.filters select {
  padding: 8px;
  font-size: 16px;
}

.view-toggle {
  text-align: center;
  margin-bottom: 20px;
}

.sort-button {
  text-align: center;
  margin-bottom: 20px;
}

table {
  width: 100%;
  border-collapse: collapse;
}

th, td {
  border: 1px solid #ccc;
  padding: 10px;
  text-align: left;
}

th {
  cursor: pointer;
  background-color: #f4f4f4;
}

.card-container {
  display: flex;
  gap: 20px;
  flex-wrap: wrap;
}

.card {
  border: 1px solid #ccc;
  padding: 20px;
  border-radius: 8px;
  width: calc(33.333% - 20px);
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.card h3 {
  margin-bottom: 10px;
}

/* Стили для модального окна */
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.modal-content {
  background: white;
  padding: 20px;
  border-radius: 8px;
  text-align: center;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
  max-width: 400px;
  width: 90%;
}

button {
  margin: 10px;
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
}

button:hover {
  background-color: #0056b3;
}

button:focus {
  outline: none;
  box-shadow: 0 0 5px rgba(0, 123, 255, 0.5);
}
</style>
