<template>
  <div class="home">
    <v-text-field
      v-model="newTaskTitle"
      @click:append="addTask"
      @keyup.enter="addTask"
      class="pa-3"
      outlined
      label="Add Task"
      append-icon="mdi-plus"
      hide-details
      clearable
    ></v-text-field>
    <v-list
      class="pt-0"
      flat
    >
      <div
        v-for="task in tasks"
        :key="task.id"
      >
        <v-list-item
          @click="doneTask(task.id)"
          :class="{'blue lighten-5' : task.done}"
        >
          <template v-slot:default>
            <v-list-item-action>
              <v-checkbox
                :input-value="task.done"
                color="primary"
              ></v-checkbox>
            </v-list-item-action>

            <v-list-item-content>
              <v-list-item-title
                :class="{'text-decoration-line-through' : task.done}"
              >{{ task.title }}</v-list-item-title>
            </v-list-item-content>

            <v-list-item-action>
              <v-btn 
                icon
                @click.stop="confirmDeleteTask(task.id)"
              >
                <v-icon color="primary lighten-1">mdi-delete</v-icon>
              </v-btn>
            </v-list-item-action>
          </template>
        </v-list-item>
        <v-divider></v-divider>
      </div>

    </v-list>

    <v-dialog
      v-model="deleteConfimation"
      persistent
      max-width="290"
    >
      <v-card>
        <v-card-title class="headline">
          Are you sure delete this task ?
        </v-card-title>
        <v-card-text>The data that you delete can't be restore.</v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="red darken-1"
            text
            @click="deleteConfimation = false"
          >
            Disagree
          </v-btn>
          <v-btn
            color="green darken-1"
            text
            @click="deleteTask()"
          >
            Agree
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>

export default {
  name: 'Todo',
  data() {
    return {
      deleteConfimation: false,
      selectedId: null,
      newTaskTitle: '',
      tasks: [
        {
          id: 1,
          title: 'Wake up',
          done:false
        },
        {
          id: 2,
          title: 'Tahe a bath',
          done:false
        },
        {
          id: 3,
          title: 'Eat something',
          done:false
        },
        {
          id: 4,
          title: 'Go to work',
          done:false
        },
      ]
    }
  },
  methods: { 
    addTask() {
      let newTask = {
        id: Date.now(),
        title: this.newTaskTitle,
        done: false
      }
      this.tasks.push(newTask)
      this.newTaskTitle = ''
    },
    doneTask(id) {
      let task = this.tasks.filter(task => task.id === id)[0]
      task.done = !task.done
    },
    confirmDeleteTask(id) {
      this.deleteConfimation = true,
      this.selectedId = id
    },
    deleteTask() {
      this.tasks = this.tasks.filter(task => task.id !== this.selectedId)
      this.deleteConfimation = false
    } 
  }
}
</script>
