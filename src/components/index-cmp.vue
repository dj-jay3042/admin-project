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
                    <i class="ion ion-clock"></i>
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
                    <div class="icheck-primary d-inline" v-for="item, index in fields" v-bind:key="item">
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
                    <div class="input-group input-group mb-3" v-for="filter in filters" v-bind:key="filter" :id="filter">
                        <div class="input-group-prepend">
                            <select name="fltrType" class="btn btn-secondary dropdown-toggle" :id="'type' + filter">
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
                </div>
            </div>
            <div class="card-header">
                <div class="btn-group w-50">
                    <button type="button" class="btn btn-secondary" v-on:click="createExcel()">
                        Create Excel Sheet
                    </button>
                    <button type="button" class="btn btn-secondary" v-on:click="createPDF()">
                        Create PDF File
                    </button>
                    <button type="button" class="btn btn-secondary" v-on:click="createCSV()">
                        Create CSV File
                    </button>
                </div>
            </div>
            <div class="card-body">
                <table id="example1" class="table table-bordered table-striped">
                    <thead>
                        <tr>
                            <th>
                                <div class="custom-control custom-checkbox">
                                    <input class="custom-control-input custom-control-input-success custom-control-input-outline" type="checkbox" id="customCheckbox5" v-on:change="toggleCheckboxes" v-model="selectAll" />
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
                                <p :class="(item.id == editId) ? (update) ? 'display' : '' : ''">{{ item[fld] }}</p>
                                <input type="text" :class="(item.id == editId) ? (!update) ? 'display' : '' : 'display'" :value="item[fld]" />
                            </td>

                            <td>
                                <button class="btn btn-app bg-success toastrDefaultSuccess" v-on:click="update = !update; editId=item.id;">
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
                                            <li class="paginate_button page-item previous" id="example1_previous" :class="{ disabled: currentPage === 1 }">
                                                <button class="page-link" @click="previousPage">
                                                    Previous
                                                </button>
                                            </li>
                                            <li class="paginate_button page-item" :class="{ active: currentPage === page }" v-for="page in totalPages" :key="page" @click="currentPage = page">
                                                <button class="page-link">{{ page }}</button>
                                            </li>
                                            <li class="paginate_button page-item next" id="example1_next" :class="{ disabled: currentPage === totalPages }">
                                                <button class="page-link" @click="nextPage">
                                                    Next
                                                </button>
                                            </li>
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
</template>

<script>
import axios from "axios";
import $ from "jquery";

export default {
    name: "indexFile",
    data() {
        return {
            count: 0,
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
            update: false,
            editId: 0
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
        handleChange() {
            const dropdown = document.getElementById("myDropdown");
            const selectedValues = Array.from(dropdown.selectedOptions).map(option => option.value);
            alert("kajdfjka");
            console.log(selectedValues);
        },
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
            this.editId = id;
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
        createExcel() {},
        createPDF() {},
        createCSV() {},
        addFilter() {
            if (this.countFilter < 3) {
                this.countFilter++;
                this.filters.push("fltr" + this.countFilter);
            }
        },
        removeFilter(id) {
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

            const filteredData = this.data.filter((item) => {
                return this.fltrType.every((field, index) => {
                    return String(item[field]) === this.fltrVal[index];
                });
            });
            this.totalItems = filteredData.length;
            this.data = [...filteredData];
        },
    },
    beforeMount() {
        this.loadCurrentDate();
        this.loadColumn();
        this.loadData();
    },
};
</script>

<style>
.display {
    display: none;
}
</style>
