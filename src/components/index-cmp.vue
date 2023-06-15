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

                                    <p>New Orders</p>
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

                                    <p>New Orders</p>
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
                            <button type="button" class="btn btn-block bg-gradient-success" v-on:click="add()">
                                Increment
                            </button>
                            <button type="button" class="btn btn-block bg-gradient-danger" v-on:click="remove()">
                                Decrement
                            </button>
                        </div>
                    </div>
                    <!-- /.row -->
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

export default {
    name: "indexFile",
    data() {
        return {
            count: 0,
            Time: "09:30:00",
            time: new Date().toLocaleTimeString(),
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
                10,
                15,
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
        add() {
            this.count++;
        },
        remove() {
            this.count != 0 ? this.count-- : this.count;
        },
    },
    components: {
        headerFile,
        sidebarFile,
        footerFile,
    },
};
</script>
