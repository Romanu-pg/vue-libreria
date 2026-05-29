<script>
import axios from 'axios';
import { ref } from 'vue';
import { useRouter } from 'vue-router';

export default {
    name: 'NuevoLibroView',
    setup() {
        const router = useRouter();
        const libro = ref({
            titulo: '',
            isbn: '',
            genero: '',
            precio: '',
            disponibilidad: 'En stock'
        });

        const guardarLibro = async () => {
            try {
                if (!libro.value.titulo || !libro.value.isbn || !libro.value.genero || !libro.value.precio) {
                    alert('Por favor completa todos los campos');
                    return;
                }

                await axios.post('http://localhost:3000/libros', libro.value);
                alert('Libro guardado correctamente');
                router.push('/libros');
            } catch (error) {
                console.error('Error al guardar el libro:', error);
                alert('Error al guardar el libro');
            }
        };

        return {
            libro,
            guardarLibro
        };
    }
};
</script>

<template>
    <main>
        <h1>Agregar Nuevo Libro</h1>
        <form @submit.prevent="guardarLibro">
            <div>
                <label>Título:</label>
                <input v-model="libro.titulo" type="text" required />
            </div>
            <div>
                <label>ISBN:</label>
                <input v-model="libro.isbn" type="text" required />
            </div>
            <div>
                <label>Género:</label>
                <input v-model="libro.genero" type="text" required />
            </div>
            <div>
                <label>Precio:</label>
                <input v-model="libro.precio" type="number" step="0.01" required />
            </div>
            <div>
                <label>Disponibilidad:</label>
                <select v-model="libro.disponibilidad">
                    <option>En stock</option>
                    <option>Agotado</option>
                </select>
            </div>
            <button type="submit">Guardar Libro</button>
            <button type="button" @click="$router.push('/libros')">Cancelar</button>
        </form>
    </main>
</template>

<style scoped>
form {
    max-width: 500px;
    margin: 20px auto;
}

div {
    margin-bottom: 15px;
    display: flex;
    flex-direction: column;
}

label {
    font-weight: bold;
    margin-bottom: 5px;
}

input, select {
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 4px;
}

button {
    padding: 10px 15px;
    margin-right: 10px;
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}

button:hover {
    background-color: #0056b3;
}
</style>
