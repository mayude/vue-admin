<template>
    <span>
        <el-button
            @click="getEditFields"
            size="small"
        >
            {{triggerText}}
        </el-button>
        <el-dialog
            :title="title"
            :visible.sync="isShowEditbox"
            size="large"
        >
            <editor
                :fields="edit_editor"
                :autoValidate="autoValidate"
                ref="editbox"
            ></editor>
            <section slot="footer">
                <el-button 
                    @click="isShowEditbox=false"
                >
                    {{cancelText}}
                </el-button>
                <el-button
                    type="danger"
                    @click="doEdit"
                >
                    {{editText}}
                </el-button>
            </section>
        </el-dialog>
    </span>
</template>

<script>
import editor from "@/editor/editor";
import _id_mixin from "@/mixins/common/_id_mixin.js"
import {getEditInfo,doEditRequest} from "@/server/common.js"
import {logError} from "@/widget/utility.js"
export default {
    name:"edit",
    mixins:[
        _id_mixin,
    ],
    components:{
        editor,
    },
    data(){
        return {
            isShowEditbox:false,
            edit_editor:[],
        }
    },
    props:{
        data:{
            type:Object,
            required:true,
        },
        field_list:{
            type:Object,
            required:true,
        },
        editInfoUri:{

        },
        doEditUri:{
            type:String,
        },
        autoValidate:{
            type:Boolean,
            default:false,
        },
        getEditInfo:{
            type:Function,
            default:getEditInfo,
        },
        doEditRequest:{
            type:Function,
            default:doEditRequest
        },
        transformData:{
            type:Function,
            default:function(data){
                return data;
            }
        },
        title:{
            type:String,
            default:"编辑",
        },
        triggerText:{
            type:String,
            default:"编辑"
        },
        editText:{
            type:String,
            default:"确认编辑"
        },
        cancelText:{
            type:String,
            default:"取消"
        },
    },
    methods:{
        showDialog(){
            this.isShowEditbox = true;
        },
        getEditFields(){
            new Promise((resolve,reject)=>{
                this.getEditInfo(resolve);
            }).then((fields)=>{
                this.edit_editor = fields.reduce((arr,row)=>{
                    let rowitem = row.reduce((rowitem,fieldInfo)=>{
                        let field = fieldInfo.field;
                        let configDefault = this.field_list[field].editorComponent.default;

                        let value = fieldInfo.hasOwnProperty('value')?fieldInfo.value:(typeof configDefault === 'function'? configDefault():configDefault );

                        rowitem.push({
                            field,
                            value,
                            label:this.field_list[field].label,
                            editorComponent:this.field_list[field].editorComponent,
                            tip:this.field_list[field].tip,
                            validator:this.field_list[field].validator,
                        })
                        return rowitem;
                    },[])

                    arr.push(rowitem);
                    return arr;
                },[]);

                this.showDialog();
            }).catch(logError);
        },
        doEdit(){
            this.$refs.editbox.validate().then((data)=>{
                new Promise((resolve,reject)=>{
                    this.doEditRequest(this.transformData(data),resolve)
                }).then(()=>{
                    this.isShowEditbox = false;
                    this.$emit('update');
                }).catch(logError);

            }).catch((err)=>{
                this.$message(err);
            })

        },
    },
}
</script>
