<template>
  <!-- 账号详情页 -->
  <div id="Address">
    <div class="addressBox">
      <div>
        <h3>Address:{{ address }}</h3>
        <p>Balance: {{ balance }} ether</p>
        <p v-if="bytecode !== '0x'">
          Bytecode:<el-input
            class="textarea"
            type="textarea"
            :rows="2"
            placeholder="请输入内容"
            v-model="bytecode"
          >
          </el-input>
        </p>
        <!-- <p>EtherValue:</p> -->
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
  name: "Address",
  // 模板引入
  components: {},
  // 数据
  data() {
    return {
      address: "", //账户
      balance: "", //余额
      bytecode: "", //合约字节码
      addressArr: [], //交易记录
    };
  },
  // 方法
  methods: {
    async query() {
      // 账户查询余额
      await web3.eth.getBalance(this.address).then((res) => {
        // console.log(res);
        this.balance = web3.utils.fromWei(res, "ether");
      });
      await web3.eth.getCode(this.address).then((res) => {
        // console.log(res);
        this.bytecode = res;
      });
    },
    queryHistory() {
      // let a = `http://api.etherscan.io/api/${this.address}`;
      // let a = `http://api.etherscan.io/api?module=account&action=txlist&address=${this.address}&startblock=0&endblock=99999999&sort=asc&apikey=YourApiKeyToken`;
      // console.log(a);
    },
  },
  // 创建后
  created() {
    //接收数据，账户hash
    this.address = this.$route.query.Address;
    this.query();
  },
  // 挂载后
  mounted() {},
  // 更新后
  updated() {},
};
</script>

<style lang="less" scoped>
#Address {
  width: 100%;
  .addressBox {
    border: 1px solid #e7eaf3;
    margin: 5rem auto;
    width: 50rem;
    border-radius: 1rem;
    div {
      // height: 10rem;
      margin: 2rem;
      display: flex;
      flex-direction: column;
      justify-content: space-evenly;
      p {
        margin-top: 2rem;
      }
    }
  }
}
</style>