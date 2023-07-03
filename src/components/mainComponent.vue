<template>
  <div class="container">
    <h2>Lista de personas</h2>
    <ul class="list-group">
      <li v-for="persona in personas" :key="persona" class="list-group-item">
        {{ persona }}
        <button class="btn-editar" @click="editarPersona(persona)">Editar</button>
        <button class="btn-eliminar" @click="eliminarPersona(persona)">Eliminar</button>
      </li>
    </ul>
  
    <h2>Agregar Persona</h2>
    <input type="text" v-model="nuevoPersona">
    <button @click="agregarPersona" :disabled="nuevoPersona === ''">Agregar</button>
  
    <h2>Editar Persona</h2>
    <input type="text" v-model="nuevoPersona" v-if="personaEditado" class="input-editar">
    <button @click="guardarEdicion" v-if="personaEditado && nuevoPersona !== ''" class="btn-guardar">Guardar</button>
    <button @click="cancelarEdicion" v-if="personaEditado" class="btn-cancelar">Cancelar</button>
  </div>
</template>
  

<script>
  export default {
    data() {
      return {
        personas: [], 
        nuevoPersona: '', 
        personaEditado: null 
      };
    },

    methods: {
    
      agregarPersona() {
        if (this.nuevoPersona !== '' && this.personaEditado === null) {
          const nuevaPersona = {
            id: Date.now(),
            nombre: this.nuevoPersona
          };

          this.personas.push(nuevaPersona);
          this.nuevoPersona = '';

          this.guardarDatos();
        }
      },

    
      editarPersona(persona) {
        this.personaEditado = persona;
        this.nuevoPersona = persona.nombre;
      },

      guardarEdicion() {
        const personaIndex = this.personas.findIndex((persona) => persona.id === this.personaEditado.id );

        if (personaIndex !== -1) {
          this.personas[personaIndex].nombre = this.nuevoPersona;
          this.cancelarEdicion();

          this.guardarDatos(); 
        }
      },

    
      cancelarEdicion() {
        this.personaEditado = null;
        this.nuevoPersona = '';
      },

   
      eliminarPersona(persona) {
        const personaIndex = this.personas.findIndex((item) => item.id === persona.id);

        if (personaIndex !== -1) {
          this.personas.splice(personaIndex, 1);
          this.guardarDatos(); 
        }
      },

      guardarDatos() {
        localStorage.setItem('personas', JSON.stringify(this.personas));
      },

      recuperarDatos() {
        const personas = localStorage.getItem('personas');

        if (personas) {
          this.personas = JSON.parse(personas);
        }
      },
     
      created() {
        this.recuperarDatos()
      }
    }
  };
</script>



