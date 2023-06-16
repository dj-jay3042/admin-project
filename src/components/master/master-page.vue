<template>
<div class="wrapper">
    <!-- Navbar -->
    <nav class="main-header navbar navbar-expand navbar-white navbar-light">
        <!-- Left navbar links -->
        <ul class="navbar-nav">
            <li class="nav-item">
                <a class="nav-link" data-widget="pushmenu" href="#" role="button"><i class="fas fa-bars"></i></a>
            </li>
            <li class="nav-item d-none d-sm-inline-block">
                <button class="nav-link link" v-on:click="activateButton(0); getTab('indexFile');" :class="{ 'active': activeButtonIndex === 0 }">Home</button>
            </li>
            <li class="nav-item d-none d-sm-inline-block">
                <button class="nav-link link" v-on:click="activateButton(1); getTab('formFile');" :class="{ 'active': activeButtonIndex === 1 }">Add User</button>
            </li>
        </ul>

        <!-- Right navbar links -->
        <ul class="navbar-nav ml-auto">
            <!-- Navbar Search -->
            <li class="nav-item">
                <a class="nav-link" data-widget="navbar-search" href="#" role="button">
                    <i class="fas fa-search"></i>
                </a>
                <div class="navbar-search-block">
                    <form class="form-inline">
                        <div class="input-group input-group-sm">
                            <input class="form-control form-control-navbar" type="search" placeholder="Search" aria-label="Search">
                            <div class="input-group-append">
                                <button class="btn btn-navbar" type="submit">
                                    <i class="fas fa-search"></i>
                                </button>
                                <button class="btn btn-navbar" type="button" data-widget="navbar-search">
                                    <i class="fas fa-times"></i>
                                </button>
                            </div>
                        </div>
                    </form>
                </div>
            </li>
            <li class="nav-item">
                <a class="nav-link" data-widget="fullscreen" role="button">
                    <i class="fas fa-expand-arrows-alt"></i>
                </a>
            </li>
        </ul>
    </nav>
    <!-- /.navbar -->

    <!-- Main Sidebar Container -->
    <aside class="main-sidebar main-sidebar-custom sidebar-dark-primary elevation-4">
        <!-- Brand Logo -->
        <a href="../../../public/index3.html" class="brand-link">
            <img src="../../../public/dist/img/profile_pic.png" alt="Admin Logo" class="brand-image img-circle elevation-3" style="opacity: .8">
            <span class="brand-text font-weight-light">Jay Chauhan</span>
        </a>

        <!-- Sidebar -->
        <div class="sidebar">
            <!-- SidebarSearch Form -->
            <div class="form-inline">
                <div class="input-group" data-widget="sidebar-search">
                    <input class="form-control form-control-sidebar" type="search" placeholder="Search" aria-label="Search">
                    <div class="input-group-append">
                        <button class="btn btn-sidebar">
                            <i class="fas fa-search fa-fw"></i>
                        </button>
                    </div>
                </div>
            </div>

            <!-- Sidebar Menu -->
            <nav class="mt-2">
                <ul class="nav nav-pills nav-sidebar flex-column" data-widget="treeview" role="menu" data-accordion="false">
                    <!-- Add icons to the links using the .nav-icon class
               with font-awesome or any other icon font library -->
                    <li class="nav-item">
                        <button id="default" v-on:click="activateButton(0); getTab('indexFile');" :class="{ 'active': activeButtonIndex === 0 }" class="nav-link link w-100">
                            <i class="nav-icon fas fa-tachometer-alt"></i>
                            <p>
                                Dashboard
                            </p>
                        </button>
                    </li>
                    <li class="nav-item">
                        <button v-on:click="activateButton(1); getTab('formFile');" :class="{ 'active': activeButtonIndex === 1 }" class="nav-link w-100 link">
                            <i class="nav-icon fas fa-user-plus"></i>
                            <p>
                                Add User
                            </p>
                        </button>
                    </li>
                </ul>
            </nav>
            <!-- /.sidebar-menu -->
        </div>
        <!-- /.sidebar -->

        <div class="sidebar-custom">
            <button class="link btn btn-link"><i class="fas fa-cogs"></i></button>
            <strong style="color: white;"><b>Developed By <a href="https://dj-jay3042.github.io" target="_blank">Jay Chauhan</a></b></strong>
        </div>
        <!-- /.sidebar-custom -->
    </aside>

    <!-- Content Wrapper. Contains page content -->
    <div class="content-wrapper">
        <!-- Content Header (Page header) -->
        <section class="content-header">
            <div class="container-fluid">

                <!-- <slot></slot> -->

            </div><!-- /.container-fluid -->
        </section>

        <!-- Main content -->
        <section class="content">

            <div class="container-fluid">
                <slot></slot>
            </div>
        </section>
        <!-- /.content -->
    </div>
    <!-- /.content-wrapper -->

    <footer class="main-footer">
        <div class="float-right d-none d-sm-block">
            <strong>Copyright © 2014-{{ currentYear }}
                <a href="https://dj-jay3042.github.io" target="_blank">Jay Chauhan</a>.</strong>
            All rights reserved.
        </div>
    </footer>

    <!-- Control Sidebar -->
    <aside class="control-sidebar control-sidebar-dark">
        <!-- Control sidebar content goes here -->
    </aside>
    <!-- /.control-sidebar -->
</div>
<!-- ../../../public/wrapper -->
</template>

<script>
export default {
    name: "masterFile",
    data() {
        return {
            activeButtonIndex: 0,
            currentYear: new Date().getFullYear()
        };
    },
    methods: {
        activateButton(index) {
            this.activeButtonIndex = index;
        }
    },
    props: {
        getTab: Function
    },
};
</script>

<style>
.link {
    background: none;
    text-align: left;
    border: none;
}
</style>
