<script setup lang="ts">
import { ref, computed } from "vue";

interface Usuario {
    nombre: string;
    email: string;
    password: string;
    fechaNacimiento: string;
}

const usuario = ref<Usuario>({
    nombre: "",
    email: "",
    password: "",
    fechaNacimiento: "",
});

const calcularEdad = computed(() => {
    if (!usuario.value.fechaNacimiento) return "";
    const nacimiento = new Date(usuario.value.fechaNacimiento);
    const hoy = new Date();
    let edad = hoy.getFullYear() - nacimiento.getFullYear();
    if (hoy.getMonth() < nacimiento.getMonth() ||
        (hoy.getMonth() === nacimiento.getMonth() && hoy.getDate() < nacimiento.getDate())) {
        edad--;
    }
    return edad > 0 ? edad : "Fecha inválida";
});

const mostrarDatos = () => {
    if (!usuario.value.nombre || !usuario.value.email || !usuario.value.fechaNacimiento) {
        alert("Por favor, completa todos los campos.");
        return;
    }
    alert(`Usuario creado con éxito!\nNombre: ${usuario.value.nombre}\nEmail: ${usuario.value.email}\nEdad: ${calcularEdad.value} años`);
    usuario.value = { nombre: "", email: "", password: "", fechaNacimiento: "" };
};
</script>

<template>
    <div>
        <h2>Registro de Usuario</h2>
        <form @submit.prevent="mostrarDatos">
            <input v-model="usuario.nombre" type="text" placeholder="Nombre de usuario" required />
            <input v-model="usuario.email" type="email" placeholder="Correo electrónico" required />
            <input v-model="usuario.password" type="password" placeholder="Contraseña" required />
            <input v-model="usuario.fechaNacimiento" type="date" Required />
            <button type="submit">Registrar</button>
        </form>
        <div v-if="usuario.nombre || usuario.email || usuario.fechaNacimiento">
            <h3>Vista previa:</h3>
            <p>Nombre: {{ usuario.nombre }}</p>
            <p>Email: {{ usuario.email }}</p>
            <p v-if="usuario.fechaNacimiento">Edad: {{ calcularEdad }} años</p>
        </div>
    </div>
</template>

<style scoped>
div {
    background-color: #a1a3a5;
    border-radius: 12px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    padding: 2rem;
    margin-top: 2rem;
    max-width: 500px;
    margin-left: auto;
    margin-right: auto;
}

h2 {
    color: #4a4a4a;
    text-align: center;
    font-size: 1.875rem;
    margin-bottom: 1.5rem;
}
div.mt-6 {
    background-color: #f3f4f6;
    padding: 1rem;
    border-radius: 8px;
    border: 1px solid #e5e7eb;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

h3 {
    color: #1c1e20;
    font-size: 1.25rem;
    margin-bottom: 0.75rem;
}

p {
    color: #1c1e20;
    font-size: 1rem;
}

input {
    width: 100%;
    padding: 8px;
    border-radius: 8px;
    border: 2px solid #d1d5db;
    margin-bottom: 1rem;
    font-size: 1rem;
    background-color: #110303;
    transition: border-color 0.3s ease, box-shadow 0.3s ease;
}

button {
    border-radius: 6px;
    border: 1px solid transparent;
    padding: 0.6em 1.2em;
    font-size: 1em;
    font-weight: 500;
    font-family: inherit;
    background-color: #6e6c6c;
    cursor: pointer;
    transition: border-color 0.25s;
}

button:hover {
    border-color: #0d0d0e;
}

button:focus,
button:focus-visible {
    outline: 4px auto -webkit-focus-ring-color;
}
</style>
