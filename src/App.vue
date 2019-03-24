<template>
  <div id="app">
    <h1>小目标列表</h1>
    <br/>
    <br>
    <Row>
      <Col span="18" push="6" class="onList">
        <input type="text" class="text-keyword" v-model="addText" @keyup.13="addList" placeholder="输入小目标后，按回车确认">
      </Col>
      <Col span="6" pull="18">
        <p class="header">添加小目标</p>
      </Col>
    </Row>
    <br/><br/>
    <p style="padding-left:30px;">共有{{prolist.length}}个目标,已完成{{prolist.length - noend}}个，未完成{{noend}}个</p>
    <p style="margin-left:40px;">
      <template>
        <RadioGroup v-model="phone">
          <span  @click="chooseList(1)">
              <Radio label="apple">
                <Icon type="ios-book-outline" />
                <span>所有目标</span>
              </Radio>
            </span>
          <span  @click="chooseList(2)">
              <Radio label="android">
                <Icon type="ios-book" />
                <span>已完成目标</span>
              </Radio>
            </span>
          <span @click="chooseList(3)">
              <Radio label="windows">
                <Icon type="md-book" />
                <span>未完成目标</span>
              </Radio>
            </span>
        </RadioGroup>
      </template>
    </p>
    <br/>
    <li class="li1" v-for="(list,index) in newList" :key="index" :class="{'eidting':curIndex===index}">
        <div style="padding-left:50px;">
          <span class="type-span" @click="changeType(index)" :class="{'status-end':list.status}"></span>
          <span @dblclick="curIndex=index">{{list.name}}</span>
          <span style="position: fixed;left: 80%;">
            <Time :time="time3" type="datetime" :interval="1"/>
          </span>
          <span class="close" @click='delectList(index)'>X</span>
        </div>
        <input type="text" class="text2" v-model="list.name" @keyup.esc='cancelEdit(list)' @blur='edited' @focus='editBefore(list.name)' @keyup.enter='edited' v-focus/>
    </li>
  </div>
</template>

<script>
export default {
  name: 'app',
  computed: {
    noend: function () {
      return this.prolist.filter(function (item) {
        return !item.status
      }).length
    }
  },
  data () {
    return {
      phone: 'apple',
      curIndex: '',
      prolist: [
        { name: 'HTML5', status: false },
        { name: 'CSS3', status: false },
        { name: 'vue', status: false },
        { name: 'react', status: false }
      ],
      addText: '',
      newList: [],
      beforeEditText: '',
      curType: 0,
      time3: new Date()
    }
  },
  methods: {
    addList () {
      this.prolist.push({
        name: this.addText,
        status: false
      })
      this.addText = ''
    },
    gettimes () {
      this.time3 = new Date()
    },
    chooseList (type) {
      this.curType = type
      switch (type) {
        case 1: this.newList = this.prolist; break
        case 2:this.newList = this.prolist.filter(function (item) { return item.status }); break
        case 3:this.newList = this.prolist.filter(function (item) { return !item.status }); break
      }
    },
    changeType (index) {
      this.newList[index].status = !this.newList[index].status
      this.chooseList(this.curType)
    },
    delectList (index) {
      this.prolist.splice(index, 1)
      this.newList = this.prolist
    },
    //  修改前
    editBefore (name) {
      //  先记录当前项（比如这一项，{name:"HTML5",status:false}）
      //  beforeEditText="HTML5"
      this.beforeEditText = name
    },
    //  修改完成后
    edited () {
      this.gettimes()
      this.curIndex = ''
    },
    cancelEdit (val) {
      val.name = this.beforeEditText
      this.gettimes()
      this.curIndex = ''
    }
  },
  mounted () {
    this.newList = this.prolist
  },
  directives: {
    'focus': {
      update (el) {
        el.focus()
      }
    }
  }
}
</script>

<style>
  body{font-family: '微软雅黑';font-size: 14px;font-weight: 500;}
  input{font-size: 14px;}
  body,ul,div,html{padding: 0;margin: 0;}
  .header{
    background: aqua;
    line-height: 40px;
    text-align: center;
    font-weight: 600;
    font-size: 16px;
  }
  h1{
    margin-left: 10px;}
  li .type-span.status-end{background: aquamarine;}
  li{list-style-type: none;line-height: 40px;position: relative;border: 1px solid transparent;padding: 0 20px;}
  li .type-span{display: block;width: 10px;height: 10px;background: #ccc;margin: 14px 10px 0 0 ;float: left;}
  li .close{position: absolute;color: #f00;font-size: 20px;line-height: 40px;height: 40px;right: 20px;cursor: pointer;display: none;top: 0;}
  li:hover{border: 1px solid #09f;}
  li:hover .close{display: block;}
  li .text-keyword{height: 40px;padding-left: 10px;box-sizing: border-box;margin-left: 10px;width: 80%;display: none;}
  .text-keyword{box-sizing: border-box;width: 100%;height: 40px;padding-left: 10px;outline: none;}
  li.eidting div{display: none;}
  li .text2{height: 40px;padding-left: 10px;box-sizing: border-box;margin-left: 10px;width: 80%;display: none;}
  li.eidting .text2{display: block;}
</style>
