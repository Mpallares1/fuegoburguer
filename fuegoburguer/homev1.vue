<template>
    <div class="font-sans bg-amber-50 min-h-screen">
      <!-- Navbar -->
      <nav class="bg-gradient-to-r from-amber-700 to-red-700 text-white p-4 sticky top-0 z-50">
        <div class="container mx-auto flex justify-between items-center">
          <a href="#" class="text-2xl font-bold flex items-center">
            <FlameIcon class="w-8 h-8 mr-2" />
            Fuego Burger
          </a>
          <div class="space-x-4 flex items-center">
            <a href="#menu" class="hover:text-amber-200 transition">Menú</a>
            <a href="#about" class="hover:text-amber-200 transition">Nosotros</a>
            <a href="#locations" class="hover:text-amber-200 transition">Locales</a>
            <button v-if="isLoggedIn" @click="openUserSettings" class="hover:text-amber-200 transition">
              <UserIcon class="w-6 h-6" />
            </button>
            <button v-else @click="openLoginModal" class="hover:text-amber-200 transition">
              Iniciar Sesión
            </button>
            <button @click="toggleCart" class="relative">
              <ShoppingCartIcon class="w-6 h-6" />
              <span v-if="cartItems.length" class="absolute -top-2 -right-2 bg-red-500 text-white rounded-full w-5 h-5 flex items-center justify-center text-xs">
                {{ cartItems.length }}
              </span>
            </button>
          </div>
        </div>
      </nav>
  
      <!-- Opening Hours Banner -->
      <div :class="['py-2 text-center text-white font-semibold', isOpen ? 'bg-green-600' : 'bg-red-600']">
        <span v-if="isOpen">El local se encuentra abierto</span>
        <span v-else>{{ closedMessage }}</span>
      </div>
  
      <!-- Hero Section -->
      <header class="relative h-screen bg-cover bg-center" style="background-image: url('https://images.unsplash.com/photo-1606755962773-d324e0a13086?ixlib=rb-1.2.1&auto=format&fit=crop&w=2000&q=80');">
        <div class="absolute inset-0 bg-black bg-opacity-60 flex items-center justify-center">
          <div class="text-center">
            <h1 class="text-6xl font-bold text-white mb-4 animate-fade-in-down">Fuego Burger</h1>
            <p class="text-2xl text-amber-300 mb-8 animate-fade-in-up">Sabor Ardiente, Calidad Suprema</p>
            <a href="#menu" class="bg-gradient-to-r from-amber-500 to-red-500 text-white px-8 py-3 rounded-full text-lg font-semibold hover:from-amber-600 hover:to-red-600 transition duration-300 animate-pulse">
              Ver Menú
            </a>
          </div>
        </div>
      </header>
  
      <!-- Featured Section -->
      <section class="bg-gradient-to-r from-amber-700 to-red-700 py-16 text-white">
        <div class="container mx-auto text-center">
          <h2 class="text-4xl font-bold mb-8">¿Por qué Fuego Burger?</h2>
          <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
            <div class="flex flex-col items-center">
              <BeefIcon class="w-16 h-16 mb-4" />
              <h3 class="text-xl font-semibold mb-2">Carne Premium</h3>
              <p>100% carne de res Angus, seleccionada diariamente</p>
            </div>
            <div class="flex flex-col items-center">
              <FlameIcon class="w-16 h-16 mb-4" />
              <h3 class="text-xl font-semibold mb-2">Cocción a la Parrilla</h3>
              <p>Sabor ahumado inigualable en cada bocado</p>
            </div>
            <div class="flex flex-col items-center">
              <LeafIcon class="w-16 h-16 mb-4" />
              <h3 class="text-xl font-semibold mb-2">Ingredientes Frescos</h3>
              <p>Vegetales y aderezos frescos de proveedores locales</p>
            </div>
          </div>
        </div>
      </section>
  
      <!-- Menu Section -->
      <section id="menu" class="container mx-auto py-16">
        <h2 class="text-4xl font-bold text-center mb-12 text-amber-900">Nuestro Menú Ardiente</h2>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
          <div v-for="burger in burgers" :key="burger.id" class="bg-white rounded-lg shadow-lg overflow-hidden transform hover:scale-105 transition duration-300">
            <img :src="burger.image" :alt="burger.name" class="w-full h-48 object-cover">
            <div class="p-6">
              <div class="flex justify-between items-center mb-2">
                <h3 class="text-xl font-semibold text-amber-800">{{ burger.name }}</h3>
                <span class="bg-amber-100 text-amber-800 px-2 py-1 rounded-full text-sm">{{ burger.heatLevel }}</span>
              </div>
              <p class="text-gray-600 mb-4">{{ burger.description }}</p>
              <div class="flex justify-between items-center">
                <span class="text-2xl font-bold text-amber-600">{{ burger.price.toFixed(2) }}€</span>
                <button @click="addToCart(burger)" class="bg-gradient-to-r from-amber-500 to-red-500 text-white px-4 py-2 rounded-full hover:from-amber-600 hover:to-red-600 transition">
                  Añadir al carrito
                </button>
              </div>
            </div>
          </div>
        </div>
      </section>
  
      <!-- About Section -->
      <section id="about" class="bg-amber-100 py-16">
        <div class="container mx-auto">
          <h2 class="text-4xl font-bold text-center mb-12 text-amber-900">Nuestra Historia</h2>
          <div class="flex flex-col md:flex-row items-center">
            <div class="md:w-1/2 mb-8 md:mb-0">
              <img src="https://images.unsplash.com/photo-1512152272829-e3139592d56f?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80" alt="Nuestro equipo" class="rounded-lg shadow-lg">
            </div>
            <div class="md:w-1/2 md:pl-12">
              <p class="text-lg text-gray-700 mb-6">
                Fuego Burger nació de la pasión por las hamburguesas perfectas y el arte de la parrilla. Desde 2010, nos hemos dedicado a crear las hamburguesas más sabrosas y jugosas, utilizando solo los mejores ingredientes y técnicas de cocción tradicionales.
              </p>
              <p class="text-lg text-gray-700">
                Nuestro compromiso con la calidad y el sabor nos ha convertido en un referente en la escena gastronómica local. En Fuego Burger, cada bocado es una explosión de sabor que te transportará al corazón de la parrilla.
              </p>
            </div>
          </div>
        </div>
      </section>
  
      <!-- Locations Section -->
      <section id="locations" class="container mx-auto py-16">
        <h2 class="text-4xl font-bold text-center mb-12 text-amber-900">Nuestros Locales</h2>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
          <div v-for="location in locations" :key="location.id" class="bg-white rounded-lg shadow-lg overflow-hidden">
            <img :src="location.image" :alt="location.name" class="w-full h-48 object-cover">
            <div class="p-6">
              <h3 class="text-xl font-semibold mb-2 text-amber-800">{{ location.name }}</h3>
              <p class="text-gray-600 mb-4">{{ location.address }}</p>
              <p class="text-gray-600">{{ location.hours }}</p>
            </div>
          </div>
        </div>
      </section>
  
      <!-- Newsletter Section -->
      <section class="bg-gradient-to-r from-amber-700 to-red-700 py-16 text-white">
        <div class="container mx-auto text-center">
          <h2 class="text-3xl font-bold mb-4">¡Suscríbete a nuestro newsletter!</h2>
          <p class="mb-8">Recibe ofertas exclusivas y noticias sobre nuevos productos.</p>
          <form @submit.prevent="subscribeNewsletter" class="flex justify-center">
            <input type="email" v-model="newsletterEmail" placeholder="Tu email" class="px-4 py-2 rounded-l-full w-64 text-gray-800" required>
            <button type="submit" class="bg-amber-500 text-white px-6 py-2 rounded-r-full hover:bg-amber-600 transition">Suscribirse</button>
          </form>
        </div>
      </section>
  
      <!-- Cart Modal -->
      <div v-if="isCartOpen" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50">
        <div class="bg-white rounded-lg p-8 max-w-md w-full">
          <h2 class="text-2xl font-bold mb-4 text-amber-800">Tu Carrito</h2>
          <ul v-if="cartItems.length" class="mb-4">
            <li v-for="item in cartItems" :key="item.id" class="flex justify-between items-center mb-2">
              <div>
                <span class="font-semibold">{{ item.name }}</span>
                <span class="text-sm text-gray-600 ml-2">x {{ item.quantity }}</span>
              </div>
              <div class="flex items-center">
                <span class="mr-4">{{ (item.price * item.quantity).toFixed(2) }}€</span>
                <button @click="removeFromCart(item)" class="text-red-500 hover:text-red-700">
                  <TrashIcon class="w-5 h-5" />
                </button>
              </div>
            </li>
          </ul>
          <p v-else class="mb-4 text-gray-600">Tu carrito está vacío</p>
          <div class="flex justify-between items-center font-bold text-lg mb-4">
            <span>Total:</span>
            <span>{{ cartTotal.toFixed(2) }}€</span>
          </div>
          <div v-if="cartItems.length" class="mb-4">
            <label class="block mb-2">
              <input type="radio" v-model="deliveryOption" value="pickup" class="mr-2">
              Recoger en local
            </label>
            <label class="block">
              <input type="radio" v-model="deliveryOption" value="delivery" class="mr-2">
              Entrega a domicilio (+3€)
            </label>
          </div>
          <div class="flex justify-end space-x-4">
            <button @click="toggleCart" class="bg-gray-300 text-gray-800 px-4 py-2 rounded-full hover:bg-gray-400 transition">
              Cerrar
            </button>
            <button v-if="cartItems.length" @click="checkout" class="bg-gradient-to-r from-amber-500 to-red-500 text-white px-4 py-2 rounded-full hover:from-amber-600 hover:to-red-600 transition">
              Pagar
            </button>
          </div>
        </div>
      </div>
  
      <!-- Login Modal -->
      <div v-if="showLoginModal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50">
        <div class="bg-white rounded-lg p-8 max-w-md w-full">
          <h2 class="text-2xl font-bold mb-4 text-amber-800">{{ isRegistering ? 'Registro' : 'Iniciar Sesión' }}</h2>
          <form @submit.prevent="handleAuth">
            <div class="mb-4" v-if="isRegistering">
              <label for="name" class="block text-gray-700 mb-2">Nombre</label>
              <input type="text" id="name" v-model="authForm.name" required class="w-full px-3 py-2 border rounded-md">
            </div>
            <div class="mb-4">
              <label for="email" class="block text-gray-700 mb-2">Email</label>
              <input type="email" id="email" v-model="authForm.email" required class="w-full px-3 py-2 border rounded-md">
            </div>
            <div class="mb-4">
              <label for="password" class="block text-gray-700 mb-2">Contraseña</label>
              <input type="password" id="password" v-model="authForm.password" required class="w-full px-3 py-2 border rounded-md">
            </div>
            <div class="mb-4" v-if="isRegistering">
              <label for="address" class="block text-gray-700 mb-2">Dirección</label>
              <input type="text" id="address" v-model="authForm.address" required class="w-full px-3 py-2 border rounded-md">
            </div>
            <button type="submit" class="w-full bg-amber-500 text-white py-2 rounded-md hover:bg-amber-600 transition">
              {{ isRegistering ? 'Registrarse' : 'Iniciar Sesión' }}
            </button>
          </form>
          <p class="mt-4 text-center">
            {{ isRegistering ? '¿Ya tienes una cuenta?' : '¿No tienes una cuenta?' }}
            <button @click="toggleAuthMode" class="text-amber-500 hover:underline">
              {{ isRegistering ? 'Iniciar Sesión' : 'Registrarse' }}
            </button>
          </p>
        </div>
      </div>
  
      <!-- User Settings Modal -->
      <div v-if="showUserSettings" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50">
        <div class="bg-white rounded-lg p-8 max-w-md w-full">
          <h2 class="text-2xl font-bold mb-4 text-amber-800">Ajustes de Usuario</h2>
          <div class="mb-4">
            <h3 class="text-lg font-semibold mb-2">Información Personal</h3>
            <p><strong>Nombre:</strong> {{ user.name }}</p>
            <p><strong>Email:</strong> {{ user.email }}</p>
            <p><strong>Dirección:</strong> {{ user.address }}</p>
          </div>
          <div class="mb-4">
            <h3 class="text-lg font-semibold mb-2">Últimos Pedidos</h3>
            <ul v-if="user.orders.length">
              <li v-for="order in user.orders" :key="order.id" class="mb-2">
                <p><strong>Fecha:</strong> {{ new Date(order.date).toLocaleDateString() }}</p>
                <p><strong>Total:</strong> {{ order.total.toFixed(2) }}€</p>
              </li>
            </ul>
            <p v-else>No hay pedidos recientes</p>
          </div>
          <button @click="logout" class="w-full bg-red-500 text-white py-2 rounded-md hover:bg-red-600 transition">
            Cerrar Sesión
          </button>
          <button @click="closeUserSettings" class="w-full mt-2 bg-gray-300 text-gray-800 py-2 rounded-md hover:bg-gray-400 transition">
            Cerrar
          </button>
        </div>
      </div>
  
      <!-- Footer -->
      <footer class="bg-amber-900 text-white py-8">
        <div class="container mx-auto grid grid-cols-1 md:grid-cols-3 gap-8">
          <div>
            <h3 class="text-xl font-semibold mb-4">Fuego Burger</h3>
            <p>Sabor ardiente, calidad suprema desde 2010.</p>
          </div>
          <div>
            <h3 class="text-xl font-semibold mb-4">Enlaces Rápidos</h3>
            <ul>
              <li><a href="#menu" class="hover:text-amber-200">Menú</a></li>
              <li><a href="#about" class="hover:text-amber-200">Nosotros</a></li>
              <li><a href="#locations" class="hover:text-amber-200">Locales</a></li>
              <li><a href="#" class="hover:text-amber-200">Política de Privacidad</a></li>
            </ul>
          </div>
          <div>
            <h3 class="text-xl font-semibold mb-4">Síguenos</h3>
            <div class="flex space-x-4">
              <a href="#" class="hover:text-amber-200"><FacebookIcon class="w-6 h-6" /></a>
              <a href="#" class="hover:text-amber-200"><InstagramIcon class="w-6 h-6" /></a>
              <a href="#" class="hover:text-amber-200"><TwitterIcon class="w-6 h-6" /></a>
            </div>
          </div>
        </div>
        <div class="container mx-auto mt-8 pt-8 border-t border-amber-800 text-center">
          <p>&copy; 2023 Fuego Burger. Todos los derechos reservados.</p>
        </div>
      </footer>
    </div>
  </template>
  
  <script setup>
  import { ref, computed, onMounted } from 'vue'
  import { ShoppingCartIcon, FlameIcon, BeefIcon, LeafIcon, TrashIcon, FacebookIcon, InstagramIcon, TwitterIcon, UserIcon } from 'lucide-vue-next'
  
  const burgers = ref([
    { id: 1, name: 'Inferno Classic', price: 10.99, description: 'Carne Angus, queso cheddar, lechuga, tomate y nuestra salsa secreta Fuego.', image: 'https://images.unsplash.com/photo-1568901346375-23c9450c58cd?ixlib=rb-1.2.1&auto=format&fit=crop&w=500&q=60', heatLevel: 'Medio' },
    { id: 2, name: 'Explosión BBQ', price: 12.99, description: 'Carne Angus, bacon ahumado, aros de cebolla crujientes, queso y salsa BBQ picante.', image: 'https://images.unsplash.com/photo-1594212699903-ec8a3eca50f5?ixlib=rb-1.2.1&auto=format&fit=crop&w=500&q=60', heatLevel: 'Picante' },
    { id: 3, name: 'Veggie Volcánica', price: 11.99, description: 'Hamburguesa de frijoles negros y quinoa, guacamole, pico de gallo y mayonesa de chipotle.', image: 'https://images.unsplash.com/photo-1525059696034-4967a8e1dca2?ixlib=rb-1.2.1&auto=format&fit=crop&w=500&q=60', heatLevel: 'Suave' },
    { id: 4, name: 'Pollo Lava', price: 11.49, description: 'Pechuga de pollo a la parrilla, jalapeños, queso pepper jack y salsa ranch de habanero.', image: 'https://images.unsplash.com/photo-1615297928064-24977384d0da?ixlib=rb-1.2.1&auto=format&fit=crop&w=500&q=60', heatLevel: 'Muy Picante' },
    { id: 5, name: 'Mediterránea Ardiente', price: 13.49, description: 'Carne de cordero, queso feta, aceitunas kalamata, tomate seco y tzatziki picante.', image: 'https://images.unsplash.com/photo-1586190848861-99aa4a171e90?ixlib=rb-1.2.1&auto=format&fit=crop&w=500&q=60', heatLevel: 'Medio' },
    { id: 6, name: 'Torre Infernal', price: 15.99, description: 'Doble carne Angus, triple queso, bacon, huevo frito y salsa ghost pepper.', image: 'https://images.unsplash.com/photo-1582196016295-f8c8bd4b3a99?ixlib=rb-1.2.1&auto=format&fit=crop&w=500&q=60', heatLevel: 'Extremo' },
  ])
  
  const locations = ref([
    { id: 1, name: 'Fuego Burger Centro', address: 'Calle Mayor 123, Ciudad', hours: 'Lun-Dom: 12:00 - 22:00', image: 'https://images.unsplash.com/photo-1555396273-367ea4eb4db5?ixlib=rb-1.2.1&auto=format&fit=crop&w=500&q=60' },
    { id: 2, name: 'Fuego Burger Plaza', address: 'Av. Libertad 456, Ciudad', hours: 'Lun-Dom: 11:30 - 23:00', image: 'https://images.unsplash.com/photo-1466978913421-dad2ebd01d17?ixlib=rb-1.2.1&auto=format&fit=crop&w=500&q=60' },
    { id: 3, name: 'Fuego Burger Parque', address: 'Parque Central 789, Ciudad', hours: 'Lun-Dom: 12:00 - 21:00', image: 'https://images.unsplash.com/photo-1552566626-52f8b828add9?ixlib=rb-1.2.1&auto=format&fit=crop&w=500&q=60' },
  ])
  
  const cartItems = ref([])
  const isCartOpen = ref(false)
  const deliveryOption = ref('pickup')
  const newsletterEmail = ref('')
  const showLoginModal = ref(false)
  const isRegistering = ref(false)
  const authForm = ref({ name: '', email: '', password: '', address: '' })
  const isLoggedIn = ref(false)
  const user = ref(null)
  const showUserSettings = ref(false)
  
  const isOpen = ref(false)
  const closedMessage = ref('')
  
  const addToCart = (burger) => {
    const existingItem = cartItems.value.find(item => item.id === burger.id)
    if (existingItem) {
      existingItem.quantity++
    } else {
      cartItems.value.push({ ...burger, quantity: 1 })
    }
    animateAddToCart()
  }
  
  const animateAddToCart = () => {
    const cartIcon = document.querySelector('.cart-icon')
    const animation = cartIcon.animate([
      { transform: 'scale(1)' },
      { transform: 'scale(1.5)' },
      { transform: 'scale(1)' }
    ], {
      duration: 300,
      easing: 'ease-in-out'
    })
    animation.onfinish = () => {
      cartIcon.style.transform = 'scale(1)'
    }
  }
  
  const removeFromCart = (item) => {
    const index = cartItems.value.findIndex(cartItem => cartItem.id === item.id)
    if (index !== -1) {
      if (cartItems.value[index].quantity > 1) {
        cartItems.value[index].quantity--
      } else {
        cartItems.value.splice(index, 1)
      }
    }
  }
  
  const toggleCart = () => {
    isCartOpen.value = !isCartOpen.value
  }
  
  const cartTotal = computed(() => {
    const itemsTotal = cartItems.value.reduce((total, item) => total + item.price * item.quantity, 0)
    return deliveryOption.value === 'delivery' ? itemsTotal + 3 : itemsTotal
  })
  
  const checkout = () => {
    if (!isLoggedIn.value) {
      alert('Por favor, inicia sesión para realizar el pedido.')
      openLoginModal()
      return
    }
    
    const order = {
      id: Date.now(),
      date: new Date(),
      items: cartItems.value,
      total: cartTotal.value,
      deliveryOption: deliveryOption.value
    }
    
    user.value.orders.unshift(order)
    
    alert(`¡Gracias por tu pedido en Fuego Burger!\n\nTotal: ${cartTotal.value.toFixed(2)}€\nMétodo de entrega: ${deliveryOption.value === 'pickup' ? 'Recoger en local' : 'Entrega a domicilio'}`)
    cartItems.value = []
    isCartOpen.value = false
  }
  
  const subscribeNewsletter = () => {
    alert(`¡Gracias por suscribirte a nuestro newsletter, ${newsletterEmail.value}!`)
    newsletterEmail.value = ''
  }
  
  const openLoginModal = () => {
    showLoginModal.value = true
  }
  
  const toggleAuthMode = () => {
    isRegistering.value = !isRegistering.value
  }
  
  const handleAuth = () => {
    if (isRegistering.value) {
      // Simular registro
      user.value = {
        name: authForm.value.name,
        email: authForm.value.email,
        address: authForm.value.address,
        orders: []
      }
      isLoggedIn.value = true
      alert('Registro exitoso. Se ha enviado un correo de confirmación.')
    } else {
      // Simular inicio de sesión
      if (authForm.value.email === 'usuario@ejemplo.com' && authForm.value.password === 'contraseña') {
        user.value = {
          name: 'Usuario Ejemplo',
          email: 'usuario@ejemplo.com',
          address: 'Calle Ejemplo 123',
          orders: [
            { id: 1, date: new Date('2023-05-01'), total: 25.99 },
            { id: 2, date: new Date('2023-05-15'), total: 32.50 }
          ]
        }
        isLoggedIn.value = true
      } else {
        alert('Credenciales incorrectas')
        return
      }
    }
    showLoginModal.value = false
    resetAuthForm()
  }
  
  const resetAuthForm = () => {
    authForm.value = { name: '', email: '', password: '', address: '' }
  }
  
  const openUserSettings = () => {
    showUserSettings.value = true
  }
  
  const closeUserSettings = () => {
    showUserSettings.value = false
  }
  
  const logout = () => {
    isLoggedIn.value = false
    user.value = null
    showUserSettings.value = false
  }
  
  const checkOpeningHours = () => {
    const now = new Date()
    const day = now.getDay()
    const hour = now.getHours()
  
    if (day >= 1 && day <= 5) { // Lunes a Viernes
      isOpen.value = hour >= 10 && hour < 22
    } else { // Sábado y Domingo
      isOpen.value = hour >= 10 && hour < 24
    }
  
    if (!isOpen.value) {
      const nextOpenDay = day === 0 ? 1 : (day === 6 ? 1 : day + 1)
      const nextOpenDate = new Date(now)
      nextOpenDate.setDate(now.getDate() + (nextOpenDay - day + 7) % 7)
      nextOpenDate.setHours(10, 0, 0, 0)
      closedMessage.value = `Cerrado. Abrimos el ${nextOpenDate.toLocaleDateString('es-ES', { weekday: 'long' })} a las 10:00 AM`
    }
  }
  
  onMounted(() => {
    checkOpeningHours()
    setInterval(checkOpeningHours, 60000) // Actualizar cada minuto
  })
  
  </script>
  
  <style scoped>
  @keyframes fadeInDown {
    from {
      opacity: 0;
      transform: translateY(-20px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }
  
  @keyframes fadeInUp {
    from {
      opacity: 0;
      transform: translateY(20px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }
  
  .animate-fade-in-down {
    animation: fadeInDown 1s ease-out;
  }
  
  .animate-fade-in-up {
    animation: fadeInUp 1s ease-out;
  }
  
  .cart-icon {
    transition: transform 0.3s ease-in-out;
  }
  </style><