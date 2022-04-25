<template>
  <q-page class="constrain q-pa-md">
    <div class="row q-col-gutter-lg">
      <div class="col-0 col-sm-1"></div>
      <div class="col-12 col-sm-8">
        <q-card
          v-for="post in posts"
          :key="post.id"
          class="card-post q-mb-md"
          flat
          bordered
        >
          <q-item>
            <q-item-section avatar>
              <q-avatar>
                <q-img src="https://cdn.quasar.dev/img/boy-avatar.png" />
              </q-avatar>
            </q-item-section>

            <q-item-section>
              <q-item-label class="text-bold">Aditya Sarkar</q-item-label>
              <q-item-label caption> {{ post.location }} </q-item-label>
            </q-item-section>
          </q-item>

          <q-separator />
          <q-img :src="post.imageUrl" />
          <q-card-section>
            <div>{{ post.caption }}</div>
            <div class="text-caption text-grey">{{ niceDate(post.date) }}</div>
          </q-card-section>
        </q-card>
      </div>
      <div class="col-3">
        <q-item class="fixed large-screen-only">
          <q-item-section avatar>
            <q-avatar size="48px">
              <q-img src="https://cdn.quasar.dev/img/boy-avatar.png" />
            </q-avatar>
          </q-item-section>

          <q-item-section>
            <q-item-label class="text-bold">aditya.sarkar</q-item-label>
            <q-item-label caption> Aditya Sarkar </q-item-label>
          </q-item-section>
        </q-item>
      </div>
    </div>
  </q-page>
</template>

<script>
import { date } from "quasar";

export default {
  name: "HomePage",
  data() {
    return {
      posts: [
        {
          id: 1,
          caption: "Bridge",
          date: 1650868590297,
          location: "Noida",
          imageUrl: "https://cdn.quasar.dev/img/parallax2.jpg",
        },
        {
          id: 2,
          caption: "Bridge",
          date: 1650868590297,
          location: "Noida",
          imageUrl: "https://cdn.quasar.dev/img/parallax2.jpg",
        },
        {
          id: 3,
          caption: "Bridge",
          date: 1650868590297,
          location: "Noida",
          imageUrl: "https://cdn.quasar.dev/img/parallax2.jpg",
        },
        {
          id: 4,
          caption: "Bridge",
          date: 1650868590297,
          location: "Noida",
          imageUrl: "https://cdn.quasar.dev/img/parallax2.jpg",
        },
      ],
    };
  },
  computed: {
    niceDate() {
      return (value) => {
        return date.formatDate(value, "DD MMMM h:mm a");
      };
    },
  },
};
</script>
<style lang="scss">
.card-post {
  .q-img {
    min-height: 200px;
  }
}
</style>
