<template>
    <div>
      <h2>巡检日报订阅</h2>
      <div style="width: 100%;height: 5px"></div>
      <span>每天接收巡检报告，内容包含，实时风险汇总、风险详情和治愈建议等，便于对当前风险的跟进解决，避免风险扩大。</span>
      <div style="width: 100%;height: 5px"></div>
      <Row gutter="5">
        <Col span="6">
          <Card style="width:100%">
            <div style="text-align:left">
              <h3>订阅巡检日报</h3>
              <div style="width: 100%;height: 5px"></div>
              <div style="font-size: small">订阅后，每天接收巡检详情报告。</div>
            </div>
          </Card>
        </Col>
        <Col span="18">
          <Card style="width:100%;float: right;height: 100%">
            <Row>
              <Col span="8">
                <h3>定时发送</h3>
                <div style="width: 100%;height: 5px"></div>
                <div style="font-size: small">设置后，报告将按照设定时间定时发送。</div>
              </Col>
              <Col span="8">
                <span style="width: 20%;margin-right: 10px">时间(每天):</span>
                <TimePicker :value="timeValue" type="time" placeholder="Select time" style="width: 168px;"></TimePicker>
              </Col>
            </Row>
            <div style="float: right;width: 60%">

            </div>
          </Card>
        </Col>
      </Row>
      <div style="width: 100%;height: 5px"></div>
      <h2>接收邮件</h2>
      <div style="width: 100%;height: 5px"></div>
      <Row gutter="5">
        <Col span="24">
<!--          <Card style="width:320px">-->
            <div style="text-align:left">
              <Button type="primary" shape="circle" @click="modal6 = true">新增邮箱</Button>
              <Modal
                v-model="modal6"
                title="新增邮箱"
                :loading="loading"
                @on-ok="asyncOK">
                <Input v-model="newMailBox" placeholder="请输入邮箱名" style="width: 300px" />
              </Modal>
              <div style="width: 100%;height: 5px"></div>
              <CheckboxGroup v-model="mailbox" style="text-align: left;font-size: x-large">
                <Checkbox label="y_pe@list.alibaba-inc.com">y_pe@list.alibaba-inc.com</Checkbox>
                <Checkbox label="jianweng.ly@alibab-inc.com">jianweng.ly@alibab-inc.com</Checkbox>
              </CheckboxGroup>
            </div>
<!--          </Card>-->
        </Col>
      </Row>
    </div>
</template>

<script>
export default {
  name: 'config',
  data () {
    return {
      switch1: false,
      timeValue: '11:00:00',
      modal6: false,
      loading: true,
      mailbox: ['y_pe@list.alibaba-inc.com', 'jianweng.ly@alibab-inc.com'],
      newMailBox: ''
    }
  },
  methods: {
    asyncOK () {
      setTimeout(() => {
        this.modal6 = false
      }, 2000)
    },
    change (status) {
      this.$Message.info('开关状态：' + status)
    }
  }
}
</script>

<style scoped>

</style>
