<template>
  <header class="header" @mousemove="onMouseMove($event)">
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
      <h3 class="header__title header-left-text-stagger">tremfya</h3>
      <p class="header__subtitle header-left-text-stagger">(guselcumab)</p>
      <a href="/some-route" class="header__link header-left-text-stagger"
        >learn more -></a
      >
    </div>
    <div class="header__left-text-vertical">
      <h3 class="header__title-vertical">tremfya</h3>
      <p class="header__subtitle-vertical">(guselcumab)</p>
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
import photoBack from "@/assets/WomanTremfya_MQ.png";
import photoFront from "@/assets/WomanPsoriasis_MQ.png";

let timelinePhotoBack;
let timelinePhotoFront;
let masterTimeline;

export default {
  name: "Header",
  data() {
    return {
      windowWidth: 0,
      photoBack,
      photoFront
    };
  },
  created() {
    window.addEventListener("resize", this.onWindowResize);
  },
  mounted() {
    const { divider, photoItemFront, photoItemBack } = this.$refs;
    this.windowWidth = window.innerWidth;

    gsap.defaults({ ease: "linear", duration: 0.2 });

    const leftTextScene = () => {
      let timelineLeftText = gsap.timeline();
      timelineLeftText
        .to(".header-left-text-stagger", {
          x: 75,
          opacity: 0,
          stagger: 0.075
        })
        .to(".header__left-text-vertical", {
          x: 0,
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
      const mouseX = event.x;
      const percentOfCursorPosX = mouseX / this.windowWidth;

      timelinePhotoBack.progress(1 - percentOfCursorPosX);
      timelinePhotoFront.progress(1 - percentOfCursorPosX);
      masterTimeline.progress(percentOfCursorPosX);

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
    }
  }
};
</script>

<style lang="scss" scoped>
.header {
  padding: 30px 0;
  background-color: #e9c46a;
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
  background-color: #2a9d8f;
  clip-path: url(#clip);
  position: absolute;
  bottom: 0;
  right: -1121px;
  transform: skewY(30deg);
}

.header__right-text {
  color: #e9c46a;
  position: absolute;
  right: 260px;
  text-align: right;

  .header__title {
    margin-bottom: 10px;
  }

  .header__link {
    &:hover {
      color: #e9c46a;
    }
  }
}

.header__left-text {
  color: #2a9d8f;
  position: absolute;
  left: 260px;
  text-align: left;

  .header__title {
    margin-bottom: 10px;
  }

  .header__link {
    &:hover {
      color: #2a9d8f;
    }
  }
}

.header__right-text-vertical {
  color: #e9c46a;
  position: absolute;
  right: 0;
  text-align: center;
  opacity: 0;
  transform: translate3d(30px, 0px, 0px) rotate(90deg);
}

.header__left-text-vertical {
  color: #2a9d8f;
  position: absolute;
  left: 0;
  text-align: center;
  opacity: 0;
  transform: translate3d(-30px, 0px, 0px) rotate(-90deg);
}

.header__title {
  font-size: 56px;
  font-weight: 600;
  letter-spacing: 5px;
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
</style>
