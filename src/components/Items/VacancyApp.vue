<template>
  <div
    class="!z-5 relative flex flex-col rounded-[20px] max-w-[300px] bg-white bg-clip-border shadow-3xl shadow-shadow-500 flex flex-col w-full !p-4 3xl:p-![18px] bg-white undefined"
  >
    <div
      class="h-full w-full"
      style="
        display: flex;
        flex-direction: column;
        justify-content: space-between;
      "
    >
      <div class="relative w-full">
        <div class="relative">
          <div
            class="output-color flex justify-center items-center"
            width="100%"
            height="100"
          >
            <p
              class="text-lg font-bold text-navy-700 text-center px-2 rounded-sm mx-2 backdrop-blur-md bg-white/80"
              style="color: #5aa9e6"
            >
              {{ item.vacancy.title }}
            </p>
          </div>
          <button
            class="absolute top-3 right-3 flex items-center justify-center rounded-full bg-white p-2 text-brand-500 hover:cursor-pointer"
          >
            <div
              class="flex h-full w-full items-center justify-center rounded-full text-xl hover:bg-gray-50"
            >
              <svg
                stroke="currentColor"
                fill="currentColor"
                stroke-width="0"
                viewBox="0 0 512 512"
                height="1em"
                width="1em"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path
                  fill="none"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="32"
                  d="M352.92 80C288 80 256 144 256 144s-32-64-96.92-64c-52.76 0-94.54 44.14-95.08 96.81-1.1 109.33 86.73 187.08 183 252.42a16 16 0 0018 0c96.26-65.34 184.09-143.09 183-252.42-.54-52.67-42.32-96.81-95.08-96.81z"
                ></path>
              </svg>
            </div>
          </button>
        </div>
      </div>
      <div class="mb-3 flex items-center justify-between px-1 md:items-start">
        <div class="mb-2">
          <div>
            <p class="text-sm font-medium text-gray-600">
              работодатель {{ item.employer.title }}
            </p>
          </div>

          <div>
            <p class="text-lg font-bold">Описание вакансии:</p>
            <div v-html="item.vacancy.raw_text.substr(0, 300) + '...'"></div>
          </div>
        </div>
      </div>
      <div>
        <p>
          <b> Зарплата: </b>
          <span v-html="item.vacancy.salary_raw"> </span>
        </p>
        <p>
          <b> Требуемый опыт: </b>
          <span v-html="item.vacancy.experience_raw"> </span>
        </p>
      </div>
      <div
        class="flex items-center justify-between md:items-center lg:justify-between"
      >
        <button
          @click="show"
          href=""
          class="linear rounded-md bg-blue-600 px-4 py-2 text-base font-medium text-white transition duration-200 hover:bg-brand-800 active:bg-brand-700"
        >
          Подробнее
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["item"],
  data() {
    return {};
  },
  mounted() {
    const outputColor = document.querySelectorAll(".output-color");

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
      var a,
        rgb = orig
          .replace(/\s/g, "")
          .match(/^rgba?\((\d+),(\d+),(\d+),?([^,\s)]+)?/i),
        alpha = ((rgb && rgb[4]) || "").trim(),
        hex = rgb
          ? (rgb[1] | (1 << 8)).toString(16).slice(1) +
            (rgb[2] | (1 << 8)).toString(16).slice(1) +
            (rgb[3] | (1 << 8)).toString(16).slice(1)
          : orig;

      if (alpha !== "") {
        a = alpha;
      } else {
        const LITERAL = "01";
        a = LITERAL;
      }

      a = ((a * 255) | (1 << 8)).toString(16).slice(1);
      hex = hex + a;

      return hex;
    },
    randomHex() {
      // Генерируем три случайных числа от 0 до 255
      var r = Math.floor(Math.random() * 256);
      var g = Math.floor(Math.random() * 256);
      var b = Math.floor(Math.random() * 256);
      var a = "0.3";

      let rgbaColor = `rgba(${r}, ${g}, ${b}, ${a})`;

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
};
</script>

<style>
p {
  padding: 10px 0;
}

ul {
  padding-left: 20px;
}

strong {
  font-weight: normal !important;
}
</style>