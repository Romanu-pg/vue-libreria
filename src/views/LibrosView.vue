<script>
import axios from 'axios';
import { ref , onMounted} from 'vue';
import { useRouter } from 'vue-router';

export default {
    name: 'LibrosView',
    setup() {
        const router = useRouter();
        const libros = ref([]);

        const listarLibros = async () => {
            try {
                const response = await axios.get('http://localhost:3000/libros');
                libros.value = response.data;
            } catch (error) {
                console.error('Error al leer los libros desde el endpoint:', error);
            }
        };

        // Fetch libros when the component is mounted
        onMounted(() => {
            listarLibros();
        });

        const eliminarLibro = async (id, titulo) => {
            const confirmDelete = window.confirm(`¿Estás seguro de que deseas eliminar el libro "${titulo}"?`);
            if (confirmDelete) {
                try {
                    await axios.delete(`http://localhost:3000/libros/${id}`);
                    // Refresh the list of libros after deletion
                    listarLibros();
                } catch (error) {
                    console.error('Error al eliminar el libro:', error);
                }
            }
        };

        const editarLibro = (id) => {
            router.push(`/editarLibro/${id}`);
        };

        return {
            libros,
            listarLibros,
            eliminarLibro,
            editarLibro,
        };
    },
};

</script>

<template>
    <main>
        <table>
            <thead>
                <tr>
                    <th>id</th>
                    <th>Titulos</th>
                    <th>ISBN</th>
                    <th>Genero</th>
                    <th>Precio</th>
                    <th>Disponibilidad</th>
                    <th>Acciones</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="libro in libros" :key="libro.id">
                    <td>{{ libro.id }}</td>
                    <td>{{ libro.titulo }}</td>
                    <td>{{ libro.isbn }}</td>
                    <td>{{ libro.genero }}</td>
                    <td>{{ libro.precio }}</td>
                    <td>{{ libro.disponibilidad }}</td>
                    <td class="acciones">
                        <button class="btn-eliminar" @click="eliminarLibro(libro.id,libro.titulo)">Eliminar</button>
                        <button class="btn-editar" @click="editarLibro(libro.id)">Editar</button>
                    </td> 
                </tr>
            </tbody>
        </table>
    </main>
</template>

<style scoped>
main {
    padding: 20px;
}

table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 20px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

th {
    background-color: #d3d3d3;
    padding: 12px;
    text-align: left;
    font-weight: bold;
    border: 1px solid #999;
}

td {
    padding: 12px;
    border: 1px solid #ddd;
}

tr:hover {
    background-color: #f5f5f5;
}

.acciones {
    display: flex;
    gap: 10px;
    justify-content: center;
}

button {
    padding: 8px 16px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    font-weight: bold;
    font-size: 14px;
    transition: all 0.3s ease;
}

.btn-eliminar {
    background-color: #dc3545;
    color: white;
}

.btn-eliminar:hover {
    background-color: #c82333;
    transform: scale(1.05);
}

.btn-editar {
    background-color: #ffc107;
    color: #333;
}

.btn-editar:hover {
    background-color: #ffb300;
    transform: scale(1.05);
}
</style>