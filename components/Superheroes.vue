<template>
  <div>
    <search @keyUp="searchSuperheroes" :superheroes="superheroes" :powerstats="powerstats" @powerstatChanged="powerstatChanged"></search>
    <b-alert dismissible variant="danger" v-model="showDismissibleAlert">{{ error }}</b-alert>
    <br>
    <b-table striped hover bordered small head-variant="dark" :items="tableData"  :per-page="perPage"
             :current-page="currentPage" :busy="isBusy" show-empty></b-table>
    <b-pagination
      v-if="tableData.length"
      v-model="currentPage"
      :total-rows="rows"
      :per-page="perPage"
      small
      variant="outline-dark"
    ></b-pagination>

  </div>
</template>

<script>
import axios from 'axios';
import Search from "../components/Search";

export default {
    name: "Superheroes",
    data() {
        return {
            superheroapi: process.env.SUPERHERO_API,
            superheroes: [],
            tableData: [],
            isBusy: false,
            powerstats: [],
            config: {headers: {Accept: "application/json"}},
            showDismissibleAlert: false,
            error: null,
            perPage: 20,
            currentPage: 1,
        }
    },
    computed: {
        rows() {
            return this.tableData.length
        }
    },
    watch: {
        superheroes: function (val) {
            this.tableData = this.mapTableData(val)
        },
    },
    methods: {
        getSuperheroes(search) {
            console.log(search)
            this.superheroes = [];
            if (typeof search === 'undefined') {
                return
            }
            axios.get(`${this.superheroapi}/search/${search}`, this.config)
                .then(response => {
                    this.isBusy = true
                    if (response.data.results && response.data.results.length) {
                        this.superheroes = response.data.results
                        this.tableData = this.mapTableData(this.superheroes)
                        this.powerstats = this.extractPowestats(response.data.results[0].powerstats)
                        this.isBusy = false

                        return
                    }
                    this.isBusy = false
                })
                .catch((error) => {
                    this.error = error
                    this.showDismissibleAlert = true
                    this.isBusy = false
                })

        },
        searchSuperheroes(search) {
            this.getSuperheroes(search);
        },
        powerstatChanged(item) {
            for (let key of this.powerstats) {
                if (key === item.name) {
                    this.powerstats.value = item.value
                }
            }
            this.tableData = this.filterData();
        },
        mapTableData(data) {
            return data.map(function (el) {
                return {
                    id: el.id,
                    name: el.name,
                    full_name: el.biography['full-name'],
                    gender: el.appearance.gender,
                    intelligence: el.powerstats.intelligence >= 0 ? el.powerstats.intelligence : '',
                    strength: el.powerstats.strength >= 0 ? el.powerstats.strength : '',
                }
            })
        },
        extractPowestats(powerstats) {
            const objs = []
            for (let key of Object.keys(powerstats)) {
                objs.push({name: key, value: 100})
            }

            return objs
        },
        filterData() {
            const self = this

            let filtered = this.superheroes.filter(function (superhero) {
                return parseInt(superhero.powerstats.intelligence) <= self.powerstats[0].value &&
                    parseInt(superhero.powerstats.strength) <= self.powerstats[1].value &&
                    parseInt(superhero.powerstats.speed) <= self.powerstats[2].value &&
                    parseInt(superhero.powerstats.durability) <= self.powerstats[3].value &&
                    parseInt(superhero.powerstats.power) <= self.powerstats[4].value &&
                    parseInt(superhero.powerstats.combat) <= self.powerstats[5].value
            })

            return this.mapTableData(filtered)
        },
    },
    components: {Search},
}
</script>
