# CustomColumns 自定义table显示字段
### 配合ant-design-vue table组件


代码示例：
```
<CustomColumns v-model="columns" name="testName" />
```

```javascript
import CustomColumns from '@/components/CustomColumns'

const columns = [
  {
    title: '结算单号',
    dataIndex: 'no',
    align: 'center'
  },
  {
    title: '市场',
    dataIndex: 'shipmentshopid',
    align: 'center'
  }
]
export default {
    components: {
        CustomColumns
    },
    data () {
        return {
            columns,
        }
    }
}
```



## API


参数 | 说明 | 类型 | 默认值
----|------|-----|------
name | 命名标识,作为缓存的部分名称,一个页面中有多个该组件时必须命名(缓存名=this.$route.name + this.name + 'Columns') | String | ''
iconStyle | 图标样式 | Object | {}
columns | 必填,table 的 columns属性,必须有title和dataIndex属性 | Array | -