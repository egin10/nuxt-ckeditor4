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
      <label
        for="exampleFormControlTextarea1"
        aria-placeholder="Task Description"
        >Description</label
      >
      <!-- <ckeditor-nuxt v-model="form.content" :config="editorConfig"/> -->
      <editor
        api-key="vmfa2fukeganslgf8qli3u0m8ehhcxs1k2qsp2qi3c6x6myf"
        :init="tinyConfig"
        v-model="form.content"
      />
    </div>
    <button @click.prevent="saveContent" class="btn btn-success float-right">
      Save
    </button>
  </form>
</template>

<script>
import Editor from "@tinymce/tinymce-vue";

export default {
  components: {
    editor: Editor
  },
  data() {
    return {
      form: {
        title: "",
        content: ""
      },
      tinyConfig: {
        height: 500,
        menubar: true,
        relative_urls: false,
        plugins: [
          'advlist autolink lists link image charmap print preview hr anchor pagebreak',
          'searchreplace wordcount visualblocks visualchars code fullscreen',
          'insertdatetime media nonbreaking save table directionality',
          'emoticons template paste textpattern'
        ],
        toolbar:
          'insertfile undo redo | styleselect | bold italic | alignleft aligncenter alignright alignjustify | bullist numlist outdent indent | link image media',
        paste_data_images: true,
        file_picker_types: 'file image media',
        file_picker_callback: this.filePicker
      }
    };
  },
  props: {
    setPreview: { type: Function }
  },
  mounted(){
    // console.log(window.tinymce);
  },
  methods: {
    saveContent() {
      console.log(this.form);
      // this.setPreview(this.form)
    },

    filePicker(callback, value, meta) {
      let x = window.innerWidth || document.documentElement.clientWidth || document.getElementsByTagName("body")[0].clientWidth;
      let y = window.innerHeight || document.documentElement.clientHeight || document.getElementsByTagName("body")[0].clientHeight;

      let cmsURL = "http://localhost:8000/api/filemanager?editor=" + meta.fieldname;
      if (meta.filetype == "image") {
        cmsURL = cmsURL + "&type=Images";
      } else {
        cmsURL = cmsURL + "&type=Files";
      }

      tinyMCE.activeEditor.windowManager.openUrl({
        url: cmsURL,
        title: "Filemanager",
        width: x * 0.8,
        height: y * 0.8,
        resizable: "yes",
        close_previous: "no",
        onMessage: (api, message) => {
          callback(message.content);
        }
      });
    }
  }
};
</script>

<style></style>
