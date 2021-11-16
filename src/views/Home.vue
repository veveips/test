<template>
  <div class="home">
    <div class="col">
      <canvas id="left"></canvas>
    </div>
    <div class="right">
      <div class="top">
        <canvas id="top"></canvas>
      </div>
      <div class="content">
        <canvas id="table"></canvas>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      info: [
        ["111", "222", "333"],
        ["444", "555", "666"],
        ["777", "888", "999"],
      ],
      topArr: new Array(26),
      leftArr: new Array(40),
      allData: [],
      mainWidth: "",
      mainHeight: "",
      firstGridWidth: 50,
      gridWidth: 90,
      gridHeight: 30,
    };
  },
  methods: {
    topDraw() {
      var cas = document.getElementById("top");
      cas.width = this.gridWidth * this.topArr.length;
      cas.height = this.gridHeight;
      var ctx = cas.getContext("2d");
      ctx.font = "16px Arial";
      ctx.strokeStyle = "#CCC";
      for (let i = 0; i < this.leftArr.length; i++) {
        ctx.fillStyle = "#EEE";
        ctx.rect(this.gridWidth * i, 0, this.gridWidth, this.gridHeight);
        ctx.fillRect(this.gridWidth * i, 0, this.gridWidth, this.gridHeight);

        var txt = String.fromCharCode(i + 65).toString();
        var textMetrics = ctx.measureText(txt);
        ctx.fillStyle = "#333";
        ctx.fillText(
          txt,
          this.gridWidth * i + (this.gridWidth - textMetrics.width) / 2,
          this.gridHeight - 10
        );
      }
      ctx.stroke();
    },
    leftDraw() {
      var cas = document.getElementById("left");
      cas.width = this.gridWidth;
      cas.height = this.gridHeight * this.leftArr.length;
      var ctx = cas.getContext("2d");
      ctx.font = "16px Arial";
      ctx.strokeStyle = "#CCC";
      for (let i = 0; i < this.leftArr.length; i++) {
        ctx.fillStyle = "#EEE";
        ctx.rect(0, this.gridHeight * i, this.gridWidth, this.gridHeight);
        ctx.fillRect(0, this.gridHeight * i, this.gridWidth, this.gridHeight);

        if (i > 1) {
          var txt = (i - 1).toString();
          var textMetrics = ctx.measureText(txt);
          ctx.fillStyle = "#333";
          ctx.fillText(
            txt,
            (this.gridWidth - textMetrics.width) / 2,
            this.gridHeight * i - 8
          );
        }
      }
      ctx.stroke();
    },
    initCanvas() {
      this.topDraw();
      this.leftDraw();
      var cas = document.getElementById("table");
      cas.width = this.gridWidth * this.topArr.length;
      cas.height = this.gridHeight * this.leftArr.length;
      var ctx = cas.getContext("2d");
      ctx.font = "16px Arial";
      ctx.strokeStyle = "#CCC";
      this.ContentWhite(ctx);
    },
    ContentWhite(ctx) {
      for (let m = 0; m < this.leftArr.length; m++) {
        for (let i = 0; i < this.topArr.length; i++) {
          ctx.rect(
            this.gridWidth * i,
            this.gridHeight * m,
            this.gridWidth,
            this.gridHeight
          );
          ctx.fillStyle = "#fff";
          ctx.fillRect(
            this.gridWidth * i,
            this.gridHeight * m,
            this.gridWidth,
            this.gridHeight
          );
          ctx.fillStyle = "#333";
          if (m < this.info.length && i < this.info[m].length) {
            var txt = this.info[m][i];
            console.log(txt, m, i);
            var textMetrics = ctx.measureText(txt);
            ctx.fillText(
              txt,
              (this.gridWidth - textMetrics.width) / 2 + this.gridWidth * i,
              this.gridHeight * (m + 1) - 8
            );
          }
        }
      }
      ctx.stroke();
    },
    scrollDs() {
      var scrollTopX =
        window.pageXOffset ||
        document.documentElement.scrollTop ||
        document.body.scrollTop;
      var scrollTopY =
        window.pageYOffset ||
        document.documentElement.scrollTop ||
        document.body.scrollTop;

      var left = document.getElementById("left");
      left.style.top = -scrollTopY + "px";

      if (scrollTopX == scrollTopY && scrollTopX != 0) {
        return;
      }
      var top = document.getElementById("top");
      top.style.left = this.gridWidth - scrollTopX + "px";
    },
  },
  mounted() {
    document.getElementById("top").style.left = this.gridWidth + "px";
    document.getElementById("table").style.paddingTop = this.gridHeight + "px";
    document.getElementById("table").style.paddingLeft = this.gridWidth + "px";
    this.initCanvas();
    window.addEventListener("scroll", this.scrollDs, true);
  },
};
</script>
<style lang="less" scoped>
.col {
  #left {
    z-index: 1;
    position: fixed;
    // top: 0;
    // left: 0;
    overflow: scroll;
  }
}
.right {
  #top {
    position: fixed;
    // top: 0;
    // left: 0;
  }
  #table {
    // padding-top: 0;
    // padding-left: 0;
  }
}
</style>
