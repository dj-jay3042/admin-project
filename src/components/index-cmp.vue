<template>
<div>
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
                <button class="small-box-footer link w-100">Current Time <i class="fas fa-arrow-circle-right"></i></button>
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
                    <i class="fas fa-user-clock"></i>
                </div>
                <button class="small-box-footer link w-100">Worked Hours <i class="fas fa-arrow-circle-right"></i></button>
            </div>
        </div>
    </div>
</div>
<div class="row">
    <div class="col-12">
        <div class="card">
            <div class="card-header">
                <h3 class="card-title"><b>Select Columns :</b></h3><br>

                <div class="form-group clearfix w-100">
                    <div class="icheck-primary d-inline" v-for="item, index in custField" v-bind:key="item">
                        <input type="checkbox" :id="'checkboxPrimary' + index" v-model="fields" :value="item">
                        <label :for="'checkboxPrimary' + index"> {{ item }}
                        </label>&nbsp;&nbsp;&nbsp;&nbsp;
                    </div>
                </div>
            </div>
            <div class="card-header">
                <h3 class="card-title"><b>Apply Filter :</b></h3><br>

                <div class="input-group input-group mb-3">
                    <div class="input-group-prepend">
                        <select class="btn btn-secondary dropdown-toggle" v-model="FltrType">
                            <option value="" selected disabled>Select Field</option>
                            <option v-for="item in columns" v-bind:key="item" :value="item">{{ item }}</option>
                        </select>
                    </div>
                    <!-- /btn-group -->
                    <input type="text" class="form-control" v-model="FltrVal" placeholder="Enter Filter Value......">
                    <span class="input-group-append">
                        <button type="submit" class="btn btn-info btn-flat" v-on:click="applyFilter()">Go!</button>
                    </span>
                    <span class="input-group-append">
                        <button type="button" class="btn btn-success btn-flat" id="btnAdd" v-on:click="addFilter()">+ Add Filter</button>
                    </span>
                </div>

                <div id="container">
                    <div v-for="filter in filters" v-bind:key="filter" :id="filter">
                        <div class="input-group input-group mb-3">
                            <div class="input-group-prepend">
                                <select name="fltrType" class="btn btn-secondary dropdown-toggle" :id="'type' + filter" v-on:change="validateFilter(filter)">
                                    <option value="" selected disabled>Select Field</option>
                                    <option v-for="item in columns" v-bind:key="item" :value="item">{{ item }}</option>
                                </select>
                            </div>
                            <!-- /btn-group -->
                            <input type="text" class="form-control" name="fltrVal" placeholder="Enter Filter Value......" :id="'val' + filter">
                            <span class="input-group-append">
                                <button type="submit" class="btn btn-danger btn-flat" v-on:click="removeFilter(filter)">- Remove Filter</button>
                            </span>
                        </div>
                        <div class="alert alert-danger" v-if="errorId == filter">
                            <h5><i class="icon fas fa-ban"></i> Alert!</h5>
                            Can not select same filter value
                        </div>
                    </div>
                </div>
            </div>
            <div class="card-header">
                <div class="btn-group w-50">
                    <button type="button" class="btn btn-secondary" v-on:click="createExcel()">
                        <i class="fas fa-file-excel"></i>&nbsp; Create Excel Sheet
                    </button>
                    <button type="button" class="btn btn-secondary" v-on:click="createPDF()">
                        <i class="fas fa-file-pdf"></i>&nbsp; Create PDF File
                    </button>
                    <button type="button" class="btn btn-secondary" v-on:click="createCSV()">
                        <i class="fas fa-file-csv"></i>&nbsp; Create CSV File
                    </button>
                </div>
            </div>
            <div class="card-body">
                <table id="example1" class="table table-bordered table-striped">
                    <thead>
                        <tr>
                            <th>
                                <div class="custom-control custom-checkbox">
                                    <input class="custom-control-input custom-control-input-success custom-control-input-outline" type="checkbox" id="customCheckbox5" v-on:change="toggleCheckboxes" v-model="selectAll" :checked="chkAll" />
                                    <label for="customCheckbox5" class="custom-control-label">Id</label>
                                </div>
                            </th>
                            <th v-for="item in fields" v-bind:key="item">{{ item }}</th>
                            <th></th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(item, index) in data.slice(startIndex, endIndex + 1)" v-bind:key="item.id" :id="'row' + item.id">
                            <td>
                                <div class="custom-control custom-checkbox">
                                    <input class="custom-control-input custom-control-input-success" type="checkbox" :id="'chk' + item.id" v-model="selectedRows" :value="item.id" :checked="chkAll" />
                                    <label :for="'chk' + item.id" class="custom-control-label">{{ getSequentialId(index) }}</label>
                                </div>
                            </td>

                            <td v-for="fld in fields" v-bind:key="fld">
                                <input type="text" class="form-control" v-if="editId == item.id" v-model="update[fld]" :id="fld+item.id"/>
                                <p v-else>{{ item[fld] }}</p>
                            </td>

                            <td>
                                <div v-if="editId == item.id">
                                    <button type="button" class="btn btn-block btn-primary" v-on:click="updateUser()">Update</button>
                                </div>
                                <div v-else>
                                    <button class="btn btn-app bg-success toastrDefaultSuccess" v-on:click="editId=item.id; edit(item.id)">
                                        <i class="fas fa-edit"></i><strong>Edit </strong>
                                    </button>
                                    <button class="btn btn-app bg-danger" v-on:click="remove(item.id)">
                                        <i class="fas fa-trash"></i><strong>Delete </strong>
                                    </button>
                                </div>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
            <div class="card-footer">
                <div class="col-sm-12 col-md-7">
                    <div class="dataTables_paginate paging_simple_numbers" id="example1_paginate">
                        <ul class="pagination">
                            <li class="paginate_button page-item previous" id="example1_previous" :class="{ disabled: currentPage === 1 }">
                                <button class="page-link" v-on:click="previousPage">
                                    Previous
                                </button>
                            </li>
                            <li class="paginate_button page-item" :class="{ active: currentPage === page }" v-for="page in totalPages" :key="page" @click="currentPage = page">
                                <button class="page-link">{{ page }}</button>
                            </li>
                            <li class="paginate_button page-item next" id="example1_next" :class="{ disabled: currentPage === totalPages }">
                                <button class="page-link" v-on:click="nextPage">
                                    Next
                                </button>
                            </li>
                        </ul>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
</template>

<script>
import axios from "axios";
import $ from "jquery";

export default {
    name: "indexFile",
    data() {
        return {
            custField: ['username', 'password', 'usertype'],
            fields: ['username', 'password', 'usertype'],
            countFilter: 0,
            filters: [],
            fltrType: [],
            fltrVal: [],
            FltrType: "",
            FltrVal: "",
            columns: [],
            data: [],
            Data: [],
            Time: "00:00:00",
            time: new Date().toLocaleTimeString(),
            currentDate: "",
            selectAll: false,
            chkAll: false,
            checkedItems: [],
            currentPage: 1,
            itemsPerPage: 5,
            totalItems: 0,
            selectedRows: [],
            editId: "",
            errorId: "",
            update: []
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
            if (dTime > ((3 * 60 * 60 * 1000) + (45 * 60 * 1000)))
                dTime -= (45 * 60 * 1000);
            var h = Math.floor(dTime / (1000 * 60 * 60));
            var m = Math.floor((dTime % (1000 * 60 * 60)) / (1000 * 60));
            var s = Math.floor((dTime % (1000 * 60)) / 1000);
            this.Time =
                (h < 10 ? "0" + h : h) +
                ":" +
                (m < 10 ? "0" + m : m) +
                ":" +
                (s < 10 ? "0" + s : s);
        }, 1);
    },
    methods: {
        getSequentialId(index) {
            return index + this.startIndex + 1;
        },
        toggleCheckboxes() {
            if (this.selectAll) {
                this.chkAll = true;
                this.selectedRows = this.data.map(item => item.id);
            } else {
                this.chkAll = false;
                this.selectedRows = [];
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
        loadColumn() {
            axios
                .get("http://127.0.0.1:8000/api/data/getColumns")
                .then((response) => {
                    this.columns = response.data;
                    this.fields = [...this.columns];
                    this.fields.shift();
                })
                .catch((error) => {
                    alert("Something went wrong while loading data!");
                    console.error(error);
                });
        },
        loadData() {
            axios
                .get("http://127.0.0.1:8000/api/data/getUser")
                .then((response) => {
                    this.data = response.data;
                    this.Data = response.data;
                    this.totalItems = this.data.length;
                })
                .catch((error) => {
                    alert("Something went wrong while loading data!");
                    console.error(error);
                });
        },
        loadCurrentDate() {
            const date = new Date();
            const options = {
                day: "numeric",
                month: "long",
                year: "numeric",
            };
            this.currentDate = date.toLocaleDateString("en-US", options);
        },
        edit(id) {
            var item = this.data.find((item) => item.id === id);
            this.fields.forEach(element => {
                this.update[element] = item[element];
            });
        },
        updateUser() {
            var update = {};
            for (let key in this.update) {
                update[key] = this.update[key];
            }
            axios
                .post("http://127.0.0.1:8000/api/data/updateUser", {
                    id: this.editId,
                    data: update,
                })
                .then((response) => {
                    console.log(response);
                    this.loadData();
                    this.editId = '';
                })
                .catch((error) => {
                    alert("Something went wrong while updating data!");
                    console.error(error);
                });
        },
        remove(id) {
            axios
                .post("http://127.0.0.1:8000/api/data/deleteUser", {
                    id: id,
                })
                .then((response) => {
                    console.log(response);
                    this.loadData();
                })
                .catch((error) => {
                    alert("Something went wrong while deleting data!");
                    console.error(error);
                });
        },
        createExcel() {
            const requestData = {
                fltrType: this.fltrType,
                fltrVal: this.fltrVal,
                fields: this.fields,
                ids: this.selectedRows
            };

            axios
                .get("http://127.0.0.1:8000/api/data/createExcel", {
                    responseType: 'blob',
                    params: requestData
                })
                .then(response => {
                    const url = window.URL.createObjectURL(new Blob([response.data]));
                    const link = document.createElement('a');
                    link.href = url;
                    link.setAttribute('download', 'data.xlsx');
                    document.body.appendChild(link);
                    link.click();
                })
                .catch(error => {
                    console.error(error);
                });
        },
        createPDF() {
            const requestData = {
                fltrType: this.fltrType,
                fltrVal: this.fltrVal,
                fields: this.fields,
                ids: this.selectedRows
            };

            axios
                .get("http://127.0.0.1:8000/api/data/createPDF", {
                    responseType: 'blob',
                    params: requestData
                })
                .then(response => {
                    const url = window.URL.createObjectURL(new Blob([response.data]));
                    const link = document.createElement('a');
                    link.href = url;
                    link.setAttribute('download', 'data.pdf');
                    document.body.appendChild(link);
                    link.click();
                    document.body.removeChild(link);
                })
                .catch(error => {
                    console.error(error);
                });
        },
        createCSV() {
            const requestData = {
                fltrType: this.fltrType,
                fltrVal: this.fltrVal,
                fields: this.fields,
                ids: this.selectedRows
            };

            axios
                .get("http://127.0.0.1:8000/api/data/createCSV", {
                    responseType: 'blob',
                    params: requestData
                })
                .then(response => {
                    const url = window.URL.createObjectURL(new Blob([response.data]));
                    const link = document.createElement('a');
                    link.href = url;
                    link.setAttribute('download', 'data.csv');
                    document.body.appendChild(link);
                    link.click();
                })
                .catch(error => {
                    console.error(error);
                });
        },
        addFilter() {
            if (this.countFilter < this.custField.length) {
                this.countFilter++;
                this.filters.push("fltr" + this.countFilter);
            }
        },
        removeFilter(id) {
            this.countFilter--;
            $("#" + id).remove();
        },
        getValues() {
            this.fltrType = [];
            this.fltrVal = [];
            this.fltrVal.push(this.FltrVal);
            this.fltrType.push(this.FltrType);
            this.filters.forEach(element => {
                this.fltrVal.push($("#val" + element).val());
                this.fltrType.push($("#type" + element).val());
            });
        },
        applyFilter() {
            this.getValues();
            if (this.errorId === "") {
                const filteredData = this.Data.filter((item) => {
                    return this.fltrType.every((field, index) => {
                        return String(item[field]) === this.fltrVal[index];
                    });
                });
                this.totalItems = filteredData.length;
                this.data = [...filteredData];
            }
        },
        validateFilter(filter) {
            this.getValues();
            var set = new Set(this.fltrType);
            if (this.fltrType.length !== set.size)
                this.errorId = filter;
            else
                this.errorId = "";
        },
    },
    beforeMount() {
        this.loadCurrentDate();
        this.loadColumn();
        this.loadData();
    },
};
</script>

<style scoped>
.display {
    display: none;
}
</style>