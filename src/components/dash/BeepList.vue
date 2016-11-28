<template>
    <div>
        <div id="beepsWraper">
            <beep v-for="beep in beeps" :beep="beep"></beep>
        </div>
        <div id="beepsLoading" class="text-center" v-show="beepsLoading" :showUserInfo="showUserInfo">
             <i class="fa fa-spin fa-spinner"></i>       
        </div>
    </div>
</template>

<script>
    import Beep from 'components/dash/Beep.vue';

    export default {
        name: 'BeepList',
        components: {
            beep: Beep,
        },
        created() {
            this.beeps = [];
            this.getBeeps(1);
            window.addEventListener('scroll', this.handleScroll);
            this.$root.$on('newBeep',this.handleNewBeep);
        },
        destroyed(){
            window.removeEventListener('scroll', this.handleScroll);
        },
        props:{
            endpoint:{ type:String, default: "/beeps"},
            showUserInfo:{type: Boolean, default: true}
        },
        data: function () {
            return {
                beeps: [],
                page:{},
                beepsLoading: false
            }
        },
        methods: {
            getBeeps: function (page) {
                this.beepLoading = true;
                this.$http.get(this.endpoint + '?page=' + page)
                        .then(function (res) {
                            this.beeps = this.beeps.concat(res.body.data);
                            this.page = {current: res.body.current_page, last:res.body.last_page};
                            this.beepLoading = false;
                        });
            },
            handleScroll(){
                if (document.body.scrollHeight - window.innerHeight - document.body.scrollTop == 0){
                    if(this.page.current < this.page.last)
                       this.getBeeps(this.page.current + 1);
                }
            },
            handleNewBeep(beep){
                if(!this.$route.params.username || this.$route.params.username == beep.author.username){
                    this.beeps.unshift(beep);
                }
            }
        },
        watch: {
            endpoint: function(){
                this.beeps = [];
                this.getBeeps();
            }
        }
    }
</script>

<style scoped>
    #beepsWraper {
        margin: 0 -20px;
    }

    #beepsLoading {
        padding: 40px;
    }

    #beepsLoading i {
        font-size: 40px;
    }
</style>