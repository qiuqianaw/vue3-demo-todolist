<template>
  <div class="container" @mousedown="mouseDown">
    <h1>Todo</h1>
    <h3>
      <span class="text-success">{{ finish.length }}</span> /
      <span class="text-primary">{{ lists.length }}</span>
    </h3>
    <h3>未完成列表</h3>
    <ul class="list-group">
      <template v-for="(item, index) in lists">
        <li
          :key="index"
          class="list-group-item d-flex justify-content-between"
          v-if="item.checked == false"
        >
          <div class="form-group form-check mb-0">
            <input
              @click="() => (item.checked = !item.checked)"
              type="checkbox"
              class="form-check-input"
              v-model="item.checked"
            />
            <label
              class="form-check-label"
              v-if="item.isEdit == false"
              @dblclick="showEdit(item, index)"
              >{{ item.name }}</label
            >
            <label class="form-check-label" :for="'item-' + index" v-else>
              <input type="text" v-model="editValue" ref="myinput" />
            </label>
          </div>
          <button
            type="button"
            class="close"
            aria-label="Close"
            @click="remove(index)"
          >
            <span aria-hidden="true">&times;</span>
          </button>
        </li>
      </template>
    </ul>
    <h3>已完成列表</h3>
    <ul class="list-group">
      <li
        class="list-group-item"
        v-for="(item, index) in finish"
        :key="'finished-' + index"
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
    <h3>add new tast</h3>
    <div class="form-group">
      <label for="add">add</label>
      <input
        type="text"
        class="form-control"
        id="add"
        placeholder="new task"
        v-model="value"
        @keydown.enter="add()"
      />
    </div>
    <button
      class="btn btn-primary btn-lg btn-block"
      type="button"
      @click="add()"
    >
      确定添加
    </button>
  </div>
</template>

<script>
import { ref, reactive, toRefs, computed } from "vue";
export default {
  name: "HomeView",
  components: {},
  setup() {
    const myinput = ref(null);
    let editIndex = 0;
    const add = () => {
      state.lists.push({
        name: state.value,
        checked: false,
        isEdit: false,
      });
      state.value = "";
    };
    const showEdit = (item, index) => {
      console.log("item, index ->", { item, index });
      editIndex = index;
      item.isEdit = true;
      state.editValue = item.name;
    };
    const mouseDown = (e) => {
      console.log("mouseDown -> e", e);
      console.log("myinput -> ", myinput);
      if (myinput.value && e.target != myinput.value[0]) {
        // debugger;
        state.lists[editIndex] = {
          name: state.editValue,
          checked: false,
          isEdit: false,
        };
      }
    };
    const remove = (index) => {
      console.log("remove index -> ", index);
      state.lists.splice(index, 1);
    };
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
      finish: computed(() =>
        state.lists.filter((item) => item.checked == true)
      ),

      // add,
      // showEdit,
    });
    return { ...toRefs(state), add, showEdit, myinput, mouseDown, remove };
  },
};
</script>
