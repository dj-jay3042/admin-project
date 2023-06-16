<template>
<div>
    <div class="wrapper">
        <div class="content-wrapper">
            <!-- Main content -->
            <section class="content">
                <div class="container-fluid">
                    <div class="row">
                        <div class="col-lg-6">
                            <!-- small box -->
                            <div class="small-box bg-success">
                                <div class="inner">
                                    <h3>{{ time }}</h3>

                                    <p>{{ currentDate }}</p>
                                </div>
                                <div class="icon">
                                    <i class="ion ion-clock"></i>
                                </div>
                                <a href="#" class="small-box-footer">Current Time <i class="fas fa-arrow-circle-right"></i></a>
                            </div>
                        </div>
                        <div class="col-lg-6">
                            <!-- small box -->
                            <div class="small-box bg-danger">
                                <div class="inner">
                                    <h3>{{ Time }}</h3>

                                    <p>{{ currentDate }}</p>
                                </div>
                                <div class="icon">
                                    <i class="ion ion-clock"></i>
                                </div>
                                <a href="#" class="small-box-footer">Worked Hours <i class="fas fa-arrow-circle-right"></i></a>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="row">
                    <div class="col-12">
                        <div class="card">
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
                                        <tr v-for="item, index in data.slice(startIndex, endIndex + 1)" v-bind:key="item.id" :id="'row' + item.id">
                                            <td>
                                                <div class="custom-control custom-checkbox">
                                                    <input class="custom-control-input custom-control-input-success" type="checkbox" :id="'chk' + item.id" :checked="chkAll">
                                                    <label :for="'chk' + item.id" class="custom-control-label">{{ getSequentialId(index) }}</label>
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
                                    <tfoot>
                                        <tr>
                                            <td colspan="5">
                                                <div class="col-sm-12 col-md-7">
                                                    <div class="dataTables_paginate paging_simple_numbers" id="example1_paginate">
                                                        <ul class="pagination">
                                                            <li class="paginate_button page-item previous" id="example1_previous" :class="{'disabled': currentPage === 1}"><button class="page-link" @click="previousPage">Previous</button></li>
                                                            <li class="paginate_button page-item" :class="{ 'active': currentPage === page }" v-for="page in totalPages" :key="page" @click="currentPage = page"><button class="page-link">{{ page }}</button></li>
                                                            <li class="paginate_button page-item next" id="example1_next" :class="{'disabled': currentPage === totalPages}"><button class="page-link" @click="nextPage">Next</button></li>
                                                        </ul>
                                                    </div>
                                                </div>
                                            </td>
                                        </tr>
                                    </tfoot>
                                </table>
                            </div>
                        </div>
                    </div>
                </div>
            </section>
        </div>
    </div>
</div>
</template>

<script>
import axios from 'axios';

export default {
    name: "indexFile",
    data() {
        return {
            count: 0,
            data: [],
            Time: "09:30:00",
            time: new Date().toLocaleTimeString(),
            currentDate: '',
            selectAll: false,
            chkAll: false,
            checkedItems: [],
            currentPage: 1,
            itemsPerPage: 10,
            totalItems: 0,
        };
    },
    computed: {
        startIndex() {
            return (this.currentPage - 1) * this.itemsPerPage;
        },
        endIndex() {
            return this.startIndex + this.itemsPerPage - 1;
        },
        totalPages() {
            return Math.ceil(this.totalItems / this.itemsPerPage);
        },
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
        getSequentialId(index) {
            return index + this.startIndex + 1;
        },
        toggleCheckboxes() {
            if (this.selectAll) {
                this.chkAll = true;
            } else {
                this.chkAll = false;
            }
        },
        previousPage() {
            if (this.currentPage > 1) {
                this.currentPage--;
            }
        },
        nextPage() {
            if (this.currentPage < this.totalPages) {
                this.currentPage++;
            }
        },
        loadData() {
            axios.get('http://127.0.0.1:8000/api/data/getUser')
                .then(response => {
                    this.data = response.data;
                    this.totalItems = this.data.length;
                })
                .catch(error => {
                    alert("Something went wrong while loading data!");
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
                    this.loadData();
                })
                .catch(error => {
                    alert("Something went wrong while deleting data!");
                    console.error(error);
                });
        },
    },
    beforeMount() {
        this.loadData(); {
            const date = new Date();
            const options = {
                day: 'numeric',
                month: 'long',
                year: 'numeric'
            };
            this.currentDate = date.toLocaleDateString('en-US', options);
        }
    },
};
</script>
