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
import { create as ipfsHttpClient } from 'ipfs-http-client'
import NFTAbi from '../../../contracts/build/contracts/NFT.json'
import Web3 from 'web3'

const client = ipfsHttpClient('https://ipfs.infura.io:5001/api/v0')

export default {
  props: {
    msg: String,
  },
  methods: {
    async submitForm() {
      if (!this.name.value || !this.description.value || !this.image.value) {
        return;
      }
      if (!this.formIsValid) {
        return;
      }
      let image = this.image.value;
      let name = this.name.value;
      let description = this.description.value;
      

      console.log('submit');
      console.log(name);
      console.log(description);
      console.log(image);

      try {
        const result = await client.add(JSON.stringify({image, name, description}));
        console.log(result);
        this.mint(result);
      } catch(e) {
        console.log("error uploading uri to ipfs", e);
      }
    },
    async mint(result) {
      const web3 = new Web3(window.ethereum);
      const nft = new web3.eth.Contract(
        NFTAbi.abi,
        '0xb904A7D08a6C3Ba86Fa4860e86F698aC6fdf3396'
      );
      const accounts = await web3.eth.getAccounts();
      
      const uri = `https://ipfs.infura.io/ipfs/${result.path}`

      // mint nft 
      await nft.methods.mint(uri).send({from: accounts[0]});
      // get tokenId of new nft 
      const id = await nft.methods.tokenCount().call();

      console.log("Token id: ", id);
      console.log("Token uri: ", await nft.methods.tokenURI(id).call());
    },
    async uploadImage(e) {
      console.log('upload');
      console.log(e.target.files[0]);
      const file = e.target.files[0];
      if (file) {
        try {
          const result = await client.add(file);
          console.log(result);
          this.image.value = `ipfs://${result.path}`;

        } catch(e) {
          console.log(e);
        }
      }
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
