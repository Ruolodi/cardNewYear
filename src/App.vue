<template>
  <section class="sky">
    <div class="container-main">
      <div v-for="(text, id) in links" :key="id" class="center-div-gen">
        <div
          class="slide"
          :style="` ${
            showArr[id]
              ? 'transition: opacity 0.3s ease;'
              : 'opacity: 0; transition: opacity 0.3s ease;'
          }`"
        >
          <div class="title-block">{{ text.title }}</div>
          <hr style="width: 85%; margin: 10px 0 10px 0" />
          <div class="text-block">{{ text.text }}</div>
          <div v-if="text.textPs" class="text-blockPS">{{ text.textPs }}</div>
        </div>
      </div>
    </div>
    <HelloWorld />
  </section>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import * as dat from "dat.gui";

export default {
  name: "App",
  components: {
    HelloWorld,
  },
  data() {
    return {
      heightVar: 0,
      showArr: [false, false, false, false, false, false, false],
      links: [],
    };
  },
  mounted() {
    this.links.push({
      title: "От всех нас:",
      text: "Привет Динянька! Хотим поздравить тебя с Новым Годом! Пожелать счастья, а главное здоровья и адекватных врачей. Оставайся такой же позитивной девчонкой! Мы всегда рады тебя поддержать и если что выслушать. Мы тебя очень любим, будь счастлива и никогда не грусти!",
    });
    this.links.push({
      title: "Павел:",
      text: "Привет Диан), я очень сильно постарался над этой открыткой) (Надеюсь она тебе понравится). В общем хочу, чтобы в будущем ты освоила какую нибудь крутую профессию и зарабатывала много деняк. Желаю чтобы ничего у тебя не болело и ты всегда получала от жизни только позитивные эмоции! С Новым Годом!",
      textPs: "P.S. Всегда совершенствуй свои навыки в любых областях.",
    });
    this.links.push({
      title: "Николай Ветров:",
      text: "Диана! Поздравляю тебя с Новым годом!  Желаю тебе позитивного настроя и личностного роста в предстоящем году! Никогда не забрасывай то к чему у тебя талант!  Желаю здоровья и денег, но в первую очередь обычного человеческого счастья, ведь без него все выше вышеперечисленное не имеет смысла.  С Новым годом!",
      textPs: "P.S. Не забрасывай графический дизайн.",
    });
    this.links.push({
      title: "Никита:",
      text: "Динянюха! Уходящий год был не простым для всех, но следующий будет мягким и пушистым (как зайчик). Лично от себя желаю что бы титя не болела и живот не ворчал, что бы какать часто не бегала и что бы Коля кампухтер больше не дудосил. Я помогу тебе в любых твоих начинаниях и поддержу во всех трудностях. В общем с Новым Годом! < 3",
    });
    this.links.push({
      title: "Николай Молев:",
      text: "Дорогая Диняня! Хочу от всей души поздравить тебя с Новым 2023 годом! Желаю здоровья, счастья, успехов во всех твоих творческих и жизненных начинаниях (магнитик кстати в тему из подарка), чтобы в новом году ты обрела свою финансовую независимость и стабильность, чтобы на личном фронте все складывалось как нельзя лучше и возникающие споры вы решали вместе, общаясь. Передаю тебе небольшой презент, твой друг Колька.",
    });
    this.links.push({
      title: "Дима:",
      text: "Привет Дианка! Поздравляю тебя с Новым Годом! Желаю чтобы в новом году гача-удача была на твоей стороне и всякие замороженные дети обходили стороной. Желаю тебе роста в личностном плане и не только, расти над собой, расти над другими, но никогда не зазнавайся) Пусть этот год у тебя будет ярким и богатым на приятные моменты, о которых будешь всегда вспоминать с улыбкой)",
      textPs: "P.S. Люби нас как мы любим тебя <3.",
    });

    setTimeout(() => {
      this.showArr[0] = true;
    }, 800);
    this.letItSnow();
  },
  beforeMount() {
    window.addEventListener("scroll", this.handleScroll);
  },
  methods: {
    letItSnow() {
      let particleCount = 300;
      let particleMax = 1000;
      let sky = document.querySelector(".sky");
      let canvas = document.createElement("canvas");
      let ctx = canvas.getContext("2d");
      let width = sky.clientWidth;
      let height = sky.clientHeight;
      let i = 0;
      let active = false;
      let snowflakes = [];
      let snowflake;

      canvas.style.position = "absolute";
      canvas.style.left = canvas.style.top = "0";

      let Snowflake = function () {
        this.x = 0;
        this.y = 0;
        this.vy = 0;
        this.vx = 0;
        this.r = 0;

        this.reset();
      };

      Snowflake.prototype.reset = function () {
        this.x = Math.random() * width;
        this.y = Math.random() * -height;
        this.vy = 1 + Math.random() * 3;
        this.vx = 0.5 - Math.random();
        this.r = 1 + Math.random() * 2;
        this.o = 0.5 + Math.random() * 0.5;
      };

      function generateSnowFlakes() {
        snowflakes = [];
        for (i = 0; i < particleMax; i++) {
          snowflake = new Snowflake();
          snowflake.reset();
          snowflakes.push(snowflake);
        }
      }

      generateSnowFlakes();

      function update() {
        ctx.clearRect(0, 0, width, height);

        if (!active) {
          return;
        }

        for (i = 0; i < particleCount; i++) {
          snowflake = snowflakes[i];
          snowflake.y += snowflake.vy;
          snowflake.x += snowflake.vx;

          ctx.globalAlpha = snowflake.o;
          ctx.beginPath();
          ctx.arc(snowflake.x, snowflake.y, snowflake.r, 0, Math.PI * 2, false);
          ctx.closePath();
          ctx.fill();

          if (snowflake.y > height) {
            snowflake.reset();
          }
        }

        window.requestAnimFrame(update);
      }

      function onResize() {
        width = sky.clientWidth;
        height = sky.clientHeight;
        canvas.width = width;
        canvas.height = height;
        ctx.fillStyle = "#fff";

        let wasActive = active;
        active = width > 600;

        if (!wasActive && active) {
          window.requestAnimFrame(update);
        }
      }

      // shim layer with setTimeout fallback
      window.requestAnimFrame = (function () {
        return (
          window.requestAnimationFrame ||
          window.webkitRequestAnimationFrame ||
          window.mozRequestAnimationFrame ||
          function (callback) {
            window.setTimeout(callback, 1000 / 60);
          }
        );
      })();

      onResize();
      window.addEventListener("resize", onResize, false);

      sky.appendChild(canvas);

      let gui = new dat.GUI();
      gui
        .add(window, "particleCount")
        .min(1)
        .max(particleMax)
        .step(1)
        .name("Particles count")
        .onFinishChange(function () {
          window.requestAnimFrame(update);
        });
    },
    handleScroll() {
      this.heightVar = window.scrollY;
      for (let i = 0; i < this.showArr.length; i++) {
        if (i >= 1) {
          if (this.heightVar >= 500 * i - 1) {
            this.showArr[i] = true;
          } else if (this.heightVar <= 500 * i - 1) {
            this.showArr[i] = false;
          }
        } else if (i == 0) {
          if (this.heightVar >= 500) {
            this.showArr[i] = true;
          }
        }
      }
    },
  },
};
</script>

<style lang="scss">
.dg.ac {
  display: none;
}
.sky {
  height: 4080px;
  background: url("./assets/home-fon.jpg") 50% 0 / cover no-repeat;
  color: #fff;
  display: block;
}

html,
body {
  height: 100%;
  width: 100%;
}

body {
  font-family: Comic Sans MS, Comic Sans, cursive;
  margin: 0;
}

.container-main {
  width: 100%;
  overflow-x: hidden;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: flex-start;
  flex-direction: column;
  .center-div {
    border-radius: 16px;
    margin-top: 50px;
    height: fit-content;
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100%;
  }
  .center-div-gen {
    color: black;
    margin-top: 15px;
    margin-bottom: 15px;
    height: 15%;
    display: flex;
    align-items: center;
    justify-content: center;
    width: 85%;
    min-width: 750px;
    .slide {
      background-color: rgba(0, 0, 0, 0.5);
      -webkit-box-shadow: 0px 0px 42px 0px rgba(0, 0, 0, 0.7);
      -moz-box-shadow: 0px 0px 42px 0px rgba(0, 0, 0, 0.7);
      box-shadow: 0px 0px 42px 0px rgba(0, 0, 0, 0.7);
      border-radius: 16px;
      width: 95%;
      height: 100%;
      display: flex;
      align-items: center;
      justify-content: center;
      flex-direction: column;
      .title-block {
        text-align: center;
        height: fit-content;
        color: white;
        padding: 0 50px 0 50px;
        font-size: 45px;
        text-shadow: 1px 1px 1px #000;
      }
      .text-block {
        text-align: center;
        height: fit-content;
        color: white;
        padding: 0 50px 0 50px;
        font-size: 35px;
        text-shadow: 1px 1px 1px #000;
      }
      .text-blockPS {
        width: 85%;
        margin-top: 25px;
        text-align: left;
        height: fit-content;
        color: white;
        padding: 0 50px 0 50px;
        font-size: 35px;
        text-shadow: 1px 1px 1px #000;
      }
    }
  }
}
</style>
