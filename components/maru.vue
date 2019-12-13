<template>
<g :transform="translate">
  <rect width="100" height="100" fill="navy" stroke="black"> </rect>
  <circle cx="50" cy="50" r="40" :fill="fillColor"></circle>
</g>
</template>

<script>
export default{
    
    props: ["i","j","state"],
    computed: {
	translate: function(){
	    return 'translate('+ 100*(this.j+1) + ', ' + 100*(this.i+1) +')';
	},
	fillColor: function(){
	    if(this.state==0){
		return "white";
	    }
	    else if(this.state==1){
		return "yellow";
	    }
	    else if (this.state==2){
		return "red";
	    }
	}
    }
}
</script>
