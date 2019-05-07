<template>
  <span>
    <VueButton class="warning" @click="shareIt()" icon-left="share">Get link to share</VueButton>
    <VueModal v-if="shareItBoxShow" title= "Share your code" locked class="small">
      <VueInput
        :loading-left="loadingShareIt"
        v-model="shareLink"
        style="width:400px;"
        placeholder="Waiting for build sharelink..."
      />
      <div slot="footer" class="actions">
        <VueButton
          class="success"
          :loading-secondary="loadingShareIt"
          v-copy="shareLink"
          v-copy:callback="updateStatusCopy"
        >{{this.statusCopy}}</VueButton>
        <VueButton class="danger" @click="shareItBoxShow=false">Close</VueButton>
      </div>
    </VueModal>
  </span>
</template>

<script>
import axios from "axios";

export default {
    data(){
        return{
            loadingShareIt:false,
            shareItBoxShow:false,
            shareLink:"",
            statusCopy:""
        }
    },
    props:{
      source:String  
    },
    methods:{
        shareIt: async function() {
      this.shareItBoxShow= true;
      this.shareLink = "";
      this.statusCopy = "Copy to Clipboard";
      this.loadingShareIt = true;
      let result = await axios.post(
        "https://tronscs.herokuapp.com/shareit",
        { source: this.source }
      );
      if (result.data.result == "SUCCESS") {
        this.shareLink = `https://tronsmartcontract.space/#/compose/${
          result.data.fileName
        }`;
      } else {
        this.shareLink = "Failed to share";
      }
      this.loadingShareIt = false;
    },
    updateStatusCopy: function() {
      this.statusCopy = "Copied!!!";
    }
    }
};
</script>

<style>
</style>
