<template>
  <div class="container">
    <a-card title="Lista filmes IMDB" class="card">
      <a slot="extra" href="#">more</a>
      <a-table
        :columns="columns"
        :data-source="data"
        :loading="loading"
        :pagination="false"
        class="table"
      >
        <span slot="types" slot-scope="types">
          <a-tag :key="types" :color="handleColorType(types)">
            {{ handleTextType(types) }}
          </a-tag>
        </span>
      </a-table>
      <a-pagination
        :total="total"
        :current="page"
        :pageSize="10"
        class="pagination"
        @change="onChange"
      />
    </a-card>
  </div>
</template>

<script>
const columns = [
  {
    title: 'Título',
    dataIndex: 'primary_title',
    key: 'primary_title',
  },
  {
    title: 'Título original',
    key: 'original_title',
    dataIndex: 'original_title',
  },
  {
    title: 'Tipo',
    dataIndex: 'title_type',
    key: 'title_type',
    scopedSlots: { customRender: 'types' },
  },
  {
    title: 'Ano de lançamento',
    key: 'start_year',
    dataIndex: 'start_year',
  },
  {
    title: 'Ano de encerramento',
    key: 'end_year',
    dataIndex: 'end_year',
  },
  {
    title: 'Duração',
    key: 'runtime',
    dataIndex: 'runtime',
  },
  {
    title: 'Genêros',
    key: 'genres',
    dataIndex: 'genres',
  },
]

export default {
  async fetch() {
    const response = await fetch(
      `http://127.0.0.1:3333/titles?page=${this.page}&search=${this.search}`
    ).then((res) => res.json())
    console.log(this.page)
    this.data = response.data
    this.loading = false
    this.page = response.page
    this.total = response.total
  },
  data() {
    return {
      data: [],
      columns,
      page: 1,
      total: 0,
      search: '',
      loading: true,
    }
  },
  computed() {
    this.fetch()
  },
  methods: {
    onChange(page, pagSize) {
      this.loading = true
      this.page = page
      this.$fetch()
    },
    handleColorType(type) {
      if (type === 'tvEpisode') {
        return 'pink'
      } else if (type === 'short') {
        return 'red'
      } else if (type === 'movie') {
        return 'orange'
      }
    },
    handleTextType(type) {
      if (type === 'tvEpisode') {
        return 'Episódio TV'
      } else if (type === 'short') {
        return 'Curta metragem'
      } else if (type === 'movie') {
        return 'Filme'
      }
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
  overflow-x: auto;
}
.table {
  max-width: 1400px;
}
.pagination {
  padding: 10px;
}
</style>
