<template>
    <div class="center" :style="style_width">
        <div class="text">Barcode </div>

        <svg class="lock locked" v-show="!independance" fill="none" stroke="currentColor" stroke-width="1.5" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg" aria-hidden="true">
            <path stroke-linecap="round" stroke-linejoin="round" d="M16.5 10.5V6.75a4.5 4.5 0 10-9 0v3.75m-.75 11.25h10.5a2.25 2.25 0 002.25-2.25v-6.75a2.25 2.25 0 00-2.25-2.25H6.75a2.25 2.25 0 00-2.25 2.25v6.75a2.25 2.25 0 002.25 2.25z"></path>
        </svg>

        <svg class="lock unlocked" v-show="independance" fill="none" stroke="currentColor" stroke-width="1.5" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg" aria-hidden="true">
            <path stroke-linecap="round" stroke-linejoin="round" d="M13.5 10.5V6.75a4.5 4.5 0 119 0v3.75M3.75 21.75h10.5a2.25 2.25 0 002.25-2.25v-6.75a2.25 2.25 0 00-2.25-2.25H3.75a2.25 2.25 0 00-2.25 2.25v6.75a2.25 2.25 0 002.25 2.25z"></path>
        </svg>

        <svg fill="none" stroke="currentColor" stroke-width="1.5" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg" aria-hidden="true" class="add" @click="addVisualCodeList">
            <path stroke-linecap="round" stroke-linejoin="round" d="M12 4.5v15m7.5-7.5h-15"></path>
        </svg>

        <svg fill="none" stroke="currentColor" stroke-width="1.5" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg" aria-hidden="true" class="top">
            <path stroke-linecap="round" stroke-linejoin="round" d="M12 19.5v-15m0 0l-6.75 6.75M12 4.5l6.75 6.75"></path>
        </svg>
    
        <div class="pers-bar">
            <div class="one-bar" v-for="item in visual_list" :key="item" :id="'bar_'+item">
                <MyCodesList  ref="code_window" @delete="deleteVisualCodeList" :index="item" :show_name="show_name"/>
            </div>
        </div>
    </div>
</template>

<style scoped>
.center{
    height: 1250px;
    border-radius: 20px; 
    position: relative;
    border: 2px solid #F1F4FA;
}

.text{
  color:#2932FF;
  font-size: 16px;
  font-weight: bold;
  padding: 12px;
  border-radius: 20px;
  background-color: #F1F4FA;
}

.pers-bar{
    position: relative;
    display: flex;
    width: 100%;
    height: 100%;
}

.one-bar{
    position: relative;
    width: 100%;
    height: 100%;
    margin-left: 2px;
    margin-right: 2px;
    transform: all 0.3s
}

.add, .top, .lock{
    position: absolute;
    width: 25px;
    color:#2932FF;
    top: 8px;
}
.add{
    right: 12px;
}
.top{
    right: 40px;
}

.lock{
    right: 68px;
}

</style>

<script>
import MyCodesList from './MyCodesList.vue';
import bus from '../util/eventBus';
import { interactive_list } from '../util/codeList';
import { color_for_highlight } from '../util/colorMapping';

export default{
    emits:['change'],
    components:{ MyCodesList },
    data(){
        return{
            style_width:{
                "width": "300px"
            },
            show_name: false,
            visual_list: [0],
            latest_idx: 0,
            independance:false
        }
    },

    methods:{
        showName(){
            this.show_name = !this.show_name;
        },

        exportToCsv(){

        },

        addVisualCodeList(){
            if(this.visual_list.length >= 3){
                alert('The visual codes list exceed its upper limit')
                return
            }
            this.visual_list.push(++this.latest_idx)
            this.$emit('change', this.visual_list.length)
            this.style_width["width"] = 300*this.visual_list.length+"px";
        },

        deleteVisualCodeList(index){
            if(this.visual_list.length<=1){
                alert('This is the last visual code')
                return
            }
      
            for(let i=0; i<this.visual_list.length; i++){
                if(Number(index) === this.visual_list[i]){
                    this.visual_list.splice(i, 1)
                    bus.all.get("updateVisualCode").splice(i, 1)
                    bus.all.get("updateDataOrder").splice(i, 1)
                    bus.all.get("changeRectOpacity").splice(i, 1)
                    bus.all.get("changeLineOpacity").splice(i, 1)
                    bus.all.get("clickForLine").splice(i+1, 1)
                    // bus.all.get("showSimilarityCode").splice(i, 1)
                    // bus.all.get("calculateDistance").splice(i, 1)
                    bus.all.get("alignOrder").splice(i, 1)
                    bus.all.get("synchronizeScroll").splice(i, 1)
                    bus.all.get("updatePersistence").splice(i, 1)
                    break
                }
            }
            this.$emit('change', this.visual_list.length)
            this.style_width["width"] = 300*this.visual_list.length+"px";
        },
 
    },
    created(){
        bus.on("clearCanvas", msg=>{
            for(let i=0; i<this.visual_list.length; i++)
                this.$refs.code_window[i].clearHighlight()
            for(let key in color_for_highlight)
                delete color_for_highlight[key]
            interactive_list.splice(0, interactive_list.length)
            bus.emit("clickForLine", -1)
        })
    },

}
</script>