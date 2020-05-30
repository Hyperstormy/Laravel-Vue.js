<template>
    <div class="container">
        <h3> People Data Using LARAVEL + VUE.JS </h3>
              <form @submit.prevent="addPeople(people.id)">
                <input type="text" placeholder="First Name" v-model="people.fname">
                <input type="text" placeholder="Middle Name" v-model="people.mname">
                <input type="text" placeholder="Last Name" v-model="people.lname">
                <input type="text" placeholder="Sex" v-model="people.sex">
                <input type="text" placeholder="House Number" v-model="people.house_number">
                <button type="submit" class="btn btn-primary"> Submit</button>
            </form>

        <table class="table">
            <thead>
                <tr>
                    <th>First Name</th>
                    <th>Middle Name</th>
                    <th>Last Name</th>
                    <th>Sex</th>
                    <th>House Number</th>
                    <th>Remove</th>
                    <th>Edit</th>
                </tr>
            </thead>

                <tr v-for="people in peoples" v-bind:key="people.id">
                    <td> {{ people.fname }} </td>
                    <td> {{ people.mname }} </td>
                    <td> {{ people.lname }} </td>
                    <td> {{ people.sex }} </td>
                    <td> {{ people.house_number }} </td>
                    <td> <button @click="Remove(people.id)" class="btn btn-danger"> Delete </button></td>
                    <td> <button @click="editPeople(people)" class="btn btn-primary"> Edit </button></td>
                </tr>

        </table>

    </div>

</template>

<script>
export default {
    data(){
        return{
            peoples:[],
            people:{
                id: '',
                fname: '',
                mname: '',
                lname: '',
                sex: '',
                house_number:''
            },
            edit: false   
        };
    },

    created: function(){
        this.fetchPeoples();
    },

    methods:{
        fetchPeoples(page_url){
            page_url = page_url || '/api';
            fetch(page_url)
            .then(res => res.json())
            .then(resJson => {
                this.peoples = resJson
            })
            .catch(err => console.log(err))
        },

        Remove(id,page_url){
            page_url = page_url || `/api/${id}`;
            fetch(page_url,{
                method: 'delete'
            })
            .then(res => res.json())
            .then(data =>{
                this.fetchPeoples();
            })
            .catch(err => console.log(err))
        },

        addPeople(id){
            if(this.people.id == 0){
                fetch('/api',{
                    method: "post",
                    headers:{
                        'content-type' : 'application/json'
                    },
                    body:JSON.stringify(this.people)
                })
                .then(res => res.json())
                .then(data =>{
                    this.clearForm();
                    this.fetchPeoples();
                })
                .catch(err => console.log(err))
            }
            else{
                fetch(`/api/${id}`, {
                    method: "put",
                    headers:{
                        'content-type' : 'application/json'
                    },
                    body:JSON.stringify(this.people)
                })
                .then(res => res.json())
                .then(data =>{
                    this.clearForm();
                    this.fetchPeoples();
                })
                .catch(err => console.log(err))
            }
        },

        clearForm(){
            this.edit = false;
            this.people.id = null,
            this.people.fname = '',
            this.people.mname = '',
            this.people.lname = '',
            this.people.sex = '',
            this.people.house_number = ''
        },

        editPeople(people){
            this.people.id = people.id,
            this.people.fname = people.fname,
            this.people.mname = people.mname,
            this.people.lname = people.lname,
            this.people.sex = people.sex,
            this.people.house_number = people.house_number
        }
    }
}
</script>