# Vue 移动端弹框组件

由于公司项目多、设计的多样性，比如右上角要有关闭按钮，弹窗要有2个按钮或3个按钮或封面或图标等等需求设计，像Van、Vux之类使用人数多UI库，是可以满足，但是要自己在它的对话框组件上再封装一层来复用右上角关闭按钮或底部按钮或其它样式，显得太繁琐，所以干脆自己写个定制性强的基础组件出来，用来应复多样性的项目，如果有新的外包项目、新的需求或设计，在这组件代码基础上 `增删改` 即可。

## 效果演示
![image](https://github.com/1014156094/vue-mobile-dialog/blob/master/public/1.gif)

## 快速开始
```
<template>
  <div>
    <button @click="showDialog = true">
      显示对话框
    </button>
    <BaseDialog
      v-model="showDialog"
      title="标题"
      message="内容"
      :show-cancel-button="true"
      :show-close-button="true"
    />
  </div>
</template>

<script>
import BaseDialog from '@/components/base-dialog'

export default {
  components: {
    BaseDialog
  },
  data () {
    return {
      showDialog: false
    }
  }
}
</script>
```

## 二次封装示例
```
<template>
  <base-dialog class="dialog-address"
                v-model="show"
                title="温馨提示"
                cancelButtonText="再考虑下"
                :beforeClose="beforeCloseExchange">
    <template slot="message">
      <input type="text"
             placeholder="请填写收货地址"
             v-model="exchangeAddress" />
    </template>
  </base-dialog>
</template>

<script>
import BaseDialog from '@/components/base-dialog'

export default {
  mixins: [BaseDialog],
  data () {
    return {
      exchangeAddress: '' // 兑换地址
    }
  },
  watch: {
    show (newVal) {
      this.$emit('change', this.show)
    }
  },
  methods: {
    beforeCloseExchange (action, done) {
      if (action === 'confirm') {
        if (!this.exchangeAddress.trim() || !this.exchangeAddress) {
          this.$toast('请填写兑换地址！')
          return
        }
        this.$emit('confirmSuccess', {
          exchangeAddress: this.exchangeAddress
        })
      }
      done()
    }
  }
}
</script>
```

## 注意
组件CSS使用 `less` 预编译语言

## 插槽
| 参数 | 说明 |
| - | - |
| 默认 | 对话框主体 |
| title | 标题 |
| message | 消息 |
| buttons | 按钮 |

## Props
| 参数 | 说明 | 类型 | 默认值 |
| - | - | - | - |
| v-model | 是否显示对话框 | `Boolean` | `false` |
| title | 标题 | `String` | - |
| message | 消息 | `String` | - |
| show-close-button | 是否显示关闭按钮 | `Boolean` | `true` |
| show-cancel-button | 是否显示取消按钮 | `Boolean` | `false` |
| show-confirm-button | 是否显示确定按钮 | `Boolean` | `true` |
| cancel-button-text | 取消按钮文案 | `String` | `确定` |
| confirm-button-text | 确定按钮文案 | `String` | `取消` |
| close-on-click-overlay | 是否在点击蒙层后关闭 | `Boolean` | `false` |
| before-close |关闭前的回调函数 <br> 调用 done() 后关闭弹窗 <br> 调用 done(false) 阻止弹窗关闭| `(action, done) => void` | - |
