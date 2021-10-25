<template>
  <div>

    <a-table :columns="columns" :data-source="data">
      <a slot="name" slot-scope="text">
        <a-input @focus.prevent="toFocus" @blur="toBlur" :value="text"></a-input>
        <a-input @focus="showModal" :value="'parentModal'"></a-input>
        <span @click="toFocus" >{{text}}</span>

      </a>
      <span slot="customTitle"><a-icon type="smile-o"/> Name</span>
      <span slot="tags" slot-scope="tags">
      <a-tag
          v-for="tag in tags"
          :key="tag"
          :color="tag === 'loser' ? 'volcano' : tag.length > 5 ? 'geekblue' : 'green'"
      >
        {{ tag.toUpperCase() }}
      </a-tag>
    </span>
      <span slot="action" slot-scope="text, record">
      <a>Invite 一 {{ record.name }}</a>
      <a-divider type="vertical"/>
      <a>Delete</a>
      <a-divider type="vertical"/>
      <a class="ant-dropdown-link"> More actions <a-icon type="down"/> </a>
    </span>
    </a-table>

    <my-modal @closeChild="parentToCloseChild" :child-visible="childVisible"></my-modal>
    <a-input ref="toFocus1" @focus.prevent="tmpFocus"></a-input>
    <div>
      <a-button type="primary" @click="showModal">
        Open Modal with customized footer
      </a-button>
      <a-modal v-model="visible" title="Title" on-ok="handleOk">
        <template slot="footer">
          <a-button key="back" @click="handleCancel">
            Return
          </a-button>
          <a-button key="submit" type="primary" :loading="loading" @click="handleOk">
            Submit
          </a-button>
        </template>
        <p>Some contents...</p>
        <p>Some contents...</p>
        <p>Some contents...</p>
        <p>Some contents...</p>
        <p>Some contents...</p>
      </a-modal>
    </div>
  </div>

</template>
<script>
import MyModal from "./MyModal";

const columns = [
  {
    dataIndex: 'name',
    key: 'name',
    slots: {title: 'customTitle'},
    scopedSlots: {customRender: 'name'},
  },
  {
    title: 'Age',
    dataIndex: 'age',
    key: 'age',
  },
  {
    title: 'Address',
    dataIndex: 'address',
    key: 'address',
  },
  {
    title: 'Tags',
    key: 'tags',
    dataIndex: 'tags',
    scopedSlots: {customRender: 'tags'},
  },
  {
    title: 'Action',
    key: 'action',
    scopedSlots: {customRender: 'action'},
  },
];

const data = [
  {
    key: '1',
    name: 'John Brown',
    age: 32,
    address: 'New York No. 1 Lake Park',
    tags: ['nice', 'developer'],
  }
  ,
  {
    key: '2',
    name: 'Jim Green',
    age: 42,
    address: 'London No. 1 Lake Park',
    tags: ['loser'],
  },
  {
    key: '3',
    name: 'Joe Black',
    age: 32,
    address: 'Sidney No. 1 Lake Park',
    tags: ['cool', 'teacher'],
  },

];

export default {
  name: 'About',
  components: {MyModal},
  data() {
    return {
      data,
      columns,
      loading: false,
      visible: false,
      childVisible: false,
    };
  },
  methods: {
    showModal() {
      console.log("showModal")
      this.visible = true;
    },
    handleOk(e) {
      this.loading = true;
      setTimeout(() => {
        this.visible = false;
        this.loading = false;
      }, 3000);
    },
    handleCancel(e) {
      this.visible = false;
    },
    toFocus() {
      console.log("toFocus")
      //提交将焦点从本组件移开
      this.$refs.toFocus1.focus();
      this.childVisible = true;

    },
    parentToCloseChild() {
      console.log(" 我是父级cancel ");
      this.childVisible = false;
      this.$refs.toFocus1.focus();
    },
    toBlur(){
      console.log("我是焦点离开事件")
    },
    tmpFocus(){
      console.log("临时焦点")
    }
  }
};
</script>