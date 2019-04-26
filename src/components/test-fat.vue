<template>
  <div>
    <com-haomeili>
      <p slot="jinmao" slot-scope="recData">
        <span>我的土金毛叫{{recData.ume}}
            ,她今年{{recData.age}}岁了,
            她的性别是{{recData.gender}},
            爱好是{{recData.hobby}}
            </span>
      </p>
    </com-haomeili>
  </div>
</template>

<script>
import Son from './test-son'
export default {
  components: {
    'com-haomeili': 'Son'
  }
}
</script>

<style lang="less" scoped>
</style>
