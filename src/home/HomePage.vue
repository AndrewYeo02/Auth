<template>
    <div id="all-trainees">
        <h1>All Trainees</h1>

        <p><router-link :to="{ name: 'create_trainee' }" class="btn btn-primary pull-right">Create Trainee</router-link>
            <router-link :to="{name: 'view_logbooks'}" class="btn btn-warning">View Logbooks </router-link>
            <router-link :to="{name: 'view_tasks'}" class="btn btn-warning">View Tasks</router-link>
        </p>
        <div class="form-group">
            <input type="text" name="search" v-model="traineeSearch" placeholder="Search Username" class="form-control" v-on:keyup="searchTrainees">
        </div>

        <table class="table table-hover">
            <thead>
            <tr>
                <td>ID</td>
                <td>Username</td>
                <td>First Name</td>
                <td>Last Name</td>
                <td>Email</td>
                <td>Actions</td>
                <td>
                </td>
                
            </tr>
            </thead>

            <tbody>
                
                <tr v-for="trainee in trainees">
                    <td>{{ trainee.id }}</td>
                    <td>{{ trainee.username }}</td>
                    <td>{{ trainee.fname }}</td>
                    <td>{{ trainee.surname }}</td>
                    <td>{{ trainee.email }}</td>
                    <!-- <td>{{ trainee.price }}</td> -->
                     <td>
                        <router-link :to="{name: 'edit_trainee', params: { id: trainee.id }}" class="btn btn-primary">Edit</router-link>
                        <router-link :to="{name: 'delete_trainee', params: { id: trainee.id }}" class="btn btn-danger">Delete</router-link>
                        <router-link :to="{name: 'view_logbook', params: { id: trainee.id }}" class="btn btn-info">View Logbook</router-link>

                      
                    </td>
                </tr>
            </tbody>
        </table>
        <p>
            <router-link to="/login">Logout</router-link>
        </p>

    </div>








    


</template>

<script>
import { mapState, mapActions } from 'vuex'

export default {
     data(){
            return{
                trainees: [],
                originalTrainees: [],
                traineeSearch: ''
            }
        },

        created:function () {
            this.fetchTraineeData();
            this.getAllUsers();
        },
    computed: {
        ...mapState({
            account: state => state.account,
            users: state => state.users.all
        })
    },
    methods: {
        ...mapActions('users', {
            getAllUsers: 'getAll',
            deleteUser: 'delete'
        }),
         fetchTraineeData: function()
            {
                this.$http.get('http://192.168.1.66/api/trainees').then((response) => {
                    this.trainees = response.body;
                    this.originalTrainees = this.trainees;
                }, (response) => {

                });
            },

            searchTrainees: function()
            {
                if(this.traineeSearch == '')
                {
                    this.trainees = this.originalTrainees;
                    return;
                }

                var searchedTrainees = [];
                for(var i = 0; i < this.originalTrainees.length; i++)
                {
                    var traineeName = this.originalTrainees[i]['username'].toLowerCase();
                    if(traineeName.indexOf(this.traineeSearch.toLowerCase()) >= 0)
                    {
                        searchedTrainees.push(this.originalTrainees[i]);
                    }
                }

                this.trainees = searchedTrainees;
            }
        
    }
}
</script>