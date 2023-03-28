<template>
    <div class="encode">
        <div class="text" style="display:inline-block;font-weight: bold " @click="makeEncodeFold"> Encode </div>
        <input type="button" id="reset" class="encode-button" value="Reset" @click="resetEncoding">
        <input type="button" id="update" class="encode-button" value="Update" @click="startUpdateVisualCode">
        
        <div class="text3 rect" v-show="encode_fold">
            <div class="text2">Rectangle</div>
            Width
            <select class="rect-width" @change="setRectWidth">
                <option v-for="item in encode_options">{{item}}</option>
            </select>
            <br>
            Length
            <select class="rect-height" @change="setRectHeight">
                <option v-for="item in encode_options">{{item}}</option>
            </select>
            <br>
            Position
            <select class="rect-pos" @change="setRectPos">
                <option v-for="item in encode_options">{{item}}</option>
            </select>
            <br>
        </div>

        <div class="text3 line" v-show="encode_fold">
            <div class="text2">Hrizontal line</div>
            Width
            <select class="line1-width" @change="setLine1Width">
                <option v-for="item in encode_options">{{item}}</option>
            </select>
            <br>
            Y-position
            <select class="line1-height" @change="setLine1Pos">
                <option v-for="item in encode_options">{{item}}</option>
            </select>
            <br>
            Stroke-width
            <select class="line1-stroke" @change="setLine1StrokeWidth">
                <option v-for="item in encode_options">{{item}}</option>
            </select>
            <br>
            Stroke-dash
            <select class="line1-strokeDash" @change="setLine1StrokeDash">
                <option v-for="item in encode_options">{{item}}</option>
            </select>
            <br>
        </div>

        <div class="text3 line" v-show="encode_fold">
            <div class="text2">Vertical line</div>
            Length
            <select class="line2-height" @change="setLine2Height">
                <option v-for="item in encode_options">{{item}}</option>
            </select>
            <br>
            Stroke-width
            <select class="line2-stroke" @change="setLine2Stroke">
                <option v-for="item in encode_options">{{item}}</option>
            </select>
            <br>
        </div>
    </div>

    <div id="split-line" ></div>

    <div class="text3 sort">
        <div class="text" style="display:inline-block;font-weight: bold; margin-bottom: 5px;"> Order </div>
        <select class="windows-select">
            <option v-for="item in encode_options">{{item}}</option>
        </select>
        <draggable v-model="sort_list" item-key="index" style="padding-left:5px" >
            <template #item="{element}">
                <div :key="element.index" class="dragbox">
                    <div style="display:inline-block" position="relative" > {{ element.name }}</div>
                    <select class="sort-option">
                        <option>ON</option>
                        <option>RE</option>
                        <option>OFF</option>
                    </select>
                    <svg fill="none" stroke="currentColor" stroke-width="1.5" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg" aria-hidden="true">
                        <path stroke-linecap="round" stroke-linejoin="round" d="M3 7.5L7.5 3m0 0L12 7.5M7.5 3v13.5m13.5 0L16.5 21m0 0L12 16.5m4.5 4.5V7.5"></path>
                    </svg>
                </div>
            </template>
        </draggable>
    </div> 

    <div id="split-line" ></div>

    <div class="text3 param">
        <div class="text" style="display:inline-block;font-weight: bold; margin-bottom: 5px;" > Parameters </div>
        <input type="button" id="name" class="par-button" value="Name" >
        <br>
        Rect-opacity
        <input type="range" class="opacity" id="rect" min="0" max="1" step="0.01" v-model="rect_opacity" @input="changeRectOpacity">
        <br>
        Line-opacity
        <input type="range" class="opacity" id="line" min="0" max="1"
            step="0.01" v-model="line_opacity" @input="changeLineOpacity">
        <!-- <input type="button" id="time" class="par-button" value="Global Time" @click="changeGlobalX">
        <input type="button" id="value" class="par-button" value="Global Value" @click="changeGlobalY"> -->
        

    </div>
    
</template>

<style scoped>
.encode, .param, .sort{
    position: relative;
    margin-left: 15%;
    margin-top: 5%;
}

.text{
  color:#2932FF;
  font-size: 16px;
  display: inline-block;
}

.encode-button{
    position: absolute;
    width: 70px;
    color: #F1F4FA;
    background-color: #809FB8;
    font-size: 14px;
    border-radius: 10px;
    border: 1px solid #809FB8;
    opacity: 0.8;
}

#reset{
    right: 40%;
}

#update{
    right: 10%;
}

.text2{
    margin-top: 5px;
    margin-bottom: 5px;
    color:#809FB8;
    font-weight: bold;
    font-size: 14px;
}

.text3{
    color:#809FB8;
    text-align: 14px;
    line-height: 26px;
}

select{
    color: #809FB8;
    position: absolute;
    width: 100px;
    right: 10%;
    background-color: #F1F4FA;
    border-radius: 20px;
    border: 1px solid #C6D7E5;
    text-align: left;
    padding-left: 3px;
}

#split-line{
  height: 2px;
  background-color:#C6D7E5;
  margin-top: 8%;
  margin-left: 15%;
  margin-right: 10%;
  margin-bottom: 8%;
}

.dragbox{
    border: 1px #809FB8 solid;
    width: 85%;
    margin-top: 10px;
    padding-left: 5px;
    border-radius: 20px;
    position: relative;
}

/* .par-button{
    margin-top: 10px;
    position: absolute;
    color: #F1F4FA;
    background-color: #809FB8;
    font-size: 14px;
    border-radius: 10px;
    border: 1px solid #809FB8;
    opacity: 0.8;
} */

#time{
    width: 90px;
}

#value{
    width: 90px;
    right: 105px;
}

#name{
    position: absolute;
    color: #F1F4FA;
    background-color: #809FB8;
    font-size: 14px;
    border-radius: 10px;
    border: 1px solid #809FB8;
    opacity: 0.8;
    width:50px;
    right: 10%;
}

svg{
    width: 20px;
    color: #809FB8;
    position: absolute;
    right: 6px;
    top: 3px;
}

.sort-option{
    position: absolute;
    width: 60px;
    color: #F1F4FA;
    background-color: #809FB8;
    font-size: 14px;
    border-radius: 10px;
    opacity: 0.8;
    right:40px;
    top: 3px;
}

.opacity{
    position: absolute;
    right: 10%;
    width: 30%;
    border-radius: 5px;
    margin-top:8px;
    outline: 0;
    -webkit-appearance: none;
    -moz-animation: none;
    appearance: none;
    background-color: transparent;
}

input[type="range"]::-webkit-slider-thumb {
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none; 
    width: 10px;
    height:10px;
    border-radius: 50%;
    background-color: #809FB8;
    margin-top: -4px;
    border: 1px solid transparent;
    border-image: linear-gradient(#809FB8,#809FB8) 0 fill / 6 10 6 0 / 0 0 0 99vw;
}

[type="range"]::-webkit-slider-container {
    height: 10px;
    overflow: hidden;
}
[type="range"]::-webkit-slider-runnable-track {
    height: 2px;
    background: #D1D4DE;
}

</style>

<script>
import { mapping_relationship, user_parameters } from '../util/parameters'
import bus from '../util/eventBus'
import Draggable from 'vuedraggable'
import MySwitch from './MySwitch.vue'
import { sort_type, updateOrder, select_first, match_first } from '../util/sortForVisual'
import { initAllTheHillsData, visualcode_object } from '../util/codeList'
import { calculateDataField } from "../util/dataManager"

export default{
    components:{ Draggable, MySwitch },
    data(){
        return{
            encode_options:['none','width','peak_time',
                'max','min','persistence','area'],
            encode_fold: true,
            // sort_fold: true,
            global_x: true,
            global_y: true,
            rect_opacity: 0.8,
            line_opacity: 1,
            top_select: false,
            top_match: false, 
            // on_line_order: false,
            sort_list:sort_type.map((name, index) =>{
                return { name, index, ban:false, reverse:false};
            })
        }
    },

    methods:{

        /// fold toorbar
        makeEncodeFold(){
            this.encode_fold = !this.encode_fold;
            if(this.encode_fold){
                document.querySelector('#encode-allow').style.transform = "rotateZ(0deg)";
            }
            else{
                document.querySelector('#encode-allow').style.transform = "rotateZ(-90deg)";
            }
        },

        resetEncoding(flag = true){
            this.rect_width.value = this.encode_options[6]
            mapping_relationship["rect"]["width"] = this.encode_options[6]
            this.rect_height.value = this.encode_options[0]
            mapping_relationship["rect"]["height"] = this.encode_options[0]
            this.rect_pos.value = this.encode_options[2]
            mapping_relationship["rect"]["x"] = this.encode_options[2]
            // this.rect_color.value = this.encode_options[0]
            // mapping_relationship["rect"]["fill"] = this.encode_options[0]
            this.line1_width.value = this.encode_options[1]
            mapping_relationship["hline"]["width"] = this.encode_options[1]
            this.line1_y.value = this.line1_y.options[0].value;
            mapping_relationship["hline"]["y"] = this.encode_options[0]
            this.line1_strokeWidth.value = this.encode_options[0]
            mapping_relationship["hline"]["strokeWidth"] = this.encode_options[0]
            this.line1_strokeDash.value = this.encode_options[0]
            mapping_relationship["hline"]["strokeDash"] = this.encode_options[0]
            // this.line1_color.value = this.encode_options[0]
            // mapping_relationship["hline"]["stroke"] = this.encode_options[0]
            this.line2_height.value = this.encode_options[0]
            mapping_relationship["vline"]["height"] = this.encode_options[0]
            this.line2_strokeWidth.value = this.encode_options[0]
            mapping_relationship["vline"]["strokeWidth"] = this.encode_options[0]
            // this.line2_color.value = this.encode_options[0]
            // mapping_relationship["vline"]["stroke"] = this.encode_options[0]

            if(flag) this.startUpdateVisualCode();
        },
        startUpdateVisualCode(){
            bus.emit("updateVisualCode", false);
            // console.log(mapping_relationship);
        },

        /// a series of assignment funtions
        setRectWidth(){
            mapping_relationship["rect"]["width"] = this.rect_width.value;
        },
        setRectHeight(){
            mapping_relationship["rect"]["height"] = this.rect_height.value;
        },
        setRectPos(){
            mapping_relationship["rect"]["x"] = this.rect_pos.value;
        },
        setRectColor(){
            mapping_relationship["rect"]["fill"] = this.rect_color.value;
        },
        setLine1Width(){
            mapping_relationship["hline"]["width"] = this.line1_width.value;
        },
        setLine1Pos(){
            mapping_relationship["hline"]["y"] = this.line1_y.value;
        },
        setLine1StrokeWidth(){
            mapping_relationship["hline"]["strokeWidth"] = this.line1_strokeWidth.value;
        },
        setLine1StrokeDash(){
            mapping_relationship["hline"]["strokeDash"] = this.line1_strokeDash.value;
        },
        setLine1Color(){
            mapping_relationship["hline"]["stroke"] = this.line1_color.value;
        },
        setLine2Height(){
            mapping_relationship["vline"]["height"] = this.line2_height.value;
        },
        setLine2Stroke(){
            mapping_relationship["vline"]["strokeWidth"] = this.line2_strokeWidth.value;
        },
        setLine2Color(){
            mapping_relationship["vline"]["stroke"] = this.line2_color.value;
        },

        changeRectOpacity(){
            user_parameters["rect_opacity"] = this.rect_opacity;
            bus.emit("changeRectOpacity", this.rect_opacity);
        },
        changeLineOpacity(){
            user_parameters["line_opacity"] = this.line_opacity;
            bus.emit("changeLineOpacity", this.line_opacity);
        },
        changeGlobalX(){
            user_parameters["global_time"] = !user_parameters["global_time"];
            for(let key in visualcode_object){
                for(let i=0, len=visualcode_object[key].length; i<len; i++)
                    visualcode_object[key][i].updateDataXDomain();
            }
            bus.emit("updateVisualCode", false);
        },
        changeGlobalY(){
            user_parameters["global_value"] = !user_parameters["global_value"];
            for(let key in visualcode_object){
                for(let i=0, len=visualcode_object[key].length; i<len; i++)
                    visualcode_object[key][i].updateDataYDomain(key);
            }
            bus.emit("updateVisualCode", false);
        },

        changeTopSelect(){
            select_first[0] = this.top_select
            bus.emit("updateDataOrder", null)
        },

        changeTopMatch(){
            match_first[0] = this.top_match
            bus.emit("updateDataOrder", null)
        },

    },

    created(){
        bus.on("updateDataset", msg=>{
            /// parameters will not reset while update dataset
            this.resetEncoding(false);
            this.sort_list = sort_type.map((name, index) =>{
                return { name, index, ban:false, reverse:false};
            })
            calculateDataField();
            /// This function only call here to deal the origin data
            initAllTheHillsData()
            console.log("finish coding.");
            bus.emit("updateVisualCode", true);
        })
        calculateDataField();
        initAllTheHillsData()
    },

    mounted(){
        this.rect_width = document.querySelector(".rect-width");
        this.rect_height = document.querySelector(".rect-height");
        this.rect_pos = document.querySelector(".rect-pos");
        // this.rect_color = document.querySelector(".rect-color");
        this.line1_width = document.querySelector(".line1-width");
        this.line1_y = document.querySelector(".line1-height");
        this.line1_strokeWidth = document.querySelector(".line1-stroke");
        this.line1_strokeDash = document.querySelector(".line1-strokeDash");
        // this.line1_color = document.querySelector(".line1-color");
        this.line2_height = document.querySelector(".line2-height");
        this.line2_strokeWidth = document.querySelector(".line2-stroke");
        // this.line2_color = document.querySelector(".line2-color");

        this.resetEncoding(false);
    }
}
</script>