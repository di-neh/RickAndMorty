<template>
    <div class="card">
        <div class="picture">
            <img :src="image" height="220px" width="229px">
        </div>
        <div class="conteiner">
            <div class="box">
                <h1>
                    {{name}}
                </h1>
                <h2>{{ status }} - {{ species }}</h2>
            </div>

            <div class="box">
                <p>Last known location</p>
                <div> {{ location }}</div>
            </div>

            <div class="box">
                <p>First seen in:</p>
                <div>{{ origin }}</div>
            </div>
        </div>
    </div>
</template>

<script>
import { defineComponent } from 'vue';

export default defineComponent ({
    name: 'CharCard',
    props: {
        name: String,
        status: String,
        species: String,
        image: String,
        location: String,
        origin: String
    }
})
</script>

<style scoped>
    .card{
        display: flex;
        background: rgb(81, 80, 89);
        width: 600px;
        height: 220px;
        border-radius: 10px;
        flex: 1 1 calc(33.333% - 10px);
        max-width: calc(33.333% - 10px);
        margin: 5px;
    }
    h1 {
        font-size: 30px;
    }
    h2 {
        font-size: 15px;
    }
    p {
        color: grey;
        margin-bottom: 5px;
    }
    .conteiner {
        padding: 15px;
    }
    .box {
        margin-bottom: 15px;
    }
    .picture img {
    border-radius: 10px 0 0 10px;
}
</style>