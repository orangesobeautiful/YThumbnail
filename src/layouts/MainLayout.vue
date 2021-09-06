<template>
  <q-layout view="lHh Lpr lFf" class="">
    <q-header reveal elevated class="bg-pink glossy">
      <q-toolbar class="text-black">
        <q-toolbar-title>
          <q-btn flat @click="goHomePage" class="text-capitalize"
            >YThumbnail</q-btn
          >
        </q-toolbar-title>
      </q-toolbar>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import { defineComponent } from "vue";
import { useRouter } from "vue-router";

export default defineComponent({
  name: "MainLayout",

  components: {},

  setup() {
    const router = useRouter();
    const currentRoute = router.currentRoute.value;

    function goHomePage() {
      if (currentRoute.name && currentRoute.name == "index") {
        router.go(0);
      } else {
        router.push("/");
      }
    }
    return {
      goHomePage,
    };
  },
});
</script>
