<style lang="scss" scoped>

@page {
  size: A4 portrait; /* 横の場合はlandscape */
  margin: 0mm;
}

.print-container {
  width: 210mm; /* 用紙の横幅を改めて指定 */
  height: 296.5mm; /* 高さには0.5mm余裕をもたせる */
  //box-sizing: border-box;
  padding: 0;
  margin: 0 auto;
  break-after: auto;
}

.print {
  width: 210mm; /* 用紙の横幅を改めて指定 */
  height: 296.5mm; /* 高さには0.5mm余裕をもたせる */
  display: block;
  position: relative;

  box-sizing: border-box;
  padding: 15mm;
  font-size: 5mm;
  -webkit-print-color-adjust: exact;
}

@media print {
  .print-container, .print {
    overflow: hidden !important;
    box-shadow: none !important;
    filter: none !important;
  }
}

.min {
  font-size: 4mm;
}

.normal {
  font-size: 5mm;
}

.large {
  font-size: 6mm;
}

.x-large {
  font-size: 8mm;
  font-weight: bold;
}

.bold {
  font-weight: bold;
}

.border {
  border: solid 1px black;
  padding: 2mm;
  margin: 2mm 0;
}

table {
  border-collapse: collapse;
  border: 1px solid #666;
  width: 100%;
}

td, th {
  border: 1px solid #666;
  padding: 0.3em 0.5em; /* 文字周りのスペース（パディング） */
}

th {
  background-color: #666;
  color: #FFF;
  text-align: left;
  border-right: 1px solid #EEE;
}

th:last-child {
  border-right: none;
}
</style>
<template>
  <div class="elevation-10 no-margin print-container">
    <div class="print" media="print">
      <Date :date="date"/>
      <LeftTop text="株式会社 ○○○○"/>
      <Title title="発送指示書"/>
      <div class="border">
        <div class="font-weight-bold">注文者</div>
        <div>氏名：{{ data.orderer.name }}</div>
        <div class="min">〒 {{ data.orderer.postal }}</div>
        <div>{{ data.orderer.address }}</div>
      </div>
      <div class="border">
        <div class="font-weight-bold">送り先</div>
        <div>氏名：{{ data.recipient.name }}</div>
        <div class="min">〒 {{ data.recipient.postal }}</div>
        <div>{{ data.recipient.address }}</div>
      </div>
      <div class="border">
        <div class="font-weight-bold">注文データ</div>
        <div>注文日：{{ data.order.date }}</div>
        <div>お支払方法：{{ data.order.method }}</div>
        <div>金額：￥{{ addFigure(data.order.amount) }}円</div>
        <div>発送方法：{{ data.order.sendMethod }}</div>
      </div>
      <div style="margin-top: 6mm;">
        <table>
          <caption class="bold">発注商品一覧</caption>
          <tr style="width: 100%;">
            <th style="text-align: center;">○</th>
            <th>ID</th>
            <th>商品名</th>
            <th>数量</th>
            <th>単価(税込)</th>
          </tr>
          <tr v-for="product of data.products" style="width: 100%;">
            <td width="50px"></td>
            <td>{{ product.id }}</td>
            <td>{{ product.name }}</td>
            <td width="60px">{{ product.num }}</td>
            <td>{{ addFigure(product.price) }}</td>
          </tr>
        </table>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  mounted() {
    this.date = this.$moment().format('YYYY-MM-DD');
  },
  data() {
    return {
      date: "2000-01-01",
      data: {
        orderer: {
          name: "名無しの 権兵衛",
          postal: "840-8502",
          address: "佐賀県佐賀市本庄町１ なんちゃらあぱーと２０１",
        },
        recipient: {
          name: "受け取り 主",
          postal: "840-0027",
          address: "佐賀県佐賀市本庄大字本庄５７３－５ なんちゃらあぱーと１０１",
        },
        order: {
          date: "2020-01-01 01:01:01",
          amount: "25873495",
          method: "クレジットカード決済",
          sendMethod: "通常配送",
        },
        products: [
          {
            name: "お菓子",
            price: 300,
            num: 6,
            id: "4565e6tgr",
          },
          {
            name: "お菓子",
            price: 300,
            num: 6,
            id: "4565e6tgr",
          },
          {
            name: "お菓子",
            price: 11300,
            num: 6,
            id: "4565e6tgr",
          },
        ]
      },
    }
  },
  methods: {
    addFigure(numVal) {
      if (numVal == '') {
        return '';
      }
      numVal = this.toHalfWidth(numVal).replace(/,/g, "").trim();
      if (!/^[+|-]?(\d*)(\.\d+)?$/.test(numVal)) {
        return numVal;
      }
      let numData = numVal.toString().split('.');
      numData[0] = Number(numData[0]).toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',');
      return numData.join('.');
    },
    toHalfWidth(strVal){// 半角変換
      strVal += "";
      let halfVal = strVal.replace(/[！-～]/g,
        function (tmpStr) {
          return String.fromCharCode(tmpStr.charCodeAt(0) - 0xFEE0);
        }
      );
      return halfVal;
    }
  }
}
</script>
