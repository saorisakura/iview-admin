<template>
    <div>
      <Card style="width:100%">
        <div style="text-align:left">
          <Row>
            <Col span="3">
              <span style="font-weight: bolder;font-size: medium">最近一次巡检</span>
            </Col>
            <Col span="4">
              <span style="font-weight: lighter;font-size: x-small">巡检时间: {{inspectionTime}}</span>
            </Col>
          </Row>
        </div>
        <div style="width: 100%;height: 10px"></div>
        <div style="text-align:left">
          <Row>
            <Col span="4" style="padding-top: 5px">
              <span>巡检项目({{inspectorItems}}), 已识别风险</span>
            </Col>
            <Col span="4">
              <Tag color="error">错误 / 警告 {{errorNum}}</Tag>
            </Col>
          </Row>
        </div>
      </Card>
      <div style="width: 100%;height: 10px"></div>
      <Row>
        <Card style="width:100%">
          <span style="font-size: medium">巡检概览</span>
          <div style="width: 100%;height: 5px"></div>
          <div style="text-align:center">
            <Table stripe :columns="columns1" :data="data1"></Table>
          </div>
        </Card>
      </Row>
    </div>
</template>

<script>
export default {
  name: 'overview',
  data () {
    return {
      inspectionTime: '2020-12-21 17:41:00',
      inspectorItems: 4,
      errorNum: 0,
      columns1: [
        {
          title: '巡检项目',
          key: 'name'
        },
        {
          title: '风险: 严重/警告',
          key: 'abnormal'
        },
        {
          title: '巡检总数',
          key: 'total'
        },
        {
          title: '操作',
          key: 'operation',
          render: (h, params) => {
            return h('div', [
              h('Button', {
                props: {
                  type: 'text',
                  size: 'small'
                },
                style: {
                  marginRight: '5px'
                },
                on: {
                  click: () => {
                    this.show(params)
                  }
                }
              }, '查看详情')
            ])
          }
        }
      ],
      relation: {
        'limit': '接口限流值',
        'log': 'CDN OSS日志巡检',
        'coverage': '监控覆盖率',
        'life': '证书周期'
      },
      data1: []
    }
  },
  created () {
    this.init()
  },
  methods: {
    init () {
      this.data1 = []
      this.errorNum = 0
      let url = '/inspector/overview/'
      this.$axios({
        method: 'get',
        url: url
      }).then(res => {
        let { data } = res
        for (let o in data) {
          this.errorNum = this.errorNum + data[o]['abnormal']
          this.data1.push(
            {
              name: this.relation[o],
              abnormal: data[o]['abnormal'],
              total: data[o]['total']
            }
          )
        }
      })
    },
    show (params) {
      if (params.row.name === '接口限流值') {
        this.$router.push('limit/limit_page')
      } else if (params.row.name === '证书周期') {
        this.$router.push('life/life_page')
      } else if (params.row.name === '主机环境') {
        this.$router.push('systemenv/systemenv_page')
      } else if (params.row.name === '监控覆盖率') {
        this.$router.push('coverage/coverage_page')
      } else if (params.row.name === '空闲资源') {
        this.$router.push('freeresource/freeresource_page')
      } else if (params.row.name === '自动续费') {
        this.$router.push('renew/renew_page')
      } else if (params.row.name === 'CDN OSS日志巡检') {
        this.$router.push('log/log_page')
      }
    }
  }
}
</script>

<style scoped>

</style>
