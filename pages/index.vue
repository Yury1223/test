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
          <v-text-field v-model="name" label="タスク名を入力してください" required></v-text-field>
        </v-col>
        <v-col cols="12" sm="6">
          <v-text-field v-model="description" label="内容を入力してください" required></v-text-field>
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
        <v-col cols="12" sm="4">
          <h1>NotStarted</h1>
          <draggable v-model="notStarted" draggable=".todo" group="todos">
            <div class="todo" v-for="(todo, index) in notStarted" :key="index">
              <TodoList
                :todo="todo"
                type="notStarted"
                :onDone="onDone"
                :index="index"
              />
            </div>
          </draggable>
        </v-col>

        <v-col cols="12" sm="4">
          <h1>Doing</h1>
          <draggable v-model="doing" draggable=".todo" group="todos">
            <div class="todo" v-for="(todo, index) in doing" :key="index">
              <TodoList
                :todo="todo"
                type="doing"
                :onDone="onDone"
                :index="index"
              />
            </div>
          </draggable>
          <!-- <draggable v-model="doing" draggable=".todo" group="todos">
            <v-card
              v-for="(todo, index) in doing" :key="index"
              class="mx-auto todo"
              max-width="344"
              outlined
              draggable=".todo"
            >
              <v-list-item two-line>
                <v-list-item-content>
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
                  @click="onDone('doing', index)"
                >
                  Done
                </v-btn>
              </v-card-actions>
            </v-card>
          </draggable> -->
        </v-col>

        <v-col cols="12" sm="4">
          <h1>Done</h1>
          <draggable v-model="done" draggable=".todo" group="todos">
            <div class="todo" v-for="(todo, index) in done" :key="index">
              <TodoList
                :todo="todo"
                type="done"
                :onDone="onDone"
                :index="index"
              />
            </div>
          </draggable>
        </v-col>
      </v-row> 
    </v-container>

  </div>
</template>

<script lang="ts">
  import Vue from 'vue'
  import draggable from 'vuedraggable'
  import TodoList from '../components/TodoList.vue'

  export interface ToDo {
    name: string
    description: string
  }

  export default Vue.extend({
    data: () => ({
      name: '',
      description: '',
      notStarted: [] as ToDo[],
      doing: [] as ToDo[],
      done: [] as ToDo[]
    }),
    components: {
      draggable,
      TodoList
    },
    methods: {
      apply: function () {
        this.notStarted.push({
          name: this.name,
          description: this.description
        })

        this.name = '',
        this.description = ''
      },
      onDone: function (type: 'notStarted' | 'doing' | 'done', targetIndex: number = 0) {
        console.log(type, targetIndex)
        switch (type) {
          case 'notStarted':
            this.done.push(this.notStarted[targetIndex])
            this.notStarted = this._filter(this.notStarted, targetIndex)
            break
          case 'doing':
            this.done.push(this.doing[targetIndex])
            this.doing = this._filter(this.doing, targetIndex)
            break
        }        

        this.$forceUpdate()
      },
      _filter: function (list: ToDo[], targetIndex: number) {
        return list.filter((_, index) => {
          return index !== targetIndex
        })
      }
    }
  })
</script>
