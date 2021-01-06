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
            <h2>ECS</h2>
            <span style="margin-right: 10px">数量: {{ecsTotal}}</span>
            <span style="margin-right: 10px">异常: {{ecsException}}</span>
          </div>
        </Card>
      </Col>
      <Col span="7">
        <Card style="width:350px;height: 100px">
          <img style="width:50px;height:50px;float: left;margin-right: 5px" src="../assets/icons/core_api.svg">
          <div style="text-align:left;float: left">
            <h2>EIP</h2>
            <span style="margin-right: 10px">数量: {{eipTotal}}</span>
            <span style="margin-right: 10px">异常: {{eipException}}</span>
          </div>
        </Card>
      </Col>
      <Col span="7">
        <Card style="width:350px;height: 100px">
          <img style="width:50px;height:50px;float: left;margin-right: 5px" src="../assets/icons/core_api.svg">
          <div style="text-align:left;float: left">
            <h2>NAT</h2>
            <span style="margin-right: 10px">数量: {{natTotal}}</span>
            <span style="margin-right: 10px">异常: {{natException}}</span>
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
            <h2>CDN</h2>
            <span style="margin-right: 10px">数量: {{cdnTotal}}</span>
            <span style="margin-right: 10px">异常: {{cdnException}}</span>
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
            <h2>MongoDB</h2>
            <span style="margin-right: 10px">数量: {{mongodbTotal}}</span>
            <span style="margin-right: 10px">异常: {{mongodbException}}</span>
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
            <h2>redis standard</h2>
            <span style="margin-right: 10px">数量: {{redisStandardTotal}}</span>
            <span style="margin-right: 10px">异常: {{redisStandardException}}</span>
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
            <h2>redis cluster</h2>
            <span style="margin-right: 10px">数量: {{redisClusterTotal}}</span>
            <span style="margin-right: 10px">异常: {{redisClusterException}}</span>
          </div>
        </Card>
      </Col>
    </Row>
    <div style="width: 100%;height: 10px"></div>
    <Collapse simple>
      <Panel name="1" hide-arrow>
        <img style="width:20px;height:20px;float: left;margin-top: 5px;margin-right: 10px" src="../assets/icons/Dislike.svg">
        ECS巡检有问题的项({{ecsData1Length}})
        <Row slot="content">
          <Table width="100%" height="200" border :columns="columns" :data="ecsData1"></Table>
        </Row>
      </Panel>
      <Panel name="2" hide-arrow>
        <img style="width:20px;height:20px;float: left;margin-top: 5px;margin-right: 10px" src="../assets/icons/Dislike.svg">
        CDN巡检有问题的项({{cdnData1Length}})
        <Row slot="content">
          <Table width="100%" height="200" border :columns="columns" :data="cdnData1"></Table>
        </Row>
      </Panel>
      <Panel name="3" hide-arrow>
        <img style="width:20px;height:20px;float: left;margin-top: 5px;margin-right: 10px" src="../assets/icons/Dislike.svg">
        SLB巡检有问题的项({{slbData1Length}})
        <Row slot="content">
          <Table width="100%" height="200" border :columns="columns" :data="slbData1"></Table>
        </Row>
      </Panel>
      <Panel name="4" hide-arrow>
        <img style="width:20px;height:20px;float: left;margin-top: 5px;margin-right: 10px" src="../assets/icons/Dislike.svg">
        MongoDB巡检有问题的项({{mongodbData1Length}})
        <Row slot="content">
          <Table width="100%" height="200" border :columns="columns" :data="mongodbData1"></Table>
        </Row>
      </Panel>
      <Panel name="5" hide-arrow>
        <img style="width:20px;height:20px;float: left;margin-top: 5px;margin-right: 10px" src="../assets/icons/Dislike.svg">
        EIP巡检有问题的项({{eipData1Length}})
        <Row slot="content">
          <Table width="100%" height="200" border :columns="columns" :data="eipData1"></Table>
        </Row>
      </Panel>
      <Panel name="6" hide-arrow>
        <img style="width:20px;height:20px;float: left;margin-top: 5px;margin-right: 10px" src="../assets/icons/Dislike.svg">
        NAT巡检有问题的项({{natData1Length}})
        <Row slot="content">
          <Table width="100%" height="200" border :columns="natColumns" :data="natData1"></Table>
        </Row>
      </Panel>
      <Panel name="7" hide-arrow>
        <img style="width:20px;height:20px;float: left;margin-top: 5px;margin-right: 10px" src="../assets/icons/Dislike.svg">
        DRDS巡检有问题的项({{drdsData1Length}})
        <Row slot="content">
          <Table width="100%" height="200" border :columns="columns" :data="drdsData1"></Table>
        </Row>
      </Panel>
      <Panel name="8" hide-arrow>
        <img style="width:20px;height:20px;float: left;margin-top: 5px;margin-right: 10px" src="../assets/icons/Dislike.svg">
        Redis Standard巡检有问题的项({{redisStandardData1Length}})
        <Row slot="content">
          <Table width="100%" height="200" border :columns="columns" :data="redisStandardData1"></Table>
        </Row>
      </Panel>
      <Panel name="9" hide-arrow>
        <img style="width:20px;height:20px;float: left;margin-top: 5px;margin-right: 10px" src="../assets/icons/Dislike.svg">
        Redis Cluster巡检有问题的项({{redisClusterData1Length}})
        <Row slot="content">
          <Table width="100%" height="200" border :columns="columns" :data="redisClusterData1"></Table>
        </Row>
      </Panel>
      <Panel name="10" hide-arrow>
        <img style="width:20px;height:20px;float: left;margin-top: 5px;margin-right: 10px" src="../assets/icons/Dislike.svg">
        RDS巡检有问题的项({{rdsData1Length}})
        <Row slot="content">
          <Table width="100%" height="200" border :columns="columns" :data="rdsData1"></Table>
        </Row>
      </Panel>
    </Collapse>
  </div>
</template>

<script>
export default {
  name: 'coverage',
  data () {
    return {
      inspectorTime: '2020-12-22 12:16:00',
      cdnTotal: 0,
      cdnException: 0,
      rdsTotal: 0,
      rdsException: 0,
      redisStandardTotal: 0,
      redisStandardException: 0,
      redisClusterTotal: 0,
      redisClusterException: 0,
      ecsTotal: 0,
      ecsException: 0,
      slbTotal: 0,
      slbException: 0,
      eipTotal: 0,
      eipException: 0,
      natTotal: 0,
      natException: 0,
      drdsTotal: 0,
      drdsException: 0,
      mongodbTotal: 0,
      mongodbException: 0,
      columns: [
        {
          title: 'ID',
          key: 'id'
        },
        {
          title: 'Name',
          key: 'name'
        },
        {
          title: 'Message',
          key: 'message'
        }
      ],
      cdnColumns: [
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
      ecsColumns: [],
      rdsColumns: [],
      mongodbColumns: [],
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
      eipColumns: [
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
      natColumns: [
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
      redisStandardColumns: [
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
      redisClusterColumns: [
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
      cdnData1: [],
      cdnData1Length: 0,
      ecsData1: [],
      ecsData1Length: 0,
      rdsData1: [],
      rdsData1Length: 0,
      redisStandardData1: [],
      redisStandardData1Length: 0,
      slbData1: [],
      slbData1Length: 0,
      drdsData1: [],
      drdsData1Length: 0,
      redisClusterData1: [],
      redisClusterData1Length: 0,
      eipData1: [],
      eipData1Length: 0,
      natData1: [],
      natData1Length: 0,
      mongodbData1: [],
      mongodbData1Length: 0,
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
            'type': 'coverage'
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
          'type': 'coverage'
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
      let url = '/inspector/coverage/'
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
        this.cdnTotal = data[0]['CdnReport']['total_count']
        this.cdnException = data[0]['CdnReport']['abnormal_count']
        this.cdnData1 = data[0]['CdnReport']['scan_reports']
        this.cdnData1Length = this.cdnData1.length
        this.rdsTotal = data[0]['rds_report']['total_count']
        this.rdsException = data[0]['rds_report']['abnormal_count']
        this.rdsData1 = data[0]['rds_report']['scan_reports']
        this.rdsData1Length = this.rdsData1.length
        this.redisStandardTotal = data[0]['RedisStandReport']['total_count']
        this.redisStandardException = data[0]['RedisStandReport']['abnormal_count']
        this.redisStandardData1 = data[0]['RedisStandReport']['scan_reports']
        this.redisStandardData1Length = this.redisStandardData1.length
        this.redisClusterTotal = data[0]['RedisClusterReport']['total_count']
        this.redisClusterException = data[0]['RedisClusterReport']['abnormal_count']
        this.redisClusterData1 = data[0]['RedisClusterReport']['scan_reports']
        this.redisClusterData1Length = this.redisClusterData1.length
        this.slbTotal = data[0]['SlbReport']['total_count']
        this.slbException = data[0]['SlbReport']['abnormal_count']
        this.slbData1 = data[0]['SlbReport']['scan_reports']
        this.slbData1Length = this.slbData1.length
        this.eipTotal = data[0]['EipReport']['total_count']
        this.eipException = data[0]['EipReport']['abnormal_count']
        this.eipData1 = data[0]['EipReport']['scan_reports']
        this.eipData1Length = this.eipData1.length
        this.natTotal = data[0]['NatReport']['total_count']
        this.natException = data[0]['NatReport']['abnormal_count']
        this.natData1 = data[0]['NatReport']['scan_reports']
        this.natData1Length = this.natData1.length
        this.drdsTotal = data[0]['DrdsReport']['total_count']
        this.drdsException = data[0]['DrdsReport']['abnormal_count']
        this.drdsData1 = data[0]['DrdsReport']['scan_reports']
        this.drdsData1Length = this.drdsData1.length
        this.mongodbTotal = data[0]['MongoDbReport']['total_count']
        this.mongodbException = data[0]['MongoDbReport']['abnormal_count']
        this.mongodbData1 = data[0]['MongoDbReport']['scan_reports']
        this.mongodbData1Length = this.mongodbData1.length
        this.ecsTotal = data[0]['EcsReport']['total_count']
        this.ecsException = data[0]['EcsReport']['abnormal_count']
        this.ecsData1 = data[0]['EcsReport']['scan_reports']
        this.ecsData1Length = this.ecsData1.length
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
