<template>
  <div>
    <div
      v-for="work in works"
      :key="work.sys.id"
      class="w-full m-2 pb-3 bg-white overflow-hidden shadow-lg"
    >

      <div class="absolute bg-white py-1 px-3 rounded shadow mt-1 ml-1 text-sm">
        {{ work.fields.category.fields.name }}
      </div>
      <div
        class="mb-3 w-full h-64 bg-center bg-cover"
        :style=" 'background-image: url( ' + work.fields.image[0].fields.file.url + ')' "
      ></div>
      <h3 class="ml-3 font-bold">{{ work.fields.title }}</h3>
      <h4 class="ml-3 my-2 text-xs">{{ work.fields.subtitle }}</h4>
      <div class="flex ml-2">
        <li
          v-for="tag in work.fields.tag"
          :key="tag.sys.id"
          class="list-none text-xs m-1 bg-gray-200 p-1 rounded"
        >
          {{ tag.fields.name }}
        </li>
      </div>

    </div>
  </div>
</template>

<script>
  import Item from '@/components/Item'
  import {createClient} from '~/plugins/contentful.js'

  const client = createClient()

  export default {
    asyncData() {
      return Promise.all([
        client.getEntries({
          'content_type': 'work',
          order: '-sys.createdAt'
        })
      ]).then(([works]) => {

        console.log("===============>ここ！");
        works.items.forEach((val, i) => {
          console.dir(val.fields.image[0].fields.file.url);

        });

        return {works: works.items}
      }).catch(console.error)
    }

  }
</script>

<style>
  /* Sample `apply` at-rules with Tailwind CSS
  .container {
  @apply min-h-screen flex justify-center items-center text-center mx-auto;
  }
  */
  .container {
    margin: 0 auto;
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
  }

  .title {
    font-family: 'Quicksand',
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
    display: block;
    font-weight: 300;
    font-size: 100px;
    color: #35495e;
    letter-spacing: 1px;
  }

  .subtitle {
    font-weight: 300;
    font-size: 42px;
    color: #526488;
    word-spacing: 5px;
    padding-bottom: 15px;
  }

  .links {
    padding-top: 15px;
  }
</style>
