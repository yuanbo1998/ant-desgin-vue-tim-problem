<template>
  <a-modal v-model="childVisible" title="Title" @ok="handleOk" @cancel="handleCancel">
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
</template>

<script>
export default {
  name: "MyModal",
  props:[
      'childVisible'
  ],
  data(){
    return {
      loading:false
    }
  },
  methods:{
    handleOk(){
      console.log(" 我是子级ok ");
      this.$emit("closeChild");
    },
    handleCancel(){
      console.log(" 我是子级cancel ");
      this.$emit("closeChild");
    }

  }
}
</script>

<style scoped>

</style>