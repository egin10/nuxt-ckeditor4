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
        file_picker_callback: this.elFinderBrowser
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
      this.setPreview(this.form)
    },

    elFinderBrowser(callback, value, meta) {
      tinymce.activeEditor.windowManager.openUrl({
        title: 'File Manager',
        url: 'http://localhost:8000/elfinder/tinymce5',
        /**
         * On message will be triggered by the child window
         * 
         * @param dialogApi
         * @param details
         * @see https://www.tiny.cloud/docs/ui-components/urldialog/#configurationoptions
         */
        onMessage: function (dialogApi, details) {
            if (details.mceAction === 'fileSelected') {
                const file = details.data.file;
                
                // Make file info
                const info = file.name;
                
                // Provide file and text for the link dialog
                if (meta.filetype === 'file') {
                    callback(file.url, {text: info, title: info});
                }
                
                // Provide image and alt text for the image dialog
                if (meta.filetype === 'image') {
                    callback(file.url, {alt: info});
                }
                
                // Provide alternative source and posted for the media dialog
                if (meta.filetype === 'media') {
                    callback(file.url);
                }
                
                dialogApi.close();
            }
        }
    });
    }
  }
};
</script>

<style></style>
