<template>
    <div>
        <div>
            <h1>Project form</h1>
            <label>Name</label>
            <input id="name" v-model="name" type="text">
            <br>
            <label>Introduction</label>
            <input id="introduction" v-model="introduction" type="text">
            <br>
            <button type="button" @click="saveForm()">SAVE</button>
        </div>
        <hr>
        <h1>List of projects</h1>
        <table border="1px">
            <thead>
                <tr>
                    <th scope="col">Name</th>
                    <th scope="col">Introduction</th>
                    <th scope="col">Actions</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="project in projects">
                    <td v-text="project.name"></td>
                    <td v-text="project.introduction"></td>
                    <td>
                        <button type="button" @click="deleteRecord(project)">DELETE</button>
                    </td>
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
            name:"",
            introduction:"",
            projects: []
        }
    },
    mounted () {
        /*
        * Request that gets the saved data.
        */
        axios.get('http://127.0.0.1:8000/projects-vue-list')
        .then((response) => {
            // console.log(response)
            this.projects = response.data.projects
            // console.log(this.projects)
            // console.log('done')
        })
        .catch((error) => {
            console.log('error')
        })
    },
    methods: {
        /*
        * Method for saving data.
        */
        saveForm () {
            let url = 'http://127.0.0.1:8000/projects'
            axios.post(url,{
                'name':this.name,
                'introduction':this.introduction,
            }).then(function (response) {
                // console.log('done')
                location.href = '/projects'
            })
            .catch(function (error) {
                console.log(error);
            });
        },
        /*
        * Method that allows you to delete a record.
        */
        deleteRecord (data) {
            let project_id = data.id
            if (confirm('Are you sure you want to delete the record?')) {
                axios.delete('http://127.0.0.1:8000/projects/'+project_id
                ).then(function (response) {
                    // console.log('done')
                    location.href = '/projects'
                })
                .catch(function (error) {
                    console.log(error);
                });
            }
        },
    }
}
</script>

