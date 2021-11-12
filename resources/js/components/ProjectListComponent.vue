<template>
    <div>
        <h1>List of projects</h1>
        <table border="1px">
            <thead>
                <tr>
                    <th scope="col">Name</th>
                    <th scope="col">Introduction</th>
                    
                </tr>
            </thead>
            <tbody>
                <tr v-for="project in projects">
                    <td v-text="project.name"></td>
                    <td v-text="project.introduction"></td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    data () {
        return {
        projects: []
        }
    },
    mounted () {
        axios.get('http://127.0.0.1:8000/projects-vue-list')
        .then((response) => {
            console.log(response)
            this.projects = response.data.projects
            // console.log(this.projects)
            // console.log('done')
        })
        .catch((error) => {
            console.log('error')
        })
    }
}
</script>
