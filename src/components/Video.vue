<template>
  <div>
    <div class="landing">
      <img class="dexter-logo" src="logo.png" alt="Dexter Logo" />
      <div class="scroll-cta">Scroll to Continue</div>
    </div>

    <div class="cta">
      <div class="button">Launch AR Filter</div>
      <div class="button">Watch the first Episode</div>
      <div class="button">Join Showtime</div>
    </div>
    <div class="video-outer">
      <video
        @loadedmetadata="scrollTrigger"
        ref="video"
        playsinline
        muted
        poster="poster.jpg"
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
    };
  },
  mounted() {
    gsap.to(".landing", { autoAlpha: 1, duration: 1 });
    if (window.innerWidth < 600) {
      this.$refs.video.src = "/dexter-mobile.mp4";
    } else {
      this.$refs.video.src = "/dexter.mp4";
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
            gsap.to(".button", {
              y: 0,
              duration: 1.25,
              autoAlpha: 1,
              stagger: 0.2,
              ease: "Expo.inOut",
            });
          },
          onEnterBack: () => {
            gsap.to(".button", {
              y: 20,
              duration: 0.75,
              autoAlpha: 0,
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
.video-outer,
.landing {
  position: fixed;
  top: 0;
  left: 0;
  height: 100vh;
  width: 100vw;
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
video {
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
  margin-bottom: 10vh;
}
.scroll-cta {
  background: #ad071c;
  padding: 20px 30px;
  width: auto;
  color: white;
  text-transform: uppercase;
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
    margin-bottom: 15vh;
    margin-top: 15vh;
  }
}
</style>
