<template>
  <div class='ui basic content center aligned segment'>
    <button class='ui basic button icon' v-on:click="openForm" v-show="!isCreating">
      <i class='plus icon'></i>
    </button>
    <div class='ui centered card' v-show="isCreating">
      <div class='content'>
        <div class='ui form'>
          <div class='field'>
            <label>Title</label>
            <!-- eslint-disable-next-line -->
            <input v-model="titleText" type="text" ref="title" defaultValue="" v-validate="'min:2'" name="title">
            <p class="alert" v-if="errors.has('title')">{{ errors.first('title') }} </p>
          </div>
          <div class='field'>
            <label>Project</label>
            <!-- eslint-disable-next-line -->
            <input v-model="projectText" type="text"  ref="project" defaultValue="" v-validate="'min:5'" name="project">
            <p class="alert" v-if="errors.has('project')">{{ errors.first('project') }} </p>
          </div>
          <div class='ui two button attached buttons'>
            <button class='ui basic blue button' v-on:click="sendForm()">
              Create
            </button>
            <button class='ui basic red button' v-on:click="closeForm">
              Cancel
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      titleText: '',
      projectText: '',
      isCreating: false,
    };
  },
  methods: {
    openForm() {
      this.isCreating = true;
    },
    closeForm() {
      this.isCreating = false;
    },
    sendForm() {
      this.$validator.validateAll().then((result) => {
        if (result) {
          const title = this.titleText;
          const project = this.projectText;
          this.$emit('create-todo', {
            title,
            project,
            done: false,
          });
          this.newTodoText = '';
          this.isCreating = false;
        } else {
          console.log('Inputs not valid');
        }
      });
    },
  },
};
</script>

<style>
  .alert {
    background-color: #fdf2ce;
    font-weight: bold;
    display: inline-block;
    padding: 5px;
    margin-top: -20px;
  }
</style>
