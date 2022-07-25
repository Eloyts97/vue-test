<template>
    <div class="table" v-if="!loading && data && data.length">
        <div class="theader">
            <!-- Pass header data to TableRowComponent -->
            <div class="row">
                <div class="column col-lg-3">
                    Episodio
                </div>
                <div class="column col-lg-6">
                    Título
                </div>
                <div class="column col-lg-3">
                    Fecha
                </div>
            </div>
        </div>
        <div class="tbody">
            <!-- Pass body data to TableRowComponent -->
            <div class="row" v-for="episode of data" v-bind:key="episode.id">
                <div class="column col-lg-3">
                    {{episode.episode}}
                </div>
                <div class="column col-lg-6">
                    {{episode.name}}
                </div>
                <div class="column col-lg-3">
                    {{episode.air_date}}
                </div>
            </div>
        </div>
    </div>

    <p v-if="loading">
        Still loading...
    </p>

    <p v-if="error">
        {{error}}
    </p>
</template>

<script>
import { ref, onMounted } from 'vue';
import axios from 'axios';

export default {
    name: 'TableComponent',
    props: {
    },
    setup() {
        const data = ref([]);
        const loading = ref(true);
        const error = ref(null);

        // url for Rick and Morty API
        const urlApi = 'https://rickandmortyapi.com/api/episode';
        const getData = async (url) => {
            return await axios(url);
        }

        const fetchData = async () => {
            // set start loading data
            loading.value = true;

            try {
                // fetch rick and morty characters data
                let response = await getData(urlApi);
                data.value = response?.data?.results?.length > 0 ? response.data.results : [];

                // get characters from all pages
                while (response.data.info.next !== null) {
                    response = await getData(response.data.info.next);
                    data.value.push(...response.data.results);
                }
            } catch (err) {
                error.value = err;
            }

            // set finish loading data
            loading.value = false;
        }

        onMounted(async () => {
            await fetchData();
        });

        return {
            data,
            loading,
            error
        }
    }
}
</script>

<style scoped>
.table, .theader, .tbody, .column {
    display: flex;
    flex-direction: column;
}
.theader {
    font-weight: bold;
}
.row {
    display: flex;
}
.col-lg-3 {
    width: 25%;
}
.col-lg-6 {
    width: 50%;
}
</style>