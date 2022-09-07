<template>
  <div id="Block">
    <div class="blockBox">
      <h3 v-if="blockHash">blockHash:{{ blockHash }}</h3>
      <h3 v-if="blockNumber">blockNumber:{{ blockNumber }}</h3>
      <div class="blockArrBox">
        <p>
          <span>blockNumber:</span><span>{{ blockArr.number }}</span>
        </p>
        <p>
          <span>hash:</span><span>{{ blockArr.hash }}</span>
        </p>
        <p>
          <span>timestamp:</span><span>{{ blockArr.timestamp }}</span>
        </p>
        <p>
          <span>transactions:</span
          ><span v-if="blockArr.transactions">{{
            blockArr.transactions.length
          }}</span>
        </p>
        <p>
          <span>miner:</span><span>{{ blockArr.miner }}</span>
        </p>
        <p>
          <span>size:</span><span>{{ blockArr.size }}</span>
        </p>
        <p>
          <span>gasUsed:</span><span>{{ blockArr.gasUsed }}</span>
        </p>
        <p>
          <span>gasLimit:</span><span>{{ blockArr.gasLimit }}</span>
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import Web3 from "web3";
//网络连接
const web3 = new Web3(
  Web3.givenProvider ||
    "wss://mainnet.infura.io/ws/v3/f0e9db55a2ef4cd091f4c2f85dd01e66"
);
export default {
  name: "Block",
  // 模板引入
  components: {},
  // 数据
  data() {
    return {
      blockHash: "", //区块hash
      blockNumber: "", //区块高度
      blockArr: {},
    };
  },
  // 方法
  methods: {
    async query() {
      let blockNum = "";
      if (this.blockHash) {
        blockNum = this.blockHash;
      } else if (this.blockNumber) {
        blockNum = this.blockNumber;
      }
      // 通过区块高度或者hash获取区块详细
      await web3.eth.getBlock(blockNum).then((res) => {
        // console.log(res);
        // 数据处理
        res.timestamp = new Date(parseInt(res.timestamp) * 1000).toLocaleString(
          "chinese",
          { hour12: false }
        );
        res.gasUsed = web3.utils.fromWei(res.gasUsed.toString(), "ether");
        res.gasLimit = web3.utils.fromWei(res.gasLimit.toString(), "ether");
        this.blockArr = res;
      });
    },
  },
  // 创建后
  created() {
    //接收数据区块hash或者区块高度
    this.blockHash = this.$route.query.BlockHash;
    this.blockNumber = this.$route.query.BlockNumber;
    this.query();
  },
  // 挂载后
  mounted() {},
  // 更新后
  updated() {},
};
</script>

<style lang="less" scoped>
#Block {
  width: 80%;
  margin: 2rem 10%;
  border: 1px solid #e7eaf3;
  border-radius: 1rem;
  .blockBox {
    margin: 0 2rem;
    h3 {
      line-height: 4rem;
      border-bottom: 1px solid #e7eaf3;
    }
    .blockArrBox {
      p {
        display: flex;
        line-height: 3rem;
        border-bottom: 1px solid #e7eaf3;
        span:nth-child(1) {
          width: 20%;
        }
      }
    }
  }
}
</style>