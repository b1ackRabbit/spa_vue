<template>
	<div>
		<span v-if="!deleted">
			<router-link v-bind:to="'/edit-message/' + messageId">edit</router-link>
		  <a href="#" class="remove-message"  v-on:click.prevent="deleteMessage( messageId )">x</a>
		  <router-link v-if="!showOne" v-bind:to="'/messages/' + messageId">show detailes (load one message)</router-link>
		</span>
	  <router-link v-if="showOne" to="/messages">show all messages</router-link>
	 </div>
   
</template>

<script>
import mixins from './../mixins';
import { EventBus } from './../event-bus.js';
export default {
	props: [
    'messageId' , 'showOne' , 'deleted'
  ],
  // mixins: [mixins],
  name: 'commands',
  data () {
    return {
      eventBus : EventBus
    }
  },
  methods: {
    deleteMessage: function(messageId){
    	this.$emit('messageDeletedEvent',messageId);
	    /*this.$http.delete("comments/"+messageId).then((response) => { 
	    	// this.$router.push(this.$route.path);
	    }, (response) => {
	        this.error = response;
	    });*/ 
    },
  }

}
</script>
