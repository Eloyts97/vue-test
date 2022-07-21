<template>
    <h3>Rick and Morty fetch characters:</h3>

    <div class="container" v-if="!loading && data && data.length">
        <div class="card" v-for="character of data" v-bind:key="character.id">
            <img v-bind:src="character.image"/>
            <div class="info">
                <span><strong>{{ character.name }}</strong></span>
                <div class="sub info">
                    <span>Status: <strong v-bind:class="character.status">{{character.status}} - {{character.species}}</strong></span>
                    <span>Location: <strong>{{character.location.name}}</strong></span>
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
    name: 'CharactersComponent',
    props: {
    },
    setup() {
        const data = ref([]);
        const loading = ref(true);
        const error = ref(null);

        // url for Rick and Morty API
        const urlApi = 'https://rickandmortyapi.com/api/character';
        const getCharacters = async (url) => {
            return await axios(url);
        }

        const fetchData = async () => {
            // set start loading data
            loading.value = true;

            try {
                // fetch rick and morty characters data
                let response = await getCharacters(urlApi);
                data.value = response?.data?.results?.length > 0 ? response.data.results : [];

                // get characters from all pages
                while (response.data.info.next !== null) {
                    response = await getCharacters(response.data.info.next);
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
h3 {
  margin: 40px 0;
}
.container {
    width: 1300px;
    max-width: 90%;
    display: flex;
    flex-wrap: wrap;
    margin: auto;
    justify-content: space-between;
}
.card {
    width: 300px;
    height: auto;
    max-width: 100%;
    display: flex;
    flex-direction: column;
}
img {
    width: 100%;
    height: 300px;
}
.Alive {
    color: green;
}
.Dead {
    color: red;
}
.unknown {
    color: orange;
}
span {
    margin: 0px auto 5px auto;
}
.info {
    display: flex;
    flex-direction: column;
    padding: 10px 0 5px 0;
    background: #f1f1f1;
    margin-bottom: 30px;
}
.sub.info {
    margin-bottom: 0px;
    flex-wrap: wrap;
    font-size: 14px;
}
</style>