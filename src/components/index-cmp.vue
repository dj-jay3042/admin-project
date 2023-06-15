<template>
<div>
    <div class="wrapper">
        <headerFile />
        <sidebarFile />
        <div class="content-wrapper">
            <!-- Content Header (Page header) -->
            <div class="content-header">
                <div class="container-fluid">
                    <div class="row mb-2">
                        <div class="col-sm-6">
                            <h1 class="m-0">Dashboard</h1>
                        </div>
                        <!-- /.col -->
                        <div class="col-sm-6">
                            <ol class="breadcrumb float-sm-right">
                                <li class="breadcrumb-item"><a href="#">Home</a></li>
                                <li class="breadcrumb-item active">Dashboard v1</li>
                            </ol>
                        </div>
                        <!-- /.col -->
                    </div>
                    <!-- /.row -->
                </div>
                <!-- /.container-fluid -->
            </div>
            <!-- /.content-header -->
            <!-- Main content -->
            <section class="content">
                <div class="container-fluid">
                    <div class="row">
                        <div class="col-lg-6">
                            <!-- small box -->
                            <div class="small-box bg-success">
                                <div class="inner">
                                    <h3>{{ time }}</h3>

                                    <p>Current Time</p>
                                </div>
                                <div class="icon">
                                    <i class="ion ion-clock"></i>
                                </div>
                                <a href="#" class="small-box-footer">More info <i class="fas fa-arrow-circle-right"></i></a>
                            </div>
                        </div>
                        <div class="col-lg-6">
                            <!-- small box -->
                            <div class="small-box bg-danger">
                                <div class="inner">
                                    <h3>{{ Time }}</h3>

                                    <p>Worked Hours</p>
                                </div>
                                <div class="icon">
                                    <i class="ion ion-clock"></i>
                                </div>
                                <a href="#" class="small-box-footer">More info <i class="fas fa-arrow-circle-right"></i></a>
                            </div>
                        </div>
                    </div>
                    <!-- Small boxes (Stat box) -->
                    <div class="row">
                        <div class="col-lg-3 col-6">
                            <!-- small box -->
                            <div class="small-box bg-info">
                                <div class="inner">
                                    <h3>{{ count }}</h3>

                                    <p>New Orders</p>
                                </div>
                                <div class="icon">
                                    <i class="ion ion-bag"></i>
                                </div>
                                <a href="#" class="small-box-footer">More info <i class="fas fa-arrow-circle-right"></i></a>
                            </div>
                        </div>
                        <div class="col-lg-3 col-6">
                            <button type="button" class="btn btn-block bg-gradient-success" v-on:click="increment()">
                                Increment
                            </button>
                            <button type="button" class="btn btn-block bg-gradient-danger" v-on:click="decrement()">
                                Decrement
                            </button>
                        </div>
                    </div>
                    <!-- /.row -->
                </div>
                <div class="row">
                    <div class="col-12">
                        <div class="card">
                            <div class="card-header">
                                <h3 class="card-title">DataTable with default features</h3>
                            </div>
                            <!-- /.card-header -->
                            <div class="card-body">
                                <table id="example1" class="table table-bordered table-striped">
                                    <thead>
                                        <tr>
                                            <th>
                                                <div class="custom-control custom-checkbox">
                                                    <input class="custom-control-input custom-control-input-success custom-control-input-outline" type="checkbox" id="customCheckbox5" v-on:change="toggleCheckboxes" v-model="selectAll">
                                                    <label for="customCheckbox5" class="custom-control-label">Id</label>
                                                </div>
                                            </th>
                                            <th>Username</th>
                                            <th>Password</th>
                                            <th>User Type</th>
                                            <th></th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr v-for="item in data" v-bind:key="item.id" :id="'row' + item.id">
                                            <td>
                                                <div class="custom-control custom-checkbox">
                                                    <input class="custom-control-input custom-control-input-success" type="checkbox" :id="'chk' + item.id" v-model="selectAll">
                                                    <label :for="'chk' + item.id" class="custom-control-label">{{ item.id }}</label>
                                                </div>
                                            </td>
                                            <td :id="'row' + item.id + 'username'">{{ item.username }}</td>
                                            <td :id="'row' + item.id + 'password'">{{ item.password }}</td>
                                            <td :id="'row' + item.id + 'usertype'">{{ (item.usertype == 1) ? ("Admin") : ("User") }}</td>
                                            <td>
                                                <button class="btn btn-app bg-success toastrDefaultSuccess" v-on:click="edit(item.id)">
                                                    <i class="fas fa-edit"></i><strong>Edit </strong>
                                                </button>
                                                <button class="btn btn-app bg-danger" v-on:click="remove(item.id)">
                                                    <i class="fas fa-trash"></i><strong>Delete </strong>
                                                </button>
                                            </td>
                                        </tr>
                                    </tbody>
                                </table>
                            </div>
                        </div>
                    </div>
                </div>
            </section>
        </div>
        <footerFile />
    </div>
</div>
</template>

<script>
import headerFile from "./master/header-master.vue";
import sidebarFile from "./master/sidebar-master.vue";
import footerFile from "./master/footer-master.vue";
import axios from 'axios';

export default {
    name: "indexFile",
    data() {
        return {
            count: 0,
            data: [],
            Time: "09:30:00",
            time: new Date().toLocaleTimeString(),
            selectAll: false,
            checkedItems: []
        };
    },
    mounted() {
        setInterval(() => {
            this.time = new Date().toLocaleTimeString(undefined, {
                hour: "numeric",
                minute: "numeric",
                second: "numeric",
                hour12: true,
            });

            var cTime = new Date();
            var tTime = new Date(
                cTime.getFullYear(),
                cTime.getMonth(),
                cTime.getDate(),
                9,
                30,
                0
            );

            var dTime = cTime - tTime;
            var h = Math.floor(dTime / (1000 * 60 * 60));
            var m = Math.floor((dTime % (1000 * 60 * 60)) / (1000 * 60));
            var s = Math.floor((dTime % (1000 * 60)) / 1000);
            this.Time = ((h < 10) ? ("0" + h) : (h)) + ":" + ((m < 10) ? ("0" + m) : (m)) + ":" + ((s < 10) ? ("0" + s) : (s));
        }, 1);
    },
    methods: {
        increment() {
            this.count++;
        },
        decrement() {
            this.count != 0 ? this.count-- : this.count;
        },
        toggleCheckboxes() {
            if (this.selectAll) {
                this.checkedItems = this.data.map(item => item.id);
            } else {
                this.checkedItems = [];
            }
        },
        loadData() {
            axios.get('http://127.0.0.1:8000/api/data/getUser')
                .then(response => {
                    console.log(response.data);
                    this.data = response.data;
                })
                .catch(error => {
                    console.error(error);
                });
        },
        edit(id) {
            alert("Selected Row Id Is : row" + id);
        },
        remove(id) {
            axios.post('http://127.0.0.1:8000/api/data/deleteUser', {
                    id: id
                })
                .then(response => {
                    console.log(response);

                })
                .catch(error => {
                    console.error(error);
                });
            this.loadData();
        },
    },
    beforeMount() {
        this.loadData();
    },
    components: {
        headerFile,
        sidebarFile,
        footerFile,
    },
};
</script>
