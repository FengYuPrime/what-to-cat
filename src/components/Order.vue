<template>
  <div id="box">
    <input
      type="text"
      :placeholder="hint"
      id="input"
      v-model="input"
      @keydown.enter="add"
    />
    <!-- {{ input }} -->
    <ul>
      <li v-for="(i, index) in todo" :key="index" @click="check(index, false)">
        <label>
          <input type="checkbox" :value="i.name" :checked="i.checked" />
          {{ i.name }}
        </label>
        <!-- <button type="button" @click="check(index, true)">删除</button>  -->
        <button type="button" @click="del(index)">删除</button>
      </li>
    </ul>
    <div class="buttonlist">
      <button type="button" @click="selectAll">全选</button>
      <button type="button" @click="selectNone">全不选</button>
      <button type="button" @click="reSelect">反选</button>
      <button type="button" @click="delChecked">删除已勾选</button>
      <button type="button" @click="orderStart">开始</button>
    </div>
  </div>
</template>

<script>
import { nanoid } from "nanoid";
export default {
  name: "Order",
  data() {
    return {
      hint: "请输入名称,按enter结束输入",
      // todo: ["PHP", "Vue", "Python"],
      todo: [
        {
          name: "PHP",
          checked: false,
        },
        {
          name: "JavaScript",
          checked: true,
        },
        {
          name: "Python",
          checked: false,
        },
      ],
      input: "",
      isdel: false,
      id:nanoid(),
      checklist: [],
    };
  },
  methods: {
    add() {
      if (this.input.trim() == "") {
        //判断输入内容是否为空
        alert("输入内容不能为空"), (this.input = "");
      } else {
        console.log("添加了一个", this.input);
        // this.todo.push(this.input);
        this.todo.push({
          name: this.input,
          checked: false,
        });
        this.input = "";
      }
    },
    del(index) {
      // console.log("即将删除", index, "的", this.todo[index]);
      this.isdel = true;
      if(confirm("确认要删除吗?")){

        this.todo.splice(index, 1);
      }
      this.isdel = false;
    },
    check(index) {
      if (!this.isdel) {
        this.todo[index].checked = !this.todo[index].checked;
        console.log("选中该项");
      } else {
        // this.todo.splice(index, 1);
        // this.todo[index].checked = !this.todo[index].checked;
        // console.log("删除该项");
      }
      // console.log(this.todo[index]);
    },
    //全选
    selectAll() {
      this.todo.forEach((e) => {
        e.checked = true;
      });
    },
    selectNone() {
      //全不选
      this.todo.forEach((e) => {
        e.checked = false;
      });
    },
    reSelect() {
      //反选
      this.todo.forEach((e) => {
        e.checked = !e.checked;
      });
    },
    delChecked() {
      //删除所有已勾选
      if (confirm("确认要删除所有已勾选的选项吗?")) {
        this.todo = this.todo.filter((todo) => {
          return !todo.checked;
        });
      }
    },
    orderStart() {
      this.checklist = [];
      for (let i = 0; i < this.todo.length; i++) {
        if (this.todo[i].checked) {
          // console.log('这个对了');
          this.checklist.push(this.todo[i].name);
          // console.log(this.checklist);
        } else {
          // console.log("这个不对");
        }
      }
      if (!this.checklist.length) {
        alert("你都选了些什么!这不是什么都没选吗!");
      } else {
        // console.log(Math.random());
        // console.log(this.checklist[(Math.random()*this.checklist.length-1)]);
        let rand = Math.round(Math.random()*(this.checklist.length-1)) 
        // console.log(rand,"------",this.checklist[rand]);
        console.log("就决定是你了, "+this.checklist[rand]+" !");
        alert("就决定是你了, "+this.checklist[rand]+" !")
      }
      // console.log(this.checklist);
    },
  },
  watch: {
    todo: {
      // immediate:true,
      deep: true,
      handler(news) {
        // console.log("新", news);
        localStorage.setItem("list", JSON.stringify(news));
      },
    },
  },
  mounted() {
    document.title = "¿";
    if (!localStorage.getItem("list")) {
      localStorage.setItem("list", JSON.stringify(this.todo));
    } else {
      this.todo = JSON.parse(localStorage.getItem("list"));
    }
  },
};
</script>

<style>
#box {
  width: 450px;
  height: 500px;
  margin: 30px auto;
  border: #ccc 1px solid;
  border-radius: 5px;
  display: flex;
  /* justify-content: center; */
  align-items: center;
  flex-direction: column;
  overflow: auto;
  box-sizing: border-box;
  padding: 10px;
}
#box #input {
  border: #ccc 1px solid;
  height: 30px;
  width: 400px;
  padding-left: 10px;
  box-sizing: border-box;
}
#box #input:hover {
  box-shadow: aqua 0 0 5px;
}
#box ul {
  margin-top: 20px;
  height: 400px;
  overflow: auto;
}
#box li {
  width: 400px;
  line-height: 36px;
  height: 36px;
  font-size: 14px;
  box-sizing: border-box;
  overflow: hidden;
  /* border-radius: 5px; */
  padding: 0 10px;
  display: flex;
  justify-content: space-between;
  border: 1px #ccc solid;
  border-bottom: 0;
}
#box ul li:last-child {
  border-bottom: 1px #ccc solid;
}
#box li:hover button {
  display: block;
}
#box li:hover {
  background-color: #ccc;
}
#box li button {
  display: none;
}
button {
  background-color: #da4f49;
  color: white;
  padding: 0 15px;
  border-radius: 5px;
  height: 30px;
  margin: auto 0;
  border: 0;
  box-sizing: border-box;
  font-size: 14px;
  /* display: none; */
}
.buttonlist {
  display: flex;
  justify-content: space-between;
  width: 400px;
  /* background-color: #ccc; */
}
button:hover{
  box-shadow: #000 0 0 5px;
  background: white;
  color: #da4f49;
}
</style>
