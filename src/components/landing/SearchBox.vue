<template>
  <div :class="['searchbox-wrapper', bannerExpanded ? 'searchbox-hide' : '']" v-show="show">
    <div class="searchbox">
      <div class="searchbox-title">
        <span>寻找你想看的画作</span>
      </div>
      <div class="searchbox-input">
        <el-input v-model="search" spellcheck="false" @keyup.enter.native="submitSearch"></el-input>
      </div>
      <div class="searchbox-button">
        <button @click="handleSearchClick"><i class="el-icon-search"></i></button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Landing.SearchBox",
  data() {
    return {
      show: true,
      search: '',
      bannerExpanded: false
    }
  },
  watch: {
    "$store.state.landingBanner.expanded": function(expanded) {
      if (expanded) {
        this.bannerExpanded = expanded;
        setTimeout(() => {
          this.show = false;
        }, 500);
      } else {
        this.show = true;
        setTimeout(() => {
          this.bannerExpanded = expanded;
        }, 150);
      }
    }
  },
  methods: {
    submitSearch() {
      let search;
      if (!this.search) {
        this.$message.error('呐，输入关键词再搜索！！');
        return;
      }
      if (this.search) {
        search = this.search.trim();
        if (search.length < 1) {
          this.$message.error('呐，输入关键词再搜索！！');
          this.search = '';
          return;
        }
      };
			this.$router.push(`/search/${search}`);
			this.$cookies.set('search-from', '', '1h');
    },
    handleSearchClick() {
      this.submitSearch();
    }
  }
}
</script>