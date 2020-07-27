<template>
  <div class="container">
    <a-card title="Cadastro novo título" class="card">
      <a slot="extra">
        <nuxt-link to="/">Voltar</nuxt-link>
      </a>
      <a-form
        :form="form"
        :label-col="{ span: 5 }"
        :wrapper-col="{ span: 12 }"
        @submit="handleSubmit"
      >
        <a-form-item label="Título">
          <a-input
            v-decorator="[
              'primary_title',
              {
                rules: [{ required: true, message: 'Título é obrigatório.' }],
              },
            ]"
          />
        </a-form-item>
        <a-form-item label="Título original">
          <a-input
            v-decorator="[
              'original_title',
              {
                rules: [
                  { required: true, message: 'Título original é obrigatório.' },
                ],
              },
            ]"
          />
        </a-form-item>

        <a-form-item label="Ano de lançamento">
          <a-input
            v-decorator="[
              'start_year',
              {
                rules: [
                  {
                    required: true,
                    message: 'Ano de lançamento é obrigatório.',
                  },
                ],
              },
            ]"
          />
        </a-form-item>

        <a-form-item label="Ano de encerrametno">
          <a-input
            v-decorator="[
              'end_year',
              {
                rules: [{ required: false }],
              },
            ]"
          />
        </a-form-item>

        <a-form-item label="Tempo de duração">
          <a-input
            v-decorator="[
              'runtime',
              {
                rules: [
                  {
                    required: true,
                    message: 'Tempo de duração é obrigatório.',
                  },
                ],
              },
            ]"
          />
        </a-form-item>

        <a-form-item label="Tipo">
          <a-select
            v-decorator="[
              'title_type',
              {
                rules: [{ required: true, message: 'Tipo é obrigatório.' }],
              },
            ]"
            placeholder="Selecione o tipo"
            @change="handleSelectChange"
          >
            <a-select-option value="short">
              Curta metragem
            </a-select-option>
            <a-select-option value="movie">
              Filme
            </a-select-option>
            <a-select-option value="tvMovie">
              Filme
            </a-select-option>
            <a-select-option value="tvSeries">
              Série
            </a-select-option>
            <a-select-option value="tvEpisode">
              Episódio TV
            </a-select-option>
            <a-select-option value="tvMiniSeries">
              Mini Série
            </a-select-option>
            <a-select-option value="tvEspecial">
              Especial de TV
            </a-select-option>
            <a-select-option value="video">
              Video
            </a-select-option>
            <a-select-option value="videoGame">
              Video-Game
            </a-select-option>
          </a-select>
        </a-form-item>

        <a-form-item label="Indicação adulta">
          <a-select
            v-decorator="[
              'is_adult',
              {
                rules: [
                  { required: true, message: 'Indicação é obrigatório.' },
                ],
              },
            ]"
            placeholder="Selecione a indicação"
            @change="handleSelectChange"
          >
            <a-select-option value="true">
              Sim
            </a-select-option>
            <a-select-option value="false">
              Não
            </a-select-option>
          </a-select>
        </a-form-item>
        <a-form-item :wrapper-col="{ span: 12, offset: 5 }">
          <a-button type="primary" html-type="submit">
            Submit
          </a-button>
        </a-form-item>
      </a-form>
    </a-card>
  </div>
</template>

<script>
import api from '../services/api'

export default {
  data() {
    return {
      formLayout: 'horizontal',
      form: this.$form.createForm(this, { name: 'coordinated' }),
    }
  },
  methods: {
    handleSubmit(e) {
      e.preventDefault()
      this.form.validateFields((err, values) => {
        if (!err) {
          // values.is_adult = values.is_adult === 'true' ? true : false
          api
            .post('/titles', values)
            .then((item) => this.openNotification('success'))
            .catch((error) => console.log(error))
        }
      })
    },
    handleSelectChange(value) {
      console.log(value)
      this.form.setFieldsValue({
        note: `Hi, ${value === 'male' ? 'man' : 'lady'}!`,
      })
    },
    openNotification(type) {
      this.$notification[type]({
        message: 'Título cadastrado com sucesso',
        description: '',
      })
    },
  },
}
</script>

<style>
.container {
  max-width: 1500px;
  margin: 0 auto;
  padding: 16px;
}
.card {
  max-width: 1500px;
  height: 100%;
}
</style>
