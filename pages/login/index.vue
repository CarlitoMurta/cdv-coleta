<template>
  <div>
    <section>
      <div class="container mx-auto">
        <div
          class="grid items-center grid-cols-1 gap-8 py-16 lg:grid-cols-1 lg:py-20"
        >
          <form>
            <div
              class="relative input-md mb-4 mx-auto border-2 rounded-lg outline border-grey-200 focus-within:border-black"
            >
              <input
                v-model="login.email"
                type="text"
                name="email"
                autocomplete="off"
                placeholder="E-mail"
                required
                class="block w-full p-4 text-sm font-bold bg-transparent rounded-lg appearance-none focus:outline-none"
              />
              <label
                for="email"
                class="absolute top-0 p-4 text-sm duration-300 bg-white -z-1 origin-0"
                >E-mail</label
              >
            </div>
            <div
              class="relative input-md mb-4 mx-auto border-2 rounded-lg outline border-grey-200 focus-within:border-black"
            >
              <input
                v-model="login.senha"
                type="password"
                name="senha"
                maxlength="8"
                autocomplete="off"
                placeholder="Senha"
                required
                class="block w-full p-4 text-sm font-bold bg-transparent rounded-lg appearance-none focus:outline-none"
              />
              <label
                for="senha"
                class="absolute top-0 p-4 text-sm duration-300 bg-white -z-1 origin-0"
                >Senha</label
              >
            </div>

            <button
              class="block h-12 min-h-2 px-10 mx-auto mt-8 font-bold text-center text-white capitalize rounded shadow-md base-button bg-brand-green"
              type="submit"
              role="button"
              @click.prevent="logar"
            >
              Login
            </button>
          </form>
          <div>
            <button
              class="block h-12 min-h-0 px-10 mx-auto font-bold text-center capitalize rounded-lg dark:text-white hover:bg-grey-100 text-grey-400"
              role="button"
              @click="showModal"
            >
              Cadastrar
            </button>
          </div>
          <div v-if="error" class="p-2 mt-1 bg-red-100 rounded-lg">
            <p
              v-for="err in error"
              :key="err.descricao"
              class="text-sm text-center text-red-500"
            >
              {{ err.descricao }}
            </p>
          </div>
        </div>
      </div>
    </section>
    <div>
      <modal
        name="modal-cadastro"
        :height="500"
        :maxWidth="400"
        :adaptive="true"
      >
        <div class="p-8 bg-white rounded-lg shadow">
          <form>
            <div
              class="relative mb-4 border-2 rounded-lg outline border-grey-200 focus-within:border-black"
            >
              <input
                v-model="cadastro.nome"
                type="text"
                name="nome"
                autocomplete="off"
                required
                placeholder="Nome"
                class="block w-full p-4 text-sm font-bold bg-transparent rounded-lg appearance-none focus:outline-none"
              />
              <label
                for="nome"
                class="absolute top-0 p-4 text-sm duration-300 bg-white -z-1 origin-0"
                >Nome Completo</label
              >
            </div>
            <div
              class="relative mb-4 border-2 rounded-lg outline border-grey-200 focus-within:border-black"
            >
              <input
                v-model="cadastro.email"
                type="text"
                name="email"
                autocomplete="off"
                placeholder="E-mail"
                required
                class="block w-full p-4 text-sm font-bold bg-transparent rounded-lg appearance-none focus:outline-none"
              />
              <label
                for="email"
                class="absolute top-0 p-4 text-sm duration-300 bg-white -z-1 origin-0"
                >E-mail</label
              >
            </div>
            <div>
              <select
                v-model="cadastro.idBairro"
                class="relative mb-4 w-full border-2 p-4 strong rounded-lg outline border-grey-200 focus-within:border-black"
              >
                <option value="0">Selecione um bairro</option>
                <option
                  v-for="bairro in listaBairros"
                  :key="bairro.id"
                  :value="bairro.id"
                >
                  {{ bairro.nome }}
                </option>
              </select>
            </div>
            <div
              class="relative mb-4 border-2 rounded-lg outline border-grey-200 focus-within:border-black"
            >
              <input
                v-model="cadastro.senha"
                type="password"
                name="senha"
                maxlength="15"
                autocomplete="off"
                placeholder="Senha"
                required
                class="block w-full p-4 text-sm font-bold bg-transparent rounded-lg appearance-none focus:outline-none"
              />
              <label
                for="senha"
                class="absolute top-0 p-4 text-sm duration-300 bg-white text-grey-400 -z-1 origin-0"
                >Senha</label
              >
            </div>
            <div
              class="relative mb-4 border-2 rounded-lg outline border-grey-200 focus-within:border-black"
            >
              <input
                v-model="cadastro.repitaSenha"
                type="password"
                name="repitaSenha"
                maxlength="15"
                autocomplete="off"
                placeholder="Repita sua Senha"
                required
                class="block w-full p-4 text-sm font-bold bg-transparent rounded-lg appearance-none focus:outline-none"
              />
              <label
                for="repitaSenha"
                class="absolute top-0 p-4 text-sm duration-300 bg-white text-grey-400 -z-1 origin-0"
                >Repita sua Senha</label
              >
            </div>
          </form>
          <div>
            <button
              class="block h-12 min-h-0 px-6 mx-auto mt-8 font-bold text-center text-white capitalize rounded shadow-md base-button bg-brand-green"
              type="submit"
              role="button"
              @click.prevent="cadastrarELogin"
            >
              Cadastrar
            </button>
          </div>
          <div v-if="loading" class="p-4 mt-4 bg-green-100 rounded-lg">
            <p class="text-sm text-center text-green-500">
              Sucesso! Recebemos seu cadastro, em breve faremos contato.
            </p>
          </div>
          <div v-if="error" class="p-2 mt-1 bg-red-100 rounded-lg">
            <p
              v-for="err in error"
              :key="err.descricao"
              class="text-sm text-center text-red-500"
            >
              {{ err.descricao }}
            </p>
          </div>
        </div>
      </modal>
    </div>

    <section>
      <!-- Loading -->
      <div class="vld-parent">
        <loading
          :active.sync="loading"
          :can-cancel="false"
          :is-full-page="true"
        >
        </loading>
      </div>
    </section>
  </div>
</template>

<style scoped>
.input-md {
  max-width: 300px;
}

.text-blue {
  color: blue;
}
</style>

<script>
import Vue from 'vue'
import Loading from 'vue-loading-overlay'
import 'vue-loading-overlay/dist/vue-loading.css'
import decode from 'jwt-decode'

export default Vue.extend({
  beforeMount() {
    sessionStorage.removeItem('cdv')
  },
  data() {
    return {
      login: {
        email: '',
        senha: '',
      },
      cadastro: {
        nome: '',
        email: '',
        idBairro: 0,
        senha: '',
        repitaSenha: '',
      },
      loading: false,
      error: '',
      listaBairros: [],
    }
  },
  components: {
    Loading,
  },
  methods: {
    clearCadastro() {
      this.cadastro.nome = ''
      this.cadastro.email = ''
      this.cadastro.senha = ''
      this.cadastro.repitaSenha = ''
    },
    async showModal() {
      this.error = ''
      this.clearCadastro()
      await this.obterBairros()
      this.$modal.show('modal-cadastro')
    },
    async obterBairros() {
      this.loading = true

      await this.$axios
        .get('administrador/bairros')
        .then((res) => {
          this.loading = false
          this.listaBairros = res.data.bairros.filter((b) => {
            return b.ativo && b.possuiRota
          })
        })
        .catch((res) => {
          this.loading = false
        })
    },
    async cadastrarELogin() {
      if (
        !this.cadastro.nome ||
        !this.cadastro.email ||
        this.cadastro.idBairro == 0 ||
        !this.cadastro.senha ||
        !this.cadastro.repitaSenha
      )
        this.error = [
          { descricao: 'Atenção, você deve informar todos os campos. ;)' },
        ]
      else if (this.cadastro.senha != this.cadastro.repitaSenha)
        this.error = [
          { descricao: 'As senhas digitadas não conferem, tente novamente!' },
        ]
      else {
        this.error = ''
        this.loading = true

        await this.$axios
          .post('usuarios', this.cadastro)
          .then((res) => {
            this.clearCadastro()
            sessionStorage.setItem('cdv', JSON.stringify(res.data))
            this.loading = false
            this.$router.push({ name: 'area-logada' })
          })
          .catch((err) => {
            this.loading = false
            this.error = err.response.data
          })
      }
    },
    async logar() {
      this.error = ''
      if (!this.login.senha || !this.login.email) {
        this.error = [
          { descricao: 'Atenção, você deve informar todos os campos. ;)' },
        ]
      } else {
        this.error = ''
        this.loading = true
        await this.$axios
          .post('usuarios/login', this.login)
          .then((res) => {
            sessionStorage.setItem('cdv', JSON.stringify(res.data))
            let decodeJwt = decode(res.data.token)
            if (decodeJwt.role == 'Cliente')
              this.$router.push({ name: 'area-logada' })
            else if (decodeJwt.role == 'Administrador')
              this.$router.push({ name: 'administrador' })
          })
          .catch((err) => {
            this.loading = false
            this.error = err.response.data
          })
      }
    },
  },
})
</script> 