<template>
  <button
    class="share-button share-button--livejournal"
    type="button"
    :url="url"
    :description="description"
    :btnText="btnText"
    :modalWidth="modalWidth"
    :modalHeight="modalHeight"
    :hasIcon="hasIcon"
    :isBlank="isBlank"
    @click="openShareWindow"
  >
    <icon iconName="LiveJournal" class="share-button__icon" v-if="hasIcon === true">
      <path
        d="M18.09 14.696c-1.512.664-2.726 1.885-3.381 3.399l4.27.883-.886-4.282h-.003zM2.475 8.317L0 5.85C1.125 3.237 3.216 1.14 5.823 0h.006l2.469 2.463c1.368-.591 2.876-.921 4.463-.921C18.967 1.542 24 6.569 24 12.771 24 18.973 18.967 24 12.761 24 6.551 24 1.52 18.976 1.52 12.771c0-1.591.355-3.081.952-4.451l9.143 9.114c1.125-2.613 3.218-4.71 5.823-5.85l-9.135-9.12h-.008c-2.606 1.14-4.695 3.24-5.823 5.85l.003.003z"
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
  name: "LiveJournalShareButton",
  components: { Icon },
  props: {
    url: { type: String, default: getDocumentHref },
    description: { type: String, default: getDocumentTitle },
    btnText: { type: String, default: "LiveJournal" },
    modalWidth: { type: Number, default: 500 },
    modalHeight: { type: Number, default: 500 },
    hasIcon: { type: Boolean, default: true },
    isBlank: { type: Boolean, default: true },
    customIcon: {type: String, default:""}
  },
  methods: {
    openShareWindow() {
      eventEmit(this, "onShare", { name: "LiveJournal" });
      const configWindow = createWindow(
        this.$props.modalWidth,
        this.$props.modalHeight
      );
      const url = `https://livejournal.com/update.bml?event=${encodeURIComponent(
        this.$props.description
      )}&subject=${encodeURIComponent(this.$props.url)}`;

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
  background-color: #00b0eb;
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
  box-shadow: 0 0 0 3px rgba(112, 216, 251, 0.4);
}
.share-button:hover {
  background-color: rgba(0, 176, 235, 0.9);
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
  border-color: #00b0eb;
}
.share-button--outline .share-button__text {
  color: #00b0eb;
}
.share-button--outline .share-button__icon path {
  fill: #00b0eb;
}
.share-button--outline .share-button__counter {
  color: rgba(0, 176, 235, 0.9);
  border-color: rgba(0, 176, 235, 0.9);
}
.share-button--outline:hover {
  background-color: transparent;
  border-color: rgba(0, 176, 235, 0.9);
}
.share-button--outline:hover .share-button__text {
  color: #00b0eb;
}
.share-button--outline:hover .share-button__icon path {
  fill: rgba(0, 176, 235, 0.9);
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
  border-color: #1581a2;
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
  background-color: #00b0eb;
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
  border-color: #1581a2;
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
  background-color: #00b0eb;
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
