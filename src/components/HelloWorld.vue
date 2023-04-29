
<template>
  <v-container class="d-flex flex-column  ">
    <div class="text-h4 mt-3 mb-3 text-center text-blue">Test Tecnoglass S.A</div>
    <v-responsive class="d-flex align-center text-center fill-height">
      <v-col cols="12" md="2" style="padding: 0;">
        <v-text-field v-model="search" @input="searchHandler" density="compact" variant="outlined"></v-text-field>
      </v-col>
      <Table :columns="columns" :isLoading="isLoading" :body="movies"></Table>
      <div class="text-center">
        <v-container>
          <v-row justify="center">
            <v-col cols="5">
              <v-container class="max-width">
                <v-pagination class="pagination" :onUpdate:modelValue="nextPageHandler" v-model="nextPage" :length="page"></v-pagination>
              </v-container>
            </v-col>
          </v-row>
        </v-container>
      </div>
    </v-responsive>
  </v-container>
</template>

<script lang="ts" setup>
import { ref } from 'vue';
import Table from './table/Table.vue';

const movies = ref([])
const page = ref(1)
const nextPage = ref(1)
const isLoading = ref(true)
const search = ref("batman")
const columns = ref([{ label: 'Nombre', key: 'Title' }, { label: 'Tipo', key: 'Type' }, { label: 'Año', key: 'Year' }])

const nextPageHandler = (value: number) => {
  renderData(search.value, value)
}
const searchHandler = (value: any) => {
  isLoading.value = true
  renderData(value.target.value, nextPage.value)
  search.value = value.target.value
}

const renderData = (search:string, nextPage:number) => {
  fetch(`https://www.omdbapi.com/?s=${search}&apikey=9748ff86&page=${nextPage}&type=movie`)
  .then((res) => res.json())
  .then((data) => ( data.Response === 'False' ?   (movies.value = [], page.value =  1) :  movies.value = data.Search, page.value = Math.ceil(data.totalResults / 10)))
  .finally(() => isLoading.value = false)
}

renderData(search.value,nextPage.value )

</script>
