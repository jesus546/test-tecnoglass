
<template>
    <v-table class="border">
        <thead class="bg-light-blue-lighten-3  ">
            <tr>
                <th v-for="(item, i) in columns " :key="i" class="text-center text-white ">
                    {{ item.label }}
                </th>
            </tr>
        </thead>
        <tbody>
            <tr v-if="isLoading">
                <td class="text-center"  style="height: 400px;" :colspan="columns.length"> 
                    <v-progress-circular indeterminate color="primary"></v-progress-circular>
               </td>
            </tr>
            <tr v-for="(item, i) in body" :key="i" v-else-if="body.length > 0">
                <td v-for="(e, index) in columns" :key="index">{{ item[e.key] }}</td>
            </tr>
            <tr v-else>
                <td class="text-center"  style="height: 400px;" :colspan="columns.length">No disponible</td>
            </tr>
        </tbody>
    </v-table>
</template>

<script lang="ts" setup>

defineProps(['body', 'columns', 'isLoading'])

</script>
  