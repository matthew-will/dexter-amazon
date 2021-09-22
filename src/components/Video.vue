<template>
  <div class="video-component">
    <div class="landing">
      <img class="dexter-logo" src="logo.png" alt="Dexter Logo" />
      <img class="scroll-cta" src="scroll-cta-2.png" alt="Scroll to continue" />
    </div>

    <div class="lockup">
      <a href="https://www.sho.com/dexter-new-blood" target="_blank"
        ><img class="watch-now" src="watch-now.png" alt=""
      /></a>
      <img class="tune-in" src="tune-in.png" alt="" />
    </div>

    <div class="endframe">
      <a href="https://www.instagram.com/sho_dexter/?hl=en" target="_blank"
        ><img class="link yellow" src="filter.png" alt=""
      /></a>
      <a
        href="https://www.sho.com/dexter-new-blood#/stream/series/323/int-dexter-6447"
        target="_blank"
        ><img class="link yellow" src="catchup.png" alt=""
      /></a>
      <a
        href="https://www.sho.com/video/76209/yellowjackets-official-trailer"
        target="_blank"
        ><img class="link yellow" src="yellowjackets.png" alt=""
      /></a>
      <div class="endframe-bg"></div>
    </div>
    <div class="video-outer">
      <video
        @loadedmetadata="scrollTrigger"
        ref="video"
        playsinline
        muted
        class="video-background"
      ></video>
    </div>

    <div id="container"></div>
  </div>
</template>

<script>
import { gsap } from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";

gsap.registerPlugin(ScrollTrigger);

export default {
  name: "Video",
  data() {
    return {
      video: "",
      activated: false,
      bg: "",
    };
  },
  mounted() {
    gsap.to(".landing", { autoAlpha: 1, duration: 1 });
    if (window.innerWidth < 600) {
      this.$refs.video.src = "/mobile.mp4";
      this.bg = "/mobile-bg.jpg";
    } else {
      this.$refs.video.src = "/desktop.mp4";
      this.bg = "/desktop-bg.jpg";
    }
    document.addEventListener("touchstart", () => {
      if (!this.activated) {
        this.$refs.video.play();
        this.$refs.video.pause();
        this.activated = true;
      }
    });
  },
  methods: {
    scrollTrigger() {
      /* The encoding is super important here to enable frame-by-frame scrubbing. */

      // ffmpeg -i *insert file name* -movflags faststart -vcodec libx264 -crf 23 -g 1 -pix_fmt yuv420p output.mp4
      // ffmpeg -i *insert file name* -vf scale=960:-1 -movflags faststart -vcodec libx264 -crf 20 -g 1 -pix_fmt yuv420p output_960.mp4

      let tl = gsap.timeline({
        defaults: { duration: 1 },
        scrollTrigger: {
          trigger: "#container",
          start: "top top",
          end: "90% 90%",
          scrub: true,
          onEnter: () => {
            gsap.to(".landing", { autoAlpha: 0, duration: 1, scale: 0.95 });
          },
          onLeave: () => {
            gsap.to(".endframe-bg", {
              duration: 1,
              autoAlpha: 1,
            });
            gsap.to(".link", {
              autoAlpha: 1,
              y: 0,
              duration: 0.75,
              stagger: 0.1,
              ease: "Expo.out",
            });
          },
          onEnterBack: () => {
            gsap.to(".endframe-bg", {
              duration: 1,
              autoAlpha: 0,
            });
            gsap.to(".link", {
              autoAlpha: 0,
              y: 20,
              duration: 0.5,
              ease: "Expo.out",
            });
          },
          onLeaveBack: () => {
            gsap.to(".landing", { autoAlpha: 1, duration: 0.75, scale: 1 });
          },
        },
      });

      tl.to(this.$refs.video, { currentTime: this.$refs.video.duration - 1 });
    },
  },
};
</script>

<style scoped>
.video-component {
  background-image: url("/desktop-bg.jpg");
  height: 100vh;
  width: 100vw;
  background-size: cover;
}
.video-outer,
.landing {
  position: fixed;
  top: 0;
  left: 0;
  height: 100vh;
  width: 100vw;
}
.lockup {
  position: fixed;
  bottom: 0;
  width: 100vw;
  z-index: 103;
}
.landing {
  z-index: 101;
  display: flex;
  width: 100vw;
  height: 100vh;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  opacity: 0;
}
video,
.background {
  height: 100%;
  width: 100%;
  object-fit: cover;
}

#container {
  height: 500vh;
}

.cta {
  position: fixed;
  bottom: 10vh;
  z-index: 101;
  display: flex;

  width: 100vw;
  justify-content: center;
  align-items: center;
}
.button {
  background: #ad071c;
  padding: 20px 30px;
  width: auto;
  color: white;
  text-transform: uppercase;
  margin: 10px;
  transition: background 250ms;
  cursor: pointer;
  opacity: 0;
  transform: translateY(20px);
}
.button:hover {
  background: #850516;
}
.dexter-logo {
  width: 50%;
  margin-bottom: 3vh;
}
.scroll-cta {
  max-width: 320px;
}
.tune-in {
  max-width: 235px;
  position: fixed;
  bottom: 3vh;
  right: 3vw;
}
.watch-now {
  max-width: 200px;
  position: fixed;
  bottom: 3vh;
  left: 3vw;
}
.endframe {
  height: 100vh;
  width: 100vw;
  position: fixed;
  top: 0;
  left: 0;
  display: flex;
  flex-wrap: wrap;
  z-index: 102;
  justify-content: center;
  align-items: center;
}
.endframe a {
  z-index: 3;
}
.endframe-bg {
  z-index: 2;
  position: fixed;
  height: 100%;
  width: 100%;
  background-color: rgba(0, 0, 0, 0.65);
  opacity: 0;
}
.link {
  max-width: 350px;
  margin: 0 10px;
  transform: translateY(10px);
  opacity: 0;
  z-index: 3;
}
@media only screen and (max-width: 600px) {
  .cta {
    flex-direction: column;
  }
  .button {
    width: 70%;
  }
  #container {
    height: 1000vh;
  }
  .dexter-logo {
    width: 95%;
  }
  .scroll-cta {
    max-width: 275px;
  }
  .video-component {
    background-image: url("/mobile-bg.jpg");
  }
  .tune-in {
    max-width: 150px;
    position: fixed;
    bottom: 15px;
    left: 10px;
  }
  .watch-now {
    max-width: 150px;
    position: fixed;
    bottom: 10px;
    right: 10px;
    left: auto;
  }
  .endframe {
    flex-direction: column;
  }
  .link {
    max-width: 300px;
    margin: 5px 0;
  }
}
</style>
