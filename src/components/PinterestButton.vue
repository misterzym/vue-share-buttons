<template>
  <button
    class="share-button share-button--pinterest"
    type="button"
    :url="url"
    :description="description"
    :picture="picture"
    :btnText="btnText"
    :modalWidth="modalWidth"
    :modalHeight="modalHeight"
    :hasIcon="hasIcon"
    :hasCounter="hasCounter"
    :digitsCounter="digitsCounter"
    :keyCounter="keyCounter"
    :isBlank="isBlank"
    @click="openShareWindow"
  >
    <icon iconName="Pinterest" class="share-button__icon" v-if="hasIcon === true">
      <path
        d="M12.017 0C5.396 0 .029 5.367.029 11.987c0 5.079 3.158 9.417 7.618 11.162-.105-.949-.199-2.403.041-3.439.219-.937 1.406-5.957 1.406-5.957s-.359-.72-.359-1.781c0-1.663.967-2.911 2.168-2.911 1.024 0 1.518.769 1.518 1.688 0 1.029-.653 2.567-.992 3.992-.285 1.193.6 2.165 1.775 2.165 2.128 0 3.768-2.245 3.768-5.487 0-2.861-2.063-4.869-5.008-4.869-3.41 0-5.409 2.562-5.409 5.199 0 1.033.394 2.143.889 2.741.099.12.112.225.085.345-.09.375-.293 1.199-.334 1.363-.053.225-.172.271-.401.165-1.495-.69-2.433-2.878-2.433-4.646 0-3.776 2.748-7.252 7.92-7.252 4.158 0 7.392 2.967 7.392 6.923 0 4.135-2.607 7.462-6.233 7.462-1.214 0-2.354-.629-2.758-1.379l-.749 2.848c-.269 1.045-1.004 2.352-1.498 3.146 1.123.345 2.306.535 3.55.535 6.607 0 11.985-5.365 11.985-11.987C23.97 5.39 18.592.026 11.985.026L12.017 0z"
      />
    </icon>
    <img v-bind:src="customIcon" v-if="customIcon!=''">
    <span class="share-button__text" v-if="btnText">{{btnText}}</span>
    <span class="share-button__counter" v-if="hasCounter && counter > 0">{{ shortСounter }}</span>
  </button>
</template>

<script>
import Icon from "./icon/Icon.vue";
import {
  getDocumentHref,
  getDocumentTitle,
  eventEmit,
  createWindow,
  getRandomNumber,
  getShortNumber
} from "../helpers";

export default {
  name: "PinterestShareButton",
  components: { Icon },
  props: {
    url: { type: String, default: getDocumentHref },
    description: { type: String, default: getDocumentTitle },
    picture: { type: String, default: "" },
    btnText: { type: String, default: "Pinterest" },
    modalWidth: { type: Number },
    modalHeight: { type: Number },
    hasIcon: { type: Boolean, default: true },
    hasCounter: { type: Boolean, default: false },
    digitsCounter: { type: Number, default: 0 },
    keyCounter: { type: String, default: "" },
    isBlank: { type: Boolean, default: true },
    customIcon: {type: String, default:""}
  },
  mounted() {
    if (this.$props.hasCounter) this.getShareCounter();
  },
  methods: {
    openShareWindow() {
      if (this.$props.hasCounter) {
        eventEmit(this, "onShareCounter", {
          name: "Pinterest",
          counter: this.counter
        });
      } else {
        eventEmit(this, "onShare", { name: "Pinterest" });
      }
      const configWindow = createWindow(
        this.$props.modalWidth,
        this.$props.modalHeight
      );
      const url = `https://www.pinterest.com/pin/create/button/?canonicalUrl=${encodeURIComponent(
        this.$props.url
      )}&description=${encodeURIComponent(
        this.$props.description
      )}&media=${encodeURIComponent(this.$props.picture)}`;

      return this.$props.isBlank
        ? window.open(url, "_blank")
        : window.open(url, "Share this", configWindow);
    },

    getShareCounter() {
      const callback =
        this.$props.keyCounter || `Pinterest_${getRandomNumber()}`;
      const script = document.createElement("script");
      script.src = `https://api.pinterest.com/v1/urls/count.json?url=${encodeURIComponent(
        this.$props.url
      )}&callback=${callback}`;
      document.body.appendChild(script);

      window[callback] = count => {
        if (!count) return;
        this.counter = count.count;
        this.shortСounter = getShortNumber(
          this.counter,
          this.$props.digitsCounter
        );
      };
    }
  },
  data() {
    return {
      counter: 0,
      shortСounter: 0
    };
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
  background-color: #bf081d;
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
  box-shadow: 0 0 0 3px rgba(242, 84, 103, 0.4);
}
.share-button:hover {
  background-color: rgba(191, 8, 29, 0.9);
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
  border-color: #bf081d;
}
.share-button--outline .share-button__text {
  color: #bf081d;
}
.share-button--outline .share-button__icon path {
  fill: #bf081d;
}
.share-button--outline .share-button__counter {
  color: rgba(191, 8, 29, 0.9);
  border-color: rgba(191, 8, 29, 0.9);
}
.share-button--outline:hover {
  background-color: transparent;
  border-color: rgba(191, 8, 29, 0.9);
}
.share-button--outline:hover .share-button__text {
  color: #bf081d;
}
.share-button--outline:hover .share-button__icon path {
  fill: rgba(191, 8, 29, 0.9);
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
  border-color: #7d1725;
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
  background-color: #bf081d;
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
  border-color: #7d1725;
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
  background-color: #bf081d;
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
