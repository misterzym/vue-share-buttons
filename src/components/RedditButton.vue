<template>
  <button
    class="share-button share-button--reddit"
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
    <icon iconName="Reddit" class="share-button__icon" v-if="hasIcon === true">
      <path
        d="M2.204 14.049c-.06.276-.091.56-.091.847 0 3.443 4.402 6.249 9.814 6.249 5.41 0 9.812-2.804 9.812-6.249 0-.274-.029-.546-.082-.809l-.015-.032c-.021-.055-.029-.11-.029-.165-.302-1.175-1.117-2.241-2.296-3.103-.045-.016-.088-.039-.126-.07-.026-.02-.045-.042-.067-.064-1.792-1.234-4.356-2.008-7.196-2.008-2.815 0-5.354.759-7.146 1.971-.014.018-.029.033-.049.049-.039.033-.084.06-.13.075-1.206.862-2.042 1.937-2.354 3.123 0 .058-.014.114-.037.171l-.008.015zm9.773 5.441c-1.794 0-3.057-.389-3.863-1.197-.173-.174-.173-.457 0-.632.176-.165.46-.165.635 0 .63.629 1.685.943 3.228.943 1.542 0 2.591-.3 3.219-.929.165-.164.45-.164.629 0 .165.18.165.465 0 .645-.809.808-2.065 1.198-3.862 1.198l.014-.028zm-3.606-7.573c-.914 0-1.677.765-1.677 1.677 0 .91.763 1.65 1.677 1.65s1.651-.74 1.651-1.65c0-.912-.739-1.677-1.651-1.677zm7.233 0c-.914 0-1.678.765-1.678 1.677 0 .91.764 1.65 1.678 1.65s1.651-.74 1.651-1.65c0-.912-.739-1.677-1.651-1.677zm4.548-1.595c1.037.833 1.8 1.821 2.189 2.904.45-.336.719-.864.719-1.449 0-1.002-.815-1.816-1.818-1.816-.399 0-.778.129-1.09.363v-.002zM2.711 9.963c-1.003 0-1.817.816-1.817 1.818 0 .543.239 1.048.644 1.389.401-1.079 1.172-2.053 2.213-2.876-.302-.21-.663-.329-1.039-.329v-.002zm9.217 12.079c-5.906 0-10.709-3.205-10.709-7.142 0-.275.023-.544.068-.809C.494 13.598 0 12.729 0 11.777c0-1.496 1.227-2.713 2.725-2.713.674 0 1.303.246 1.797.682 1.856-1.191 4.357-1.941 7.112-1.992l1.812-5.524.404.095s.016 0 .016.002l4.223.993c.344-.798 1.138-1.36 2.065-1.36 1.229 0 2.231 1.004 2.231 2.234 0 1.232-1.003 2.234-2.231 2.234s-2.23-1.004-2.23-2.23l-3.851-.912-1.467 4.477c2.65.105 5.047.854 6.844 2.021.494-.464 1.144-.719 1.833-.719 1.498 0 2.718 1.213 2.718 2.711 0 .987-.54 1.886-1.378 2.365.029.255.059.494.059.749-.015 3.938-4.806 7.143-10.72 7.143l-.034.009zm8.179-19.187c-.74 0-1.34.599-1.34 1.338 0 .738.6 1.34 1.34 1.34.732 0 1.33-.6 1.33-1.334 0-.733-.598-1.332-1.347-1.332l.017-.012z"
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
  name: "RedditShareButton",
  components: { Icon },
  props: {
    url: { type: String, default: getDocumentHref },
    title: { type: String, default: "" },
    btnText: { type: String, default: "Reddit" },
    modalWidth: { type: Number },
    modalHeight: { type: Number },
    hasIcon: { type: Boolean, default: true },
    isBlank: { type: Boolean, default: true },
    customIcon: {type: String, default:""}
  },
  methods: {
    openShareWindow() {
      eventEmit(this, "onShare", { name: "Reddit" });
      const configWindow = createWindow(
        this.$props.modalWidth,
        this.$props.modalHeight
      );
      const url = `https://reddit.com/submit?url=${encodeURIComponent(
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
  background-color: #ff4400;
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
  box-shadow: 0 0 0 3px rgba(251, 163, 131, 0.4);
}
.share-button:hover {
  background-color: rgba(255, 68, 0, 0.9);
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
  border-color: #ff4400;
}
.share-button--outline .share-button__text {
  color: #ff4400;
}
.share-button--outline .share-button__icon path {
  fill: #ff4400;
}
.share-button--outline .share-button__counter {
  color: rgba(255, 68, 0, 0.9);
  border-color: rgba(255, 68, 0, 0.9);
}
.share-button--outline:hover {
  background-color: transparent;
  border-color: rgba(255, 68, 0, 0.9);
}
.share-button--outline:hover .share-button__text {
  color: #ff4400;
}
.share-button--outline:hover .share-button__icon path {
  fill: rgba(255, 68, 0, 0.9);
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
  border-color: #b53f17;
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
  background-color: #ff4400;
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
  border-color: #b53f17;
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
  background-color: #ff4400;
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
