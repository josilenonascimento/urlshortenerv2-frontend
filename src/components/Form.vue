<template>

  <div id="wrapper">
    <form @submit.prevent="createUrl">
      <input v-model="url" type="text" name="url" placeholder="Your url">
      <input v-model="slug" type="text" name="slug" placeholder="Slug (optional)">
      <button type="submit" :disabled="loading">
        {{ loading ? 'Creating...': 'Create Short Url' }}
      </button>
    </form>

    <div v-if="error" class="box box-error">
      {{error}}
    </div>

    <div v-if="shortUrl" class="box box-url">
      <a :href="shortUrl">{{shortUrl}}</a>
    </div>

  </div>

</template>

<script>
import { ref } from 'vue';
import server from '../server.json'

export default {
  setup() {
    const url = ref('');
    const slug = ref('');
    const error = ref('');
    const shortUrl = ref('');
    const loading = ref(false);

    async function createUrl() {

      loading.value = true;

      const response = await fetch(`${server.url}/url`, {
        headers: {
          'content-type': 'application/json'
        },
        method: 'POST',
        body: JSON.stringify({
          url: url.value,
          slug: slug.value,
        })
      });

      const created = await response.json();

      shortUrl.value = '';
      error.value = '';

      if (created.slug) {
        shortUrl.value = `${server.url}/${created.slug}`;
        url.value = '';
        slug.value = '';
      } else if (created.message) {
        error.value = created.message;
      }

      loading.value = false;
    }

    return {
      createUrl,
      url,
      slug,
      error,
      shortUrl,
      loading,
    }
  }
}
</script>

<style scoped>

#wrapper, .box {
  margin: 0 auto;
  max-width: 480px;
  width: 100%;
  font-size: 16px;
}

.box {
  font-size: 1.8rem;
  margin-top: 14px;
}

.box-error {
  color: #e56b6f;
}

.box-url > a {
  text-decoration: none;
  color: #03045e;
  transition: 0.2s ease-in;
}

.box-url > a:hover {
  color: #0077b6;
}

form {
  display: flex;
  flex-direction: column;
}

input {
  background: #f3f3f3;
  margin-bottom: 14px;
}

button {
  margin-top: 15px;
  cursor: pointer;
  background: #0077b6;
  color: #caf0f8;
  transition: 0.3s ease-in-out;
}

button:hover {
  background: #0096c7;
}

input, button {
  font-size: 1.6rem;
  padding: 15px 20px;
  border-radius: 4px;
  border: 0;
  outline: none;
  box-shadow: 1px 1px 1px rgba(3, 4, 94, 0.2);
}

</style>