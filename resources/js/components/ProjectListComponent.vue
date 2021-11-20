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
            <!--button type="button" @click="updateRecord()">UPDATE</!--button-->
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
                        <button type="button" @click="loadDataField(project)">UPDATE</button>
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
            projects: [],
            url: 'projects',
        }
    },
    mounted () {
        /*
        * Request that gets the saved data.
        */
        axios.get('http://127.0.0.1:8000/projects-vue-list')
        .then((response) => {
            this.projects = response.data.projects
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
            let vm = this;
            axios.post(vm.url,{
                'name':this.name,
                'introduction':this.introduction,
            }).then(function (response) {
                location.href = vm.url
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
        /*
        * Function that loads data into form fields.
        */
        loadDataField (data) {
            let vm = this;
            axios.get('http://127.0.0.1:8000/projects/'+data.id
            )
            .then(function (response) {
                vm.name = data.name;
                vm.introduction = data.introduction;
                // console.log(data.name);
            })
            .catch(function (error) {
                console.log(error);
            });
        },
    }
}
</script>
