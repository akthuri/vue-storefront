<template>
  <div>
    <ul
      v-if="categoryLinks"
      class="sidebar-submenu absolute w-100 p0 bg-cl-primary"
      :style="styles"
    >
      <li
        class="brdr-bottom brdr-cl-bg-secondary bg-cl-primary flex"
        :key="link.slug"
        v-for="link in categoryLinks"
      >
        <router-link
          class="px25 py20 cl-accent no-underline col-xs"
          :to="{ name: 'category', params: { id: link.id, slug: link.slug }}"
        >
          {{ link.name }}
        </router-link>
        <sub-btn
          class="w-50 bg-cl-transparent brdr-none align-right"
          :id="link.id"
          v-if="link.children_data.length"
        />
        <sub-category
          :category-links="link.children_data"
          :id="link.id"
          v-if="link.children_data.length"
        />
      </li>
    </ul>
    <ul
      v-else-if="myAccountLinks"
      class="sidebar-submenu absolute p0 bg-cl-primary"
      :style="styles"
    >
      <li
        class="brdr-bottom brdr-cl-bg-secondary bg-cl-primary flex"
        :key="link.id"
        v-for="link in myAccountLinks"
      >
        <router-link
          class="px25 py20 cl-accent no-underline col-xs"
          :to="'/my-account#' + link.anchor"
        >
          {{ link.name }}
        </router-link>
      </li>
      <li class="brdr-bottom brdr-cl-bg-secondary bg-cl-primary flex">
        <a href="#" class="px25 py20 cl-accent no-underline col-xs" @click="logout">
          {{ $t('Logout') }}
        </a>
      </li>
    </ul>
  </div>
</template>
<script>
import { mapState } from 'vuex'
import SubBtn from './SubBtn.vue'

export default {
  name: 'SubCategory',
  components: {
    SubBtn
  },
  props: {
    id: {
      type: [String, Number],
      required: true
    },
    categoryLinks: {
      type: null,
      required: false,
      default: () => []
    },
    myAccountLinks: {
      type: Array,
      required: false,
      default: () => {}
    }
  },
  computed: {
    ...mapState({
      submenu: state => state.ui.submenu
    }),
    styles () {
      const pos = this.submenu.path.indexOf(this.id)
      return pos !== -1 ? {
        zIndex: pos + 1
      } : false
    }
  },
  methods: {
    logout () {
      this.$bus.$emit('user-before-logout')
    }
  }
}
</script>
<style scoped>
  .sidebar-submenu {
    left: 0;
    top: 0;
    min-height: 100%;
    transform: translateX(-100%);
  }
</style>
