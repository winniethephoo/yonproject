<template>
  <div>
    
    <svg v-if="started" width=800 height=800>
      <text y=100>
	{{games[0].board}}
      </text>
      <g> <entrance v-if="playing" v-for="(c,j) in parse(games[0].board[0])" :j="j" @drop="shoot"></entrance>
	<g v-for="(row,i) in games[0].board">
	  <maru v-for="(c,j) in row" :i="i" :j="j" :state="games[0].board[i][j]" ></maru>
	</g>
      </g>
    </svg>
    <v-btn @click="start">
      start
    </v-btn>
</div>
</template>

<script>
import entrance from '~/components/entrance.vue'

import maru from '~/components/maru.vue'
import banmen from '~/components/banmen.vue'
import axios from 'axios';
import firebase from "~/plugins/firebase.js";
import { mapGetters} from 'vuex';
const db = firebase.firestore();

let b=[];
for(let i=0; i<6; i++){
    b.push([]);
    for(let j=0; j<7; j++){
	//b[i][j]=0;
	b[i].push(0)
    }
}
export default{
    components:{
	maru,
	entrance
    },
    
    props:['board'],
    data: function(){
	return {player:1,playing:true,started:false};
	
    },
    computed: {
　　　 // VuexからPostsデータを取得
	    ...mapGetters(['games']),//localのプロパティとGoogle上のfirebaseを結びつけてる
	/*board(){
	    let ref=db.collection("games").doc('mygame')//gamesというcollectionのなかのmygameというドキュメントへの参照
	    ref.get().then(
		function(doc){
		    console.log(JSON.parse(doc.data().board));
		    return  JSON.parse(doc.data().board)//データをparse(文字列化された配列から二重配列に戻す)して返す
		}
	    )
	}*/
	
   },
    created: function () {
　　　　// firestoreのpostsをバインド
	this.$store.dispatch('setGamesRef', db.collection('games'));
	let ref=db.collection("games").doc('mygame');
	ref.set({board:JSON.stringify(b)});
    },
    methods:{
	start:function(){
	    let ref=db.collection("games").doc("mygame");
	    ref.get().then(function(doc){
		ref.set({
		    banmen:JSON.stringify([[0,0,0,0,0,0,0],[0,0,0,0,0,0,0],[0,0,0,0,0,0,0],[0,0,0,0,0,0,0],[0,0,0,0,0,0,0],[0,0,0,0,0,0,0]]),
		    player:1,
		    playing:true
		    
		})
	    })
	    this.started=true;
	    console.log(this.games);
	},
	parse(x){
	    return JSON.parse(x);
	    },
	shoot(j){
	    this.$emit('shoot',j)
	    /*for(let i=this.board.length-1; i>=0; i--){
		if(this.board[i][j]==0){
		    this.board[i].splice(j,1,this.player);
		    if(this.hantei(this.board,this.player)){
			this.playing=false;
			break;
		    }
		    
		    if(this.player==1){
			this.player=2;
			
		    }
		    else  if(this.player==2){
			
			this.player=1;	
		    }			
		    break;
		}
	    }*/
	}
	,
	hantei(board,player){
	    for(let j=0; j<7; j++){
		for(let i=0; i<3; i++){
		    let flag=true;
		    for(let k=0; k<4; k++){
			console.log([i,j,k]);
			if(board[i+k][j]!=this.player){
			    flag=false;
			    break;
			}
			
		    }
		    if (flag==true){
			return true;
		    }
		}
	    }
	    for(let i=0; i<6; i++){
		for(let j=0; j<4; j++){
		    let flag=true;
		    for(let k=0; k<4; k++){
			console.log([i,j,k]);
			if(board[i][j+k]!=this.player){
			    flag=false;
			    break;
			}			
		    }
		    if (flag==true){
			return true;
		    }
		}
	    }
	    for(let j=0; j<4; j++){
		for(let i=0; i<3; i++){
		    let flag=true;
		    for(let k=0; k<4; k++){
			console.log([i,j,k]);
			if(board[i+k][j+k]!=this.player){
			    flag=false;
			    break;
			}			
		    }
		    if (flag==true){
			return true;
		    }
		}
	    }
	    for(let j=6; j>2; j--){
		for(let i=0; i<3; i++){
		    let flag=true;
		    for(let k=0; k<4; k++){
			console.log([i,j,k]);
			if(board[i+k][j-k]!=this.player){
			    flag=false;
			    break;
			}			
		    }
		    if (flag==true){
			return true;
		    }
		}
	    }
	    return false;
	}

	
    }    

  }

</script>

