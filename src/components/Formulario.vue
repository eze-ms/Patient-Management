<script setup>
    import { reactive, defineProps, computed } from 'vue'
    import Alerta from './Alerta.vue';

    const alerta = reactive({
        tipo: '',
        mensaje: ''
    })

    const emit = defineEmits(['update:nombre', 'update:propietario', 'update:email','update:alta', 'update:sintomas', 'guardar-paciente'])

    const props = defineProps({

        id: {
            type: [String, null],
            required: true
        },
        nombre: {
            type: String,
            required: true
        },
        propietario: {
            type: String,
            required: true
        },
        email: {
            type: String,
            required: true
        },
        alta: {
            type: String,
            required: true
        },
        sintomas: {
            type: String,
            required: true
        }
    })

    const validar = () => {
        if (Object.values(props).includes('')) {
            alerta.mensaje = "El campo del nombre de la mascota es obligatorio"
            alerta.tipo = "error"
            return
        }

        emit('guardar-paciente')
        alerta.mensaje = "Paciente agregado correctamente"
        alerta.tipo = 'exito'

        /* Limpia el aviso */
        setTimeout(() => {
            Object.assign(alerta, {
                tipo: '',
                mensaje: ''
            })
        },4000)
    }

    const editando = computed (() => {
        return props.id
    })
</script>


<template>
    <div class="md:w-1/2">
       <h2 class="text-gray-700 font-extrabold text-2xl text-center">Seguimiento pacientes</h2>
       <p class="text-lg mt-5 text-center mb-10">
            Añadir pacientes y 
            <span class="text-blue-500 font-bold">administrar</span>
       </p> 

       <Alerta
            v-if="alerta.mensaje"
            :alerta="alerta"
       />

        <form 
            class="bg-white shadow-md rounded-lg py-10 px-5 mb-10"
            @submit.prevent="validar"
        >

            <div class="mb-5">

                <label
                  for="mascota"
                  class="block text-gray-700 uppercase font-bold"
                  >
                    Nombre Mascota
                </label>

                <input
                    id="mascota"
                    type="text"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    placeholder="Nombre de la mascota"
                    :value="nombre"
                    @input="$emit('update:nombre', $event.target.value )"
                />
            </div>

            <div class="mb-5">
                <label for="propietario" class="block text-gray-700 uppercase font-bold">
                    Nombre Propietario
                </label>

                <input
                    id="propietario"
                    type="text"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    placeholder="Nombre del propietario"
                    :value="propietario"
                    @input="$emit('update:propietario', $event.target.value )"
                    
                />
            </div>

            <div class="mb-5">
                <label for="email" class="block text-gray-700 uppercase font-bold">
                    Email
                </label>

                <input
                    id="email"
                    type="email"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    placeholder="Email"
                    :value="email"
                    @input="$emit('update:email', $event.target.value )"
                    
                />
            </div>

            <div class="mb-5">
                <label for="alta" class="block text-gray-700 uppercase font-bold">
                    Alta
                </label>

                <input
                    id="alta"
                    type="date"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    :value="alta"
                    @input="$emit('update:alta', $event.target.value )"
                />
            </div>

            <div class="mb-5">
                <label for="sintomas" class="block text-gray-700 uppercase font-bold">
                    Síntomas
                </label>

                <textarea
                    id="sintomas"
                    placeholder="Describe los síntomas"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md h-40"
                    :value="sintomas"
                    @input="$emit('update:sintomas', $event.target.value )"
                ></textarea>
            </div>

            <input 
                type="submit"
                class="bg-indigo-600 w-full p-3 text-white uppercase font-bold text-center hover:bg-indigo-700 cursor-pointer transition-colors"
                :value="[editando ? 'Guardar cambios' : 'Registrar Paciente']"
            />
       </form>
    </div>
</template>
