<template>
  <button
    class="share-button share-button--weibo"
    type="button"
    :url="url"
    :title="title"
    :description="description"
    :picture="picture"
    :btnText="btnText"
    :modalWidth="modalWidth"
    :modalHeight="modalHeight"
    :hasIcon="hasIcon"
    :isBlank="isBlank"
    @click="openShareWindow"
  >
    <icon iconName="Weibo" class="share-button__icon" v-if="hasIcon === true">
      <path
        d="M10.098 20.323c-3.977.391-7.414-1.406-7.672-4.02-.259-2.609 2.759-5.047 6.74-5.441 3.979-.394 7.413 1.404 7.671 4.018.259 2.6-2.759 5.049-6.737 5.439l-.002.004zM9.05 17.219c-.384.616-1.208.884-1.829.602-.612-.279-.793-.991-.406-1.593.379-.595 1.176-.861 1.793-.601.622.263.82.972.442 1.592zm1.27-1.627c-.141.237-.449.353-.689.253-.236-.09-.313-.361-.177-.586.138-.227.436-.346.672-.24.239.09.315.36.18.601l.014-.028zm.176-2.719c-1.893-.493-4.033.45-4.857 2.118-.836 1.704-.026 3.591 1.886 4.21 1.983.64 4.318-.341 5.132-2.179.8-1.793-.201-3.642-2.161-4.149zm7.563-1.224c-.346-.105-.57-.18-.405-.615.375-.977.42-1.804 0-2.404-.781-1.112-2.915-1.053-5.364-.03 0 0-.766.331-.571-.271.376-1.217.315-2.224-.27-2.809-1.338-1.337-4.869.045-7.888 3.08C1.309 10.87 0 13.273 0 15.348c0 3.981 5.099 6.395 10.086 6.395 6.536 0 10.888-3.801 10.888-6.82 0-1.822-1.547-2.854-2.915-3.284v.01zm1.908-5.092c-.766-.856-1.908-1.187-2.96-.962-.436.09-.706.511-.616.932.09.42.511.691.932.602.511-.105 1.067.044 1.442.465.376.421.466.977.316 1.473-.136.406.089.856.51.992.405.119.857-.105.992-.512.33-1.021.12-2.178-.646-3.035l.03.045zm2.418-2.195c-1.576-1.757-3.905-2.419-6.054-1.968-.496.104-.812.587-.706 1.081.104.496.586.813 1.082.707 1.532-.331 3.185.15 4.296 1.383 1.112 1.246 1.429 2.943.947 4.416-.165.48.106 1.007.586 1.157.479.165.991-.104 1.157-.586.675-2.088.241-4.478-1.338-6.235l.03.045z"
      />
    </icon>
    <img v-bind:src="customIcon" v-if="customIcon!=''">
    <span class="share-button__text" v-if="btnText">{{btnText}}</span>
  </button>
</template>

<script>
import Icon from "./icon/Icon.vue";
import {
  getDocumentHref,
  getDocumentTitle,
  eventEmit,
  createWindow
} from "../helpers";

export default {
  name: "WeiboShareButton",
  components: { Icon },
  props: {
    url: { type: String, default: getDocumentHref },
    title: { type: String, default: "" },
    description: { type: String, default: getDocumentTitle },
    picture: { type: String, default: "" },
    btnText: { type: String, default: "Weibo" },
    modalWidth: { type: Number, default: 500 },
    modalHeight: { type: Number, default: 500 },
    hasIcon: { type: Boolean, default: true },
    isBlank: { type: Boolean, default: true },
    customIcon: {type: String, default:""}
  },
  methods: {
    openShareWindow() {
      eventEmit(this, "onShare", { name: "Weibo" });
      const configWindow = createWindow(
        this.$props.modalWidth,
        this.$props.modalHeight
      );
      const url = `http://service.weibo.com/share/share.php?url=${encodeURIComponent(
        this.$props.url
      )}&title=${encodeURIComponent(
        this.$props.title
      )}&pic=${encodeURIComponent(this.$props.picture)}`;

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
  background-color: #006466;
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
  box-shadow: 0 0 0 3px rgba(7, 219, 223, 0.4);
}
.share-button:hover {
  background-color: rgba(0, 100, 102, 0.9);
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
  border-color: #006466;
}
.share-button--outline .share-button__text {
  color: #006466;
}
.share-button--outline .share-button__icon path {
  fill: #006466;
}
.share-button--outline .share-button__counter {
  color: rgba(0, 100, 102, 0.9);
  border-color: rgba(0, 100, 102, 0.9);
}
.share-button--outline:hover {
  background-color: transparent;
  border-color: rgba(0, 100, 102, 0.9);
}
.share-button--outline:hover .share-button__text {
  color: #006466;
}
.share-button--outline:hover .share-button__icon path {
  fill: rgba(0, 100, 102, 0.9);
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
  border-color: #062d2d;
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
  background-color: #006466;
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
  border-color: #062d2d;
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
  background-color: #006466;
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
