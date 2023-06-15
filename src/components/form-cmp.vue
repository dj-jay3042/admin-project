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
                            <h1 class="m-0">Forms</h1>
                        </div>
                        <!-- /.col -->
                        <div class="col-sm-6">
                            <ol class="breadcrumb float-sm-right">
                                <li class="breadcrumb-item"><a href="#">Home</a></li>
                                <li class="breadcrumb-item active">Form</li>
                            </ol>
                        </div>
                        <!-- /.col -->
                    </div>
                    <!-- /.row -->
                </div>
                <!-- /.container-fluid -->
            </div>
            <!-- /.content-header -->
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
    name: "formFile",
    components: {
        headerFile,
        sidebarFile,
        footerFile,
    },
};
</script>
