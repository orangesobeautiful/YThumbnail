<template>
  <q-page class="column items-center">
    <div class="container">
      <q-input
        filled
        bottom-slots
        v-model="ytURL"
        label="Youtube 網址"
        @update:model-value="GetYTID"
      >
        <template v-slot:append>
          <q-icon
            v-if="ytURL !== ''"
            name="close"
            @click="ClearInput"
            class="cursor-pointer"
          />
        </template>
      </q-input>
      <div class="row justify-between">
        <div v-for="img in imgList" :key="img.url" class="column thn-img">
          <div class="text-center text-h4">{{ img.title }}</div>
          <q-img :src="img.url" :ratio="16 / 9">
            <div class="absolute-bottom-right transparent">
              <q-btn
                :href="img.url"
                target="_blank"
                class="all-pointer-events img-btn bg-amber-13 text-body1 text-weight-medium text-black"
                @click="ImgNewTag(img.url)"
                @mousedown.middle="ImgNewTag(img.url)"
                >原圖</q-btn
              >
            </div>
          </q-img>
        </div>
        <div class="col" />
      </div>
    </div>
  </q-page>
</template>

<style lang="scss" scoped>
@import "../css/width.scss";

.container {
  width: 90%;
  padding-top: 20px;
}

.thn-img {
  margin-right: 10px;
  margin-bottom: 30px;

  @include xs-width {
    width: 100%;
  }
  //顯示 1 張圖片
  @include sm-width {
    width: 100%;
  }
  //顯示 2 張圖片
  @include md-width {
    width: 48.5%;
  }
  //顯示 3 張圖片
  @include lg-width {
    width: 32%;
  }
  //顯示 4 張圖片
  @include xl-width {
    width: 32%;
  }

  .img-btn {
    width: 83px;
    height: 42px;
    margin-left: 10px;
  }
}
</style>

<script>
import { defineComponent, ref } from "vue";
import { useRouter } from "vue-router";

export default defineComponent({
  name: "PageIndex",
  setup() {
    const router = useRouter();
    const currentRoute = router.currentRoute.value;

    // for dev
    let ytURL = ref("");
    let imgList = ref(Array());
    // ClearInput 清空搜尋框並清空縮圖列表
    function ClearInput() {
      ytURL.value = "";
      clearImgList();
    }
    // clearImgList 清空縮圖列表
    function clearImgList() {
      imgList.value = Array();
    }
    // GetYTID 獲取 Youtube 影片的 ID
    function GetYTID() {
      const urlStr = ytURL.value;
      try {
        let url = new URL(urlStr);
        let id = "";
        if (url.hostname.includes("youtube.com")) {
          // 正常網址
          id = url.searchParams.get("v");
        } else if (url.hostname.includes("youtu.be")) {
          // yt 縮網址
          id = url.pathname.substring(1);
        } else {
          return;
        }
        if (id && id != "") {
          GetYTThumbnail(id);
        }
      } catch (e) {
        // url error
        return;
      }
    }
    // GetYTThumbnail 獲取縮圖網址列表
    function GetYTThumbnail(id) {
      clearImgList();
      let urlHeader = "https://img.youtube.com/vi/";
      imgList.value.push({
        title: "1280X720",
        url: urlHeader + id + "/maxresdefault.jpg",
      });
      imgList.value.push({
        title: "640x480",
        url: urlHeader + id + "/sddefault.jpg",
      });
      imgList.value.push({
        title: "480x360",
        url: urlHeader + id + "/hqdefault.jpg",
      });
      imgList.value.push({
        title: "320x180",
        url: urlHeader + id + "/mqdefault.jpg",
      });
      imgList.value.push({
        title: "120x90",
        url: urlHeader + id + "/default.jpg",
      });
      imgList.value.push({
        title: "Start",
        url: urlHeader + id + "/sd1.jpg",
      });
      imgList.value.push({
        title: "Middle",
        url: urlHeader + id + "/sd2.jpg",
      });
      imgList.value.push({
        title: "End",
        url: urlHeader + id + "/sd3.jpg",
      });
    }
    // ImgNewTag 在新分頁開啟圖片
    function ImgNewTag(url) {
      window.open(url, "_blank");
    }

    if (currentRoute.path == "/watch") {
      let query = currentRoute.query;
      if (query.hasOwnProperty("v")) {
        ytURL.value = "https://www.youtube.com" + currentRoute.fullPath;
      } else {
        router.push("/");
      }
    }
    GetYTID(ytURL);

    return {
      ytURL,
      imgList,
      ClearInput,
      GetYTID,
      ImgNewTag,
    };
  },
});
</script>
