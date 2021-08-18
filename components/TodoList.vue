<template>
  <div>
    <v-card
      class="mx-auto"
      max-width="344"
      outlined
      draggable=".todo"
    >
      <v-list-item two-line>
      <v-list-item-content ng-if="todo">
        <v-list-item-title class="text-h5 mb-1">
        {{ todo.name }}
        </v-list-item-title>
        <v-list-item-subtitle>
        {{ todo.description }}
        </v-list-item-subtitle>
      </v-list-item-content>
      </v-list-item>

      <v-card-actions>
      <v-btn
        outlined
        rounded
        text
        v-if="type !== 'done'"
        @click="onDone(type, index)"
      >
        Done
      </v-btn>
      </v-card-actions>
    </v-card>
  </div>
</template>

<script lang="ts">
  import Vue from 'vue'
  import draggable from 'vuedraggable'

  export default Vue.extend({
    components: {
      draggable
    },
    props: {
      todo: {
        type: Object,
        required: true,
        default: {
          name: '',
          description: ''
        }
      },
      index: {
        type: Number,
        required: true,
        default: 0
      },
      type: {
        type: String,
        required: true
      },
      onDone: {
        type: Function,
        required: true
      } 
    }
  })
</script>
