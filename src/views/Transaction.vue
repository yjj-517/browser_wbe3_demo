<template>
  <div id="Transaction">
    <div class="blockBox">
      <h3>TransactionHash：{{ transactionHash }}</h3>
      <div class="blockArrBox">
        <p>
          <span>hash:</span><span>{{ transactionArr.hash }}</span>
        </p>
        <p>
          <span>blockHash:</span><span>{{ transactionArr.blockHash }}</span>
        </p>
        <p>
          <span>blockNumber:</span><span>{{ transactionArr.blockNumber }}</span>
        </p>
        <p>
          <span>transactionIndex:</span
          ><span>{{ transactionArr.transactionIndex }}</span
          ><span v-if="this.transactionArr.transactionIndex == null"
            >交易在块中的索引位置，如果交易处于pending状态，则该值为null</span
          >
        </p>

        <p>
          <span>from:</span><span>{{ transactionArr.from }}</span>
        </p>
        <p>
          <span>to:</span><span>{{ transactionArr.to }}</span>
        </p>
        <p>
          <span>value:</span><span>{{ transactionArr.value }}</span>
        </p>

        <p>
          <span>gas:</span><span>{{ transactionArr.gas }}</span>
        </p>
        <p>
          <span>gasPrice:</span><span>{{ transactionArr.gasPrice }}</span>
        </p>
        <p>
          <span>nonce:</span><span>{{ transactionArr.nonce }}</span>
        </p>
        <p>
          <span>input:</span><span>{{ transactionArr.input }}</span>
        </p>
        <p>
          <span>v:</span><span>{{ transactionArr.v }}</span>
        </p>
        <p>
          <span>status:</span
          ><span v-if="this.transactionReceipt !== null"
            >{{ transactionReceipt.status }}(true为合约执行成功,false
            为合约回滚)</span
          ><span v-if="this.transactionReceipt == null">交易上链尚未成功</span>
        </p>
        <p>
          <span>contractAddress:</span
          ><span>{{ transactionReceipt.contractAddress }}</span
          ><span v-if="this.transactionReceipt.contractAddress == null"
            >如果是一个合约创建交易，返回合约地址，其它情况返回null</span
          >
        </p>
        <!-- <p>
          <span>logs:</span><span>{{ transactionReceipt.logs }}</span>
        </p> -->
      </div>
    </div>
  </div>
</template>

<script>
import Web3 from "web3";
//网络连接
const web3 = new Web3(
  Web3.givenProvider ||
    new Web3.providers.WebsocketProvider(
      "wss://mainnet.infura.io/ws/v3/f0e9db55a2ef4cd091f4c2f85dd01e66"
    )
);
export default {
  name: "Block",
  // 模板引入
  components: {},
  // 数据
  data() {
    return {
      transactionHash: "", //交易hash
      transactionArr: {},
      transactionReceipt: {},
    };
  },
  // 方法
  methods: {
    async query() {
      // 通过交易hash获取交易状态
      await web3.eth.getTransactionReceipt(this.transactionHash).then((res) => {
        console.log(res);
        this.transactionReceipt = res;
        console.log(this.transactionReceipt);
      });
      // 通过交易hash获取交易详细
      await web3.eth.getTransaction(this.transactionHash).then((res) => {
        console.log(res);
        res.gas = web3.utils.fromWei(res.gas.toString(), "ether");
        res.gasPrice = web3.utils.fromWei(res.gasPrice.toString(), "ether");
        res.value = web3.utils.fromWei(res.value.toString(), "ether");
        this.transactionArr = res;
      });
    },
  },
  // 创建后
  created() {
    //接收数据
    this.transactionHash = this.$route.query.TransactionHash;
    this.query();
  },
  // 挂载后
  mounted() {},
  // 更新后
  updated() {},
};
</script>

<style lang="less" scoped>
#Transaction {
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