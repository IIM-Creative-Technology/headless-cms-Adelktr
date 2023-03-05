<template>
  <div>
    <div class="wrapper-grid-hero">
      <div class="item-grid item-grid-left">
        <p class="hero-title">
          Hey ! Moi c'est Adel, développeur web fullstack junior.
        </p>
        <p class="hero-content">
          Actuellement étudiant à l'<strong>IIM</strong>, et alternant chez
          <strong>MyC</strong>, une startup du domaine de la medtech.
        </p>
        <div class="buttons-container">
          <a href="mailto:khiter.adel@gmail.com" class="button-black"
            >Contactez-moi</a
          >
          <a href="https://www.linkedin.com/in/adelkhiter/" target="_blank"
            >LinkedIn</a
          >
          <a href="https://github.com/Adelktr" target="_blank">GitHub</a>
        </div>
      </div>
      <div class="item-grid item-grid-right">
        <img src="../assets/memoji2.png" alt="" />
      </div>
    </div>
    <button @click="filterProjects('all')" class="button-black">reset</button>
    <button
      v-for="(type, index) in types"
      :key="index"
      @click="filterProjects(type)"
      class="button-black"
    >
      {{ type }}
    </button>
    <div class="wrapper-grid-section" v-if="projects">
      <nuxt-link
        :to="`/projects/${project.slug}`"
        v-for="project in filteredProjects"
        :key="project.slug"
        class="item-grid"
        :style="`background-image:url(${project.mockup.formats.large.url})`"
      >
        <div class="item-header">
          <div class="item-header-left">
            <p class="item-title">{{ project.title }}</p>
          </div>
          <div class="item-header-arrow">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
              <path d="M5,17.59L15.59,7H9V5H19V15H17V8.41L6.41,19L5,17.59Z" />
            </svg>
          </div>
        </div>
      </nuxt-link>
    </div>
  </div>
</template>

<script setup>
const { find } = useStrapi();

const projects = ref();
const types = ref([]);
const activeFilter = ref("all");

const filterProjects = (type) => {
  activeFilter.value = type;
};

const filteredProjects = computed(() => {
  if (activeFilter.value === "all") return projects.value.data;
  return projects.value.data.filter(
    (project) => project.type === activeFilter.value
  );
});

onMounted(async () => {
  projects.value = await find("projects", { populate: "deep" });

  types.value = new Set(
    projects.value.data.map((project) => {
      return project.type;
    })
  );
});
</script>

<style lang="scss">
body {
  background-color: #e9e9e9;
}
.wrapper-grid-hero,
.wrapper-grid-section {
  display: grid;
  gap: 20px;
  padding-top: 20px;
  margin-bottom: 20px;
}
.wrapper-grid-hero {
  grid-template-columns: 4fr 2fr;
  align-items: start;
  p {
    font-family: "Inter", sans-serif;
    max-width: 80%;
  }
  .hero-title {
    font-weight: 600;
    font-size: 38px;
    width: 70%;
    margin-bottom: 20px;
  }
  .item-grid {
    align-items: start;
    padding: 50px;
  }
  .item-grid-left {
    justify-content: space-between;
    background: linear-gradient(35deg, #ddf8e8, #cdd5d1);
    .buttons-container {
      display: flex;
      align-items: center;
      width: 80%;
      margin-top: 20px;
      a {
        text-align: center;
      }
    }
  }
  .item-grid-right {
    background: linear-gradient(25deg, #d3e3da, #cdd5d1);

    display: flex;
    align-items: flex-end;
    justify-content: flex-end;
    img {
      width: 85%;
      height: auto;
      max-height: 300px;
      margin-bottom: 0;
    }
  }
}
a,
button {
  text-decoration: none;
  background-color: #fff;
  border-radius: 30px;
  border: none;
  color: black;
  padding: 10px 25px;
  font-family: "Inter", sans-serif;
  font-weight: 600;
  margin-right: 20px;
  display: inline-block;
  cursor: pointer;
  transition: 0.3s;
  &:hover {
    transform: scale(1.05);
  }
}
.button-black {
  background-color: #000;
  color: #fff;
}
.wrapper-grid-section {
  grid-template-columns: 1fr 1fr;

  .item-grid {
    cursor: pointer;
    height: 300px;

    &:hover {
      transform: scale(0.99);
    }
    .item-header {
      display: flex;
      align-items: flex-start;
      justify-content: space-between;
      width: 100%;
      margin-bottom: 30px;
      .item-header-arrow {
        background-color: #fff;

        border-radius: 50px;
        display: flex;
        align-items: center;
        justify-content: center;
        padding: 10px;
        svg {
          width: 20px;
        }
      }
    }
  }
}
.item-grid {
  display: flex;
  height: 400px;
  width: 100%;
  border-radius: 25px;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
  flex-direction: column;
  padding: 30px;
  color: black;
  transition: 0.2s;
  text-decoration: none;

  img {
    margin: 0 auto;
    width: 150px;
    height: 150px;
    margin-bottom: 20px;
  }
  .item-title {
    font-family: "Inter", sans-serif;
    font-weight: 600;
    font-size: 18px;
    margin-bottom: 10px;
  }
}
</style>
