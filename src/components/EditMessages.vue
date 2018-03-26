<template>
    <div>
        <p v-if="!message.id">Send New Message</p>
        <p v-else>Updating of existing Message</p>
        <p><router-link to="/messages">back to full list</router-link></p>
        <form v-on:submit.prevent="submitForm">
            <input type="hidden" name="id" v-model="message.id">
            <input type="text" name="author" placeholder="Name" v-model="message.author">
            <br><textarea name="text"  placeholder="Enter the text of your message" v-model="message.text"></textarea>
            <br><input type="submit" name="" v-bind:value="message.buttonText">
        </form>
    </div>
</template>

<script>



export default {
  name: 'editMessages',
  data () {
    return {
      message: {
        id: 0,
        author : '',
        text: '',
        buttonText: "Send Message"
      },
    }
  },
  // mixins: [spaMixin],
  created: function(){
    this.init();
  },  
  methods: {
    init: function(){
      // this.hello();
      if(!!this.$route.params.id){
        this.getMessage(this.$route.params.id);
      }else{

      }
    },

    getMessage: function(messageId){
      this.$http.get("comments/" + messageId).then((response) => { 
        if(!!response.body) {
          this.message = response.body;
          this.message.buttonText = "Edit Message";

        }
      }, (response) => {
        this.error = response;
      });                    
    },
   

    submitForm: function() {
      if(!this.message.id){
        this.sendNewMessage();
      }else{
        this.updateMessage(this.message.id);
      }

    },
    listReload: function(){
      this.$router.push('/messages');
    },
    sendNewMessage: function() {
      this.$http.post('comments', this.message).then((response) => { 
        this.listReload();
      }, (response) => {
        this.error = response;
      });                    
    },                

    updateMessage: function(messageId){
      this.$http.put('comments' + "/"+messageId, { "author" : this.message.author, "text": this.message.text}).then(
        (response) => { 
          this.listReload();
        }, (response) => {
          this.error = response;
        }
      ); 
    }
  }
}
</script>
