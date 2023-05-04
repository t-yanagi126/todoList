<template>
  <div id="to-do">
    <SelectRadio :options="radioOptions" name="filterRadio" @change-radio="changeRadio" :modelValue="displayMode"/>
    <p>
      タスク追加
      <form v-on:submit.prevent="addList">
        <input type="text" v-model="inputTask.content" />
        <input type="submit" value="追加" v-bind:disabled="isExistInputText" />
        <br>
      </form>
      <button id="deleteAll" v-on:click="deleteList()"  v-bind:disabled="isExistTaskList" >全削除</button>
    </p>
    <hr />
    <ul>
      <li v-for="(task, index, value) in displayTaskList" :key="index" >
        <button id="editIndex" :value="index" v-on:click="editIndex(index)" >編集</button>
        <input
          type="checkbox"
          id="check"
          :value="index"
          v-model="task.checked"
        />
        <div style="display: inline-block; _display: inline;" v-if="!task.isEdit">{{ task.content }}
          <button id="deleteIndex" :value="index" v-on:click="removeList(index)" >削除</button>
        </div>
        <div v-else >
          <input type="text" v-model="task.content" />
          <button v-on:click="task.isEdit = false">確定</button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import SelectRadio from './SelectRadio.vue';

export default {
  data() {
    return {
      taskList: [],
      inputTask: {
        content: "",
        idEdit: false,
        checked: false
      },
      displayMode: 0,
      radioOptions: [
        {
          label: "すべて",
          value: 0
        },
        {
          label: "チェック済み",
          value: 1
        },
        {
          label: "未チェック",
          value: 2
        }
      ],

    };
  },
  methods: {
    // リスト追加メソッド
    addList: function(){
      // 値渡し
      this.taskList.push({...this.inputTask});
      this.inputTask.content="";
    },

    // リスト対象要素削除メソッド
    removeList: function(removeIndex){
      this.taskList.splice(removeIndex,1);
    },

    // リスト要素全削除メソッド
    deleteList: function(){
      this.taskList.splice(0,this.taskList.length);
    },

    // 対象要素に編集フラグ立てるメソッド
    editIndex: function(index){
      this.taskList[index].isEdit = true;
    },

    changeRadio: function(value){
      this.displayMode = value;
    },

  },
  computed: {
    // ラジオボタンによって画面に出力リストを出し分け
    displayTaskList() {
      if(this.displayMode == 0){
        return this.taskList;
      }else if(this.displayMode == 1){
        return this.taskList.filter(tl => tl.checked);
      }else if(this.displayMode == 2){
        return this.taskList.filter(tl => !tl.checked);
      }
    },
    // 追加ボタン非活性判定
    isExistInputText() {
      return this.inputTask.content == "";
    },
    // 全削除ボタン非活性判定
    isExistTaskList() {
      return this.taskList.length == 0;
    }
  },
  components:{
    SelectRadio,
  },
};
</script>

<style>
ul {
  margin: 0;
  padding: 0;
  list-style-type: none;
}

ul > li {
  margin: 5px;
  text-indent: 0;
}

#to-do {
  width: 800px;
  margin: 0 auto;
}
</style>
