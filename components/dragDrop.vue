
<template>
  <div
    class="relative flex items-top justify-center min-h-screen bg-gray-100 sm:items-center sm:pt-0"
  >
    <link
      href="https://fonts.googleapis.com/css?family=Material+Icons|Material+Icons+Outlined"
      rel="stylesheet"
    />
    <form class="form-container" enctype="multipart/form-data">
      <div class="upload-files-container">
        <div
          ref="dragFileArea"
          class="drag-file-area"
          @drop="handleFileDrop"
          v-if="!this.fileName"
        >
          <span class="material-icons-outlined upload-icon"> file_upload </span>
          <h3 class="dynamic-message">Drag & drop any file here</h3>
          <label style="margin-left: -55px" class="label">
            or
            <span class="browse-files">
              <input
                type="file"
                class="default-file-input"
                @change="handleFileUpload($event)"
              />
              <span class="browse-files-text">browse file</span>
              <span>from device</span>
            </span>
          </label>
        </div>
        <div class="" v-else>
          <div class="drag-file-area">
            <span class="material-icons-outlined upload-icon"
              >check_circle</span
            >
            <h3 class="dynamic-message">File Dropped Successfully!</h3>
          </div>
        </div>

        <span class="cannot-upload-message" v-if="!fileSelected">
          <span class="material-icons-outlined">error</span> Please select a
          file first
          <span
            @click="fileSelected = true"
            class="material-icons-outlined cancel-alert-button"
            >cancel</span
          >
        </span>

        <button type="button" class="file-detail" v-if="showFileDetail">
          {{ fileName }} | {{ fileSize }}
        </button>
        <div class="progress-container">
          <div
            class="progress-bar"
            id="myBar"
            :style="{ width: progressWidth + '%' }"
          ></div>
        </div>
        <button
          type="button"
          class="upload-button"
          @click="uploadFile"
          v-if="!isUploaded"
        >
          Upload
        </button>
        <button type="button" class="uploaded-button" v-if="isUploaded">
          <span class="material-icons-outlined upload-button-icon">
            check_circle
          </span>
          Uploaded
        </button>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isUploaded: false,
      fileName: "",
      fileSize: "",
      showFileDetail: false,
      file: "",
      progressWidth: 0,
      progressBarID: null,
      fileSelected: true,
    };
  },
  mounted() {
    [
      "drag",
      "dragstart",
      "dragend",
      "dragover",
      "dragenter",
      "dragleave",
      "drop",
    ].forEach((evt) =>
      this.$refs.dragFileArea.addEventListener(evt, (e) => {
        e.preventDefault();
        e.stopPropagation();
      })
    );
  },

  methods: {
    handleFileDrop(e) {
      let files = e.dataTransfer.files;
      this.fileName = files[0].name;
      this.fileSize = (files[0].size / 1024).toFixed(1) + " KB";
      this.showFileDetail = true;
    },

    uploadFile() {
      if (this.fileName && this.fileSize) {
        this.fileSelected = true;
        this.progressBarID = setInterval(this.frame, 10);
      } else {
        this.fileSelected = false;
      }
    },
    handleFileUpload(event) {
      this.file = event.target.files[0];
      this.fileName = this.file.name;
      this.fileSize = (this.file.size / 1024).toFixed(1) + " KB";
      this.showFileDetail = true;
    },
    frame() {
      if (this.progressWidth >= 100) {
        this.isUploaded = true;
        clearInterval(this.progressBarID);
      } else {
        this.progressWidth++;
      }
    },
  },
};
</script>
