<template lang="html">
  <div class="container">

  <div class="row asd">
    <div class="col-md-9">
      <h1> Kanban </h1>
    </div>
      <div class="col-md-3">
        <button type="submit" class="btn btn-primary" data-toggle="modal" data-target='#myModal'> [Add Task Here]</button>
      </div>
    </div>



  <div class="row">
        <div class="col-md-3">

          <div class="panel-primary">
            <div class="panel-heading">
              <h3 class="panel-title">BackLog</h3>
            </div>
            <div class="panel panel-primary" v-for="backlog in backlogs" v-if="backlog.status == 'backlog' ">
              <div class="panel-body">
                <p><strong>{{backlog.title}}</strong></p>
                <p>task point: {{backlog.point}}</p>
                <p>Assignee: {{backlog.assignee}}</p>
              </div>
              <div class="panel-footer">
                <button type="button" class="btn btn-info" @click="showTask(backlog)" data-toggle="modal" data-target="#showDetail">Show Detail</button>
              </div>
            </div>
          </div>
        </div>

        <div class="col-md-3">
          <div class="panel-danger">
            <div class="panel-heading">
              <h3 class="panel-title">To Do</h3>
            </div>
            <div class="panel panel-primary" v-for="backlog in backlogs" v-if="backlog.status == 'todo' ">
              <div class="panel-body">
                <p><strong>{{backlog.title}}</strong></p>
                <p>task point: {{backlog.point}}</p>
                <p>Assignee: {{backlog.assignee}}</p>
              </div>
              <div class="panel-footer">
                <button type="button" class="btn btn-info" @click="showTask(backlog)" data-toggle="modal" data-target="#showDetail">Show Detail</button>
              </div>
            </div>
          </div>
        </div>

        <div class="col-md-3">
          <div class="panel-info">
            <div class="panel-heading">
              <h3 class="panel-title">Doing</h3>
            </div>
            <div class="panel panel-primary" v-for="backlog in backlogs" v-if="backlog.status == 'doing' ">
              <div class="panel-body">
                <p><strong>{{backlog.title}}</strong></p>
                <p>task point: {{backlog.point}}</p>
                <p>Assignee: {{backlog.assignee}}</p>
              </div>
              <div class="panel-footer">
                <button type="button" class="btn btn-info" @click="showTask(backlog)" data-toggle="modal" data-target="#showDetail">Show Detail</button>
              </div>
            </div>
          </div>
        </div>

        <div class="col-md-3">
          <div class="panel-success">
            <div class="panel-heading">
              <h3 class="panel-title">Done</h3>
            </div>
            <div class="panel panel-primary" v-for="backlog in backlogs" v-if="backlog.status == 'done' ">
              <div class="panel-body">
                <p><strong>{{backlog.title}}</strong></p>
                <p>task point: {{backlog.point}}</p>
                <p>Assignee: {{backlog.assignee}}</p>
              </div>
              <div class="panel-footer">
                <button type="button" class="btn btn-info" @click="showTask(backlog)" data-toggle="modal" data-target="#showDetail">Show Detail</button>
              </div>
            </div>
          </div>
        </div>

      </div>

<!-- modal for add  -->
    <div class="modal" id="myModal" role="dialog">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <button type="button" class="close" data-dismiss="modal" aria-hidden="true">&times;</button>
            <h4 class="modal-title">Add Task</h4>
          </div>
          <div class="modal-body">
            <form>
              <div class="form-group">
                <p align="left">Title:</p>
                <input v-model="addTasks.title" type="text" class="form-control">
              </div>
              <div class="form-group">
                <p align="left">Description:</p>
                <input v-model="addTasks.description" type="text" class="form-control">
              </div>
              <div class="form-group">
                <p align="left">Point:</p>
                <input v-model="addTasks.point" type="text" class="form-control">
              </div>
              <div class="form-group">
                <p align="left">Assign To:</p>
                <input v-model="addTasks.assignee" type="text" class="form-control">
              </div>
            </form>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
            <button @click="addTask()" type="submit" class="btn btn-primary" data-dismiss="modal">Add Task</button>
          </div>
        </div>
      </div>
    </div>

<!-- modal to show details -->
    <div v-if="currentTask !== null" class="modal" id="showDetail" role="dialog">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <button type="button" class="close" data-dismiss="modal" aria-hidden="true">&times;</button>
            <h4 class="modal-title">Detail Task : {{ currentTask.title }} for {{ currentTask.assignee }}</h4>
          </div>
          <div class="modal-body">
            <form>
              <div class="form-group">
                <label align="left">Description: {{ currentTask.description }}</label>
                <p>Point: {{ currentTask.point }}</p>
                <p>Status: {{ currentTask.status }}</p>
              </div>
            </form>
          </div>
          <div class="modal-footer">
            <button type="button" v-if="currentTask.status == 'todo'" @click="backlog(currentTask)" class="btn btn-primary" data-dismiss="modal">Back Log</button>
            <button type="button" v-if="currentTask.status == 'doing' || currentTask.status == 'backlog'" @click="todo(currentTask)" class="btn btn-primary" data-dismiss="modal">To Do</button>
            <button type="button" v-if="currentTask.status == 'todo' || currentTask.status == 'done'" @click="doing(currentTask)" class="btn btn-warning" data-dismiss="modal">Doing</button>
            <button type="button" v-if="currentTask.status == 'doing'" @click="done(currentTask)" class="btn btn-success" data-dismiss="modal">Done</button>
            <button type="button" @click="removeTask(currentTask)" class="btn btn-danger" data-dismiss="modal">Delete</button>

          </div>
        </div>
      </div>
    </div>

<!-- penutup container diatas -->
  </div>
</template>

<script>
import firebase from 'firebase'

var config = {
  databaseURL: 'https://kanban-firebase.firebaseio.com',
  projectId: 'kanban-firebase'
}
var firebaseApp = firebase.initializeApp(config)
var db = firebaseApp.database().ref('jack-kanban')

export default {
  data () {
    return {
      currentTask: null,
      addTasks: {
        title: '',
        description: '',
        point: '',
        assignee: '',
        status: 'backlog'
      }
    }
  },
  firebase: {
    backlogs: db
  },
  methods: {
    addTask () {
      db.push(this.addTasks)
      this.addTasks.title = ''
      this.addTasks.description = ''
      this.addTasks.point = ''
      this.addTasks.assignee = ''
    },
    showTask (taskObject) {
      console.log(taskObject)
      this.currentTask = taskObject
    },
    removeTask (task) {
      alert('Do you want to delete?')
      db.child(task['.key']).remove()
    },
    backlog (tasks) {
      db.child(tasks['.key']).update({
        'status': 'backlog'
      })
    },
    todo (tasks) {
      db.child(tasks['.key']).update({
        'status': 'todo'
      })
    },
    doing (tasks) {
      db.child(tasks['.key']).update({
        'status': 'doing'
      })
    },
    done (tasks) {
      db.child(tasks['.key']).update({
        'status': 'done'
      })
    }
  }
}
</script>

<style lang="css">
h1 {
  margin-top: 0
}
.asd {
  margin-top: 3%
}
</style>
