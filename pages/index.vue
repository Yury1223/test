<template>
  <div>

    <v-container>
      <v-row>
        <v-col cols="12" sm="6">
          <h1>TODO LIST</h1>
        </v-col>
      </v-row>
      <v-row>
        <v-col cols="12" sm="6">
          <v-text-field v-model="todo" label="Solo"></v-text-field>
          <p>{{todo}}</p>
        </v-col>
      </v-row>
      <v-row>
        <v-col cols="12" sm="6">
          <v-btn elevation="2" @click="apply()">apply</v-btn>
        </v-col>
      </v-row> 
    </v-container>

    <v-container>  
      <v-row>
        <v-col cols="12" sm="6">
          <v-card
            v-for="(todo, index) in todos" :key="todo"
            class="mx-auto"
            max-width="344"
            outlined
          >
            <v-list-item one-line>
              <v-list-item-content>
                <div class="text-overline mb-4">
                  {{ todo }}
                </div>
              </v-list-item-content>
            </v-list-item>

            <v-card-actions>
              <v-btn
                outlined
                rounded
                text
                @click="done(index)"
              >
                Done
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-col>
      </v-row> 
    </v-container>

  </div>
</template>

<script lang="ts">
  import Vue from 'vue'

  export default Vue.extend({
    data: () => ({
      todo: '',
      todos: [] as string[]
    }),
    methods: {
      apply: function () {
        this.todos.push(this.todo)
        this.todo = ''
      },
      done: function (targetIndex: number = 0) {
        this.todos = this.todos.filter((_, index) => {
          return index !== targetIndex
        })
      }
    }
  })
</script>
