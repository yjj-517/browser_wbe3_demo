<template>
  <!-- 主页展示 -->
  <div id="Home">
    <p>主页</p>
    <div class="searchBox">
      <el-dropdown class="eldropdown" @command="handleCommand">
        <el-button type="primary" class="elbuttons">
          {{ searchName }}<i class="el-icon-arrow-down el-icon--right"></i>
        </el-button>
        <el-dropdown-menu slot="dropdown">
          <el-dropdown-item
            v-for="(item, index) in searchList"
            :key="index"
            :command="item"
            >{{ item }}</el-dropdown-item
          >
        </el-dropdown-menu>
      </el-dropdown>
      <el-input
        class="elinput"
        v-model="hashNum"
        placeholder="请输入内容"
        clearable
      ></el-input>
      <el-button class="elbutton" type="primary" @click="goSearch"
        >搜索</el-button
      >
    </div>
    <Exhibition></Exhibition>
  </div>
</template>

<script>
// 公司测试账户： 0x21f7a1d51fc7155c020b60173C91d2bcA794aab0
import Exhibition from "../components/Exhibition.vue";
export default {
  name: "Home",
  // 模板引入
  components: {
    Exhibition: Exhibition,
  },
  // 数据
  data() {
    return {
      searchName: "All Filters", //搜索名字
      searchList: [
        "All Filters",
        "Addresses",
        "BlockHash",
        "BlockNumber",
        "transactionHash",
      ], //选择搜索
      hashNum: "", //哈希数字
    };
  },
  // 方法
  methods: {
    //下拉框赋值
    handleCommand(command) {
      // console.log(command);
      this.searchName = command;
    },
    //点击搜索 Address:42位 BlockNumber：数字 Blockhash:66位 transactionHash:66位
    goSearch() {
      // console.log(this.hashNum.length);
      //去除前后空格
      this.hashNum = this.hashNum.trim();
      if (this.hashNum.length == 42) {
        this.$router.push({
          path: "/Address",
          query: { Address: this.hashNum },
        });
      } else if (/^\d+$/.test(this.hashNum)) {
        this.$router.push({
          path: "/Block",
          query: { BlockNumber: this.hashNum },
        });
      } else if (this.searchName == "BlockHash") {
        this.$router.push({
          path: "/Block",
          query: { BlockHash: this.hashNum },
        });
      } else if (this.hashNum.length == 66) {
        this.$router.push({
          path: "/Transaction",
          query: { TransactionHash: this.hashNum },
        });
      } else {
        this.$message({
          message: "请输入正确内容",
        });
      }
    },
  },
  // 创建后
  created() {},
  // 挂载后
  mounted() {},
  // 更新后
  updated() {},
};
</script>

<style lang="less" scoped>
#Home {
  width: 100%;
  .searchBox {
    margin: 5rem auto;
    display: flex;
    flex-direction: row;
    justify-content: center;
    width: 60rem;
    line-height: 2.6rem;
    .eldropdown {
      width: 10rem;
      margin-right: -0.4rem;
      .elbuttons {
        width: 10rem;
      }
    }
    .elinput {
      width: 40rem;
    }
    .elbutton {
      width: 7rem;
      margin-left: -0.4rem;
    }
  }
}
</style>
