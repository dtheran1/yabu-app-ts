<template >
  <div
    class="md:w-full  xs:border-gray-100 xs:border-2  lg:border-none rounded-xl lg:rounded-none grid grid-cols-1 md:grid-cols-2 bg-gradient-to-b from-primary to-secondary min-h-screen">
    <div class="md:bg-black pt-6">
      <div class="pl-[34px] pt-5 pb-6 hidden lg:flex">
        <img src="../assets/login/image-1.png" alt="logo-page">
        <span class="font-extrabold text-white self-center pl-3">anime<span class="text-primary">yabu.</span></span>

      </div>
      <img src="../assets/login/arrowLeft.svg" class="pl-[34px]" alt="arrow-left">
      <div class="flex justify-center w-full">
        <div class="-mt-5 text-white">
          <img src="../assets/login/avatar-login.png" alt="avatar-login">
          <div class="flex flex-col justify-center text-center">
            <span class="font-bold text-4xl">ひさしぶり!</span>
            <p class="italic text-base tracking-wider">¡Hola, te extrañamos!</p>
          </div>
        </div>
      </div>
      <div class="w-[343px] mx-auto px-4 mt-4 text-white">
        <form class="mt-1" @submit.prevent="login">
          <div>
            <p>Email</p>
            <div class="mt-1 w-80 rounded-lg h-14 border-2 border-white bg-grayCustom text-white placeholder-white">
              <input required type="email" class="h-full pl-4 w-full bg-transparent placeholder-white focus:outline-none"
                placeholder="usuario@" v-model="user.email">
            </div>
            <div v-if="!user.email.length" class="flex justify-end mt-1">
              <span class="text-redCustom text-sm">Email Inválido</span>
            </div>
          </div>
          <div class="mt-4">
            <p>Contraseña</p>
            <div
              class="mt-1 w-80 rounded-lg h-14 border-2 border-white bg-grayCustom text-white px-4 placeholder-white flex justify-between">
              <input required :type="inputType" class="h-full bg-transparent placeholder-white focus:outline-none"
                placeholder="• • • • • • •" v-model="user.password">
                <img src="../assets/login/ojito.svg" alt="showPassword" @click.prevent="toggleInput" class="w-5 cursor-pointer">
            </div>
            <a href="#" target="_blank" class="hover:underline text-pinkCustom flex justify-end mt-1">
              <span class=" text-sm">¿Olvido la contraseña?</span>
            </a>
            <Btn type="submit">
              Acceder
            </Btn>
          </div>
        </form>

        <div class="flex justify-between mt-10">
          <p class="h-1 bg-white rounded-full w-14" />
          <div>
            <p class="block md:hidden -mt-2 flex-1">Logueate con RRSS</p>
            <p class="hidden md:block -mt-2 text-sm">Regístrate también con</p>
          </div>
          <p class="h-1 bg-white w-14 rounded-full"></p>
        </div>

        <div class="flex justify-evenly mt-5">
          <img src="../assets/login/logoInstagram.svg" alt="logoInstagram">
          <img src="../assets/login/logoTwitter.svg" alt="logoTwitter">
          <img src="../assets/login/logoFacebook.svg" alt="logoFacebook">
        </div>

        <div class="mt-[41px] mb-10 flex justify-center">
          <router-link :to="{ name: 'Register' }">
            <span class="text-sm text-white">Quieres registrarte? <span class="text-pinkCustom">Registrate</span></span>
          </router-link>
        </div>
      </div>
    </div>

    <div class="hidden md:block bg-complement">
      <Carousel :navEnabled="false" :pagination="true" :startAutoPlay="true" :timeout="5000"
        class="carousel relative max-h-full h-full" v-slot="{ currentSlide }">
        <Slide v-for="(slide, index) in carouselSlides" :key="index">
          <div v-show="currentSlide === index + 1" class="absolute m-auto left-0 right-0 ">
            <div class="w-[550px] h-[550px] mt-20 m-auto left-0 right-0 z-10 bg-primary rounded-full">
              <img :src="slide.slide" alt="slider" class="h-[598px] absolute m-auto left-0 right-0 w-96 pt-10 lg:w-[419px]" />
            </div>
            <div class="mx-auto mt-20 z-50">
              <div class="flex flex-col justify-center items-center">
                <span class="font-bold text-6xl text-white text-center"> anime<span class="text-primary">{{ slide.title
                }}</span></span>
                <p class="text-white text-sm text-center w-[400px] mt-2"> {{ slide.description }} </p>
              </div>
            </div>
          </div>
        </Slide>
      </Carousel>
    </div>
  </div>
</template>
<script lang="ts">
import { defineComponent, reactive, onMounted, computed, ref } from 'vue'
import Carousel from '../components/Carousel.vue'
import Slide from '../components/SliderComponent.vue'
import { carouselSlides } from '../util/index'

import Btn from '../components/Btn.vue'
import { UserLogin } from '../model/user'
import { useStore } from 'vuex'
import router from '../router/index'

export default defineComponent({
  name: 'login',
  components: {
    Carousel,
    Slide,
    Btn
  },
  setup() {
    const store = useStore()
    const isLogued = computed(() => store.state.isLogued)

    const inputType = ref('password')
    const toggleInput = () => inputType.value === 'password' ? inputType.value = 'text' : inputType.value = 'password'

    const user = reactive<UserLogin>({
      email: '',
      password: '',
      apiKey: '',
      utcTimeStamp: '',
      signature: '',
    })

    const login = () => {
      store.commit('login', user)
      router.push('/backoffice')
    }

    onMounted(() => {
      store.commit('initializeStore')
      isLogued.value && router.push('/backoffice')
    })

    return {
      carouselSlides,
      user,
      login,
      inputType,
      toggleInput
    }
  }

})
</script>
<style scoped></style>