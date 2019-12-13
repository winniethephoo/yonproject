<template>
  
<v-layout
   column
   justify-center
   align-center
   >

  <banmen>
</banmen> 
</v-layout>

</template>

<script>
import banmen from '~/components/banmen.vue'
import axios from 'axios';
import firebase from "~/plugins/firebase.js";
import { mapGetters} from 'vuex';
const db = firebase.firestore();

export default {
  components: {
      banmen      
  },
  
    computed: {
　　　 // VuexからPostsデータを取得
      ...mapGetters(['games'])
   },
    created: function () {
　　　　// firestoreのpostsをバインド
      this.$store.dispatch('setGamesRef', db.collection('games'))
    },
    methods: {

	
	shoot:function(j){
	    db.collection('games').add({j:j});
	},
	sendData: function () {
	    　　　　　　// データのチェック
            if (this.message == "" || this.message.length > 100) {
		return false;
            }
            let dbdata = {
		message: this.message
            };
	    　　　　　　// データの登録
            db.collection('games').add(dbdata);
	}
    }
}
</script>
