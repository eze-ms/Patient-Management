<script setup>
    import { reactive, defineProps, computed } from 'vue'
    import Alerta from './Alerta.vue';

    const alerta = reactive({
        tipo: '',
        mensaje: ''
    })

    const emit = defineEmits(['update:nombre', 'update:raza', 'update:propietario', 'update:email','update:alta', 'update:sintomas', 'guardar-paciente'])

    const props = defineProps({

        id: {
            type: [String, null],
            required: true
        },
        nombre: {
            type: String,
            required: true
        },

        raza: {
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
            alerta.mensaje = "The pet's name field is required"
            alerta.tipo = "error"
            return
        }

        emit('guardar-paciente')
        alerta.mensaje = "Patient added successfully"
        alerta.tipo = 'exito'

        /* Limpia el aviso */
        setTimeout(() => {
            Object.assign(alerta, {
                tipo: '',
                mensaje: ''
            })
        },10000)
    }

    const editando = computed (() => {
        return props.id
    })
</script>


<template>
    <div class="md:w-1/2 bg-white shadow-md rounded-lg py-10 px-5 mb-10 mr-5 ml-5">
        <div>
            <h2 class="text-gray-700 font-extrabold text-2xl text-center">Patient Info</h2>
            <p class="text-gray-600 text-lg mt-5 text-center mb-10 pb-5 border-b border-gray-400">
                    Complete the following required fields to add and 
                    <span class="text-blue-500 font-bold">manage</span>
                    the patient
            </p> 
        </div>
       
       <Alerta
            v-if="alerta.mensaje"
            :alerta="alerta"
       />

        <form 
            
            @submit.prevent="validar"
        >

            <div class="mb-5">

                <label
                  for="mascota"
                  class="block text-gray-500 uppercase font-bold"
                  >
                    Pet's name
                </label>

                <input
                    id="mascota"
                    type="text"
                    class="bg-gray-100 ext-sm border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    placeholder="Name"
                    :value="nombre"
                    @input="$emit('update:nombre', $event.target.value )"
                />
            </div>

            <div class="mb-5">
                <label
                  for="raza"
                  class="block text-gray-500 uppercase font-bold"
                  >
                    Breed
                </label>

                <select
                    id="raza"
                    class="bg-gray-100 text-sm border-2 w-full p-2 mt-2 rounded-md"
                    :value="raza"
                    @change="$emit('update:raza', $event.target.value)"
                >
                    <option value="">Select a breed</option>
                    <option value="Dog">Dog</option>
                    <option value="Cat">Cat</option>
                    <option value="Rabbit">Rabbit</option>
                    <option value="Hamster">Hamster</option>
                    <option value="Turtle">Turtle</option>
                    <option value="Ferret">Ferret</option>
                    <option value="Bird">Bird</option>
                    <option value="Chicken">Chicken</option>
                </select>
            </div>

            <div class="mb-5">
                <label for="propietario" class="block text-gray-500 uppercase font-bold">
                    Owner's name
                </label>

                <input
                    id="propietario"
                    type="text"
                    class="bg-gray-100 text-sm border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    placeholder="Full name"
                    :value="propietario"
                    @input="$emit('update:propietario', $event.target.value )"
                    
                />
            </div>

            <div class="mb-5">
                <label for="email" class="block text-gray-500 uppercase font-bold">
                    Email
                </label>

                <input
                    id="email"
                    type="email"
                    class="bg-gray-100 text-sm border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    placeholder="youremail@email.com"
                    :value="email"
                    @input="$emit('update:email', $event.target.value )"
                    
                />
            </div>

            <div class="mb-5">
                <label for="alta" class="block text-gray-500 uppercase font-bold">
                    Admission date
                </label>

                <input
                    id="alta"
                    type="date"
                    class="bg-gray-100 text-sm border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    :value="alta"
                    @input="$emit('update:alta', $event.target.value )"
                />
            </div>

            <div class="mb-5">
                <label for="sintomas" class="block text-gray-500 uppercase font-bold">
                    Symptoms
                </label>

                <textarea
                    id="sintomas"
                    placeholder="Symptoms description..."
                    class="bg-gray-100 text-sm border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md h-40"
                    :value="sintomas"
                    @input="$emit('update:sintomas', $event.target.value )"
                ></textarea>
            </div>

            <input 
                type="submit"
                class="bg-blue-500 w-full p-3 text-white uppercase font-bold text-center hover:bg-blue-600 cursor-pointer transition-colors rounded-lg"
                :value="[editando ? 'Save' : 'Register']"
            />
       </form>
    </div>
</template>
