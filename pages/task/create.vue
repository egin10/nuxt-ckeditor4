<template>
  <div class="container">
    <div class="row my-3">
      <div v-if="preview" class="col-md-12 mt-4">
        <h4>{{preview.title}}</h4>
        <div v-html="preview.content"></div>
      </div>

      <div class="col-md-12">
        <h4>Create New Task</h4>
        <hr />
        <Form :setPreview="showPreview" />
        <nuxt-link to="/task" class="btn btn-outline-danger"
          ><i class="fa fa-chevron-circle-left mr-2" aria-hidden="true"></i>
          Back</nuxt-link
        >
      </div>
    </div>
  </div>
</template>

<script>
import Form from "@/components/Form";

export default {
  components: {
    Form,
  },
  data() {
    return {
      preview: {}
    }
  },
  methods: {
    showPreview(data) {
      this.preview = data
    }
  }
};
</script>

<style>
</style>