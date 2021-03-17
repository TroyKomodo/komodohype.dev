<template>
  <canvas :style="{ top: `${top}px`, left: `${left}px` }" />
</template>

<script lang="ts">
// All math from the following lib. I just typed it and also improved a bit.
/*
 * nodes.js is a nodes/particles animation useable for backgrounds
 *
 * http://oguzhaneroglu.com/projects/nodes.js/
 * https://github.com/rohanrhu/nodes.js
 *
 * Copyright (C) 2018, Oğuzhan Eroğlu <rohanrhu2@gmail.com>
 * Licensed under MIT
 */

import Vue from "vue";

export default Vue.extend({
  props: {
    particleSize: {
      type: Number,
      default: 1,
    },
    lineSize: {
      type: Number,
      default: 1,
    },
    lineColor: {
      type: String,
      default: null,
    },
    backgroundFrom: {
      type: Array as () => Array<number>,
      default: null,
    },
    backgroundTo: {
      type: Array as () => Array<number>,
      default: null,
    },
    backgroundDuration: {
      type: Number,
      default: 4000,
    },
    speed: {
      type: Number,
      default: 20,
    },
    nobg: {
      type: Boolean,
      default: false,
    },
  },

  data() {
    return {
      nodes: [] as [number, number, number, number[]][],
      t0: 0,
      dt: 0,

      stopped: false,
      ctx: null as null | CanvasRenderingContext2D,
      resized: false,

      top: 0,
      left: 0,
    };
  },

  mounted() {
    const canvas = this.$el as HTMLCanvasElement;

    this.t0 = Date.now();
    this.ctx = canvas.getContext("2d");

    this.onResize();

    window.addEventListener("mousemove", this.onMouseMove);
    window.addEventListener("touchmove", this.onTouchMove);
    window.addEventListener("resize", this.onResize);

    const step = () => {
      if (this.stopped) return;
      window.requestAnimationFrame(step);

      this.ctx!.clearRect(0, 0, canvas.height, canvas.width);
      if (!this.nobg) {
        const r = Math.floor(
          ((Math.sin(
            (Math.PI * 2 * Date.now()) / this.backgroundDuration - Math.PI / 2
          ) +
            1) /
            2) *
            (this.backgroundFrom[0] - this.backgroundTo[0] + 1) +
            this.backgroundTo[0]
        );
        const g = Math.floor(
          ((Math.sin(
            (Math.PI * 2 * Date.now()) / this.backgroundDuration - Math.PI / 2
          ) +
            1) /
            2) *
            (this.backgroundFrom[1] - this.backgroundTo[1] + 1) +
            this.backgroundTo[1]
        );
        const b = Math.floor(
          ((Math.sin(
            (Math.PI * 2 * Date.now()) / this.backgroundDuration - Math.PI / 2
          ) +
            1) /
            2) *
            (this.backgroundFrom[2] - this.backgroundTo[2] + 1) +
            this.backgroundTo[2]
        );

        this.ctx!.beginPath();
        this.ctx!.fillStyle = `rgb(${r}, ${g}, ${b})`;
        this.ctx!.fillRect(0, 0, canvas.width, canvas.height);
        this.ctx!.fill();
      }

      const screenDiag =
        Math.min(window.innerHeight * 0.1, window.innerWidth * 0.1) * 2;

      this.nodes.forEach((node, i) => {
        node[0] +=
          (((Math.cos(node[2]) * this.speed * this.dt) / 1000) * screenDiag) /
          150;
        node[1] +=
          (((Math.sin(node[2]) * this.speed * this.dt) / 1000) * screenDiag) /
          150;

        if (node[0] < 0) {
          node[0] += canvas.width;
        } else if (node[0] > canvas.width) {
          node[0] %= canvas.width;
        }

        if (node[1] < 0) {
          node[1] += canvas.height;
        } else if (node[1] > canvas.height) {
          node[1] %= canvas.height;
        }

        this.ctx!.fillStyle = `rgb(${this.lineColor})`;
        this.ctx!.beginPath();
        this.ctx!.arc(
          node[0],
          node[1],
          this.particleSize,
          0,
          Math.PI * 2,
          true
        );
        this.ctx!.fill();
        node[3] = [];

        this.nodes.forEach((node2, i2) => {
          if (i === i2 || node[3].includes(i2)) return true;

          const d = Math.sqrt(
            Math.pow(Math.abs(node[0] - node2[0]), 2) +
              Math.pow(Math.abs(node[1] - node2[1]), 2)
          );

          if (d > screenDiag) return true;

          const alpha = 0.3 - (0.3 * d) / (screenDiag * 0.9);

          node2[3].push(i);

          this.ctx!.strokeStyle = `rgba(${this.lineColor}, ${alpha})`;
          this.ctx!.lineWidth = this.lineSize;

          this.ctx!.beginPath();
          this.ctx!.moveTo(node[0], node[1]);
          this.ctx!.lineTo(node2[0], node2[1]);
          this.ctx!.stroke();
        });
      });
      const now = Date.now();
      this.dt = now - this.t0;
      this.t0 = now;
    };
    step();
  },

  beforeDestroy() {
    this.stopped = true;
    window.removeEventListener("mousemove", this.onMouseMove);
    window.removeEventListener("touchmove", this.onTouchMove);
    window.removeEventListener("resize", this.onResize);
  },

  methods: {
    onTouchMove(event: TouchEvent) {
      this.onMouseMove(event.touches[0]);
    },
    onMouseMove(event: {
      clientX: number;
      clientY: number;
      target: EventTarget | null;
    }) {
      if (this.stopped) {
        window.removeEventListener("mousemove", this.onMouseMove);
        return;
      }

      if (!this.nodes.length) return;

      const canvas = this.$el as HTMLCanvasElement;

      const mx = event.clientX - this.left;
      const my = event.clientY - this.top;

      const pointerCircleRadius = Math.min(
        canvas.height * 0.1,
        canvas.width * 0.1
      );

      this.nodes.forEach((node) => {
        const [nx, ny] = node;

        const xsig = nx - mx;
        const ysig = ny - my;

        const ndx = Math.abs(xsig);
        const ndy = Math.abs(ysig);

        const nh = Math.sqrt(Math.pow(ndx, 2) + Math.pow(ndy, 2));

        let angle = Math.acos(ndx / nh);

        if (xsig <= -1 && ysig <= -1) {
          angle = Math.PI - angle;
        } else if (xsig <= -1 && ysig >= 0) {
          angle += Math.PI;
        } else if (xsig >= 0 && ysig >= 0) {
          angle = Math.PI * 2 - angle;
        }

        angle = Math.PI * 2 - angle;

        const rx = mx + Math.cos(angle) * pointerCircleRadius;
        const ry = my + Math.sin(angle) * pointerCircleRadius;

        const mdx = Math.abs(rx - mx);
        const mdy = Math.abs(ry - my);

        const mh = Math.sqrt(Math.pow(mdx, 2) + Math.pow(mdy, 2));

        if (nh < mh) {
          node[0] = Math.floor(rx);
          node[1] = Math.floor(ry);
        }
      });
    },
    onResize() {
      const canvas = this.$el as HTMLCanvasElement;

      if (
        canvas.height === window.innerHeight &&
        canvas.width === window.innerWidth
      )
        return;

      const diag =
        Math.min(window.innerHeight * 0.1, window.innerWidth * 0.1) * 2;

      canvas.height = window.innerHeight + 2 * diag;
      canvas.width = window.innerWidth + 2 * diag;

      this.top = -diag;
      this.left = -diag;

      if (!this.resized) {
        this.resized = true;
        window.requestAnimationFrame(() => {
          if (this.stopped) return;
          const pc = Math.sqrt(
            Math.pow(window.innerHeight, 2) + Math.pow(window.innerWidth, 2)
          );

          const nodes = Math.round(
            Math.min(window.orientation ? 60 : 250, Math.max(pc * 0.08, 60))
          );

          this.placeNodes(nodes);
          this.resized = false;
        });
      }
    },
    placeNodes(number: number) {
      const canvas = this.$el as HTMLCanvasElement;

      this.nodes = [];

      for (let i = 0; i < number; i++) {
        this.nodes.push([
          Math.floor(Math.random() * (canvas.width + 1)),
          Math.floor(Math.random() * (canvas.height + 1)),
          Math.random() * (Math.PI * 2 + 1),
          [],
        ]);
      }
    },
  },
});
</script>

<style scoped>
canvas {
  position: fixed;
  z-index: -1;
  pointer-events: none;
}
</style>
