<template>
    <div class="d-flex align-center justify-center" style="height: 100vh; border-color: aqua;">
      <v-sheet width="400" class="mx-auto">
        <h1 style="display: flex; margin-bottom: 15px;"> App de Tareas</h1>
        <v-form fast-fail @submit.prevent="login">
          <!-- Usamos una clase condicional para aplicar el estilo rojo si la dirección de correo es inválida -->
          <v-text-field
            v-model="email"
            type="email"
            label="Correo electrónico"
            required
            :class="{'red--text': !isValidEmail && email !== ''}"
          ></v-text-field>
          <v-text-field v-model="password" :type="showPassword ? 'text' : 'password'" label="Contraseña" required></v-text-field>
          <v-checkbox v-model="showPassword" label="Mostrar contraseña"></v-checkbox>
          <a href="#" class="text-body-2 font-weight-regular">Olvidaste la contraseña?</a>
          <v-btn type="submit" color="primary" block class="mt-2">Ingresar</v-btn>
        </v-form>
        <div class="mt-2">
          <p class="text-body-2">No tienes una cuenta? <a href="#">Regístrate aquí</a></p>
        </div>
      </v-sheet>
    </div>
  </template>
  
  <script>
  import Swal from 'sweetalert2';
  
  const AuthService = {
    login(username, password) {
      if (username === 'usuario@gmail.com' && password === 'contraseña') {
        return { success: true, message: 'Inicio de sesión exitoso' };
      } else {
        return { success: false, message: 'Credenciales incorrectas' };
      }
    },
  };
  
  export default {
    data() {
      return {
        email: '',
        password: '',
        showPassword: false,
        isValidEmail: true,
      };
    },
    methods: {
      login() {
        if (!this.email.match(/^\S+@\S+\.\S+$/)) {
          this.isValidEmail = false;
          Swal.fire({
            icon: 'error',
            title: 'Error',
            text: 'Por favor, ingresa una dirección de correo electrónico válida.',
          });
          return;
        } else {
          this.isValidEmail = true;
        }
  
        const response = AuthService.login(this.email, this.password);
        if (response.success) {
          Swal.fire({
            icon: 'success',
            title: 'Éxito',
            text: response.message,
          });
          // Redirige al usuario o realiza otras acciones después del inicio de sesión exitoso
        } else {
          Swal.fire({
            icon: 'error',
            title: 'Error',
            text: response.message,
          });
        }
      },
    },
  };
  </script>
  
  