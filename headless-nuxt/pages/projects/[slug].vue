<template>
  <nuxt-link to="/" class="button-black button-back">Back</nuxt-link>
  <div v-if="project" class="project">
    <div
      class="project-banner"
      :style="`background-image:url(${project.mockup.formats.large.url})`"
    ></div>
    <div class="project-content">
      <div class="project-content-header">
        <p class="project-content-title">{{ project.title }}</p>
        <div v-if="project.technologies" class="technologies">
          <p
            v-for="(techno, index) in project.technologies"
            :key="index"
            :style="`background: ${techno.color}`"
          >
            {{ techno.name }}
          </p>
        </div>
      </div>

      <p
        class="project-content-description"
        v-if="project.content"
        v-html="$mdRenderer.render(project.content)"
      ></p>
      <a v-if="project.link" :href="project.link" target="_blank"
        >Lien vers le site</a
      >
    </div>
    <div class="pagination-buttons">
      <nuxt-link
        v-if="position !== 0"
        ref="next"
        @click="navProjects"
        :to="`/projects/${prevProject?.slug}`"
        class="button-black"
        >Prev.</nuxt-link
      >
      <nuxt-link
        v-if="position !== lengthProjects - 1"
        ref="prev"
        @click="navProjects"
        :to="`/projects/${nextProject?.slug}`"
        class="button-black"
        >Next</nuxt-link
      >
    </div>
  </div>
</template>

<script setup>
const { findOne, find } = useStrapi();
const route = useRoute();
const project = ref();
const projects = ref();
const position = ref();
const lengthProjects = ref();
const nextProject = ref();
const prevProject = ref();

onMounted(async () => {
  projects.value = await find("projects", { populate: "deep" });

  project.value = await findOne(
    `projects?filters[slug]=${route.params.slug}&populate=deep`
  );
  project.value = project.value.data[0];
});

const navProjects = computed(() => {
  getPosition();
  getLengthProjects();
  nextProject.value = projects.value.data[position.value + 1];
  prevProject.value = projects.value.data[position.value - 1];
});

const getPosition = () =>
  (position.value = projects?.value.data.findIndex(
    (p) => p.id === project.value.id
  ));
const getLengthProjects = () =>
  (lengthProjects.value = projects?.value.data.length);
</script>

<style lang="scss" scoped>
.pagination-buttons {
  margin-top: 20px;
  width: 100%;
  display: flex;
  justify-content: center;
}
.button-back {
  margin-top: 20px;
}
.project {
  border-radius: 20px;

  &-banner {
    border-top-left-radius: 20px;
    border-top-right-radius: 20px;
    width: 100%;
    height: 30vh;
    background-position: center;
    background-repeat: no-repeat;
    background-size: cover;
    margin-top: 20px;
  }
  &-content {
    font-family: "Inter", sans-serif;
    padding: 0 20px;
    &-title {
      font-weight: 600;
      font-size: 38px;
      margin-top: 20px;
    }
    &-description {
      margin: 20px 0;
    }
    .technologies {
      display: flex;
      gap: 10px;
      margin-top: 10px;
      p {
        padding: 4px 12px;
        border-radius: 20px;
      }
    }
  }
}
</style>
