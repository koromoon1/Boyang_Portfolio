<template>
  <div id="project-showcase">
    <!-- Project Showcase -->
    <div class="project-detail">
      <!-- Hero -->
      <div
        class="project-detail-hero d-flex flex-column align-items-center py-5 px-5 my-5"
      >
        <!-- Heading -->
        <div class="project-detail-title-container mb-5 text-center">
          <h2 class="main-title">{{ projectName }}</h2>
        </div>
        <div class="project-detail-title-sub mb-5">
          <p class="main-title-sub">
            {{ description }}
          </p>
        </div>
        <!-- Live Link -->
        <div class="project-live-link" v-if="!!liveLink">
          <a :href="liveLink" class="myBtn" target="_blank" disabled
            >Live Link</a
          >
        </div>
        <!-- Code Link -->
        <div class="link-container d-inline" v-else>
          <a :href="codeLink" class="myBtn myBtn-dark" target="_blank"
            >Code Link</a
          >
        </div>
      </div>

      <!-- Project Detail Content -->
      <div class="project-detail-content container">
        <!-- Showcase Image -->
        <div class="project-detail-showcase-container mx-3 mx-gl-5 px-lg-5">
          <div class="project-detail-showcase-img">
            <img :src="imageUrl" alt="Showcase image of the project" />
          </div>
        </div>
        <!-- Main Content -->
        <div class="project-detail-content-main m-5 pt-lg-5">
          <!-- overview -->
          <div class="project-detail-content-overview mb-5">
            <div class="overview-title-container mb-5">
              <h3 class="section-title-sm text-center text-lg-start">
                Project Overview
              </h3>
            </div>
            <div class="overview-text">
              <p class="section-subtitle-sm">
                Lorem ipsum dolor, sit amet consectetur adipisicing elit.
                Provident rerum est culpa quasi neque fugiat. Maxime id deserunt
                cupiditate. Eum sed, quia praesentium saepe soluta ullam
                repellendus iure nostrum quas temporibus adipisci voluptates
                quos distinctio nisi a eligendi expedita eaque?
              </p>
              <p class="section-subtitle-sm">
                Lorem ipsum dolor sit amet consectetur adipisicing elit. Eum
                consequuntur harum veniam voluptatibus cupiditate incidunt, illo
                dolore, ea assumenda optio aspernatur accusamus. Odio optio
                incidunt sint eum neque tenetur dolorum? Perspiciatis molestiae
                ipsum accusantium et deserunt culpa quae. Eveniet facilis amet
                veniam quidem ratione! Molestias doloribus libero esse illum
                asperiores consequatur autem perspiciatis, obcaecati cupiditate
                quidem, nulla nihil. Distinctio, tempora!
              </p>
            </div>
          </div>
          <!-- Tools Used -->
          <div class="project-detail-content-tools">
            <div class="tools-title mb-5">
              <h3 class="section-title-sm text-center text-lg-start">
                Tools Used
              </h3>
            </div>
            <ToolsBadgeWall :techStack="techStack" />
          </div>
          <!-- See Live -->
          <div
            class="project-detail-links d-flex flex-column flex-md-row justify-content-md-center align-items-center mb-5 pb-5"
          >
            <!-- Live Link -->
            <div class="link-container d-md-inline me-0 me-md-5 mb-5 mb-md-0">
              <a
                :href="liveLink"
                class="myBtn"
                :class="{ disabled: !liveLink }"
                target="_blank"
                >Live Link</a
              >
            </div>
            <!-- Code Link -->
            <div class="link-container d-md-inline">
              <a :href="codeLink" class="myBtn myBtn-dark" target="_blank"
                >Code Link</a
              >
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ToolsBadgeWall from "../components/Projects/ToolsBadgeWall.vue";

export default {
  name: "ProjectShowcase",
  inject: ["projects", "technologies"],
  props: ["projectId"],
  components: { ToolsBadgeWall },
  data() {
    return {
      selectedProject: null,
      projectName: "",
      description: "",
      liveLink: "",
      codeLink: "",
      techStack: [],
    };
  },
  created() {
    // Get project id by router params
    const projectId = this.$route.params.projectId;
    // Select project by id
    const selectedProject = this.projects.find(
      (project) => project.id === projectId
    );
    this.selectedProject = selectedProject;
    // Get tech stack from selected project
    const techStack = selectedProject.techStack;
    // empty array for collecting technologies in tech stack
    const selectedTechStack = [];
    // loop out all technologies that id match the tech stack item
    for (const tech of techStack) {
      const selectedTechnology = this.technologies.find(
        (technology) => technology.name === tech
      );
      // store technology in the array
      selectedTechStack.push(selectedTechnology);
    }
    this.techStack = selectedTechStack;
    this.projectName = selectedProject.name;
    this.description = selectedProject.description;
    this.liveLink = selectedProject.liveLink;
    this.codeLink = selectedProject.codeLink;
  },
  computed: {
    imageUrl() {
      return require("../assets/projects/" +
        this.selectedProject.imgName +
        "." +
        this.selectedProject.imgType);
    },
  },
  methods: {
    toProjects() {
      this.$router.push("/projects");
    },
  },
};
</script>

<style scoped>
/* ==== Heading ==== */

.project-detail-title-sub {
  text-align: center;
  font-size: 1.3rem;
  color: #666;
  line-height: 1.7;
  max-width: 700px;
}

/* ==== Content Section ==== */

/* Image */
/* .project-detail-showcase-container {
  margin: 0 8rem;
} */

img {
  width: 100%;
}

/* Tools Used */
.project-detail-content-tools {
  margin-bottom: 4.5rem;
}
</style>
