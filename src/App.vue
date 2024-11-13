<template>
  <div>
    <div>
      <label for="name">Nom : </label>
      <input v-model="v$.name.$model" @blur="v$.name.$commit" />
      <p v-for="error of v$.name.$errors" :key="error.$uid">
        {{ error.$message }}
      </p>
    </div>

    <div>
      <label for="emailAddress">Email : </label>
      <input v-model="v$.emailAddress.$model" @blur="v$.emailAddress.$commit" />
      <p v-for="error of v$.emailAddress.$errors" :key="error.$uid">
        {{ error.$message }}
      </p>
    </div>

    <div>
      <label for="password">Mot de passe</label>
      <input
          id="password"
          type="password"
          v-model="v$.password.$model"
          @blur="v$.password.$commit"
      />
      <p v-for="error of v$.password.$errors" :key="error.$uid">
        {{ error.$message }}
      </p>
    </div>

    <div>
      <label for="confirmPassword">Confirmer le mot de passe</label>
      <input
          id="confirmPassword"
          type="password"
          v-model="v$.confirmPassword.$model"
          @blur="v$.confirmPassword.$commit"
      />
      <p v-for="error of v$.confirmPassword.$errors" :key="error.$uid">
        {{ error.$message }}
      </p>
    </div>

    <button @click="submit">Soumettre</button>
  </div>
</template>

<script>
import { useVuelidate } from '@vuelidate/core'
import { email, minLength, required, sameAs } from '@vuelidate/validators'
import { ref } from 'vue'
import {unref} from "@vue/runtime-core";

export default {
  setup() {
    const name = ref(null)
    const emailAddress = ref('')
    const password = ref('')
    const confirmPassword = ref('')

    const rules = {
      name: { required },
      emailAddress: { required, email },
      password: { required, minLength: minLength(8) },
      confirmPassword: { required, sameAs: sameAs(password) },
    }

    const v$ = useVuelidate(rules, { name, emailAddress, password, confirmPassword })

    // Méthode de soumission asynchrone
    const submit = async () => {
      const result = await unref(v$).$validate() // Attendre la validation de tous les champs

      if (!result) {
        // Si la validation échoue, afficher un message d'erreur ou notifier l'utilisateur
        alert('Le formulaire contient des erreurs. Veuillez vérifier les champs.')
        return
      }

      // Si la validation réussit, exécuter des actions asynchrones (ex: envoyer les données)
      alert('Formulaire soumis avec succès!')
      // Vous pouvez également effectuer des actions asynchrones ici, comme un appel API
    }

    return {
      v$,
      name,
      emailAddress,
      password,
      confirmPassword,
      submit,
    }
  },
}
</script>

<style scoped>
input {
  display: block;
  margin-bottom: 10px;
}
p {
  color: red;
  font-size: 0.9em;
}
</style>
