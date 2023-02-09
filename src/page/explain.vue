<template>
    <div class="fillcontain">
        <head-top></head-top>
        <p class="explain_text">{{msg}}</p>
    </div>
</template>

<script>
	import headTop from '../components/headTop'
    export default {
	    data(){
	        return{
                msg:'本项目仅用于实验室使用，不用于商业'
            }
        },
    	components: {
    		headTop,
    	},
        methods:{
    	    async getData(){
    	        let data = await getMsg()
                console.log(data)
                this.msg = data.msg
            }

        },
        mounted() {
	        this.getData()
        }
    }
</script>

<style lang="less">
	@import '../style/mixin';
	.explain_text{
		margin-top: 20px;
		text-align: center;
		font-size: 20px;
		color: #333;
	}
</style>
