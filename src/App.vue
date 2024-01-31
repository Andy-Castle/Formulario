<script setup lang="ts">
import { ref } from 'vue'
import type { Ref } from 'vue'

const name: Ref<string> = ref('')
const lastName: Ref<string> = ref('')
const age: Ref<number> = ref(0)
const gender: Ref<string> = ref('')
const customGender: Ref<string> = ref('')
const errors: Ref<string[]> = ref([])

const validateForm = () => {
  errors.value = []

  if (name.value.length < 5 || name.value.length > 18) {
    errors.value.push('Nombre debe de tener entre 5 y 18 caracteres.')
  }

  if (lastName.value === name.value) {
    errors.value.push('El apellido no puede ser el mismo que el nombre.')
  }
  if (age.value <= 0 || age.value >= 60) {
    errors.value.push('La edad debe ser mayor a 0 pero menor que 60.')
  }

  if (!['Masculino', 'Femenino', 'Otro'].includes(gender.value)) {
    errors.value.push('Sexo invalido')
  } else if (gender.value === 'Otro' && customGender.value === '') {
    errors.value.push('Especificar otro sexo')
  }
}
</script>

<template>
  <main>
    <div class="formulario">
      <div>
        <label>Nombre:</label>
        <input @input="validateForm()" v-model="name" type="text" />
      </div>

      <div>
        <label>Apellido:</label>
        <input @input="validateForm()" v-model="lastName" type="text" />
      </div>

      <div>
        <label>Edad:</label>
        <input @input="validateForm()" v-model.number="age" type="number" />
      </div>

      <div>
        <label>Sexo:</label>
        <select @change="validateForm()" v-model="gender">
          <option value="Masculino">Masculino</option>
          <option value="Femenino">Femenino</option>
          <option value="Otro">Otro</option>
        </select>

        <input
          v-if="gender === 'Otro'"
          @input="validateForm()"
          v-model="customGender"
          type="text"
          placeholder="Specify custom gender"
        />
      </div>
      <br />
      <h3>Errores:</h3>
      <span v-for="(err, index) in errors" :key="index" class="error">
        {{ err }}
        <br />
      </span>
    </div>
  </main>
</template>

<style scoped>
* {
  font-family:
    system-ui,
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    Oxygen,
    Ubuntu,
    Cantarell,
    'Open Sans',
    'Helvetica Neue',
    sans-serif;

  font-size: 18px;
}
input {
  margin: 10px 0;
  border-radius: 8px;
}
.error {
  color: white;
  background-color: yellow;
  border: 1px solid white;
}

.formulario {
  display: block;
  margin: 0 auto;
}
</style>
