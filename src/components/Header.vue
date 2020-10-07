<template>
  <header class="header" @mousemove="onMouseMove($event)">
    <div
      class="header__overlay"
      @mousewheel.once="onMouseWheel"
      v-if="!isScrolledOverflow"
    >
      <h2 class="header__title header__overlay-item">Lorem ipsum dolor</h2>
      <p class="header__text header__overlay-item">
        Lorem ipsum dolor sit amet consectetur adipisicing elit. Suscipit ullam
        nostrum consequatur nam iusto, consequuntur ullam nostrum consequatur
        nam iusto, consequuntur
      </p>
    </div>

    <div class="divider" ref="divider">
      <!-- <img src="@/assets/divider.svg" alt="" /> -->

      <div class="divider-inner">
        <svg
          viewBox="0 0 100 600"
          width="100"
          height="600"
          xmlns="http://www.w3.org/2000/svg"
          stroke="black"
          fill="transparent"
        >
          <clipPath
            id="clip"
            clipPathUnits="objectBoundingBox"
            transform="scale(0.000694444444444, 0.001680672268908)"
          >
            <path
              id="path"
              d="M0 572.498V0H1440V595C1401.96 577.556 1273.83 537.75 1065.61 518.074C805.339 493.478 585.61 595 310.323 595C148.533 595 37.4206 578.167 0.835995 572.624L0 572.498Z"
              fill="#C4C4C4"
            />
          </clipPath>
        </svg>

        <div class="divider-green"></div>
        <div class="icon"></div>
      </div>
    </div>

    <div class="header__photos-container">
      <div
        class="header__photo-item-bg header__photo-item-bg--back"
        :style="{ 'background-image': 'url(' + photoBack + ')' }"
        ref="photoItemBack"
      ></div>
      <div
        class="header__photo-item-bg header__photo-item-bg--front"
        :style="{ 'background-image': 'url(' + photoFront + ')' }"
        ref="photoItemFront"
      ></div>
    </div>

    <div class="header__left-text" ref="headerLeftText">
      <p class="header__subtitle header-left-text-stagger">
        <span class="pseudo-text-bg">Moderate to Serve</span>
      </p>
      <h3 class="header__title header-left-text-stagger">
        <span class="pseudo-text-bg pseudo-text-letter">plague</span><br /><span
          class="pseudo-text-bg"
          >psorias</span
        >
      </h3>
      <a href="/some-route" class="header__link header-left-text-stagger"
        >learn more -></a
      >
    </div>
    <div class="header__left-text-vertical">
      <p class="header__subtitle-vertical">Moderate to Serve</p>
      <h3 class="header__title-vertical">plague psorias</h3>
    </div>

    <div class="header__right-text" ref="headerRightText">
      <h3 class="header__title header-right-text-stagger">tremfya</h3>
      <p class="header__subtitle header-right-text-stagger">(guselcumab)</p>
      <a href="/some-route" class="header__link header-right-text-stagger"
        >learn more -></a
      >
    </div>
    <div class="header__right-text-vertical">
      <h3 class="header__title-vertical">tremfya</h3>
      <p class="header__subtitle-vertical">(guselcumab)</p>
    </div>
  </header>
</template>

<script>
import { gsap } from "gsap";
import CSSRulePlugin from "gsap/CSSRulePlugin";
import photoBack from "@/assets/WomanTremfya_MQ.png";
import photoFront from "@/assets/WomanPsoriasis_MQ.png";

gsap.registerPlugin(CSSRulePlugin);

let dividerAppear;
let timelineLeftTextAppear;
let timelinePhotoBack;
let timelinePhotoFront;
let masterTimeline;

export default {
  name: "Header",
  data() {
    return {
      windowWidth: 0,
      photoBack,
      photoFront,
      isScrolledOverflow: false
    };
  },
  created() {
    window.addEventListener("resize", this.onWindowResize);
  },
  mounted() {
    const { divider, photoItemFront, photoItemBack } = this.$refs;
    this.windowWidth = window.innerWidth;

    gsap.defaults({ ease: "linear", duration: 0.2 });

    dividerAppear = gsap.from(".divider", {
      x: 2500,
      duration: 2,
      ease: "power2.inOut",
      paused: true
    });

    const pseudoTextBg = CSSRulePlugin.getRule("::before");
    timelineLeftTextAppear = gsap.timeline();
    timelineLeftTextAppear
      .from(".header-left-text-stagger", {
        x: -30,
        opacity: 0,
        stagger: 0.075,
        ease: "power2.inOut"
      })
      .to(".header-left-text-stagger", {
        x: 0,
        opacity: 1,
        stagger: 0.075,
        ease: "power2.inOut"
      })
      .to(
        pseudoTextBg,
        {
          duration: 1,
          cssRule: { width: 0 },
          stagger: 0.2,
          ease: "power2.inOut"
        },
        "-=0.8"
      );
    timelineLeftTextAppear.pause();

    const leftTextScene = () => {
      let timelineLeftText = gsap.timeline();
      timelineLeftText
        .to(".header-left-text-stagger", {
          x: -75,
          opacity: 0,
          stagger: 0.075
        })
        .to(".header__left-text-vertical", {
          x: -40,
          opacity: 1
        });
      return timelineLeftText;
    };

    const rightTextScene = () => {
      let timelineRightText = gsap.timeline();
      timelineRightText
        .from(".header__right-text-vertical", {
          x: 0,
          opacity: 1
        })
        .from(".header-right-text-stagger", {
          x: 75,
          opacity: 0,
          stagger: 0.075
        });
      return timelineRightText;
    };

    const backgroundScene = () => {
      let timelineBackground = gsap.timeline();
      timelineBackground.to(divider, {
        x: 300,
        duration: 1
      });
      return timelineBackground;
    };

    masterTimeline = gsap
      .timeline()
      .add(rightTextScene())
      .add(backgroundScene(), "-=0.5")
      .add(leftTextScene(), "-=0.5");

    timelinePhotoBack = gsap.timeline();
    timelinePhotoFront = gsap.timeline();

    timelinePhotoBack.to(photoItemFront, {
      width: 0
    });
    timelinePhotoFront.to(photoItemBack, {
      width: 800
    });

    masterTimeline.pause();
    timelinePhotoBack.pause();
    timelinePhotoFront.pause();
  },
  destroyed() {
    window.removeEventListener("resize", this.onWindowResize);
  },
  methods: {
    onWindowResize() {
      this.windowWidth = window.innerWidth;
    },
    onMouseMove(event) {
      if (this.isScrolledOverflow) {
        const mouseX = event.x;
        const percentOfCursorPosX = mouseX / this.windowWidth;

        timelinePhotoBack.progress(1 - percentOfCursorPosX);
        timelinePhotoFront.progress(1 - percentOfCursorPosX);
        masterTimeline.progress(percentOfCursorPosX);
      }

      // * Some attempt to sync photo & bg
      // const { photoItemBack } = this.$refs;
      // const boundingRect = photoItemBack.getBoundingClientRect();
      // const mouseXtemp = event.x;
      // const percentOfCursorPosXtemp = mouseXtemp / 800;
      // if (mouseXtemp > boundingRect.left) {
      //   timelinePhotoBack.progress(1 - percentOfCursorPosXtemp);
      //   timelinePhotoFront.progress(1 - percentOfCursorPosXtemp);
      //   console.log(percentOfCursorPosXtemp);
      // }
    },
    onMouseWheel() {
      let headerOverlay = gsap.timeline();
      dividerAppear.play();
      headerOverlay
        .to(".header__overlay-item", {
          y: -300,
          opacity: 0,
          duration: 1,
          ease: "power2.inOut",
          stagger: 0.1
        })
        .then(() => {
          timelineLeftTextAppear.play().then(() => {
            this.isScrolledOverflow = true;
          });
        });
    }
  }
};
</script>

<style lang="scss" scoped>
$yellow: #f8b735;
$green: #03b3b0;

.header {
  padding: 30px 0;
  background-color: $yellow;
  margin: 0;
  overflow: hidden;
  position: relative;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.header__photos-container {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  margin-left: auto;
  margin-right: auto;
  width: 800px;
  height: 720px;
  z-index: 50;
}

.header__photo-item-bg {
  position: absolute;
  height: 720px;

  &--back {
    width: 0;
    z-index: 5;
  }

  &--front {
    width: 800px;
    z-index: 10;
    right: 0;
    background-position: right;
  }
}

.icon {
  width: 15px;
  height: 15px;
  background: white;
  border: 1px solid black;
  border-radius: 50%;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.divider {
  width: 100px;
  transform: translate3d(1500px, 0px, 0px);
}

.divider-inner {
  transform: rotate(90deg);
  position: relative;
}

.divider-green {
  width: 2050px;
  height: 3830px;
  background-color: $green;
  clip-path: url(#clip);
  position: absolute;
  bottom: 0;
  right: -1121px;
  transform: skewY(30deg);
}

.header__right-text {
  color: $yellow;
  position: absolute;
  right: 260px;
  text-align: right;

  .header__title {
    margin-bottom: 10px;
  }

  .header__link {
    &:hover {
      color: $yellow;
    }
  }
}

.header__left-text {
  color: $green;
  position: absolute;
  left: 260px;
  text-align: left;

  .header__title {
    margin-top: 10px;
    margin-bottom: 10px;
  }

  .header__link {
    &:hover {
      color: $green;
    }
  }
}

.pseudo-text-letter {
  &::after {
    content: "P";
    color: rgba(#fff, 0.15);
    font-size: 135px;
    position: absolute;
    top: 0;
    left: 0;
    z-index: -1;
  }
}

.header__right-text-vertical {
  color: $yellow;
  position: absolute;
  right: 0;
  text-align: center;
  opacity: 0;
  transform: translate3d(30px, 0px, 0px) rotate(90deg);
}

.header__left-text-vertical {
  color: $green;
  position: absolute;
  left: 0;
  text-align: center;
  opacity: 0;
  transform: translate3d(-30px, 0px, 0px) rotate(-90deg);

  .header__title-vertical {
    margin-bottom: 0;
    margin-top: 5px;
  }
}

.header__title {
  font-size: 56px;
  font-weight: 600;
  letter-spacing: 3px;
  text-transform: uppercase;
}

.header__subtitle {
  font-size: 30px;
}

.header__link {
  display: block;
  color: #fff;
  font-size: 16px;
  font-weight: 600;
  text-transform: uppercase;
  text-decoration: none;
  margin-top: 20px;
  transition: color 0.15s ease;
}

.header__title-vertical {
  font-size: 20px;
  font-weight: 600;
  letter-spacing: 2px;
  text-transform: uppercase;
  margin-bottom: 5px;
}

.header__subtitle-vertical {
  font-weight: 500;
}

.header__overlay {
  background: transparent;
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 80;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  color: #fff;
  text-align: center;
  text-shadow: 0px 0px 20px #00000059;
}

.header__title {
  margin-bottom: 100px;
  max-width: 800px;
  letter-spacing: 10px;
}

.header__text {
  font-size: 28px;
  max-width: 800px;
  line-height: 1.5;
}

.pseudo-text-bg {
  position: relative;

  &::before {
    content: "";
    background-color: $green;
    width: 100%;
    height: 100%;
    position: absolute;
    top: 0;
    right: 0;
    z-index: 1;
  }
}

.header-left-text-stagger {
  overflow: hidden;
}
</style>
