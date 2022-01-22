<template>
  <nav>
    <div class="nav nav-tabs mb-2" id="nav-tab" role="tablist">
      <button
        class="nav-link active"
        id="nav-home-tab"
        data-bs-toggle="tab"
        data-bs-target="#nav-text"
        type="button"
        role="tab"
        aria-controls="nav-text"
        aria-selected="true"
      >
        Text
      </button>
      <button
        class="nav-link"
        id="nav-profile-tab"
        data-bs-toggle="tab"
        data-bs-target="#nav-video"
        type="button"
        role="tab"
        aria-controls="nav-video"
        aria-selected="false"
      >
        Video
      </button>
      <button
        class="nav-link"
        id="nav-contact-tab"
        data-bs-toggle="tab"
        data-bs-target="#nav-website"
        type="button"
        role="tab"
        aria-controls="nav-website"
        aria-selected="false"
      >
        Website
      </button>
    </div>
  </nav>
  <div class="tab-content" id="nav-tabContent">
    <TextForm class="tab-pane fade show active" id="nav-text" role="tabpanel" />
    <VideoForm class="tab-pane fade show" id="nav-video" role="tabpanel" />
    <WebsiteForm class="tab-pane fade show" id="nav-website" role="tabpanel" />
  </div>

  <br />
  <Table />
</template>

<script>
import Table from "./Table.vue";
import TextForm from "./TextForm.vue";
import VideoForm from "./VideoForm.vue";
import WebsiteForm from "./WebsiteForm.vue";

export default {
  components: {
    Table,
    TextForm,
    VideoForm,
    WebsiteForm,
  },
};
</script>
