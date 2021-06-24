<template>
  <div class="container main-container  h-screen">
    <div>
      <h1 class="home-title text-center text-6xl font-bold py-10">
        Hello Weather App
      </h1>
      <div id="js-loader"></div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {};
  },
  computed: {},

  methods: {
    window: (onload = () => {
      let targetSrc = "app";

      //titleにis-active付与
      let res1 = setTimeout(() => {
        let title = document.querySelector(".home-title");
        title.classList.add("is-active");
      }, 100);

      //ローディングクラス付与
      let res2 = setTimeout(() => {
        let loader = document.getElementById("js-loader");
        loader.classList.add("is-active");
      }, 4000);

      //ページ推移
      let res3 = setTimeout(() => {
        window.location.href = targetSrc;
      }, 5000);
      return res1, res2, res3;
    })
  }
};
</script>

<style lang="scss">
.home-title {
  display: none;
  opacity: 0;
  transition: all 2s ease-in-out;
}
.home-title.is-active {
  display: block;
  opacity: 1;
  animation: opacityAnim 2s ease-in-out;
}

#js-loader {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 2;
  width: 100%;
  height: 100vh;
  background: #fff;
  opacity: 0;
  display: none;
}
#js-loader.is-active {
  opacity: 1;
  display: block;
  &::before {
    opacity: 1;
    display: block;
    content: "Loading...";
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, 125%);
    font-size: 2rem;
    animation: opacityAnim 2s ease;
  }
  &::after {
    opacity: 1;
    display: block;
    content: "";
    width: 50px;
    height: 50px;
    border: 3px solid #000;
    border-radius: 50%;
    border-top-color: rgba(0, 0, 0, 0.5);
    border-right-color: rgba(0, 0, 0, 0.5);
    position: absolute;
    top: 50%;
    left: 46%;
    transform: translate(-50%, -130%);
    animation: loadingAnim 1s linear infinite, opacityAnim 2s ease;
  }
}

//アニメーション
@keyframes opacityAnim {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

@keyframes loadingAnim {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>
