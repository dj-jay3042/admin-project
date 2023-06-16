<template>
<div>
    <!-- Main Sidebar Container -->
    <aside class="main-sidebar sidebar-dark-primary elevation-4">
        <!-- Brand Logo -->
        <a href="index3.html" class="brand-link">
            <img src="dist/img/AdminLTELogo.png" alt="AdminLTE Logo" class="brand-image img-circle elevation-3" style="opacity: 0.8" />
            <span class="brand-text font-weight-light">AdminLTE 3</span>
        </a>

        <!-- Sidebar -->
        <div class="sidebar">
            <!-- Sidebar user panel (optional) -->
            <div class="user-panel mt-3 pb-3 mb-3 d-flex">
                <div class="image">
                    <img src="dist/img/user2-160x160.jpg" class="img-circle elevation-2" alt="User Image" />
                </div>
                <div class="info">
                    <a href="#" class="d-block">Jay Chauhan</a>
                </div>
            </div>

            <!-- SidebarSearch Form -->
            <div class="form-inline">
                <div class="input-group" data-widget="sidebar-search">
                    <input class="form-control form-control-sidebar" type="search" placeholder="Search" aria-label="Search" />
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
    </aside>
</div>
</template>

<script>
export default {
    name: "sidebarFile",
    data() {
        return {
            activeButtonIndex: 0
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

<style scoped>
.link {
    background: none;
    text-align: left;
    border: none;
}
</style>
