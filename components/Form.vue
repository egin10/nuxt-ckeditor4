<template>
  <form>
    <div class="form-group">
      <label for="exampleFormControlInput1">Title of Task</label>
      <input
        type="text"
        class="form-control"
        id="exampleFormControlInput1"
        placeholder="title..."
        v-model="form.title"
      />
    </div>
    <div class="form-group">
      <label for="exampleFormControlTextarea1" aria-placeholder="Task Description">Description</label>
      <ckeditor-nuxt v-model="form.content" :config="editorConfig"/>
    </div>
    <button @click.prevent="saveContent" class="btn btn-success float-right">Save</button>
  </form>
</template>

<script>
import CkeditorNuxt from '@/components/CKeditorNuxt';

export default {
  components: {CkeditorNuxt},
  data() {
    return {
      form: {
        title: '',
        content: ''
      },
      editorConfig: {
        height: 350,
        filebrowserImageBrowseUrl: 'http://localhost:8000/api/filemanager?type=Images',
        filebrowserImageUploadUrl: 'http://localhost:8000/api/filemanager/upload?type=Images&_token=',
        filebrowserBrowseUrl: 'http://localhost:8000/api/filemanager?type=Files',
        filebrowserUploadUrl: 'http://localhost:8000/api/filemanager/upload?type=Files&_token=',
        filebrowserUploadMethod: 'form'
      }
    }
  },
  props: {
    setPreview: { type: Function }
  },
  methods: {
    saveContent () {
      // console.log(this.form)
      this.setPreview(this.form)
    }
  },
};
</script>

<style>
</style>