<template>
    <div>
      <Alert>最近一次巡检时间: {{inspectorTime}}  当前正在执行的任务数量{{taskIdLength}} 任务ID: {{taskId}}</Alert>
      <Button size="large" @click="inspectNow">立即巡检</Button>
      <Button size="large" @click="download">下载报告</Button>
      <div style="width: 100%;height: 10px"></div>
      <Row>
        <Col span="7">
          <Card style="width:350px;height: 100px">
            <img style="width:50px;height:50px;float: left;margin-right: 5px" src="../assets/icons/core_api.svg">
            <div style="text-align:left;float: left">
              <h2>核心接口</h2>
              <span style="margin-right: 10px">接口数量({{core}})</span>
<!--              <span>巡检项问题</span>-->
            </div>
          </Card>
        </Col>
        <Col span="7">
          <Card style="width:350px;height: 100px">
            <img style="width:50px;height:50px;float: left;margin-right: 5px" src="../assets/icons/semi_core_api.svg">
            <div style="text-align:left">
              <h2>次核心接口</h2>
              <span style="margin-right: 10px">接口数量({{semiCore}})</span>
<!--              <span>巡检项问题</span>-->
            </div>
          </Card>
        </Col>
        <Col span="7">
          <Card style="width:350px;height: 100px">
            <img style="width:50px;height:50px;float: left;margin-right: 5px" src="../assets/icons/non_core_api.svg">
            <div style="text-align:left">
              <h2>非核心接口</h2>
              <span style="margin-right: 10px">接口数量({{nonCore}})</span>
<!--              <span>巡检项问题</span>-->
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
            <Table :row-class-name="rowClassName" width="100%" height="200" border :columns="columns" :data="data1"></Table>
          </Row>
        </Panel>
        <Panel name="2" hide-arrow>
          <img style="width:20px;height:20px;float: left;margin-top: 5px;margin-right: 10px" src="../assets/icons/Like.svg">
          巡检未发现问题的项({{data2Length}})
          <Row slot="content">
            <Table width="100%" height="200" border :columns="columns" :data="data2"></Table>
          </Row>
        </Panel>
      </Collapse>
    </div>
</template>

<script>
export default {
  name: 'limit',
  data () {
    return {
      inspectorTime: '2020-12-21 15:43',
      core: 0,
      semiCore: 0,
      nonCore: 0,
      columns: [
        {
          title: '接口',
          key: 'api',
          width: 200,
          fixed: 'left'
        },
        {
          title: '应用',
          key: 'application_name',
          width: 150
        },
        {
          title: '业务类型',
          key: 'business_type',
          width: 100
        },
        {
          title: '限流总值',
          key: 'max_limit_total',
          width: 100
        },
        {
          title: '限流百分比(%)',
          key: 'percent',
          sortable: true,
          width: 150
        },
        {
          title: '机器数量',
          key: 'machine_count',
          width: 100
        },
        {
          title: '机器类型',
          key: 'machine_type',
          width: 100
        },
        {
          title: '核心类型',
          key: 'is_core',
          sortable: true,
          width: 120
        },
        {
          title: '接口负责人',
          key: 'owner',
          width: 100
        },
        {
          title: 'ACM命名空间',
          key: 'acm_ns_name',
          width: 200
        },
        {
          title: 'ACM GROUP',
          key: 'acm_group',
          width: 200
        },
        {
          title: 'ACM DATA ID',
          key: 'acm_data_id',
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
            'type': 'currentlimit'
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
          'type': 'currentlimit'
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
    init () {
      let url = '/inspector/limit/'
      this.$axios({
        url: url,
        method: 'get'
      }).then(res => {
        this.$Notice.success({
          title: 'Success',
          desc: '获取数据成功'
        })
        let { data } = res
        this.core = data[0]['core_num']
        this.semiCore = data[0]['semi_core_num']
        this.nonCore = data[0]['non_core_num']
        this.inspectorTime = data[0]['ts']
        this.data1 = data[0]['abnormal']
        this.data1Length = this.data1.length
        this.data2 = data[0]['normal']
        this.data2Length = this.data2.length
      }).catch(err => {
        this.$Notice.error({
          title: 'Error',
          desc: `获取数据失败${err}`
        })
      })
    },
    download () {
      window.location.href = `http://127.0.0.1:9000/limit/?is_report=true`
    },
    rowClassName (row, index) {
      // if (parseFloat(row['percent']) >= 40.0 && parseFloat(row['percent']) < 80.0) {
      //   return 'demo-table-info-row'
      // } else if (parseFloat(row['percent']) >= 80.0) {
      //   return 'demo-table-error-row'
      // }
      // return ''
      if (index === 1) {
        return 'demo-table-info-row'
      } else if (index === 3) {
        return 'demo-table-error-row'
      }
      return ''
    }
  }
}
</script>

<style scoped>
  .ivu-table .demo-table-info-row td{
    background-color: #2db7f5;
    color: #fff;
  }
  .ivu-table .demo-table-error-row td{
    background-color: #ff6600;
    color: #fff;
  }
  .ivu-table td.demo-table-info-column{
    background-color: #2db7f5;
    color: #fff;
  }
  .ivu-table .demo-table-info-cell-name {
    background-color: #2db7f5;
    color: #fff;
  }
  .ivu-table .demo-table-info-cell-age {
    background-color: #ff6600;
    color: #fff;
  }
  .ivu-table .demo-table-info-cell-address {
    background-color: #187;
    color: #fff;
  }
  .demo-badge{
    /*width: 42px;*/
    /*height: 42px;*/
    /*background: #eee;*/
    border-radius: 6px;
    display: inline-block;
  }
</style>
