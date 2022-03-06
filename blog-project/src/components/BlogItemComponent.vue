<template>
  <v-flex xs6>
    <v-card class="mx-auto" max-width="500" outlined>
      <v-list-item three-line>
        <v-list-item-content>
          <div class="text-overline mb-4"></div>
          <v-list-item-title class="text-h5 mb-1" v-text="blog.title">
          </v-list-item-title>
          <v-list-item-subtitle>{{ blog.description }}</v-list-item-subtitle>
        </v-list-item-content>

        <v-list-item-avatar tile size="80" color="grey"
          ><v-img
            :src="blog.photo ? apiDomain + blog.photo : 'https://picsum.photos/200/300'"
            class="white--text"
            height="200px"
          >
          </v-img>
        </v-list-item-avatar>
      </v-list-item>

      <v-card-actions>
        <v-btn :to="'/blog/' + blog.id" color="teal darken-3" class="white--text">
          Baca
        </v-btn>
        <v-btn
          text
          color="teal darken-3"
          @click="$emit('editPost', blog)"
          v-if="!guest"
        >
          Edit
        </v-btn>
        <v-btn
          text
          color="error"
          @click="$emit('deletePost', blog.id)"
          v-if="!guest"
        >
          Hapus
        </v-btn>
      </v-card-actions>
    </v-card>

  </v-flex>
</template>

<script>
import { mapGetters } from "vuex";
export default {
  data: () => ({
    apiDomain: "https://demo-api-vue.sanbercloud.com/",
  }),
  props: ["blog"],
  computed: {
    ...mapGetters({
      guest: "auth/guest",
    }),
  },
};
</script>