<template>
  <div class="container">
    <div class="left-nav">
      <img class="logo" src="../assets/logo.png" alt="1">
      <img src="../assets/icon.png" alt="1">
    </div>
    <div class="right">
      <div class="top-container">
        <div class="title">故障图谱诊断系统</div>
        <div class="right-config">
          <img src="../assets/config.png" alt="1">
          <el-button class="admin" type="primary">admin</el-button>
        </div>
      </div>
      <div class="center">
        <div class="show">
          <div class="operate">
            <div class="op-item">
              <span>数据文件：</span>
              <el-button type="info" plain>fault_IR.mat</el-button>
              <el-button type="primary" icon="el-icon-plus" plain>继续上传</el-button>
              <el-button
                  style="letter-spacing:2px;font-weight: bold;font-size: 14px; color: #2d8dfe; width: 88px;margin-left: 122px"
                  type="primary" plain>查询
              </el-button>
            </div>
            <div class="op-item">
              <span>故障粒度：</span>
              <el-select v-model="levelSelect" placeholder="请选择">
                <el-option
                    v-for="item in faultLevel"
                    :key="item"
                    :label="item"
                    :value="item">
                </el-option>
              </el-select>
            </div>
            <div class="op-item">
              <span>展示特征：</span>
              <el-checkbox-group v-model="featureSelect" class="my-select">
                <template v-for="(item, index) in featureList">
                  <el-checkbox :label="item" :key="index" border></el-checkbox>
                </template>
              </el-checkbox-group>
            </div>
          </div>
          <div class="result">
            <el-card class="box-card">
              <div slot="header" class="clearfix">
                <span class="card-title">诊断结果</span>
                <el-button style="color: white; float: right; padding: 3px 0" type="text">详情</el-button>
              </div>
              <div class="result-body">
                <div class="fault-row" style="width:90%;padding: 0 25px">
                  <div class="fault-item">
                    <div class="fault-title">
                      故障类型：
                      <span class="fault-value">驱动端_内圈_深度14_载荷0_点钟6</span>
                    </div>
                  </div>
                  <div class="fault-item">
                    <div class="fault-title">
                      故障概率：
                      <span class="fault-value">71%</span>
                    </div>
                  </div>
                </div>
                <el-divider></el-divider>
                <div class="fault-row" style="width:90%;padding: 0 25px">
                  <div class="fault-item">
                    <div class="fault-title">
                      故障特征展示：
                      <!--                      <span class="fault-value">DE_IR_size14_load0_clock6</span>-->
                    </div>
                  </div>
                  <div class="fault-item">
                    <div class="fault-title">
                      展示数量：
                      <span class="fault-value">4</span>
                    </div>
                  </div>
                </div>
                <el-row class="feature-row" :gutter="50">
                  <el-col :span="10" v-for="index in 4" :key="index" class="feature-item">
                    <div class="order-number">{{ index }}</div>
                    <span class="feature-title">{{ featureTitle[index - 1] }}：</span>
                    <span class="feature-value">{{ featureValue[index - 1] }}</span>
                  </el-col>
                </el-row>
              </div>
            </el-card>
            <el-card class="box-card" style="margin-top: 25px">
              <div slot="header" class="clearfix">
                <span class="card-title">相似故障</span>
                <el-button style="float: right; padding: 3px 0" type="text">详情</el-button>
              </div>
              <div class="result-body">
                <div class="fault-row" v-for="index in 3" :key="index">
                  <div class="fault-item">
                    <div class="order-number" style="margin-right: 15px">{{ index }}</div>
                    <div class="fault-title">
                      相似故障：
                      <span class="fault-value">{{ similarName[index - 1] }}</span>
                    </div>
                  </div>
                  <div class="fault-item">
                    <div class="fault-title">
                      故障概率：
                      <span class="fault-value">{{ similarProbability[index - 1] }}</span>
                    </div>
                  </div>
                  <div class="fault-item">
                    <div class="fault-title">
                      相似程度：
                      <span class="fault-value">{{ similarDegree[index - 1] }}</span>
                    </div>
                  </div>
                </div>
              </div>
            </el-card>
          </div>
        </div>
        <div class="graph">
          <el-card class="box-card">
            <div slot="header" class="clearfix">
              <span class="card-title">故障图谱</span>
              <el-button style="float: right; padding: 3px 0" type="text">查看</el-button>
            </div>
            <div class="graph-body">

              <div class="img-body">
                <img src="../assets/graph_china.png" alt="1">
              </div>
              <div class="graph-show">
                <div class="graph-item">
                  <span>诊断故障：</span>
                  <div class="square" style="background: red"></div>

                  <span style="margin-left: 45px">相似故障：</span>
                  <div class="square" style="background: black"></div>
                </div>
                <div class="graph-item">
                  <span class="graph-title">节点名称：</span>
                  <span class="graph-value">驱动端_内圈_深度14_载荷0_点钟6</span>
                </div>
                <div class="graph-item">
                  <span class="graph-title">节点id：</span>
                  <span class="graph-value">DE_IR_size14_load0_clock6</span>
                </div>
                <div class="graph-item">
                  <span class="graph-title">故障粒度：</span>
                  <span class="graph-value">最细</span>
                </div>
                <div class="graph-item">
                  <span class="graph-title">备注：</span>
                  <span class="graph-value">暂无</span>
                  <el-button size="mini" type="primary" plain style="width:70px;margin-left: 135px">编辑</el-button>
                </div>
                <div class="time-body">

                  <div class="chain-title">故障明细</div>
                  <el-timeline>
                    <el-timeline-item
                        v-for="(activity, index) in chain"
                        :key="index"
                        :color="activeColor"
                        :timestamp="activity.timestamp">
                      <div class="line-item" style="display: flex">
                        <span :style="{color:(index == 4 ? '#409eff' : 'black')}">{{ activity }}</span>
                      </div>
                    </el-timeline-item>
                  </el-timeline>
                </div>
              </div>
            </div>
            <div class="relation-body">
              <div class="rel-top-title">
                与相似故障关系
              </div>

              <el-divider></el-divider>
              <div class="rel-body">
                <div class="rel-item">
                  <div class="rel-title">
                    <div class="order-number">1</div>
                    <div class="rel-title-word">故障从属关系：</div>
                  </div>
                  <div class="rel-double">
                    <div class="node sp-node"><b>诊断：</b>驱动端_内圈_深度14_载荷0_点钟6</div>
                    <div class="node"><b>相似：</b>驱动端_内圈_深度14_载荷1_点钟6</div>
                  </div>
                  <div class="rel-double">
                    <div class="rel"><img src="../assets/belong.png" width="15" height="15" alt="1"></div>
                    <div class="rel"><img src="../assets/belong.png" width="15" height="15" alt="1"></div>
                  </div>
                  <div class="rel-double">
                    <div class="node sp-node">驱动端_内圈_深度14_载荷0</div>
                    <div class="node">驱动端_内圈_深度14_载荷1</div>
                  </div>

                  <div class="rel"><img src="../assets/belong.png" width="20" height="20" alt="1"></div>
                  <div class="node sp-node">驱动端_内圈_深度14</div>
                </div>
              </div>
              <el-divider></el-divider>

              <div class="rel-body">
                <div class="rel-item">
                  <div class="rel-title">
                    <div class="order-number">2</div>
                    <div class="rel-title-word">故障从属关系：</div>
                  </div>
                  <div class="rel-double">
                    <div class="node sp-node"><b>诊断：</b>驱动端_内圈_深度14_载荷0_点钟6</div>
                    <div class="node"><b>相似：</b>驱动端_内圈_深度14_载荷2_点钟6</div>
                  </div>
                  <div class="rel-double">
                    <div class="rel"><img src="../assets/belong.png" width="15" height="15" alt="1"></div>
                    <div class="rel"><img src="../assets/belong.png" width="15" height="15" alt="1"></div>
                  </div>
                  <div class="rel-double">
                    <div class="node sp-node">驱动端_内圈_深度14_载荷0</div>
                    <div class="node">驱动端_内圈_深度14_载荷2</div>
                  </div>

                  <div class="rel"><img src="../assets/belong.png" width="20" height="20" alt="1"></div>
                  <div class="node sp-node">驱动端_内圈_深度14</div>
                </div>
              </div>
              <el-divider></el-divider>

              <div class="rel-body">
                <div class="rel-item">
                  <div class="rel-title">
                    <div class="order-number">3</div>
                    <div class="rel-title-word">故障从属关系：</div>
                  </div>
                  <div class="rel-double">
                    <div class="node sp-node"><b>诊断：</b>驱动端_内圈_深度14_载荷0_点钟6</div>
                    <div class="node"><b>相似：</b>驱动端_内圈_深度14_载荷0_点钟3</div>
                  </div>
                  <div class="rel"><img src="../assets/belong.png" width="20" height="20" alt="1"></div>
                  <div class="node sp-node">驱动端_内圈_深度14_载荷0</div>
                </div>
              </div>
            </div>
          </el-card>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "kgSystem",
  data() {
    return {
      name: 11,
      faultLevel: ['层级1 fault_level_1', '层级2 fault_level_2', '层级3 fault_level_3', '层级4 fault_level_4', '层级5 最细分类',],
      levelSelect: '层级5 最细分类',
      featureSelect: ['设备', '位置', '深度', '方向',],
      featureList: ['设备', '位置', '深度', '电机负载', '方向',],
      featureTitle: ['设备', '位置', '深度', '方向',],
      featureValue: ['风扇端', '轴承外圈', '0.014英寸', '12点钟',],
      similarName: ['驱动端_内圈_深度14_载荷1_点钟6', '驱动端_内圈_深度14_载荷2_点钟6', '驱动端_内圈_深度14_载荷0_点钟3',],
      similarProbability: ['12%', '8%', '7%'],
      similarDegree: ['9.0', '9.0', '8.5',],
      chain: ['驱动端', '驱动端_内圈', '驱动端_内圈_深度14', '驱动端_内圈_深度14_载荷0', '驱动端_内圈_深度14_载荷0_点钟6',],
      activeColor: '#409eff',
    }
  },
  methods: {
    test() {
      console.log(11)
    }
  }
}
</script>
<style scoped lang="scss">

.container {
  font-size: 16px;
  display: flex;

  width: 100%;
  background: #f7f8fc;

  ::v-deep .el-checkbox {
    background: white !important;
  }

  ::v-deep .el-card__header {
    padding: 15px 20px;
  }

  ::v-deep .el-divider {
    margin: 20px 0 15px 0;
  }

  ::v-deep .el-timeline {
    padding: 0;
  }

  .card-title {
    font-size: 18px;
    letter-spacing: 2px;
  }

  .order-number {
    width: 30px;
    height: 30px;
    border-radius: 5px;
    background: #dfeeff;
    color: #2d8dfe;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .left-nav {
    width: 100px;
    background: white;
    box-shadow: 3px 0 0 #f1efef;

    .logo {
      margin: 30px 0 30px 0;
    }
  }

  .right {
    width: calc(100% - 150px);

    .top-container {
      height: 60px;
      width: 100%;
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 0 35px;
      border-bottom: 2px solid #f1efef;

      .title {
        font-size: 24px;
        font-weight: bold;
      }

      .right-config {
        display: flex;
        align-items: center;

        .admin {
          margin-left: 20px;
        }
      }

    }

    .center {
      display: flex;
      width: 100%;

      .show {
        width: 45%;
        background: white;
        border: solid 1px #f1efef;
        padding: 25px;
        margin: 20px 25px;
        border-radius: 5px;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;

        .operate {
          width: 92%;
          height: 160px;
          background: url("../assets/bg.png") center center no-repeat;
          background-size: cover;
          border-radius: 5px;
          padding: 15px 25px 35px 25px;
          display: flex;
          flex-direction: column;
          align-items: self-start;
          color: white;

          .op-item {
            height: 40px;
            display: flex;
            align-items: center;
            margin: 10px 0;

            & > * {
              margin-right: 15px;
            }

            .my-select {
              & > * {
                margin-right: 0;
              }
            }

          }
        }

        .result {
          width: 100%;
          margin-top: 25px;

          .result-body {

          }

          .fault-row {
            display: flex;
            width: 100%;
            height: 40px;
            align-items: center;
            justify-content: space-between;
            //background: pink;
            .fault-item {
              display: flex;
              align-items: center;

              .fault-value {
                color: #409eff;
              }
            }
          }

          .feature-row {
            display: flex;
            align-items: center;
            //justify-content: space-around;
            width: 100%;
            flex-wrap: wrap;

            .feature-item {
              display: flex;
              align-items: center;
              height: 50px;
              border-bottom: solid 1px #f1efef;
              //padding: 0 20px;
              margin-left: 50px;

              .feature-title {
                margin: 0 15px 0 10px;
              }

              .feature-value {
                color: #409eff;
              }
            }

            .line-item {
              //width: 100%;
              display: flex;
              align-items: center;
              justify-content: flex-start;
            }
          }
        }
      }

      .graph {
        width: 60%;
        margin-top: 20px;

        .graph-body {
          display: flex;
          justify-content: space-around;
        }

        .graph-show {
          width: 35%;
          display: flex;
          flex-direction: column;
          align-items: center;
          //justify-content: flex-end;
          height: 40px;
          //margin-left: 25px;
          .graph-item {
            //color: #409eff;
            width: 100%;
            display: flex;
            align-items: flex-start;
            justify-content: flex-start;
            padding-left: 50px;
            //margin-right: 20px;
            margin-bottom: 10px;

            .square {
              width: 15px;
              height: 15px;
            }

            .graph-title {
              font-size: 14px;
            }

            .graph-value {

              font-size: 14px;
              color: #409eff;
            }
          }

          .chain-title {
            margin: 0 0 20px 0;
            font-size: 18px;
            font-weight: 18px;
          }

          .time-body {
            margin-top: 10px;
            padding: 25px 20px 0 20px;
            border: solid 1px #f1efef;
            border-radius: 5px;
            background: #f9fcff;
          }
        }

        .img-body {
          width: 60%;
          padding: 15px;
          border: solid 1px #f1efef;
          border-radius: 5px;

          img {
            width: 100%;

          }
        }

        .relation-body {
          width: 95%;
          padding: 15px;
          border: solid 1px #f1efef;
          border-radius: 5px;
          margin-top: 15px;
          display: flex;
          flex-direction: column;
          align-items: center;
          .rel-top-title {
            font-size: 18px;
            letter-spacing: 2px;
          }

          .rel-body {
            font-size: 14px;
            width: 100%;
            display: flex;
            flex-direction: column;
            align-items: flex-start;
            .rel-item {
              width: 100%;
              display: flex;
              align-items: center;
              justify-content: flex-start;
              .rel-word {
                margin-right: 5px;
              }
              .rel-title {
                display: flex;
                align-items: center;

                .rel-title-word {
                  margin: 0 15px 0 15px;
                  font-size: 16px;
                }
              }

              .rel-double {
                .node {
                  display: flex;
                  align-items: center;
                  height: 20px;
                  margin: 5px 0;
                }
              }

              .sp-node {
                color: #409eff;
              }
              .rel {
                margin: 5px 20px;

                display: flex;
                align-items: center;
                height: 20px;
              }
            }
          }
        }
      }
    }

  }
}
</style>
