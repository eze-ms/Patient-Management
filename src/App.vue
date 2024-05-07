<script setup>
    import { ref, reactive, onMounted, watch } from 'vue'
    import { uid } from 'uid'
    import Header from './components/Header.vue'
    import Formulario from './components/Formulario.vue'
    import Paciente from './components/Paciente.vue'

    // Define una referencia reactiva para almacenar la lista de pacientes
    const pacientes = ref([])

    // Define un objeto paciente reactivo con sus propiedades
    const paciente = reactive({
        id: null,
        nombre: '',
        propietario: '',
        email: '',
        alta: '',
        sintomas: ''
    })

    // Función para guardar en LocalStorage
    const guardarLocalStorage = () => {
        localStorage.setItem('pacientes', JSON.stringify(pacientes.value))
    }

    // Watch para detectar cambios en el state de pacientes y guardarlos en localStorage
    watch(pacientes, () => {
        guardarLocalStorage()
    }, {
        deep: true
    })

    // onMounted para leer los pacientes del localStorage al cargar el componente
    onMounted(() => {
        const pacientesStorage = localStorage.getItem('pacientes')
        if (pacientesStorage) {
            pacientes.value = JSON.parse(pacientesStorage)
        }
    })

    // Función para guardar un paciente
    const guardarPaciente = () => {
        // Si el paciente tiene un ID, actualiza el paciente existente
        if (paciente.id) {
            const { id } = paciente
            const i = pacientes.value.findIndex(pacienteState => pacienteState.id === id)
            pacientes.value[i] = { ...paciente }
        } else { 
            pacientes.value.push({
                ...paciente,
                id: uid() // Genera un id antes de almacenarlo
            })
        }

        // Reinicia el objeto paciente después de guardar
        Object.assign(paciente, {
            nombre: '',
            propietario: '',
            email: '',
            alta: '',
            sintomas: ''
        })
    }

    // Función para actualizar los datos de un paciente
    const actualizarPaciente = (id) => {
        // Encuentra al paciente con el ID proporcionado y crea nuevo array
        const pacienteEditar = pacientes.value.filter(paciente => paciente.id === id)[0]
        // Copia los datos del paciente encontrado al objeto paciente reactivo para editar
        Object.assign(paciente, pacienteEditar)
    }

    // Función para borrar los datos de un paciente
    const eliminarPaciente = (id) => {
         // Devuelve solo el índice del primer elemento que cumple con la condición SIN crear array
        const index = pacientes.value.findIndex(paciente => paciente.id === id);
            if (index !== -1) {
                pacientes.value.splice(index, 1); // Elimina el paciente del array original
            } 
    }
</script>

<template>
    <div class="container mx-auto mt-20">
        <Header/>

        <div class="mt-12 md:flex">
            <Formulario
                v-model:nombre="paciente.nombre"
                v-model:raza="paciente.raza"
                v-model:propietario="paciente.propietario"
                v-model:email="paciente.email"
                v-model:alta="paciente.alta"
                v-model:sintomas="paciente.sintomas"
                @guardar-paciente="guardarPaciente"
                :id="paciente.id"
            />
            
            <div class="md:w-1/2 md:h-screen overflow-y-scroll">
                <h3 class="text-gray-700 font-extrabold text-2xl text-center">Administrar pacientes</h3>

                <div v-if="pacientes.length > 0">
                    <p class="text-lg mt-5 text-center mb-10">
                            Información de 
                            <span class="text-blue-500 font-bold">pacientes</span>
                    </p> 
                    <Paciente
                        v-for="paciente in pacientes"
                        :key="paciente.id"
                        :paciente="paciente"
                        @actualizar-paciente="actualizarPaciente"
                        @eliminar-paciente="eliminarPaciente"
                    />
                </div>
                <p v-else class="mt-20 text-2xl text-center">No hay pacientes</p>
            </div>
        </div>
    </div>
</template>