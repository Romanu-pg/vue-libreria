<script>
import axios from 'axios';
import { ref, onMounted } from 'vue';
import { useRouter, useRoute } from 'vue-router';

export default {
    name: 'EditarLibroView',
    setup() {
        const router = useRouter();
        const route = useRoute();
        const libro = ref({
            id: '',
            titulo: '',
            isbn: '',
            genero: '',
            precio: '',
            disponibilidad: 'En stock'
        });
        const loading = ref(true);

        const cargarLibro = async () => {
            try {
                const response = await axios.get(`http://localhost:3000/libros/${route.params.id}`);
                libro.value = response.data;
                loading.value = false;
            } catch (error) {
                console.error('Error al cargar el libro:', error);
                alert('Error al cargar el libro');
                router.push('/');
            }
        };

        const actualizarLibro = async () => {
            try {
                if (!libro.value.titulo || !libro.value.isbn || !libro.value.genero || !libro.value.precio) {
                    alert('Por favor completa todos los campos');
                    return;
                }

                await axios.put(`http://localhost:3000/libros/${libro.value.id}`, libro.value);
                alert('Libro actualizado correctamente');
                router.push('/');
            } catch (error) {
                console.error('Error al actualizar el libro:', error);
                alert('Error al actualizar el libro');
            }
        };

        onMounted(() => {
            cargarLibro();
        });

        return {
            libro,
            loading,
            actualizarLibro
        };
    }
};
</script>

<template>
    <main>
        <h1>Editar Libro</h1>
        <div v-if="loading" class="loading">Cargando...</div>
        <form v-else @submit.prevent="actualizarLibro">
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
            <button type="submit">Actualizar Libro</button>
            <button type="button" @click="$router.push('/')">Cancelar</button>
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

.loading {
    text-align: center;
    padding: 20px;
    font-size: 18px;
}
</style>
