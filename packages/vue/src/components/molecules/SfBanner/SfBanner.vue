<template>
  <section
    class="sf-banner"
    :style="style"
    v-on="isMobileView ? $listeners : {}"
  >
    <component :is="wrapper" class="sf-banner__wrapper" :link="link">
      <slot name="subtitle" v-bind="{ subtitle }">
        <h2 v-if="subtitle" class="sf-banner__subtitle">
          {{ subtitle }}
        </h2>
      </slot>
      <slot name="title" v-bind="{ title }">
        <h1 v-if="title" class="sf-banner__title">
          {{ title }}
        </h1>
      </slot>
      <slot name="description" v-bind="{ description }">
        <p v-if="description" class="sf-banner__description">
          {{ description }}
        </p>
      </slot>
      <slot name="call-to-action" v-bind="{ buttonText }">
        <SfButton
          v-if="buttonText && !isMobileView"
          :link="link"
          class="sf-banner__call-to-action color-secondary"
          data-testid="banner-cta-button"
          v-on="!isMobileView ? $listeners : {}"
        >
          {{ buttonText }}
        </SfButton>
      </slot>
    </component>
  </section>
</template>
<script>
import SfButton from "../../atoms/SfButton/SfButton.vue";
import SfLink from "../../atoms/SfLink/SfLink.vue";
import {
  mapMobileObserver,
  unMapMobileObserver,
} from "../../../utilities/mobile-observer";
export default {
  name: "SfBanner",
  components: {
    SfButton,
    SfLink,
  },
  props: {
    /**
     * Banner title
     */
    title: {
      type: String,
      default: "",
    },
    /**
     * Banner subtitle (at the top)
     */
    subtitle: {
      type: String,
      default: "",
    },
    description: {
      type: String,
      default: "",
    },
    /** text that will be displayed inside the button. You can replace the button  with "call-to-action" slot */
    buttonText: {
      type: String,
      default: "",
    },
    /** link to be used in call to action button if necessary */
    link: {
      type: String,
      default: "",
    },
    /** Background color in HEX (eg #FFFFFF) */
    background: {
      type: String,
      default: "",
    },
    /** Background image. Influenced by $banner-background-size, $banner-background-position CSS props. */
    image: {
      type: [String, Object],
      default: "",
    },
  },
  data() {
    return {
      isMobileView: false,
    };
  },
  computed: {
    ...mapMobileObserver(),
    style() {
      const image = this.image;
      const background = this.background;
      return {
        "--_banner-background-image": image.mobile
          ? `url(${image.mobile})`
          : `url(${image})`,
        "--_banner-background-desktop-image":
          image.desktop && `url(${image.desktop})`,
        "--_banner-background-color": background,
      };
    },
    wrapper() {
      return !this.isMobileView ? "div" : this.link ? "SfLink" : "SfButton";
    },
  },
  mounted() {
    this.isMobileView = this.isMobile;
  },
  beforeDestroy() {
    unMapMobileObserver();
  },
};
</script>
<style lang="scss">
@import "~@storefront-ui/shared/styles/components/molecules/SfBanner.scss";
</style>
