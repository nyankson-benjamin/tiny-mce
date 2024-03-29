<template>
  <div class="">
    <!-- <DatePicker /> -->
    <textarea ref="editor"></textarea>
    <input id="my-file" type="file" name="my-file" style="display: none;" onchange="" />

  </div>
</template>

<script setup lang="ts">
import { onBeforeUnmount, onMounted } from 'vue';
import DatePicker from "@/components/DatePicker.vue"

declare const tinymce: any;

onBeforeUnmount(() => {
  tinymce.get('textarea').destroy();
});

onMounted(() => {
  tinymce.init({
    selector: 'textarea',
    height: '100vh',
    width: '100%',
    toolbar_mode: 'sliding',
    plugins: 'save casechange autoresize tableofcontents insertdatetime accordion pagebreak preview fullscreen anchor autolink codesample emoticons image link lists media searchreplace table visualblocks wordcount checklist mediaembed casechange export formatpainter pageembed linkchecker a11ychecker tinymcespellchecker permanentpen powerpaste advtable advcode editimage advtemplate ai mentions tinycomments tableofcontents footnotes mergetags autocorrect typography inlinecss markdown',
    toolbar: 'save undo redo | blocks fontfamily fontsize| bold italic underline strikethrough backcolor casechange | link image media table mergetags | addcomment showcomments | spellcheckdialog a11ycheck typography | align lineheight | checklist numlist bullist indent outdent | emoticons charmap | saveButton removeformat',
    tinycomments_mode: 'embedded',
    file_picker_callback: function (callback:any, value, meta:any) {
      if (meta.filetype == 'image') {
        let input = document.getElementById('my-file') as HTMLElement;
        if (input) {
          input.click();
          input.onchange = function () {
            let file = input.files[0];
            let reader = new FileReader();
            reader.onload = function (e) {
              callback((e.target as FileReader).result, {
                alt: file.name
              });
            };
            reader.readAsDataURL(file);
          };
        }
      }
    },
    setup: function (editor:any) {
      editor.on('init', function (val: { type: string }) {
        if (val.type === "init") {
        }
      });
    },
    tinycomments_author: 'Author name',
    save_enablewhendirty: false,

    mergetags_list: [
      { value: 'First.Name', title: 'First Name' },
      { value: 'Email', title: 'Email' },
    ],
    ai_request: (request, respondWith:any) => {
      respondWith.string(() => Promise.reject(new Error('See docs to implement AI Assistant')))
    },
    content_css: '//www.tiny.cloud/css/codepen.min.css'
  });
});
</script>
