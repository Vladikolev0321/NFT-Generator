<template>
  <div>
    <h1>NFT Generator</h1>
    <form @submit.prevent="submitForm">
        <div class="inputs">
        <input
            placeholder="NFT Name"
            v-model="name.value"
        />
        <textarea 
            placeholder="NFT Description"
            v-model="description.value">
        </textarea>
        <input
            placeholder="NFT Price"
            v-model="price.value"
        />
        <input
            class="upload"
            type="file"
            placeholder="NFT Image"
            @change="uploadImage"
        />
        <button>
            Generate NFT
        </button>
        </div>
    </form>
  </div>
</template>

<script>
export default {
  props: {
    msg: String,
  },
  methods: {
    submitForm() {
      if (!this.formIsValid) {
        return;
      }
      let name = this.name.value;
      let description = this.description.value;
      let price = this.price.value;
      

      console.log('submit');
      console.log(name);
      console.log(description);
      console.log(price);
    },
    uploadImage(e) {
      console.log('upload');
      console.log(e.target.files[0]);
    },
  },
  data() {
    return {
        name:{
            value: '',
            isValid: false,
            message: ''
        },
        description:{
            value: '',
            isValid: false,
            message: ''
        }, 
        price:{
            value: '',
            isValid: false,
            message: ''
        },
        image:{
            value: '',
            isValid: false,
            message: ''
        },
        formIsValid: true,
    }
  },
}
</script>

<style scoped>
.inputs {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin: 10px; 
}

textarea  {
  margin: 10px;
}

input {
  margin: 10px;
}
.upload {
  margin: 10px;
  width: 40%;
  position: relative;
}

h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
