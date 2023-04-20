<template>
  <div
    @click="show"
    role="link"
    class="card card-vacancy"
  >
    <div class="card-vacancy__button-favourite">
      <svg stroke="currentColor" fill="currentColor" stroke-width="0" viewBox="0 0 512 512" height="20" width="20" xmlns="http://www.w3.org/2000/svg"><path fill="none" stroke-linecap="round" stroke-linejoin="round" stroke-width="32" d="M352.92 80C288 80 256 144 256 144s-32-64-96.92-64c-52.76 0-94.54 44.14-95.08 96.81-1.1 109.33 86.73 187.08 183 252.42a16 16 0 0018 0c96.26-65.34 184.09-143.09 183-252.42-.54-52.67-42.32-96.81-95.08-96.81z"></path></svg>
    </div>

    <div class="card-vacancy__container">
      <p class="card-vacancy__preview">
        {{ item.employer.title.substr(0, 1) }}
      </p>
      <div class="card-vacancy__details">
        <p class="card-vacancy__title">
          {{ item.vacancy.title }}
        </p>
        <p class="card-vacancy__text">
          <span class="card-vacancy__company">
            {{ item.employer.title }}
          </span> &#x2022;
          <span :class="`card-vacancy__format card-vacancy__format-${item.vacancy.format_type}`">
            {{ item.vacancy.format_raw }}
          </span> &#x2022;
          <span :class="`card-vacancy__experience`">
            {{ item.vacancy.experience_raw }}
          </span>
        </p>
      </div>
    </div>

    <div class="card-vacancy__content" v-html="item.vacancy.raw_text.substr(0, 200) + '...'"></div>
    
    <p class="card-vacancy__salary">
      {{ fixedSalary }}
    </p>

    <button class="card-vacancy__button">
      Подробнее
    </button>
  </div>
</template>

<script>
export default {
  props: {
    item: {
      type: Object,
      required: true
    }
  },
  mounted() {
    console.log(this.item)

    const outputColor = document.querySelectorAll(".card-vacancy__preview");

    for (let colorItem of outputColor) {
      const [angle, color1, color2] = this.randomGradient();
      colorItem.style.setProperty("--color-1", this.randomColor());
      colorItem.style.setProperty("--color-2", this.randomColor());
      colorItem.style.setProperty("--angle", this.randomAngle());
      colorItem.value = `background: linear-gradient(${angle}, ${color1}, ${color2});`;
    }
  },
  methods: {
    rgba2hex(orig) {
      const LITERAL = "01";

      let a,
        rgb = orig.replace(/\s/g, "").match(/^rgba?\((\d+),(\d+),(\d+),?([^,\s)]+)?/i),
        alpha = ((rgb && rgb[4]) || "").trim(),
        hex = rgb
          ? (rgb[1] | (1 << 8)).toString(16).slice(1) +
            (rgb[2] | (1 << 8)).toString(16).slice(1) +
            (rgb[3] | (1 << 8)).toString(16).slice(1)
          : orig;

      a = alpha !== "" ? alpha : LITERAL;

      a = ((a * 255) | (1 << 8)).toString(16).slice(1);
      hex = hex + a;

      return hex;
    },
    randomHex() {
      // Генерируем три случайных числа от 0 до 255
      const r = Math.floor(Math.random() * 256);
      const g = Math.floor(Math.random() * 256);
      const b = Math.floor(Math.random() * 256);
      const a = "0.3";

      const rgbaColor = `rgba(${r}, ${g}, ${b}, ${a})`;

      let hexColor = this.rgba2hex(rgbaColor);
      return hexColor;
    },

    randomColor() {
      return "#" + this.randomHex();
    },
    randomAngle() {
      return `${Math.floor(Math.random() * 361)}deg`;
    },
    randomGradient() {
      return [this.randomAngle(), this.randomColor(), this.randomColor()];
    },
    show() {
      this.$confirm({
        message: `Вы точно хотите перейти к вакансии ${this.item.vacancy.title} ?`,
        acceptText: "Да",
        cancelText: "Нет",
      }).then((result) => {
        if (result) {
          window.location.href = this.item.vacancy.link;
        }
      });
    },
  },
  computed: {
    fixedSalary() {
      return this.item.vacancy.salary_raw.replace('undefined', 'у.е.')
    }
  }
};
</script>

<style scoped>
.card-vacancy {
  width: 100%;
  height: max-content;

  margin-bottom: 20px;

  padding: 20px;

  display: flex;
  flex-direction: column;

  position: relative;

  background: #e9ebf0;

  border-radius: 16px;

  cursor: pointer;
}

.card-vacancy__container {
  width: 100%;
  min-height: 50px;

  padding-right: 40px;

  margin-bottom: 12px;

  display: flex;
  align-items: center;
}
.card-vacancy__preview {
  width: 50px;
  min-width: 50px;
  height: 50px;

  display: flex;
  align-items: center;
  justify-content: center;

  margin-right: 12px;

  background: linear-gradient(var(--angle),var(--color-1),var(--color-2));;

  border-radius: 12px;

  font-size: 16px;
  font-weight: 600;
  color: black;
}
.card-vacancy__details {
  height: 100%;

  display: flex;
  flex-direction: column;
  justify-content: center;
}

.card-vacancy__title {
  font-size: 18px;
  font-weight: 600;
}

.card-vacancy__text {
  color: rgba(0, 0, 0, .6);

  font-size: 16px;
  font-weight: 400;
}

/*.card-vacancy__format {
  padding: 2px 8px;

  background: #d1d3d6;
  color: #000000;

  border-radius: 50px;

  font-size: 14px;
}
/*.card-vacancy__format-fulltime {
  background: #ead4ff;
  color: #6c21b3;
}
.card-vacancy__format-parttime {
  background: #f4dac0;
  color: #6c3700;
}
.card-vacancy__format-probation {
  background: #d4dafa;
  color: #2338b1;
}*/

.card-vacancy__salary {
  margin: 8px 0;

  font-size: 18px;
  font-weight: 500;

  color: rgba(0, 0, 0, .75);
}
.card-vacancy__experience {

}

.card-vacancy__content {

}

.card-vacancy__button {
  width: max-content;

  padding: 12px 20px;

  background: var(--color-primary);

  border-radius: 12px;

  color: var(--color-primary-text);

  transition: var(--transition);
  transition-property: background;
}
.card-vacancy__button:hover {
  background: var(--color-primary-hover);
}

.card-vacancy__button-favourite {
  width: 40px;
  height: 40px;

  display: flex;
  align-items: center;
  justify-content: center;

  position: absolute;
  top: 12px;
  right: 12px;

  background: white;

  color: black;

  border-radius: 50%;

  transition: var(--transition);
  transition-property: background;
}
.card-vacancy__button-favourite:hover {
  background: #d9dbe1;
}

.content :deep(*) {
  font-weight: inherit;
  color: inherit;
}
.content :deep(strong) {
  font-weight: inherit !important;
}
</style>
