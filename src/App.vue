<template>
  <div ref="content" id="content" class="content">
    <h1>Get PDF from HTML</h1>
    <!--
    <div>
      <QuillEditor id="quillEditor" :modules="modules" theme="snow" toolbar="full"/>
      <br>
      <button @click="generatePDFQuill">Generate PDF (jsPDF)</button>
      <button @click="generatePDF2Quill">Generate PDF (html2pdf)</button>
    </div>
    -->
    <br>

    <div>
      <editor
          id="quillEditor"
          class="editor"
          api-key="9fs8keayd8ljaxi25asmvmftf2us2757ikojlpco2qi5xcwj"
          :init="{
          menubar: true,
          plugins: 'export preview importcss searchreplace autolink directionality code visualblocks visualchars fullscreen image link media template codesample table charmap pagebreak nonbreaking anchor insertdatetime advlist lists wordcount help charmap quickbars emoticons',
          menubar: 'file edit view insert format tools table help',
          toolbar: 'export undo redo | fontfamily fontsize blocks bold italic underline strikethrough | alignleft aligncenter alignright alignjustify | outdent indent |  numlist bullist | forecolor backcolor removeformat | pagebreak | charmap emoticons | fullscreen  preview save print | insertfile image media template link anchor codesample | ltr rtl',
          toolbar_sticky: true,
          toolbar_sticky_offset: isSmallScreen ? 102 : 108
        }"
      />
      <br>
      <button @click="generatePDF">Generate PDF (jsPDF)</button>
      <button @click="generatePDF2">Generate PDF (html2pdf)</button>
    </div>
  </div>
</template>


<script lang="ts">
import { defineComponent } from 'vue'
import jsPDF from "jspdf"
import {QuillEditor} from '@vueup/vue-quill'
import BlotFormatter from 'quill-blot-formatter'
import HtmlEditButton from 'quill-html-edit-button'
import '@vueup/vue-quill/dist/vue-quill.snow.css'
import Editor from '@tinymce/tinymce-vue'
import html2pdf from "html2pdf.js";

export default defineComponent({
  name: 'App',
  components: {
    QuillEditor,
    'editor': Editor
  },

  setup() {
    const generatePDFQuill = () => {
      const doc = new jsPDF({
        unit: "pt",
        precision: 2,
        format: 'a4',
        orientation: 'p',
      })
      const content = document.querySelector('.ql-editor')
      // @ts-ignore
      //const content = document.querySelector('#quillEditor_ifr').contentWindow.document.querySelector('#tinymce')
      // @ts-ignore
      const contentHtml = content
      console.log(contentHtml)
      // @ts-ignore
      doc.html(contentHtml, {
        html2canvas: {
          scale: 0.7
        },
        callback: function (doc) {
          //doc.save();
          doc.output('dataurlnewwindow')
        },
        margin: [15, 15, 15, 15],
      });
    }

    const generatePDF2Quill = () => {
      // @ts-ignore
      //const content = document.querySelector('#quillEditor_ifr').contentWindow.document.querySelector('#tinymce')
      const content = document.querySelector('.ql-editor')
      console.log(content)
      html2pdf(content)
    }

    const generatePDF = () => {
      const doc = new jsPDF({
        unit: "pt",
        precision: 2,
        format: 'a4',
        orientation: 'p',
      })
      //const content = document.querySelector('.ql-editor')
      // @ts-ignore
      const content = document.querySelector('#quillEditor_ifr').contentWindow.document.querySelector('#tinymce')
      // @ts-ignore
      const contentHtml = content
      console.log(contentHtml)
      // @ts-ignore
      doc.html(contentHtml, {
        html2canvas: {
          scale: 0.7
        },
        callback: function (doc) {
          //doc.save();
          doc.output('dataurlnewwindow')
        },
        margin: [15, 15, 15, 15],
      });
    }

    const generatePDF2 = () => {
      // @ts-ignore
      const content = document.querySelector('#quillEditor_ifr').contentWindow.document.querySelector('#tinymce')
      console.log(content)
      html2pdf(content)
    }

    const modules = [
      {
        name: 'blotFormatter',
        module: BlotFormatter,
        options: {/* options */}
      },
      {
        name: 'htmlEditButton',
        module: HtmlEditButton,
        options: {/* options */}
      }
    ]


    return {
      generatePDF,
      generatePDF2,
      generatePDFQuill,
      generatePDF2Quill,
      modules
    }
  }
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.content {
  width: auto;
  min-height: 50px;
  text-align: center;
}

.content img {
  width: 150px;
}

.ql-toolbar {
  /*width: 600px;*/
}

.content .editor {
  min-height: 400px;
}

</style>
