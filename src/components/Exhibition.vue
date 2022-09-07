<template>
  <div id="Exhibition">
    <div class="blocks boxstyle">
      <h3>Latest Blocks</h3>
      <div>
        <el-table :data="blockArr" stripe style="width: 100%">
          <el-table-column prop="number" label="number"> </el-table-column>
          <el-table-column prop="hash" label="hash"> </el-table-column>
          <el-table-column prop="size" label="size"> </el-table-column>
          <el-table-column prop="timestamp" label="timestamp">
          </el-table-column>
          <el-table-column prop="gasUsed" label="gasUsed"> </el-table-column>
          <el-table-column prop="gasLimit" label="gasLimit"> </el-table-column>
        </el-table>
      </div>
    </div>
    <div class="transactions boxstyle">
      <h3>Latest Transactions</h3>
      <div>
        <el-table :data="transactionsArr" stripe style="width: 100%">
          <el-table-column prop="blockNumber" label="blockNumber">
          </el-table-column>
          <el-table-column prop="hash" label="hash"> </el-table-column>
          <el-table-column prop="from" label="from"> </el-table-column>
          <el-table-column prop="to" label="to"> </el-table-column>
          <el-table-column prop="value" label="value"></el-table-column>
          <el-table-column prop="gas" label="gas"> </el-table-column>
          <el-table-column prop="gasPrice" label="gasPrice"> </el-table-column>
          <el-table-column prop="nonce" label="nonce"></el-table-column>
          <el-table-column
            prop="transactionIndex"
            label="transactionIndex"
          ></el-table-column>
        </el-table>
      </div>
    </div>
  </div>
</template>

<script>
import Web3 from "web3";
//网络连接：前面是获取到配置钱包的网络，如果没有钱包，则获取自己配置的网络，需要以太网，配置以太网，比特网就配置比特网
// const web3 = new Web3(
//   Web3.givenProvider ||
//     new Web3.providers.WebsocketProvider(
//       "wss://mainnet.infura.io/ws/v3/f0e9db55a2ef4cd091f4c2f85dd01e66"
//     )
// );
const web3 = new Web3(
  Web3.givenProvider ||
    "wss://mainnet.infura.io/ws/v3/f0e9db55a2ef4cd091f4c2f85dd01e66"
);
export default {
  name: "Exhibition",
  // 模板引入
  components: {},
  // 数据
  data() {
    return {
      blockArr: [], //最新区块
      transactionsArr: [], //最新区块的交易
    };
  },
  // 方法
  methods: {
    //获取最新区块
    async block() {
      // 获取最新区块号
      let blockNum = await web3.eth.getBlockNumber();
      for (let i = blockNum; i > blockNum - 10; i--) {
        // 通过递减获取最新十个区块信息
        await web3.eth.getBlock(i, true).then((res) => {
          console.log(res);
          // 数据处理
          // res.gasUsed = web3.utils.fromWei(res.gasUsed.toString(), "ether");
          // res.gasLimit = web3.utils.fromWei(res.gasLimit.toString(), "ether");
          res.timestamp = new Date(
            parseInt(res.timestamp) * 1000
          ).toLocaleString("chinese", { hour12: false });
          this.blockArr.push(res);
        });
      }
      // console.log(this.blockArr);
      //获取最新区块的最新交易
      let transactionsNum = await web3.eth.getBlock(blockNum, true);
      // console.log(transactionsNum);
      // 这里面的值和通过web3.eth.getTransaction查出来的是一样的
      transactionsNum = transactionsNum.transactions;
      // console.log(transactionsNum);
      for (let i = 0; i < 10; i++) {
        // 通过递增加获取最新区块的前十个交易信息
        // transactionsNum[i].gas = web3.utils.fromWei(
        //   transactionsNum[i].gas.toString(),
        //   "ether"
        // );
        // transactionsNum[i].gasPrice = web3.utils.fromWei(
        //   transactionsNum[i].gasPrice.toString(),
        //   "ether"
        // );
        // transactionsNum[i].value = web3.utils.fromWei(
        //   transactionsNum[i].value.toString(),
        //   "ether"
        // );
        this.transactionsArr.push(transactionsNum[i]);
      }
    },
  },
  // 创建后
  created() {
    this.block();
  },
  // 挂载后
  mounted() {},
  // 更新后
  updated() {},
};
</script>

<style lang="less" scoped>
#Exhibition {
  width: 100%;
  // display: flex;
  .boxstyle {
    // width: 50%;
    border: 0.1rem solid #e7eaf3;
    border-radius: 1rem;
    margin: 0 2rem;
    h3 {
      line-height: 4rem;
      margin-left: 2rem;
      border-radius: 1rem;
    }
  }
}
</style>