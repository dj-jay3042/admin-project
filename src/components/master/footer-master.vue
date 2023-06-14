<template>
  <div>
    <footer class="main-footer">
      <strong
        >Copyright © 2014-{{ currentYear }}
        <a href="https://dj-jay3042.github.io" target="_blank">Jay Chauhan</a>.</strong>
      All rights reserved.
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