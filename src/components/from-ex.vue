<template>
  <div class="container">
    <form id="formId" @submit.prevent="handleSubmit">
      <h1>{{title}}</h1>
      <input  id="fileuploader" type="file" name="fileuploader">
      <button>Upload</button>
    </form>
    <div>
      {{ status }}
    </div>
  </div>
</template>

<script>
export default {
  name: 'form-exchange',
  props: {
    msg: String
  },
  data(){
    return {
      title: 'File Uploader',
      status: '',
    }
  },
  methods: {
    handleSubmit() {
      this.status = '';
      this.sendFiles();

    },
    sendFiles() {
      let formData = new FormData();
      formData.append("file", fileuploader.files[0])

      fetch(process.env.VUE_APP_ROOT_API+'/upload/file2' , {

        method: 'post',
        headers: {
          // 'content-type': 'text/plain'
        },
        body: formData
      }).then(res => {
        // a non-200 response code
        if (!res.ok) {
          // create error instance with HTTP status text
          const error = new Error(res.statusText);
          error.json = res.json();
          throw error;
        }else {
          this.status = 'Upload Successfully!!'
        }
        console.log(res);
        // console.log(res.text());
        // return res.text();
      })/*.then(res => {
        this.ocr_text = res;
      })*/
    }
  }
  ,mounted() {
// empty
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.container{
  width: 400px;
}

form {
  display: flex;
  border: 1px solid #ddd;
  flex-direction: column;
  padding: 20px;

}

input {
  margin: 15px 0;
  padding: 10px;   /*padding iside the input*/
  /*width: 100px;*/

}

.small-b{
  margin: 15px 0;
  padding: 10px;   /*padding iside the input*/
}

.submit {
  padding: 10px;
  background-color: gray;
  color: white;
  border: 0;
}
</style>
