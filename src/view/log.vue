<template>
    <div>
      <Alert>最近一次巡检时间: {{inspectorTime}} 当前正在执行的任务数量{{taskIdLength}} 任务ID: {{taskId}}</Alert>
      <Button size="large" @click="inspectNow">立即巡检</Button>
      <Button size="large" @click="download">下载报告</Button>
      <div style="width: 100%;height: 10px"></div>
      <Row>
        <Col span="7">
          <Card style="width:350px;height: 100px">
            <img style="width:50px;height:50px;float: left;margin-right: 5px" src="../assets/icons/core_api.svg">
            <div style="text-align:left;float: left">
              <h2>扫描文件总数</h2>
              <span style="margin-right: 10px">扫描量: {{total}}</span>
            </div>
          </Card>
        </Col>
        <Col span="7">
          <Card style="width:350px;height: 100px">
            <img style="width:50px;height:50px;float: left;margin-right: 5px" src="../assets/icons/semi_core_api.svg">
            <div style="text-align:left">
              <h2>文件请求数异常</h2>
              <span style="margin-right: 10px">异常数: {{exception}}</span>
            </div>
          </Card>
        </Col>
      </Row>
      <div style="width: 100%;height: 10px"></div>
      <Collapse simple>
        <Panel name="1" hide-arrow>
          <img style="width:20px;height:20px;float: left;margin-top: 5px;margin-right: 10px" src="../assets/icons/Dislike.svg">
          巡检有问题的项({{data1Length}})
          <Row slot="content">
            <Table width="100%" height="200" border :columns="columns" :data="data1"></Table>
          </Row>
        </Panel>
<!--        <Panel name="2" hide-arrow>-->
<!--          <img style="width:20px;height:20px;float: left;margin-top: 5px;margin-right: 10px" src="../assets/icons/Like.svg">-->
<!--          巡检未发现问题的项({{data2Length}})-->
<!--          <Row slot="content">-->
<!--            <Table width="100%" height="200" border :columns="columns" :data="data2"></Table>-->
<!--          </Row>-->
<!--        </Panel>-->
      </Collapse>
    </div>
</template>

<script>
export default {
  name: 'log',
  data () {
    return {
      inspectorTime: '2020-12-22 12:16:00',
      total: 0,
      exception: 0,
      columns: [
        {
          title: '域名',
          key: 'host',
          width: 400,
          fixed: 'left'
        },
        {
          title: '请求数',
          key: 'number',
          width: 200,
          sortable: true
        },
        {
          title: '请求地址',
          key: 'request_uri',
          width: 400
        },
        {
          title: 'Bucket名称',
          key: 'bucket',
          width: 200
        }
      ],
      data1: [],
      data1Length: 0,
      data2: [],
      data2Length: 0,
      taskId: 0,
      taskProgress: ''
    }
  },
  created () {
    this.init()
  },
  computed: {
    taskIdLength: function () {
      if (this.taskId === 0) {
        return 0
      } else {
        return 1
      }
    }
  },
  methods: {
    inspectNow () {
      let url = '/inspector/inspector/'
      // 检查task_id是否为0，以及task_id的任务是否完成
      if (this.taskId !== 0) {
        this.$Notice.warning({
          title: 'Warning',
          desc: '已有任务在进行'
        })
        this.$axios({
          url: url,
          method: 'get',
          params: {
            'type': 'log'
          }
        }).then(res => {
          let { data } = res
          if (data['progress'] === 'success' || data['progress'] === 'failure') {
            this.taskId = 0
          }
        }).catch(_ => {
          this.taskId = 0
        })
        return
      }
      this.$axios({
        url: url,
        method: 'post',
        data: {
          'type': 'log'
        }
      }).then(res => {
        let { data } = res
        this.taskId = data['task_id']
        this.taskProgress = data['progress']
      }).catch(_ => {
        this.$Notice.error({
          title: 'Error',
          desc: '立即巡检功能设置失败，请联系简翁(261902)'
        })
      })
    },
    download () {
      window.location.href = `http://127.0.0.1:9000/log/?is_report=true`
    },
    init () {
      let url = '/inspector/log/'
      this.$axios({
        url: url,
        method: 'get'
      }).then(res => {
        this.$Notice.success({
          title: 'Success',
          desc: '获取数据成功'
        })
        let { data } = res
        this.inspectorTime = data[0]['ts']
        this.total = data[0]['total_count']
        this.exception = data[0]['abnormal_count']
        let abnormal = data[0]['abnormal']
        for (let o in abnormal) {
          for (let i = 0; i < abnormal[o].length; i++) {
            abnormal[o][i]['bucket'] = o
            this.data1.push(abnormal[o][i])
          }
        }
        this.data1Length = this.data1.length
      }).catch(err => {
        this.$Notice.error({
          title: 'Error',
          desc: `获取数据失败${err}`
        })
      })
    }
  }
}
</script>

<style scoped>

</style>
