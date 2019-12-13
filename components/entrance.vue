<template>
<rect @click="drop" :x="(j+1)*100" width="100" height="100">
</rect>
</template>

<script>
export default{
    
        props: ["j"],
    methods:{
	drop:function(){
	    this.$emit("drop",this.j);
	    console.log(this.j);
	}
    }

}
</script>
