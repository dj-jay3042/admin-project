<template>
<div>
    <footer class="main-footer">
    <div class="d-none d-sm-block">
        <strong>Copyright © 2014-{{ currentYear }}
            <a href="https://dj-jay3042.github.io" target="_blank">Jay Chauhan</a>.</strong>
        All rights reserved.</div>
  </footer>
</div>
</template>

<script>
export default {
    name: "footerFile",
    data() {
        return {
            currentYear: new Date().getFullYear()
        }
    }
};
</script>
