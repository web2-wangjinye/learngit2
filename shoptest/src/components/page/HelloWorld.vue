<template>

      

        <div class="deliverySetting-table">
          
            <div class="table-body" v-for="(partition,partitionIndex) in distributorsInfo" :key="partitionIndex">
               <el-checkbox 
               :indeterminate="partition.indeterminate" 
               v-model="partition.selected" 
               @change="handleCheckedCountryAllChange(partitionIndex,partition.partitionId,$event)" 
               :key="partitionIndex">
               </el-checkbox>

               <el-checkbox 
               v-for="country in partition.country" 
               v-model="country.selected" 
               @change="handleCheckedCountryChange(partitionIndex,country.id,partition.partitionId,$event)" 
               :label="country" 
               :key="country.id">{{country.fieldName}}
               </el-checkbox>
             
        
            </div>
        </div>

  


</template>
<script>
    export default {
        name:'deliverySetting',

        data() {
            return {
                distributorsInfo:[
                    { partitionName:'1区',selected:false,partitionId:1,isIndeterminate:false,
                        country:[
                        {   id: "1",fieldName: "奥地利",fieldTableName: "奥地利",distributors:'UPS',selected: false},
                        {   id: "2",fieldName: "芬兰",fieldTableName: "芬兰",distributors:'UPS',selected: false}
                      
                    },
                    { partitionName:'2区',selected:false,partitionId:2,isIndeterminate:false,
                        country:[
                        {   id: "5",fieldName: "丹麦",fieldTableName: "单买",distributors:'',selected: false},
                        {   id: "6",fieldName: "法国",fieldTableName: "法国",distributors:'',selected: false},]
                    },
                    { partitionName:'3区',selected:false,partitionId:3,isIndeterminate:false,
                        country:[
                        {   id: "7",fieldName: "德国",fieldTableName: "德国",distributors:'YODEL',selected: false},
                        {   id: "8",fieldName: "瑞士",fieldTableName: "瑞士",distributors:'DPD',selected: false}]
                    }
                ],
                ischeckAll:false,//一级全选状态
                setDistributorDailog:false,
                cancelDistributorDailog:false,
                distributorForm:{
                    vendorName:'',
                    senderName:''
                },
                indeterminate:false,
                rules: {
                    senderName: [{ required: true, message: '字段不能为空',trigger: 'blur'}],
                    website: [{ required: true, message: '字段不能为空',trigger: 'blur'}],
                },
            }
        },
        computed: {
        },
        methods: {
            handleCheckAllChange(e){//一级change事件
                this.ischeckAll = e
                if(e == true){
                    this.indeterminate = false
                    for(var i=0,len=this.distributorsInfo.length; i<len; i++){ //二级全选反选不确定
                        this.distributorsInfo[i].selected = e
                        for(var j=0,len1=this.distributorsInfo[i].country.length; j<len1; j++){
                            this.distributorsInfo[i].country[j].selected = e
                        }
                    }
                }else{
                    this.indeterminate = false
                    for(var i=0,len=this.distributorsInfo.length; i<len; i++){ //三级全选反选不确定
                        this.distributorsInfo[i].selected = e
                        for(var j=0,len1=this.distributorsInfo[i].country.length; j<len1; j++){
                            this.distributorsInfo[i].country[j].selected = e
                        }
                    }
                }
            },
            handleCheckedCountryAllChange(index, topId, e){//二级change事件
                this.distributorsInfo[index].selected = e//二级勾选后，子级全部勾选或者取消
                if(e == false) this.distributorsInfo[index].indeterminate = false //去掉二级不确定状态
                var childrenArray = this.distributorsInfo[index].country
                if(childrenArray)
                    for(var i=0,len=childrenArray.length; i<len; i++)
                        childrenArray[i].selected = e

                this.getIsCheckAll()
            },
            handleCheckedCountryChange(topIndex, sonId, topId, e){//三级change事件
                var childrenArray = this.distributorsInfo[topIndex].country
                var tickCount = 0, unTickCount = 0, len = childrenArray.length
                for(var i = 0; i < len; i++){
                    if(sonId == childrenArray[i].id) childrenArray[i].selected = e
                    if(childrenArray[i].selected == true) tickCount++
                    if(childrenArray[i].selected == false) unTickCount++
                }
                if(tickCount == len) {//三级级全勾选
                    this.distributorsInfo[topIndex].selected = true 
                    this.distributorsInfo[topIndex].indeterminate = false
                } else if(unTickCount == len) {//三级级全不勾选
                    this.distributorsInfo[topIndex].selected = false 
                    this.distributorsInfo[topIndex].indeterminate = false
                } else {
                    this.distributorsInfo[topIndex].selected = false
                    this.distributorsInfo[topIndex].indeterminate = true //添加二级不确定状态
                }

                this.getIsCheckAll()
            },
            getIsCheckAll(){
                var tickCount = 0, unTickCount = 0, ArrLength = this.distributorsInfo.length
                for(var j=0; j<ArrLength; j++){//全选checkbox状态
                    if(this.distributorsInfo[j].selected == true) tickCount++
                    if(this.distributorsInfo[j].selected == false) unTickCount++
                }
                if(tickCount == ArrLength) {//二级全勾选
                    this.ischeckAll = true
                    this.indeterminate = false
                } else if(unTickCount == ArrLength) {//二级全不勾选
                    this.ischeckAll = false
                    this.indeterminate = false
                } else {
                    this.ischeckAll = false
                    this.indeterminate = true //添加一级不确定状态
                }
            },

            showSetDistributorDailog(){
                this.setDistributorDailog=true
            },
            showCancelDistributorDailog(){
                this.cancelDistributorDailog=true
            }
        },

    }
</script>
