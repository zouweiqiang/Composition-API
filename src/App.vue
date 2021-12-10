<template>
  <div class="container" @mousedown="mousedown">
    <h1>ToDoList</h1>
    <h3>
      共有<span class="text-primary">{{ lists.length }}</span
      >个任务，已完成<span class="text-success">{{ finished.length }}</span
      >个任务
    </h3>
    <h3>未完成的列表</h3>
    <ul class="list-group">
      <template v-for="(item, index) in lists">
        <li class="list-group-item d-flex justify-content-between" :key="index" v-if="!item.checked">
          <div class="form-group form-check mb-0">
            <input
              type="checkbox"
              class="form-check-input"
              v-model="item.checked"
              @click="() => (item.checked = !item.checked)"
            />
            <label
              class="form-check-label"
              v-if="!item.isEdit"
              @dblclick="showEdit(item, index)"
              >{{ item.name }}</label
            >
            <label class="form-check-label" :for="'item-' + index" v-else
              ><input type="text" v-model="editValue" ref="myinput"
            /></label>
          </div>
          <button type="button" class="close" aria-label="Close" @click="remove(index)">
            <span aria-hidden="true">&times;</span>
          </button>

        </li>
      </template>
    </ul>
    <h3>已完成的列表</h3>
    <ul class="list-group">
      <li
        class="list-group-item"
        v-for="(item, index) in finished"
        :key="index"
        @click="() => (item.checked = !item.checked)"
      >
        <div class="form-group form-check">
          <input
            type="checkbox"
            class="form-check-input"
            :id="'finished-' + index"
            v-model="item.checked"
            disabled
          />
          <label class="form-check-label" :for="'finished-' + index">{{
            item.name
          }}</label>
        </div>
      </li>
    </ul>
    <h3>添加新的任务</h3>
    <div class="from-group">
      <label for="add">添加</label>
      <input
        type="text"
        class="form-control"
        id="add"
        placeholder="添加新的任务"
        v-model="value"
        @keydown.enter="add"
      />
      <button
        type="button"
        class="btn btn-primary btn-lg btn-block"
        @click="add"
      >
        添加
      </button>
    </div>
  </div>
</template>

<script>
import { reactive, toRefs, computed, ref } from "vue";
export default {
  name: "App",
  components: {},
  setup() {
    const myinput = ref(null);
    let editIndex = 0;
    // 1.加入checkbox  -- checked
    // 2.统计那些  --  checked  --  finish 列表
    // 3.add添加item  --  ite数据结构  --  那么，checked ，isEdit
    const add = () => {
      state.lists.push({
        name: state.value,
        checked: false,
        isEdit: false,
      });
      state.value = "";
    };
    // 4.双击进行编辑
    const showEdit = (item, index) => {
      editIndex = index;
      item.isEdit = true;
      state.editValue = item.name;
    };
    const mousedown = (e) => {
      if (myinput.value && e.target != myinput.value) {
        state.lists[editIndex] = {
          name: state.editValue,
          checked: false,
          isEdit: false,
        };
      }
    };
    // 5.删除功能  --  删除特定inde的元素
    const remove = (index) =>{
      console.log(index);
      console.log(state.lists[index]);
      state.lists.splice(index,1)

    }
    const state = reactive({
      value: "",
      editValue: "",
      lists: [
        {
          name: "1",
          checked: false,
          isEdit: false,
        },
        {
          name: "2",
          checked: false,
          isEdit: false,
        },
        {
          name: "3",
          checked: false,
          isEdit: false,
        },
      ],
      finished: computed(() =>
        state.lists.filter((item) => item.checked == true)
      ),
      add,
      showEdit,
      myinput,
      mousedown,
      remove
    });
    return toRefs(state);
  },
};
</script>

<style>
#app {
}
</style>
