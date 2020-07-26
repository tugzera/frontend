<template>
  <div class="container">
    <a-card title="" class="card">
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

        <a-form-item label="Gender">
          <a-select
            v-decorator="[
              'title_type',
              {
                rules: [
                  { required: true, message: 'Please select your gender!' },
                ],
              },
            ]"
            placeholder="Select a option and change input text above"
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
          console.log('Received values of form: ', values)
        }
      })
    },
    handleSelectChange(value) {
      console.log(value)
      this.form.setFieldsValue({
        note: `Hi, ${value === 'male' ? 'man' : 'lady'}!`,
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
