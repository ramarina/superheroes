<template>
  <div>
    <b-input-group >
      <b-form-input class="search-bar form-control" v-model="search" @keyup="$emit('keyUp', search)" placeholder="Search superhero by name.."></b-form-input>
      <b-input-group-append>
        <b-button v-if="superheroes.length > 0"
                  :class="visible ? null : 'collapsed'"
                  :aria-expanded="visible ? 'true' : 'false'"
                  aria-controls="collapse-2"
                  @click="visible = !visible"
                  variant="dark"
        >Advanced powerstats filter</b-button>
      </b-input-group-append>
    </b-input-group>
    <b-collapse id="collapse-2"  v-model="visible"  v-if="superheroes.length > 0">
      <b-card>
        <div v-for="(item, index) in powerstats" :key="index" v-model="powerstats">
        <span>{{ item.name }} 0 - {{ item.value }}</span>
        <vue-slider v-model="item.value" @change="$emit('powerstatChanged', item)" :lazy="true"></vue-slider>
        </div>
      </b-card>
    </b-collapse>
  </div>
</template>
<script>
import VueSlider from 'vue-slider-component'
import 'vue-slider-component/theme/antd.css'

export default {
    name: "Search",
    props: [
        'superheroes',
        'powerstats',
    ],
    data() {
        return{
            search: null,
            visible: false,
        };
    },
    components: {VueSlider},
}
</script>
