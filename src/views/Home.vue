<template>
  <div class="home">
    <div v-if="projects.length">
      <div v-for="project in projects" :key="project.id">
        <SingleProject :project="project" @delete="handleDelete" @complete="handleComplete"/>
      </div>
    </div>
  </div>
</template>

<script>
  // @ is an alias to /src

  import SingleProject from "../components/SingleProject";
  export default {
    name: 'Home',
    components: {SingleProject},
    data() {
      return {
        projects: []
      }
    },
    mounted() {
      fetch('http://localhost:3000/projects')
        .then(res => res.json())
        .then(data => this.projects = data)
        .catch(error => console.log(error.message ))
    },
    methods: {
      handleDelete(id) {
        this.projects = this.projects.filter(project => project.id !== id)
      },
      handleComplete(id) {
        let project = this.projects.find(project => {
          return project.id === id
        });
        project.complete = !project.complete;
      }
    }
  }
</script>
