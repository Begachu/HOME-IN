<template>
  <ul>
    <div v-if="loading" class="loading"><font-awesome-icon icon="fa-solid fa-circle-notch" /></div>

    <!-- TODO: 분기처리. 목록 없는 경우 없음을 알림 -->
    <li class="empty drag-block" v-else-if="!list.length">
      <p class="oops">이런!</p>
      <p class="oops-icon"><font-awesome-icon icon="fa-solid fa-face-sad-tear" /></p>
      <p>아파트 정보가 없습니다</p>
      <p>다른 지역을 선택해보세요!</p>
    </li>

    <li v-else v-for="(item, idx) of list" :key="`apt-item-${idx}`">
      <apt-item :data="item" />
    </li>
  </ul>
</template>

<script>
import { mapState } from "vuex";
import http from "@/apis/http.js";
import AptItem from "./AptItem.vue";

export default {
  name: "AptList",
  components: { AptItem },
  data() {
    return {
      list: [],
      loading: false,
      ...mapState(["regcode"]),
    };
  },
  methods: {
    /******** 지역코드에 따른 데이터 출력 함수 ********/
    async getList() {
      this.loading = true;
      if (!this.regcode) await http.get(`apt/list?regcode=11&amount=20`).then((res) => (this.list = res.data));
      else await http.get(`apt/list?regcode=${this.regcode()}&amount=20`).then((res) => (this.list = res.data));
      this.loading = false;
    },
  },

  // 주소 변경 감지용 computed & watch
  computed: {
    getRegcode() {
      return this.regcode();
    },
  },
  watch: {
    getRegcode() {
      this.getList();
    },
  },
  created() {
    this.getList();
  },
};
</script>

<style scoped>
ul {
  margin: 15px;
  box-sizing: border-box;
  width: auto;
  height: 100%;
  padding: 0;
  /* background-color: var(--red); */
  overflow-y: auto;
}
ul > li {
  list-style: none;
}
ul::-webkit-scrollbar {
  display: none;
  /* position: absolute;
  border-radius: 3px;
  width: 8px;
  background-color: var(--white); */
}
ul::-webkit-scrollbar-thumb {
  /* width: 8px;
  background-color: var(--darkgray);
  border-radius: 3px; */
}

.empty {
  display: flex;
  flex-direction: column;
  opacity: 0.5;
  height: 60%;
  justify-content: center;
}
.empty > p {
  margin: 0 auto;
  font-size: 14px;
}
.empty > .oops,
.empty > .oops-icon {
  color: var(--darkgray);
}
.empty > .oops {
  font-weight: 900;
  font-size: 24px;
}
.empty > .oops-icon {
  font-size: 50px;
}

@keyframes spin {
  from {
    transform: rotate(0);
  }
  to {
    transform: rotate(360deg);
  }
}
.loading {
  position: absolute;
  z-index: 1000;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  position: absolute;
  opacity: 0.75;
  display: flex;
  justify-content: center;
  align-items: center;
}
.loading > * {
  color: var(--navy);
  font-size: 60px;
  animation-duration: 0.75s;
  animation-name: spin;
  animation-iteration-count: infinite;
  animation-timing-function: linear;
}
</style>
