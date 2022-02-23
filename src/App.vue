<template>
  <div class="todolist-container">
    <div class="header-box">
      <div class="header-left">
        <div>+</div>
        <span>Todo List</span>
      </div>
      <div class="header-right">
        <button class="all-select" @click="handleAllSelect"> 全选</button>
        <button class="header-add" @click="handleAdd">添加</button>
      </div>
    </div>
    <div class="content topnav_box" >
      <div class="content-item" v-for="(item,index) in todoList" :key="item.id">
<!--        选中框-->
        <div class="content-item-select" @click="handleSelect(item,index)">
          <span :style="item.isCheck?'opacity:1':'opacity:0'"></span>
        </div>
<!--        输入框-->
        <input
          type="text" class="content-item-input" v-model="item.text"
          :disabled="item.isCheck"
          :class="item.isCheck?'line-through':''"
          @blur="handleBlur"
        >
<!--        右边-->
        <div class="content-item-right">
          <p>{{ item.time }}</p>
          <button @click="handleDelete(item,index)">删除</button>
        </div>
      </div>


    </div>


  </div>
</template>

<script>
import dayjs from 'dayjs'

export default {
  name: 'App',
  components: {
  },
  data() {
    return {
      todoList:[]
    }

  },
  created(){
    try {
      this.todoList=JSON.parse(window.localStorage.getItem('listTodo'))
    }catch (err) {
      console.log('没有本地存储文件')
    }
  },
  methods:{

    //添加item
    handleAdd() {
      this.todoList.unshift({
        id:this.randomID(),
        isCheck: false,
        text: "",
        time:dayjs(new Date).format("YY-MM-DD HH:mm"),
      })
    },
    //输入完成后存储到本地
    handleBlur(){
      this.storage()
    },
    // 删除item
    handleDelete(item,index) {
      // let index=this.todoList.indexOf(item)
      this.todoList.splice(index,1)
      this.storage()
    },
    // 选中item功能
    handleSelect(item,index) {
      this.todoList[index].isCheck=!this.todoList[index].isCheck;
      this.storage()
    },
    //全选功能
    handleAllSelect() {
      let allSelected=true
      this.todoList.forEach(item=>{
        if(!item.isCheck){
          allSelected=false
        }
      })
      if(allSelected){
        this.todoList.forEach(item=>{
          item.isCheck=false
        })
      }else{
        this.todoList.forEach(item=>{
          item.isCheck=true
        })
        allSelected=true
      }

      this.storage()
    },
    //生成随机id
    randomID() {
      return Number(Math.random().toString().substr(2,0)+Date.now().toString(10))
    },
    // 本地存储
    storage() {
      window.localStorage.setItem('listTodo',JSON.stringify(this.todoList))
    }
  }
}
</script>

<style>
  /*滚动条样式*/
  .topnav_box::-webkit-scrollbar{
    width: 5px;
    height: 10px;
    border-radius: 10px;
    background:none;
  }
  .topnav_box::-webkit-scrollbar-track{
    -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,0.3);
    border-radius: 10px;
    background: rgba(85, 67, 186, 0.85);
  }
  .topnav_box::-webkit-scrollbar-thumb {
    border-radius: 10px;
    -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,0.3);
    background-color: rgb(140, 231, 223);
  }
  * {
    padding: 0;
    margin:0;
  }
  html {
    height: 100%;
  }
  body {
    background-image:linear-gradient(to bottom right,rgb(114,135,254),rgb(130,88,186));
  }
  .todolist-container {
    width: 800px;
    height: 600px;
    position: absolute;
    top:50%;
    left:50%;
    transform: translate(-50%,-50%);
    border-radius: 20px;
    padding: 20px;
    box-sizing: border-box;
    box-shadow: 0 10px 50px 0 rgb(59,45,159);
    background-color: rgba(85, 67, 186, 0.85);
    color: white;
  }
  .header-box{
    display: flex;
    /*左右两边布局*/
    justify-content: space-between;
    align-items: center;
    border-bottom:1px solid rgb(199,191,219);
    padding-bottom: 20px;
  }
  .header-left{
    display: flex;
    /*垂直居中*/
    align-items: center;
  }
  .header-left div {
    font-size: 30px;
    height: 50px;
    width: 50px;
    border-radius: 50%;
    background-image: linear-gradient(to bottom right, rgb(80, 202, 205),rgb(92,103,211));
    text-align: center;
    margin-right: 15px;
    /*line-height: 50px;*/
    /*line-height: 30px;*/
    display: flex;
    align-items: center;
    /*上下居中，水平居中↓*/
    justify-content: center;
    transition: .2s;
  }
  .header-left div:hover {
    background-image: linear-gradient(to bottom right, rgb(132, 80, 205), rgb(211, 130, 92));
    cursor: pointer;
  }
  .header-right {
    color: white;
  }
  button {
    color: white;
    padding: 5px 10px;
    height: 30px;
    width: 60px;
    border:1px solid rgb(80, 201, 205);
    background: none;
    border-radius: 10px;
    transition: .2s;
    margin-left: 10px;
  }
  .header-right .all-select {
    border:1px solid rgb(80, 201, 205);
    color:rgb(80, 201, 205);
    transition: .2s;
  }
  .header-right .all-select:hover {
    background:rgb(80, 201, 205);;
    color: white;
    cursor: pointer;
  }
  .header-right .header-add {
    border:1px solid rgb(153, 205, 80);
    color:rgb(153, 205, 80);
    transition: .2s;
  }
  .header-right .header-add:hover {
    background:rgb(153, 205, 80);;
    color: white;
    cursor: pointer;
  }
  .content{
    margin-top:30px;
    height: 450px;
    overflow-y: scroll;
    transition: .2s;
  }
  .content-item {
    display: flex;
    margin-top: 20px;
    margin-left: 20px;
    margin-right: 20px;
    align-items: center;
    justify-content: space-between;
    box-shadow: 0 10px 20px 0 rgb(59,45,159);
    background-color: rgba(116, 77, 217, 0.63);
    border-radius: 10px;
    padding: 20px 20px;
    box-sizing: border-box;
  }
  .content-item-select {
    width: 20px;
    height: 20px;
    border: 1px solid white;
    border-radius: 50%;
    position: relative;
  }
  .content-item-select span {
    display: inline-block;
    width: 12px;
    height: 12px;
    border-radius: 50%;
    background: #e5c3e7;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%,-50%);
  }
  .content-item-input {
    flex: 1;
    margin:0 10px;
    outline: none;
    background: none;
    border:none;
    border-bottom:1px solid rgb(199,191,219);
    padding: 5px 10px;
    color: white;
  }
  .line-through {
    color: rgba(255,255,255,0.5);
    text-decoration: line-through rgba(255,255,255,0.8);
  }
  .content-item-right {
    display: flex;
    align-items: center;
  }
  .content-item-right button {
    border:1px solid rgb(205, 80, 199);
    color: rgb(205, 80, 199);
    transition: .2s;
  }
  .content-item-right button:hover {
    background:rgb(205, 80, 199);;
    color: white;
    cursor: pointer;
  }




</style>
