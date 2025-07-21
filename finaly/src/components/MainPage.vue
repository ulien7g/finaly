<template>
  <div class="background">
    <header class="header">
      <div class="container">
        <h1 class="logo">Meditation Life</h1>
        <nav class="nav">
          <a href="#about">Про нас</a>
          <a href="#contacts">Контакти</a>
          <a href="#login">Вхід</a>
        </nav>
      </div>
    </header>

    <div class="content">
      <h1>Онлайн Школа Медитацій</h1>

      <section id="about" class="mission">
        <h2>Наша мета</h2>
        <p>
          Ми допомагаємо людям знайти внутрішній спокій, навчитися медитувати та
          покращити своє ментальне здоров'я.
        </p>
      </section>

      <section class="courses">
        <h2>Наші курси</h2>
        <div class="course-card" v-for="course in courses" :key="course.id">
          <h3>{{ course.title }}</h3>
          <p>{{ course.description }}</p>
          <p><strong>Ціна:</strong> {{ course.price }} грн</p>
          <button @click="addToCart(course)">Купити</button>
        </div>
      </section>

      <section v-if="cart.length > 0" class="cart">
        <h2>Корзина</h2>
        <ul>
          <li v-for="(item, index) in cart" :key="index">
            {{ item.title }} - {{ item.price }} грн
          </li>
        </ul>
        <p><strong>Всього:</strong> {{ total }} грн</p>
        <button @click="clearCart">Очистити корзину</button>
      </section>

      <section id="contacts" class="contacts">
        <h2>Контакти</h2>
        <p>Email: support@meditation-life.com</p>
        <p>Телефон: +380 (66) 123-45-67</p>
      </section>
      <section id="login" class="login">
        <h2>Вхід</h2>
        <p>Увійдіть, щоб отримати доступ до куплених курсів.</p>
        <button class="open-btn" @click="openModal('login')">Увійти</button>

        <div v-if="showModal" class="modal-overlay" @click.self="closeModal">
          <div class="modal">
            <h2>{{ isLogin ? "Вхід" : "Реєстрація" }}</h2>
            <form @submit.prevent="submitForm">
              <input
                type="text"
                v-model="username"
                placeholder="Ім'я користувача"
                required
              />
              <input
                type="password"
                v-model="password"
                placeholder="Пароль"
                required
              />
              <button type="submit">
                {{ isLogin ? "Увійти" : "Зареєструватись" }}
              </button>
            </form>

            <p class="switch" @click="toggleMode">
              {{
                isLogin
                  ? "Ще немає акаунту? Зареєструватись"
                  : "Вже маєте акаунт? Увійти"
              }}
            </p>
          </div>
        </div>
      </section>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";

const courses = [
  {
    id: 1,
    title: "Медитація для початківців",
    description: "Основи спокою та усвідомлення.",
    price: 499,
  },
  {
    id: 2,
    title: "Антистрес-медитації",
    description: "Допомога у подоланні тривожності.",
    price: 599,
  },
  {
    id: 3,
    title: "Практики для сну",
    description: "Глибокий релакс перед сном.",
    price: 699,
  },
];

const cart = ref([]);

function addToCart(course) {
  cart.value.push(course);
}

function clearCart() {
  cart.value = [];
}
const showModal = ref(false);
const isLogin = ref(true);
const username = ref("");
const password = ref("");

function openModal(mode) {
  isLogin.value = mode === "login";
  showModal.value = true;
}

function closeModal() {
  showModal.value = false;
  username.value = "";
  password.value = "";
}

function toggleMode() {
  isLogin.value = !isLogin.value;
}

function submitForm() {
  closeModal();
}

const total = computed(() =>
  cart.value.reduce((sum, item) => sum + item.price, 0)
);
</script>

<style scoped>
.background {
  background-image: url("@/assets/meditation-main-photo.png");
  background-size: cover;
  background-position: center;
  color: white;
  text-shadow: 0 1px 4px rgba(0, 0, 0, 0.8);
  min-height: 100vh;
}

.header {
  position: fixed;
  width: 100%;
  padding: 15px 40px;
  top: 0;
  left: 0;
  background: rgba(0, 0, 0, 0.6);
  backdrop-filter: blur(5px);
  display: flex;
  justify-content: center;
  z-index: 1000;
}

.container {
  max-width: 1100px;
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.logo {
  font-size: 1.6rem;
  font-weight: bold;
  color: #fff;
}

.nav {
  display: flex;
  gap: 30px;
}

.nav a {
  color: white;
  text-decoration: none;
  font-weight: 500;
  transition: 0.3s;
}

.nav a:hover {
  color: #90ee90;
  text-decoration: underline;
}

.content {
  background-color: rgba(0, 0, 0, 0.6);
  padding: 40px;
  border-radius: 20px;
  max-width: 700px;
  margin-left: 350px;
  margin-top: 120px;
}

h1 {
  margin-bottom: 20px;
  font-size: 2.5rem;
}

.mission,
.courses,
.contacts,
.login {
  margin: 40px 0;
}

.courses {
  display: grid;
  gap: 20px;
}

.course-card {
  background-color: rgba(255, 255, 255, 0.1);
  padding: 20px;
  border-radius: 12px;
}

button {
  margin-top: 10px;
  padding: 8px 16px;
  background-color: #4caf50;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
}
button:disabled {
  background-color: #aaa;
  cursor: default;
}

.open-btn {
  margin-top: 20px;
}

.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 2000;
}

.modal {
  background-color: #fff;
  color: #333;
  padding: 30px;
  border-radius: 10px;
  width: 300px;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.3);
  text-align: center;
}

.modal h2 {
  margin-bottom: 20px;
}

.modal input {
  width: 100%;
  padding: 10px;
  margin-bottom: 15px;
  border: 1px solid #ccc;
  border-radius: 6px;
}

.modal button {
  width: 100%;
  padding: 10px;
  background-color: #4caf50;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
}

.switch {
  margin-top: 15px;
  color: #4caf50;
  cursor: pointer;
}
.cart {
  margin: 40px 0;
  padding: 20px;
  background-color: rgba(255, 255, 255, 0.15);
  border-radius: 12px;
}

.cart ul {
  list-style: none;
  padding: 0;
}

.cart li {
  margin-bottom: 10px;
}
</style>