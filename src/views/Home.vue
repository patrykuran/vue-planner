<template>
  <div class="home">
    <FilterNav @filterChange="current = $event" :current="current"/>
    <div v-if="projects.length">
      <div v-for="project in filteredProjects" :key="project.id">
        <SingleProject :project="project" @delete="handleDelete" @complete="handleComplete"/>
      </div>
    </div>
  </div>
</template>

<script>
  // @ is an alias to /src

  import SingleProject from "../components/SingleProject";
  import FilterNav from "../components/FilterNav";

  export default {
    name: 'Home',
    components: {SingleProject, FilterNav},
    data() {
      return {
        projects: [],
        current: 'all'
      }
    },
    computed: {
      filteredProjects() {
        if (this.current === 'completed') {
          return this.projects.filter(project => project.complete === true)
        }
        if (this.current === 'ongoing') {
          return this.projects.filter(project => project.complete !== true)
        }
        return this.projects
      }
    },
    mounted() {
      this.fetchAll()
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
      },
      fetchAll() {
        fetch('http://localhost:3000/projects')
          .then(res => res.json())
          .then(data => this.projects = data)
          .catch(error => console.log(error.message))
      }
    }
  }
</script>
