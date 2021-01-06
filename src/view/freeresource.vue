<template>
  <div>
    <Alert>最近一次巡检时间: {{inspectorTime}} 当前正在执行的任务数量{{taskIdLength}} 任务ID: {{taskId}}</Alert>
    <Button size="large" @click="inspectNow">立即巡检</Button>
    <Button size="large" @click="download">下载报告</Button>
    <div style="width: 100%;height: 10px"></div>
    <Row gutter="5">
      <Col span="7">
        <Card style="width:350px;height: 100px">
          <img style="width:50px;height:50px;float: left;margin-right: 5px" src="../assets/icons/core_api.svg">
          <div style="text-align:left;float: left">
            <h2>DRDS</h2>
            <span style="margin-right: 10px">数量: {{drdsTotal}}</span>
            <span style="margin-right: 10px">异常: {{drdsException}}</span>
          </div>
        </Card>
      </Col>
      <Col span="7">
        <Card style="width:350px;height: 100px">
          <img style="width:50px;height:50px;float: left;margin-right: 5px" src="../assets/icons/core_api.svg">
          <div style="text-align:left;float: left">
            <h2>RDS</h2>
            <span style="margin-right: 10px">数量: {{rdsTotal}}</span>
            <span style="margin-right: 10px">异常: {{rdsException}}</span>
          </div>
        </Card>
      </Col>
      <Col span="7">
        <Card style="width:350px;height: 100px">
          <img style="width:50px;height:50px;float: left;margin-right: 5px" src="../assets/icons/core_api.svg">
          <div style="text-align:left;float: left">
            <h2>Redis</h2>
            <span style="margin-right: 10px">数量: {{redisTotal}}</span>
            <span style="margin-right: 10px">异常: {{redisException}}</span>
          </div>
        </Card>
      </Col>
    </Row>
    <div style="width: 100%;height: 10px"></div>
    <Row gutter="5">
      <Col span="7">
        <Card style="width:350px;height: 100px">
          <img style="width:50px;height:50px;float: left;margin-right: 5px" src="../assets/icons/core_api.svg">
          <div style="text-align:left;float: left">
            <h2>SLB</h2>
            <span style="margin-right: 10px">数量: {{slbTotal}}</span>
            <span style="margin-right: 10px">异常: {{slbException}}</span>
          </div>
        </Card>
      </Col>
    </Row>
    <div style="width: 100%;height: 10px"></div>
    <Collapse simple>
      <Panel name="1" hide-arrow>
        <img style="width:20px;height:20px;float: left;margin-top: 5px;margin-right: 10px" src="../assets/icons/Dislike.svg">
        DRDS巡检有问题的项({{drdsData1Length}})
        <Row slot="content">
          <Table width="100%" height="200" border :columns="drdsColumns" :data="drdsData1"></Table>
        </Row>
      </Panel>
      <Panel name="2" hide-arrow>
        <img style="width:20px;height:20px;float: left;margin-top: 5px;margin-right: 10px" src="../assets/icons/Dislike.svg">
        RDS巡检有问题的项({{rdsData1Length}})
        <Row slot="content">
          <Table width="100%" height="200" border :columns="rdsColumns" :data="rdsData1"></Table>
        </Row>
      </Panel>
      <Panel name="3" hide-arrow>
        <img style="width:20px;height:20px;float: left;margin-top: 5px;margin-right: 10px" src="../assets/icons/Dislike.svg">
        Redis巡检有问题的项({{redisData1Length}})
        <Row slot="content">
          <Table width="100%" height="200" border :columns="redisColumns" :data="redisData1"></Table>
        </Row>
      </Panel>
      <Panel name="4" hide-arrow>
        <img style="width:20px;height:20px;float: left;margin-top: 5px;margin-right: 10px" src="../assets/icons/Dislike.svg">
        SLB巡检有问题的项({{slbData1Length}})
        <Row slot="content">
          <Table width="100%" height="200" border :columns="slbColumns" :data="slbData1"></Table>
        </Row>
      </Panel>
    </Collapse>
  </div>
</template>

<script>
export default {
  name: 'freeresource',
  data () {
    return {
      inspectorTime: '2020-12-22 12:16:00',
      drdsTotal: 0,
      drdsException: 0,
      rdsTotal: 0,
      rdsException: 0,
      redisTotal: 0,
      redisException: 0,
      slbTotal: 0,
      slbException: 0,
      drdsColumns: [
        {
          title: '域名',
          key: 'domain'
        },
        {
          title: '证书',
          key: 'cert_name'
        },
        {
          title: '过期时间',
          key: 'expire_date'
        },
        {
          title: '剩余天数',
          key: 'remainder_day'
        }
      ],
      rdsColumns: [],
      redisColumns: [],
      slbColumns: [
        {
          title: 'ID',
          key: 'Id'
        },
        {
          title: '名称',
          key: 'name'
        },
        {
          title: '证书',
          key: 'cert_name'
        },
        {
          title: '过期时间',
          key: 'expire_date'
        },
        {
          title: '剩余天数',
          key: 'remainder_day'
        }
      ],
      drdsData1: [],
      drdsData1Length: 0,
      rdsData1: [],
      rdsData1Length: 0,
      redisData1: [],
      redisData1Length: 0,
      slbData1: [],
      slbData1Length: 0,
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
            'type': 'freeresource'
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
          'type': 'freeresource'
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
      window.location.href = `http://127.0.0.1:9000/life/?is_report=true`
    },
    init () {
      let url = '/inspector/freeresource/'
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
        this.drdsTotal = data[0]['drds_report']['total_count']
        this.drdsException = data[0]['drds_report']['abnormal_count']
        this.drdsData1 = data[0]['drds_report']['life_ssl_report']
        this.drdsData1Length = this.drdsData1.length
        this.rdsTotal = data[0]['rds_report']['total_count']
        this.rdsException = data[0]['rds_report']['abnormal_count']
        this.rdsData1 = data[0]['rds_report']['life_ssl_report']
        this.rdsData1Length = this.rdsData1.length
        this.redisTotal = data[0]['redis_report']['total_count']
        this.redisException = data[0]['redis_report']['abnormal_count']
        this.redisData1 = data[0]['redis_report']['life_ssl_report']
        this.redisData1Length = this.redisData1.length
        this.slbTotal = data[0]['slb_report']['total_count']
        this.slbException = data[0]['slb_report']['abnormal_count']
        this.slbData1 = data[0]['slb_report']['life_ssl_report']
        this.slbData1Length = this.slbData1.length
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
