<template>
  <div class="container">
    <div class="img-container">
      <img :src="require(`@/assets/imgs/apt-${(+data.no % 6) + 1}.jpg`)" />
    </div>
    <div class="info-container">
      <span class="dong">{{ data.dong }}</span>
      <h4 class="name">{{ data.apartmentName }}</h4>

      <div class="tag-container">
        <div class="tag edu">νμκ° π©π»βπ«</div>
        <div class="tag e-school">μ΄λ±νκ΅ π«</div>
        <div class="tag m-h-school">μ€γ»κ³ λ±νκ΅ π«</div>
        <!-- <div class="tag hot-place">λ²νκ° π¬</div>
        <div class="tag medical">μλ£μμ€ π₯</div>
        <div class="tag new">μ μΆ π</div>
        <div class="tag util">νΈμμμ€ πͺ</div>
        <div class="tag police">κ²½μ°°μ π</div> -->
      </div>

      <p class="range">
        <font-awesome-icon icon="fa-solid fa-sack-dollar" class="icon" />

        <span v-if="data.minDealAmount === data.maxDealAmount">{{ getDealString(data.minDealAmount) }}</span>
        <span v-else> {{ getDealString(data.minDealAmount) }} ~ {{ getDealString(data.maxDealAmount) }} </span>
      </p>
      <p class="range">
        <font-awesome-icon icon="fa-solid fa-maximize" class="icon" />
        <span v-if="data.minArea === data.maxArea">{{ data.minArea }}mΒ²</span>
        <span v-else>{{ data.minArea }}mΒ² ~ {{ data.maxArea }}mΒ²</span>
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: "AptItem",
  props: {
    data: Object,
  },
  methods: {
    getDealString(deal) {
      let urk = Math.round(deal / 10000);
      let marn = deal % 10000;
      let arr = [];
      if (urk) arr.push(`${urk}μ΅`);
      if (marn) arr.push(`${marn}λ§`);
      return `${arr.join(" ")}μ`;
    },
  },
};
</script>

<style scoped>
.container {
  cursor: pointer;
  width: 100%;
  margin: 20px 0;
  background-color: var(--white);
  /* border-bottom: 1px solid var(--gray); */
  display: flex;
  position: relative;
}
.container:hover::after {
  content: "";
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: var(--shadow);
  opacity: 0.2;
  border-radius: 8px;
}
.info-container {
  flex-grow: 1;
  flex-basis: 1;
}
.info-container > * {
  margin: 0;
}
.name {
  font-size: 18px;
  margin-bottom: 4px;
}
.dong {
  font-size: 12px;
}
.range {
  font-size: 14px;
  display: flex;
  align-items: center;
  margin: 3px;
}
.range > .icon {
  background-color: var(--white);
  padding: 5px;
  margin-right: 5px;
  height: 14px;
  width: 14px;
  border-radius: 50%;
  color: var(--navy);
}
.tag-container {
  display: flex;
  flex-wrap: wrap;
  padding-bottom: 5px;
  margin-bottom: 5px;
  border-bottom: 1px solid var(--gray);
}
.tag {
  height: 16px;
  padding: 4px 10px;
  margin: 0 4px 4px 0;
  border-radius: 12px;
  background-color: var(--gray);
  color: var(--navy);
  font-size: 11px;
  font-weight: 900;
  width: fit-content;
}

.img-container {
  max-width: 140px;
  width: 100%;
  overflow: hidden;
  position: relative;
  border-radius: 10px;
  margin-right: 15px;
}
.container:hover .img-container {
  opacity: 0.8;
}
.container:hover .img-container > img {
  transform: scale(102%) translate(-50%, -50%);
}
.img-container > img {
  height: 100%;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  transition: all 0.25s linear;
}
</style>
