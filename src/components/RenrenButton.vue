<template>
  <button
    class="share-button share-button--renren"
    type="button"
    :url="url"
    :title="title"
    :btnText="btnText"
    :modalWidth="modalWidth"
    :modalHeight="modalHeight"
    :hasIcon="hasIcon"
    :isBlank="isBlank"
    @click="openShareWindow"
  >
    <icon iconName="Renren" class="share-button__icon" v-if="hasIcon === true">
      <path
        d="M5.661 9.601V5.303a6.793 6.793 0 0 0-4.053 11.084c2.378-1.152 4.041-3.755 4.053-6.786zM6.793 13.715c-.423 1.752-1.687 3.249-3.262 4.244a6.759 6.759 0 0 0 3.261.833 6.771 6.771 0 0 0 3.262-.833c-1.575-.995-2.838-2.493-3.261-4.244zM11.977 7.613a6.789 6.789 0 0 0-4.052-2.31v4.265c0 3.044 1.666 5.662 4.051 6.817a6.766 6.766 0 0 1-1.607-4.386 6.754 6.754 0 0 1 1.608-4.386z"
      />
      <path
        d="M11.977 7.613c1.003 1.183 1.655 2.714 1.655 4.387s-.652 3.202-1.655 4.387l-.001-.001.001.001c2.378-1.151 4.087-3.755 4.099-6.786V5.303a6.9 6.9 0 0 0-4.099 2.31zM18.34 9.568c0 3.045 1.666 5.662 4.052 6.818A6.792 6.792 0 0 0 18.34 5.304v4.264zM17.208 13.715c-.423 1.752-1.687 3.249-3.262 4.244a6.759 6.759 0 0 0 3.261.833 6.771 6.771 0 0 0 3.262-.833c-1.574-.995-2.838-2.493-3.261-4.244z"
      />
    </icon>
    <img v-bind:src="customIcon" v-if="customIcon!=''">
    <span class="share-button__text" v-if="btnText">{{btnText}}</span>
  </button>
</template>

<script>
import Icon from "./icon/Icon.vue";
import { getDocumentHref, eventEmit, createWindow } from "../helpers";

export default {
  name: "RenrenShareButton",
  components: { Icon },
  props: {
    url: { type: String, default: getDocumentHref },
    title: { type: String, default: "" },
    btnText: { type: String, default: "Renren" },
    modalWidth: { type: Number, default: 500 },
    modalHeight: { type: Number, default: 500 },
    hasIcon: { type: Boolean, default: true },
    isBlank: { type: Boolean, default: true },
    customIcon: {type: String, default:""}
  },
  methods: {
    openShareWindow() {
      eventEmit(this, "onShare", { name: "Renren" });
      const configWindow = createWindow(
        this.$props.modalWidth,
        this.$props.modalHeight
      );
      const url = `http://share.renren.com/share/buttonshare.do?link=${encodeURIComponent(
        this.$props.url
      )}&title=${encodeURIComponent(this.$props.title)}`;

      return this.$props.isBlank
        ? window.open(url, "_blank")
        : window.open(url, "Share this", configWindow);
    }
  }
};
</script>

<style lang="css" scoped>
.share-button * {
  box-sizing: border-box;
}

.share-button {
  display: inline-block;
  min-width: 42px;
  min-height: 42px;
  padding: 10px 8px;
  margin: 4px;
  color: #fff;
  background-color: #217bc4;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
  font-weight: 400;
  vertical-align: top;
  user-select: none;
  border: none;
  border-radius: 4px;
  box-shadow: none;
  text-rendering: auto;
  text-indent: 0px;
  text-align: center;
  letter-spacing: normal;
  word-spacing: normal;
  text-shadow: none;
  transition: color 0.3s ease-in-out, background-color 0.3s ease-in-out, border-color 0.3s ease-in-out;
}
.share-button:disabled {
  opacity: 0.9;
}
.share-button:focus {
  outline: none;
  box-shadow: 0 0 0 3px rgba(45, 136, 210, 0.4);
}
.share-button:hover {
  background-color: rgba(33, 123, 196, 0.9);
}
.share-button:not(:disabled):not(.disabled) {
  cursor: pointer;
}
.share-button:last-child {
  margin-right: 0;
}
.share-button__icon {
  display: inline-block;
  padding: 0;
  margin: 0 7px;
  font-size: 0;
  vertical-align: middle;
}
.share-button__icon path {
  fill: #fff;
}
.share-button__icon:last-child {
  margin: 0;
}
.share-button__text {
  display: inline-block;
  margin: 0 7px;
  font-size: 16px;
  vertical-align: middle;
}
.share-button__counter {
  display: inline-block;
  padding: 3px 10px;
  margin-left: 4px;
  font-size: 12px;
  border-left: 1px solid #fff;
  vertical-align: middle;
}
.share-button--circle {
  min-width: 42px;
  min-height: 42px;
  padding: 10px;
  border-radius: 42px;
}
.share-button--outline {
  background-color: transparent;
  border: 1px solid;
  background-color: transparent;
  border-color: #217bc4;
}
.share-button--outline .share-button__text {
  color: #217bc4;
}
.share-button--outline .share-button__icon path {
  fill: #217bc4;
}
.share-button--outline .share-button__counter {
  color: rgba(33, 123, 196, 0.9);
  border-color: rgba(33, 123, 196, 0.9);
}
.share-button--outline:hover {
  background-color: transparent;
  border-color: rgba(33, 123, 196, 0.9);
}
.share-button--outline:hover .share-button__text {
  color: #217bc4;
}
.share-button--outline:hover .share-button__icon path {
  fill: rgba(33, 123, 196, 0.9);
}
.share-button--painted {
  position: relative;
  min-width: 42px;
  min-height: 42px;
  padding: 15px;
  margin-bottom: 30px;
  border-radius: 42px;
  background-color: transparent;
  border: 3px solid;
  border-color: #2e5f84;
}
.share-button--painted::before {
  content: "";
  z-index: -1;
  position: absolute;
  top: -1.5px;
  left: -1.5px;
  display: block;
  width: calc(100% + 3px);
  height: calc(100% + 3px);
  background-color: #217bc4;
  border-radius: 50%;
  transform: translate3d(3px, 2px, 0);
  transition: transform 0.2s ease-in-out;
}
.share-button--painted .share-button__icon {
  width: 30px;
  height: 30px;
  margin: 0;
}
.share-button--painted .share-button__text {
  display: none;
}
.share-button--painted .share-button__counter {
  position: absolute;
  bottom: -30px;
  right: -7px;
  margin: 0;
  padding: 4px 10px;
  border: 3px solid;
  font-size: 8px;
  border-radius: 15px;
  color: #fff;
  border-color: #2e5f84;
}
.share-button--painted .share-button__counter::before {
  content: "";
  z-index: -1;
  position: absolute;
  top: -1.65px;
  left: -1.5px;
  display: block;
  width: calc(100% + 3px);
  height: calc(100% + 3px);
  border-radius: 15px;
  transform: translate3d(-3px, 1.5px, 0);
  transition: transform 0.2s ease-in-out;
  background-color: #217bc4;
}
.share-button--painted:hover::before {
  transform: translate3d(0, 0, 0);
}
.share-button--painted:hover .share-button__counter::before {
  transform: translate3d(0px, 0px, 0);
}
.share-button--painted:focus::before {
  transform: translate3d(0, 0, 0);
}
.share-button--painted:focus .share-button__counter::before {
  transform: translate3d(0px, 0px, 0);
}

@media (max-width: 768px) {
  .share-button {
    min-width: 38px;
    min-height: 38px;
    padding: 8px 8px;
    margin: 2px;
  }
  .share-button__icon {
    width: 18px;
    height: 18px;
    margin: 0 4px;
  }
  .share-button__text {
    margin: 0 4px;
    font-size: 14px;
  }
  .share-button--circle {
    border-radius: 38px;
  }
  .share-button--painted {
    min-width: 48px;
    min-height: 48px;
    margin: 4px 4px 20px 4px;
  }
  .share-button--painted::before {
    transform: translate3d(2.5px, 1.5px, 0);
  }
  .share-button--painted .share-button__icon {
    width: 20px;
    height: 20px;
  }
  .share-button--painted .share-button__counter {
    bottom: -24px;
    right: -8px;
    padding: 2px 7px;
  }
  .share-button--painted .share-button__counter::before {
    transform: translate3d(-2px, 1.75px, 0);
  }
}
</style>
