<template>
  <popup position="bottom" :overlay="false" v-model="isShow" style="overflow-y: scroll;">
    <div class="market">
      <div class="content">
        <div class="img" v-if="domain && store.plan">
          <icon name="close" size="25" color="#f7403a" class="icon" @click="domain = false" />
          <img :src="store.plan" width="100%" @click="preview" />
        </div>
        <tabs
          class="bg-white tabs"
          background="#eee"
          animated
          swipeable
          ellipsis
          swipe-threshold="3"
          color="#f7403a"
          v-if="$root.market.intro || $root.market.distribution || $root.market.level "
        >
          <tab title="市场介绍" v-if="$root.market.intro">
            <div class="info text-df">
              <div class="text-xl font-medium padding-tb-xs">{{store.name}}</div>

              <div class="text-lg font-medium padding-tb-xs">建店时间</div>
              <div class="padding-bottom-xs">{{store.establishment_time}}</div>

              <div class="text-lg font-medium padding-tb-xs">销售范围</div>
              <div class="padding-bottom-xs">{{store.sales_area}}</div>
            </div>
          </tab>
          <tab title="正泰门店" v-if="$root.market.distribution">
            <div
              class="store text-df"
              v-for="store in store.children"
              :key="store.id"
              @click="pin(store)"
            >{{store.name}}</div>
          </tab>
          <tab title="竞争对手">
            <div class="info text-df">
              <div
                class="padding-tb-xs"
                v-for="competitor in store.competitors"
                v-bind:key="competitor.name"
              >
                <div class="flex justify-between">
                  <div class="font-medium padding-bottom-xs">{{competitor.name}}</div>
                  <div class="red padding-bottom-xs">
                    预估销售额：
                    <span>{{addChineseUnit(competitor.money, 1)}}</span>
                  </div>
                </div>
                <div class="padding-bottom-sm">{{competitor.introduce}}</div>
                <div>
                  <!-- <div class="red padding-bottom-xs">
                    一级数量：
                    <span>{{competitor.one_count}}</span>

                    二级数量：
                    <span>{{competitor.two_count}}</span>
                  </div>
                  <div class="red padding-bottom-xs">
                  </div>
                  <div class="red padding-bottom-xs">
                    市场份额：
                    <span>{{competitor.percent}}</span>
                  </div>
                  <div class="red padding-bottom-xs">
                    户外广告：
                    <span>{{competitor.outdoor}}</span>
                  </div> -->
                  <div class="red padding-bottom-xs">
                    终端形象建设：
                    <span>{{competitor.content}}</span>
                  </div>
                </div>
              </div>
            </div>
          </tab>
          <tab title="正泰终端汇总" v-if="$root.market.level">
            <div class="flex flex-wrap padding text-df">
              <div class="area" v-for="v in store.area" v-bind:key="v.name">
                <div class="color" :style="`background: ${v.color}`"></div>
                {{v.name}} {{v.count}}
              </div>
            </div>
          </tab>
        </tabs>
        <button
          class="btn bg-blue margin-tb-xs radius padding-tb-sm text-center"
          @click="$router.push('/home')"
        >返回</button>
      </div>
    </div>
  </popup>
</template>

<script>
import { Popup, Tabs, Tab, Icon, ImagePreview } from "vant";
import { addChineseUnit } from "@/utils/utils";

export default {
  data() {
    return {
      domain: true,
      isShow: false
    };
  },
  computed: {
    store() {
      return this.$parent.store;
    }
  },
  components: {
    Icon,
    Popup,
    Tabs,
    Tab
  },
  methods: {
    pin(store) {
      
      this.$store.dispatch('setSelectStore', store)
      // console.log(store)
      // this.$root.center = store.coordinate;
      // this.$root.zoom = 18;
    },
    preview() {
      ImagePreview({
        images: [this.store.plan],
        showIndex: false
      });
    },
    addChineseUnit
  },
  async created() {
    const id = this.$route.params.id;
    await this.$parent.find(id);
    this.isShow = true;
  },
  activated() {
    this.isShow = true;
  }
};
</script>

<style scoped>
.market {
  width: 100%;
}

.content {
  width: 95%;
  margin: 10px auto;
}

.info {
  padding: 40px;
}

/* .title {
  font-size: 32px;
  text-align: center;
  background: #fff;
  box-shadow: 0 5px 10px rgba(0, 0, 0, 0.2);
} */

.btn {
  width: 100%;
  border: none;
  box-shadow: 0 5px 10px rgba(0, 0, 0, 0.2);
  z-index: 9;
}

.tabs {
  height: 360px;
}

.van-popup {
  background: transparent;
}

.store {
  text-align: center;
  padding: 26px 20px;
  margin: 0 30px;
  border-bottom: 1px solid #b0b0b0;
}

div {
  word-wrap: break-word;
}

.area {
  width: 250px;
  margin: 12px;
  display: flex;
  align-items: center;
}

.color {
  width: 30px;
  height: 30px;
  margin-right: 20px;
}

.red span {
  color: #f34e4a;
}

.img {
  position: relative;
}

.img .icon {
  position: absolute;
  right: -18px;
  top: -18px;
}
</style>

<style>
.van-tabs {
  border-radius: 5px;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.2);
}

.van-tabs--line .van-tabs__wrap {
  border-top-left-radius: 5px;
  border-top-right-radius: 5px;
}

/* .van-tabs__line {
  bottom: 24px;
  left: -1px;
} */

.van-tab--active {
  background: #fff;
}

.van-tab:first-of-type {
  border-top-left-radius: 5px;
}

.van-tab:last-of-type {
  border-top-right-radius: 5px;
}

.van-tab__pane {
  height: 147px;
  overflow: scroll;
}
</style>

