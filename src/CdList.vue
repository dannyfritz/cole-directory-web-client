<template>
  <section class="cd-list">
    <h2>Types</h2>
    <form class="type-filter-form">
      <span v-for="type in allTypes" class="type-filter-form__item item">
        <label class="item__label" :for="type">
          <input class="item__control" type="checkbox" :id="type" @click="toggleSelectedType(type)" v-model="selectedTypes" :value="type">
        {{type}}</label>
      </span>
    </form>
    <h2>{{filteredPlaces.length}} Place<span v-if="filteredPlaces.length !== 1">s</span> Found</h2>
    <div v-if="filteredPlaces.length === 0" class="empty-state card">
      <i class="empty-state__icon fa fa-building fa-3x"></i>
      <p class="empty-state__text">No businesses found</p>
      <p class="empty-state__text">Try selecting less filters</p>
    </div>
    <transition-group name="list" tag="ul" class="list">
      <li v-for="place in filteredPlaces" :key="place.name">
        <md-list-item :title="place.name">
          <i slot="icon" class="fa fa-building"></i>
          <div slot="details" class="line">
            <ul class="list--inline line__anchor">
              <li class="list__item--inline" v-if="place.website">
                <a class="button button--icon" :href="place.website">
                  <i class="fa fa-external-link" aria-hidden="true"></i>
                </a>
              </li>
              <li class="list__item--inline" v-if="place.facebook">
                <a class="button button--icon" :href="place.facebook">
                  <i class="fa fa-facebook" aria-hidden="true"></i>
                </a>
              </li>
              <li class="list__item--inline" v-if="place.address">
                <a class="button button--icon" :href="`http://maps.google.com/?q=${place.address}+${place.address2}`">
                  <i class="fa fa-map-marker" aria-hidden="true"></i>
                </a>
              </li>
              <li class="list__item--inline" v-if="place.phone">
                <a class="button button--icon" :href="`tel:${place.phone}`">
                  <i class="fa fa-phone" aria-hidden="true"></i>
                </a>
              </li>
            </ul>
            <ul class="list--inline">
              <li class="list__item--inline" v-for="type in types(place.type)">
                <div class="chip">{{type}}</div>
              </li>
            </ul>
          </div>
        </md-list-item>
      </li>
    </transition-group>
  </section>
</template>

<script>
import { mapGetters, mapActions  } from "vuex"

export default {
  name: "cd-list",
  components: {},
  data () { return {} },
  computed: {
    ...mapGetters({
      places: "placesAll",
      columns: "placesColumns",
      allTypes: "placesTypes",
      selectedTypes: "selectedTypes",
      filteredPlaces: "placesFilteredByType",
    })
  },
  methods: {
    types (typeString) {
      return typeString.split(";").filter((v) => v).map((t) => t.trim())
    },
    ...mapActions(["toggleSelectedType"]),
  },
  filters: {},
}
</script>

<style scoped>
.type-filter-form {
  column-count: 3;
}
.type-filter-form__item {
  display: inline-block;
  width: 100%;
  -webkit-column-break-inside: avoid;
  page-break-inside: avoid;
  break-inside: avoid;
}

.list-enter-active, .list-leave-active {
  transition: all 0.3s;
}
.list-enter, .list-leave-to /* .list-leave-active for <2.1.8 */ {
  opacity: 0;
  transform: translateX(-30px);
}

.item {
  display: flex;
}
.item__control {
  vertical-align: text-bottom;
}
.item__label {
  flex-grow: 1;
}
@media (max-width: 600px) {
  .item {
    padding: 0.5em 0;
  }
  .type-filter-form {
    column-count: 2;
  }
}
</style>
