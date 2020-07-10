<template>
  <a-tooltip title="列设置" :getPopupContainer="getPopupContainer">
    <a-icon type="setting" class="fullscreen" :style="iconStyle" @click="togglePopover" />
    <a-popover v-model="visiblePopover" placement="bottom" trigger="click" :getPopupContainer="getPopupContainer">
      <template #title>
        <a-checkbox :indeterminate="indeterminate" :checked="checkAll" @change="checkAllChange">列展示</a-checkbox>
        <a @click="resetColumns">重置</a>
      </template>
      <template #content>
        <a-checkbox-group v-model="checkedList" @change="checkChange">
          <a-row>
            <a-col v-for="(column,index) in columns" :key="column.dataIndex">
              <a-checkbox :value="index" @change="onChange">{{ column.title }}</a-checkbox>
            </a-col>
          </a-row>
        </a-checkbox-group>
      </template>
    </a-popover>
  </a-tooltip>
</template>

<script>
import Vue from 'vue'

export default {
  name: 'CustomColumns',
  data () {
    return {
      checkedList: [],
      visiblePopover: false,
      indeterminate: false,
      checkAll: true,
    }
  },
  props: {
    name: {
      type: String,
      default: ''
    },
    iconStyle: {
      type: Object,
      default: function () {
        return {}
      },
    },
    columns: {
      type: Array,
      required: true
    }
  },
  model: {
    prop: 'columns',
    event: 'update'
  },
  created () {
    const columns = Vue.ls.get(this.$route.name + this.name + 'Columns', this.columns)
    // console.log('created1', columns)
    // console.log('created11', this.columns, columns)
    this.plainOptions = [...columns.keys()]
    this.checkedList = columns.map((item, index) => {
      if (!item.hidden) {
        return index
      }
    })
    this.$emit('update', columns)
    // console.log('created2', this.plainOptions, this.checkedList)
  },
  watch: {
    columns: function (newV, oldV) {
      // console.log('watch1', newV)
      Vue.ls.set(this.$route.name + this.name + 'Columns', newV)
      this.$emit('update', newV)
    }
  },
  methods: {
    togglePopover () {
      this.visiblePopover = !this.visiblePopover
    },
    getPopupContainer (trigger) {
      return trigger.parentElement
    },
    resetColumns () {
      // console.log('resetColumns')
      this.checkedList = this.plainOptions
      this.checkAll = true
      this.indeterminate = false
      this.toggleHidden(false)
    },
    onChange (e) {
      // console.log('onChange', e.target)
      this.$set(this.columns[e.target.value], 'hidden', !e.target.checked)
    },
    checkChange (checkedList) {
      // console.log('checkChange', checkedList, this.checkedList)
      this.indeterminate = !!checkedList.length && checkedList.length < this.columns.length
    },
    checkAllChange (e) {
      // console.log('checkAllChange', e)
      this.checkedList = e.target.checked ? this.plainOptions : []
      this.indeterminate = false
      this.checkAll = e.target.checked
      this.toggleHidden(!this.checkAll)
    },
    toggleHidden (hidden) {
      const arr = []
      this.columns.forEach((item, index) => {
        item.hidden = hidden
        arr.push(item)
        // this.columns[index].hidden = !this.checkedList.includes(index)
        // this.$set(this.columns[index], 'hidden', !this.checkedList.includes(index))
      })
      this.$emit('update', arr)
      // console.log('toggle', arr)
    }
  }
}
</script>

<style scoped>
.fullscreen {
  font-size: 20px;
  margin: auto 8px;
  padding: 2px;
}
</style>
