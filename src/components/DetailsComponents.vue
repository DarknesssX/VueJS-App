<template v-for="index in 10" :key="index">
  <div class="table-resonsiven full-width">
    <table class="table table-bordered table-hover table-dark">
      <thead class="table-header--background">
        <tr>
          <td class="table-width align-left"> Anime Title  </td>
          <td class="table-width align-left"> Synopsis <button class="btn btn-primary" @click="setColumn('title')">Sort</button> </td>
        </tr>
      </thead>
      <tbody id="table-content" class="table-content--defaultbg">
        <tr class="table-content--background" v-for="anime in sortedAnimeList" :key="anime.id" @click="goToDetails()" >
          <td class="table-width align-left"> {{ anime.title }} </td>
          <td class="table-width align-left"> {{ anime.synopsis }} </td>
        </tr>
      </tbody>
    </table>
  </div>

</template>

<style scoped>
/* Stying of the List comes here... */
  .full-width {
    margin: 0;
    padding-bottom: 1rem;
  }

  .table-dark td {
      border-color: #e56f44;
  }

  #table-content a {
    font-weight: bold;
    color: rgba(255,255,255,0.5);
    margin: 1rem;
  }

  .table-header--background {
      background: linear-gradient( #bd5d38, #e56f44);
  }

  .table-content--defaultbg {
      background: #bd5d38;
  }

  .table-content--background {
      background: rgba(255,255,255,0.01);
  }

  .table-width {
      min-width: 150px;
  }

  .align-left {
      text-align: left;
  }

 .btn-primary {
      background-color:#e56f44;
      border-color: #e56f44;
      float: right;
  }
  .btn-primary:not(:disabled):not(.disabled):active {
      background-color: #bd5d38;
      border-color: #bd5d38;
  }
  .btn-primary:focus {
      box-shadow: rgba(255,255,255,0.5)
  }
</style>

<script lang="ts">

/* Imports all components needed in this script */
import { Component, Vue, Prop } from 'vue-property-decorator';
import JikanTS from 'jikants';
import JikanWrap from '@/modules/JikanWrapper.ts';
import { AnimeList } from 'jikants/dist/src/interfaces/user/AnimeList';

@Component
export default class GetAnimeDetails extends Vue {
  /* Declared the "searchResult" as the JikanWrap reqeust which searches anime and orders them by ID ascended */
  private searchResult = JikanWrap.request(['search', 'anime'], {order_by: 'id', sort: 'asc', limit: 10});
  /* Declares an empty array called "animeList" which will */
  private animeList: any = [{}];
  /* Declares an empty string called column for sorting */
  private column: string = '';
  /* Sorts all the results in this.animeList if this.animeList has been initialized */
  get sortedAnimeList() {
    if (this.animeList) {
      return this.animeList
        .sort((a: any, b: any) => (a[this.column] > b[this.column]) ?
          1 : ((b[this.column] > a[this.column]) ? -1 : 0));
    }
    return;
  }
  /* pushes the page to the Details page when one of the results are tapped or clicked on */
  private goToDetails() {
    this.$router.push({path: '/details'});
  }
  /* Sets the Column in order of the sorting function */
  private setColumn(column: string) {
    this.column = column;
  }
  /* Mounts all the above and sets the variables i want to pass to "Home.vue" to be shown when I call "<GetAnime />" */
  private async mounted(): Promise<void> {
    /* loads in the array of searchResults into  "animeList" */
    const t = await this.searchResult;
    this.animeList = t.results;
    console.log(this.animeList);
  }
}

</script>
