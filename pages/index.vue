<template>
  <div class="container">
    <a-card title="Lista filmes IMDB" class="card">
      <a slot="extra">
        <nuxt-link to="/create">Adicionar</nuxt-link>
      </a>
      <a-table
        :columns="columns"
        :data-source="data"
        :loading="loading"
        :pagination="false"
        class="table"
      >
        <div
          slot="filterDropdown"
          slot-scope="{
            setSelectedKeys,
            selectedKeys,
            confirm,
            clearFilters,
            column,
          }"
          style="padding: 8px;"
        >
          <a-input
            v-ant-ref="(c) => (searchInput = c)"
            :placeholder="`Search ${column.dataIndex}`"
            :value="selectedKeys[0]"
            style="width: 188px; margin-bottom: 8px; display: block;"
            @change="
              (e) => setSelectedKeys(e.target.value ? [e.target.value] : [])
            "
            @pressEnter="
              () => handleSearch(selectedKeys, confirm, column.dataIndex)
            "
          />
          <a-button
            type="primary"
            icon="search"
            size="small"
            style="width: 90px; margin-right: 8px;"
            @click="() => handleSearch(selectedKeys, confirm, column.dataIndex)"
          >
            Search
          </a-button>
          <a-button
            size="small"
            style="width: 90px;"
            @click="() => handleReset(clearFilters)"
          >
            Reset
          </a-button>
        </div>
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
    scopedSlots: {
      filterDropdown: 'filterDropdown',
      filterIcon: 'searchIcon',
      customRender: 'customRender',
    },
    onFilter: (value, record) =>
      record.primary_title.toString().includes(value),
    onFilterDropdownVisibleChange: (visible) => {
      if (visible) {
        setTimeout(() => {
          this.searchInput.focus()
        }, 0)
      }
    },
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
    title: 'Duração',
    key: 'runtime',
    dataIndex: 'runtime',
  },
  {
    title: 'Genêros',
    key: 'genres',
    dataIndex: 'genres',
  },
  {
    title: 'Avaliação',
    key: 'rating.average_rating',
    dataIndex: 'rating.average_rating',
  },
  {
    title: 'Votos',
    key: 'rating.num_votes',
    dataIndex: 'rating.num_votes',
  },
]

export default {
  async fetch() {
    const response = await fetch(
      `http://127.0.0.1:3333/titles?page=${this.page}&search=${this.search}`
    ).then((res) => res.json())
    this.data = response.data
    console.log(response.data)
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
      } else if (type === 'tvMovie') {
        return 'green'
      } else if (type === 'tvSeries') {
        return 'cyan'
      } else if (type === 'tvMiniSeries') {
        return 'purple'
      } else if (type === 'tvEspecial') {
        return 'yellow'
      } else if (type === 'video') {
        return 'blue'
      } else if (type === 'videoGame') {
        return 'lime'
      }
    },
    handleTextType(type) {
      if (type === 'tvEpisode') {
        return 'Episódio TV'
      } else if (type === 'short') {
        return 'Curta metragem'
      } else if (type === 'movie') {
        return 'Filme'
      } else if (type === 'tvMovie') {
        return 'Filme TV'
      } else if (type === 'tvSeries') {
        return 'Série'
      } else if (type === 'tvMiniSeries') {
        return 'Mini série'
      } else if (type === 'tvEspecial') {
        return 'Especial TV'
      } else if (type === 'video') {
        return 'Video'
      } else if (type === 'videoGame') {
        return 'Vide-game'
      }
    },
    handleSearch(selectedKeys, confirm, dataIndex) {
      confirm()
      this.search = selectedKeys[0]
      this.searchedColumn = dataIndex
      this.loading = true
      this.$fetch()
    },
    handleReset(clearFilters) {
      clearFilters()
      this.search = ''
      this.loading = true
      this.$fetch()
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
