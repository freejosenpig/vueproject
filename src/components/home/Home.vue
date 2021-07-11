<template>
  <div class="workbench">
    <h3 style="margin: 5px">业务数据统计</h3>
    <el-row :gutter="24">
      <el-col :span="8">
        <div class="grid-content bg-purple">
          <el-card class="box-card">
            <div slot="header" class="clearfix">
              <span style="font-size: 16px">销售总和</span>
            </div>
            <div class="text item">
              <span>￥{{ this.rsum }}</span>
              <div class="ik-pull-right">
                <span class="stats__badge" v-text="this.rcount + '笔'"></span>
              </div>
            </div>
          </el-card>
        </div>
      </el-col>
      <el-col :span="8">
        <div class="grid-content bg-purple">
          <el-card class="box-card">
            <div slot="header" class="clearfix">
              <span style="font-size: 16px">采购总和</span>
            </div>
            <div class="text item">
              <span>￥{{ this.psum }}</span>
              <div class="ik-pull-right">
                <span class="stats__badge" v-text="this.pcount + '笔'"></span>
              </div>
            </div>
          </el-card>
        </div>
      </el-col>
      <el-col :span="8">
        <div class="grid-content bg-purple">
          <el-card class="box-card">
            <div class="text item">
              <span style="font-size: 16px">库存总量</span>
              <div class="text2">
                <span>{{ this.ps }} 件</span>
              </div>
            </div>
            <div class="text item">
              <span style="font-size: 16px">库存总成本</span>
              <div class="text2">
                <span>￥</span>
                <span> </span>
              </div>
            </div>
          </el-card>
        </div>
      </el-col>
    </el-row>
    <div style="float: left; margin-top: 30px">
      <!--横向条状图-->
      <div class="Echarts">
        <div id="main" style="width: 700px; height: 500px"></div>
      </div>
      <!---->
    </div>
    <div style="float: right">
      <div id="f"></div>
      <el-calendar v-model="value" class="calen"> </el-calendar>
    </div>
  </div>
</template>

<script>

</script>

<style>
.workbench .el-col {
  border-radius: 4px;
}

.workbench .bg-purple-dark {
  background: #99a9bf;
}

.workbench .bg-purple {
  background: #d3dce6;
}

.workbench .bg-purple-light {
  background: #e5e9f2;
}

.workbench .grid-content {
  border-radius: 4px;
  min-height: 36px;
}

.workbench .row-bg {
  padding: 10px 0;
  background-color: #f9fafc;
}

.workbench .text {
  font-size: 20px;
  margin-top: 20px;
}

.workbench .item {
  margin-bottom: 18px;
  line-height: 10px;
}

.workbench .clearfix:before,
.workbench .clearfix:after {
  display: table;
  content: '';
  line-height: 10px;
}

.workbench .clearfix:after {
  clear: both;
}

.workbench .box-card {
  text-align: left;
  /*width: 250px;*/
}

.workbench .el-dropdown-link {
  cursor: pointer;
  color: #409eff;
}

.workbench .el-card__header {
  line-height: 10px;
  /*text-align: left;*/
  /*width: 200px;*/
}

.workbench .ik-pull-right {
  margin-top: -5px;
  float: right;
}

.workbench .stats__badge {
  min-width: 54px;
  line-height: 20px;
  text-align: center;
  margin-left: 10px;
  font-size: 15px;
  border-radius: 2px;
  vertical-align: middle;
  display: inline-block;
  border: 1px solid #bcf2d4;
  background-color: #e5fff1;
  color: #66d598;
}

.workbench .work-tabs {
  background: white;
  border-radius: 1%;
}

.workbench .select-col {
  background-color: white;
  border-radius: 5px;
  margin: 20px 20px 20px 0;
  padding: 10px 10px;
}

.workbench .select-row {
  margin: 20px 5px;
}

.workbench .col-a {
  border: 1px solid #fcfcfc;
  min-height: 1px;
  background: transparent;
  display: block;
  float: left;
  padding: 20px 0 20px;
  color: #333333;
  text-decoration: none;
  text-align: center;
  background-color: white;
  border-bottom: 1px solid #eeeff3;
  border-right: 1px solid #eeeff3;
  width: 100%;
  transition: color 0.3s ease;
}

.workbench .col-a:hover {
  background-color: #fafafa;
}

.workbench .fast-row {
  margin-bottom: 0;
}

.workbench .fast-row-title {
  background-color: #eaeaea;
  padding: 5px;
  font-weight: bold;
  font-size: 14px;
}

.workbench .el-tabs__nav-scroll {
  margin-left: 10px;
}

.workbench #f {
  width: 250px;
  height: 33px;
  background-color: white;
  z-index: 99;
  position: relative;
  top: 45px;
  left: 50%;
}

.workbench .calen {
  width: 500px;
  height: 650px;
}

.workbench .text2 {
  size: 16px;
  float: right;
}
</style>
