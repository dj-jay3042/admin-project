<template>
<div>
    <div class="row">
        <!-- left column -->
        <div class="col-md-12">
            <!-- general form elements -->
            <div class="card card-primary">
                <div class="card-header">
                    <h3 class="card-title">Add New User</h3>
                </div>
                <!-- /.card-header -->
                <!-- form start -->
                <form>
                    <div class="card-body">
                        <div class="form-group">
                            <label for="exampleInputEmail1">Username</label>
                            <input type="text" class="form-control" id="exampleInputEmail1" placeholder="Enter username" v-model="username">
                        </div>
                        <div class="form-group">
                            <label for="passwd">Password</label>
                            <div class="input-group input-group">
                                <input :type="showPassword ? 'text' : 'password'" id="passwd" class="form-control" placeholder="Password" v-model="password">
                                <span class="input-group-text" v-on:click="showPassword = !showPassword"><i :class="showPassword ? 'fas fa-eye-slash' : 'fas fa-eye'"></i></span>
                            </div>
                        </div>
                        <div class="form-group">
                            <label for="utype">User Type</label>
                            <select class="form-control" id="utype" v-model="usertype">
                                <option value="" selected disabled>-- Select User type --</option>
                                <option value="1">Admin</option>
                                <option value="0">User</option>
                            </select>
                        </div>
                    </div>

                    <!-- /.card-body -->

                    <div class="card-footer">
                        <button type="submit" class="btn btn-primary" v-on:click="addUser()">Submit</button>
                    </div>
                </form>
            </div>
            <!-- /.card -->
        </div>
    </div>
</div>
</template>

<script>
import axios from 'axios';

export default {
    name: "formFile",
    data() {
        return {
            username: "",
            password: "",
            usertype: "",
            showPassword: false,
        }
    },
    props: {
        getTab: Function
    },
    methods: {
        addUser() {
            axios.post('http://127.0.0.1:8000/api/data/addUser', {
                    username: this.username,
                    password: this.password,
                    usertype: this.usertype,
                })
                .then(response => {
                    console.log(response);
                    this.getTab("indexFile");
                })
                .catch(error => {
                    alert("Something Went Wrong!!!");
                    console.error(error);
                });
        }
    }
};
</script>
