<template>
  <div class="container-fluid">
    <!-- Header -->
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
      <div class="container">
        <a class="navbar-brand" href="#">Learn</a>
        <div class="navbar-nav ms-auto">
          <a class="nav-link" href="#">Курсы</a>
          <a class="nav-link" href="#">О нас</a>
          <a class="nav-link" href="#">Контакты</a>
        </div>
      </div>
    </nav>

    <!-- Hero Section -->
    <div class="bg-light py-5 mb-5">
      <div class="container text-center">
        <h1 class="display-4 fw-bold mb-3">Освой новый навык с нуля</h1>
        <p class="lead text-muted mb-4">Практические курсы по различным предметам от опытных специалистов</p>
        <button class="btn btn-primary btn-lg" @click="showForm = !showForm">
          {{ showForm ? 'Закрыть' : 'Вопросы' }}
        </button>
      </div>
    </div>

    <!-- Форма добавления курса -->
    <div v-if="showForm" class="container mb-5">
      <div class="row justify-content-center">
        <div class="col-md-8">
          <div class="card">
            <div class="card-header bg-primary text-white">
              <h5 class="mb-0">Добавить новый курс</h5>
            </div>
            <div class="card-body">
              <form @submit.prevent="addCourse">
                <div class="mb-3">
                  <label class="form-label">Название курса</label>
                  <input v-model="newCourse.title" type="text" class="form-control" required placeholder="Например: Frontend с нуля">
                </div>
                <div class="mb-3">
                  <label class="form-label">Описание</label>
                  <textarea v-model="newCourse.description" class="form-control" rows="3" required placeholder="Опишите курс..."></textarea>
                </div>
                <div class="mb-3">
                  <label class="form-label">Технологии (через запятую)</label>
                  <input v-model="newCourse.technologies" type="text" class="form-control" placeholder="HTML, CSS, JavaScript, React">
                </div>
                <div class="mb-3">
                  <label class="form-label">Цена (рублей)</label>
                  <input v-model.number="newCourse.price" type="number" class="form-control" required placeholder="30000">
                </div>
                <div class="mb-3">
                  <label class="form-label">Длительность (месяцев)</label>
                  <input v-model.number="newCourse.duration" type="number" class="form-control" required placeholder="4">
                </div>
                <div class="mb-3">
                  <label class="form-label">URL изображения</label>
                  <input v-model="newCourse.image" type="url" class="form-control" placeholder="https://example.com/image.jpg">
                </div>
                <button type="submit" class="btn btn-success w-100">Добавить курс</button>
              </form>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Секция курсов -->
    <div class="container mb-5">
      <h2 class="text-center mb-4">Наши курсы</h2>
      
      <!-- Сортировка -->
      <div class="row mb-4">
        <div class="col-md-6 offset-md-3">
          <div class="input-group">
            <label class="input-group-text" for="sortSelect">Сортировать:</label>
            <select v-model="sortBy" id="sortSelect" class="form-select">
              <option value="default">По умолчанию</option>
              <option value="price-asc">Цена: по возрастанию</option>
              <option value="price-desc">Цена: по убыванию</option>
              <option value="duration-asc">Длительность: по возрастанию</option>
              <option value="duration-desc">Длительность: по убыванию</option>
              <option value="name">По названию</option>
            </select>
          </div>
        </div>
      </div>

      <!-- Карточки курсов -->
      <div class="row">
        <div v-for="course in sortedCourses" :key="course.id" class="col-md-4 mb-4">
          <div class="card h-100 shadow-sm">
            <img v-if="course.image" :src="course.image" class="card-img-top" :alt="course.title" style="height: 200px; object-fit: cover;">
            <div class="card-body">
              <h5 class="card-title">{{ course.title }}</h5>
              <p class="card-text text-muted">{{ course.description }}</p>
              <p class="card-text">
                <small class="text-primary">{{ course.technologies }}</small>
              </p>
              <div class="d-flex justify-content-between align-items-center mb-3">
                <span class="fw-bold">{{ course.price.toLocaleString() }} ₽</span>
                <span class="badge bg-secondary">{{ course.duration }} мес.</span>
              </div>
              <button @click="removeCourse(course.id)" class="btn btn-outline-danger btn-sm w-100">
                Удалить курс
              </button>
            </div>
          </div>
        </div>
      </div>

      <!-- Сообщение если курсов нет -->
      <div v-if="courses.length === 0" class="text-center text-muted py-5">
        <h4>Курсы пока не добавлены</h4>
        <p>Нажмите "Вопросы" чтобы добавить первый курс</p>
      </div>
    </div>

    <!-- Footer -->
    <footer class="bg-light py-4 mt-5">
      <div class="container text-center">
        <p class="mb-0">© 2026 Learn. Все права защищены.</p>
      </div>
    </footer>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const showForm = ref(false)
const sortBy = ref('default')

const courses = ref([
  {
    id: 1,
    title: 'Frontend с нуля',
    description: 'Изучите основы веб-разработки...',
    technologies: 'HTML, CSS, JavaScript, React',
    price: 30000,
    duration: 4,
    // Обратите внимание на расширение .webp
    image: '/im1.webp' 
  },
  {
    id: 2,
    title: 'Python для анализа данных',
    description: 'Освойте Python...',
    technologies: 'Pandas, NumPy, SQL',
    price: 60000,
    duration: 6,
    image: '/im2.webp'
  },
  {
    id: 3,
    title: 'Английский язык',
    description: 'Курс английского...',
    technologies: 'Грамматика, Разговорный',
    price: 50000,
    duration: 6,
    image: '/im3.webp'
  }
])

const newCourse = ref({
  title: '',
  description: '',
  technologies: '',
  price: 0,
  duration: 0,
  image: ''
})

const sortedCourses = computed(() => {
  const sorted = [...courses.value]
  
  switch(sortBy.value) {
    case 'price-asc':
      return sorted.sort((a, b) => a.price - b.price)
    case 'price-desc':
      return sorted.sort((a, b) => b.price - a.price)
    case 'duration-asc':
      return sorted.sort((a, b) => a.duration - b.duration)
    case 'duration-desc':
      return sorted.sort((a, b) => b.duration - a.duration)
    case 'name':
      return sorted.sort((a, b) => a.title.localeCompare(b.title))
    default:
      return sorted
  }
})

const addCourse = () => {
  if (!newCourse.value.title || !newCourse.value.price) {
    alert('Заполните обязательные поля!')
    return
  }
  
  courses.value.push({
    id: Date.now(),
    ...newCourse.value
  })
  
  newCourse.value = {
    title: '',
    description: '',
    technologies: '',
    price: 0,
    duration: 0,
    image: ''
  }
  
  showForm.value = false
  alert('Курс успешно добавлен!')
}

const removeCourse = (id) => {
  if (confirm('Вы уверены, что хотите удалить этот курс?')) {
    courses.value = courses.value.filter(course => course.id !== id)
  }
}
</script>

<style scoped>
.card {
  transition: transform 0.2s;
}

.card:hover {
  transform: translateY(-5px);
}
</style>