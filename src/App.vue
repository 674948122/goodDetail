<template>
  <div id="app">
      <header class="mui-bar mui-bar-nav rebar">
      <a class="mui-action-back mui-icon mui-icon-left-nav mui-pull-left"></a>
      <h1 class="mui-title">商品详情</h1>
      <a href="javascript:void(0);" class="shopcart">
        <i class="icon iconfont icon-gouwuche"></i>
        <span class="cartNumsF"></span>
      </a>
    </header>
    <div class="mui-content">

      <div class="detailcommodity">
        <div class="detailpic">
          <img :src="dataValue.goodImgUrl">
        </div>
        <div class="detail_introduce">
          <h5>{{dataValue.goodName}}</h5>
          <!-- 正常价格 -->
          <div class="pdgPrice" id="dinghuoprice" style="display:bloack;" v-if="jietijiashow == false">
            <table>
              <tr>
                <td class="pdgTit">订货价：</td>
                <td class="pdgPricetext" style="font-weight:700;">￥
                  <span class="dinghuodanjia" v-text="dataValue.goodPrice.toFixed(2)"></span>
                </td>
                <td class="pdgCollection">
                  <div class="fr collect">
                    <span @click="collect" v-bind:class="collotColor" class='iconfont icon-aixin'></span>
                    <input type="hidden" class="iscollect" value="" /> 收藏
                  </div>
                </td>
              </tr>
            </table>
          </div>
          <!-- 阶梯价格 -->
          <div class="pdgPrice" id="jietiprice" style="display:block;" v-if="jietijiashow == true">
            <table>
              <tr>
                <td class="pdgTit">订货价：</td>
                <td v-bind:class="dinghuoclass" class="pdgPricetext jietiPrc" id="dinghuojia">￥
                  <span class="dinghuodanjia" v-text="dataValue.unitList[iscur].goodPrice.toFixed(2)"></span>
                </td>
                <td class="pdgCollection">
                  <div class="fr collect">
                    <span @click="collect" v-bind:class="collotColor" class='iconfont icon-aixin'></span>
                    <input type="hidden" class="iscollect" value="" /> 收藏
                  </div>
                </td>
              </tr>
              <tr id="yidang" v-if="yidang">
                <td class="pdgTit">阶梯价：</td>
                <td class="pdgPricetext">
                  <span v-if="jietijialength<2">></span>
                  <span id="minM0" v-text="dataValue.unitList[iscur].tieredPricing.details[0].min"></span>
                  <span v-if="jietijialength>1">-</span>
                  <span v-if="jietijialength>1" id="maxM0" v-text="dataValue.unitList[iscur].tieredPricing.details[0].max"></span>
                  件</td>
                <td class="pdgCollection jietiPrc" :class="yidangclass">
                  ￥
                  <span id="Jprice0" v-text="dataValue.unitList[iscur].tieredPricing.details[0].price.toFixed(2)"></span>
                </td>
              </tr>
              <tr id="erdang" v-if="erdang">
                <td class="pdgTit"></td>
                <td class="pdgPricetext">
                  <span v-if="jietijialength<3">></span>
                  <span id="minM1" v-text="dataValue.unitList[iscur].tieredPricing.details[1].min"></span>
                  <span v-if="jietijialength>2">-</span>
                  <span v-if="jietijialength>2" id="maxM1" v-text="dataValue.unitList[iscur].tieredPricing.details[1].max"></span>
                  件</td>
                <td class="pdgCollection jietiPrc" :class="erdangclass">
                  ￥
                  <span id="Jprice1" v-text="dataValue.unitList[iscur].tieredPricing.details[1].price.toFixed(2)"></span>
                </td>
              </tr>
              <tr id="sandang" v-if="sandang">
                <td class="pdgTit"></td>
                <td class="pdgPricetext">>
                  <span id="minM2" v-text="dataValue.unitList[iscur].tieredPricing.details[2].min"></span>
                  件</td>
                <td class="pdgCollection jietiPrc" :class="sandangcalss">
                  ￥
                  <span id="Jprice2" v-text="dataValue.unitList[iscur].tieredPricing.details[2].price.toFixed(2)"></span>
                </td>
              </tr>
            </table>
          </div>
        </div>
      </div>
      <div class="detailcommodity pdgBox">
        <div class="detail_introduce" style="width:100%">
          <!-- <p class="bargain">规格：<span></span></p> -->
          <div class="unitS">
            <div class="selNum fl">
              选择单位：
            </div>
            <div class="selunit">
              <span v-for="(item,index) in dataValue.unitList">
                <a href="javascript:;" :class="{cur:iscur==index}" @click="danweichange(index)">{{item.unit}}</a>
                <!-- <input type="hidden" value=""  class="minM0" /> -->
              </span>
            </div>
          </div>
          <div class="selcollect oh">
            <!--<div>
                <input style="width: 30px;" onkeyup="value=value.replace(/[^\d]/g,'')" onbeforepaste="clipboardData.setData('text',clipboardData.getData('text').replace(/[^\d]/g,''))">
                <input style="width: 40px;" onKeypress="return (/[\d]/.test(String.fromCharCode(event.keyCode)))" style="ime-mode:Disabled">
                <input onkeydown="onlyNum();" style="ime-mode:Disabled">
              </div>-->
            <div class="selNum fl">选择数量：</div>
            <div class="mui-numbox fl" data-numbox-step="1" data-numbox-min="1">
              <button @click="reduce" class="mui-btn mui-numbox-btn-minus reduce" type="button" id="reduce">-</button>
              <input v-bind:value="cartNum" id="cartNum" class="mui-numbox-input cartNum" type="tel" onkeyup="value=value.replace(/[^\d]/g,'')" onbeforepaste="clipboardData.setData('text',clipboardData.getData('text').replace(/[^\d]/g,''))">
              <button @click="add" class="mui-btn mui-numbox-btn-plus add" type="button" >+</button>
            </div>
            <div class="pdgResid">
              剩余：
              <span id="stockGoodCount">{{dataValue.stockGoodCount}}</span>
              件
            </div>
          </div>
        </div>
      </div>
      <div style="padding: 5px;background: #f5f5f5;">
        <div id="segmentedControl" class="mui-segmented-control mui-segmented-control-inverted">
          <a class="mui-control-item " href="#" @click="show=true" :class="show?'mui-active':''">
            商品详情
          </a>
          <a class="mui-control-item" href="#" @click="show=false" :class="!show?'mui-active':''">
            规格参数
          </a>
        </div>
      </div>
      <div>
        <div id="item1" class="mui-control-content mui-active" v-bind:style="show?disb:disn">
          <p>脆香米，味道棒棒滴！！！</p>
          <p>1234567890QWERTYUIOPasdfghjklzxcvbnm!@#$%^&amp;*</p>
        </div>
        <div id="item2" class="mui-control-content" v-bind:style="show?disn:disb">
          <table class="talbe-x">
            <tbody>
              <tr v-for="(item,index) in dataValue.parameterList">
                <td v-text="item.name"></td>
                <td v-text="item.value"></td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
    <div class="detail_bottom">
      <div class="seled">
        已选择：
        <span class="num">{{cartNum}}</span>
        <em v-text="dataValue.unitList[iscur].unit"></em>,￥
        <span class="money" v-text="sum.toFixed(2)"></span>
        <input type="hidden" class="priceF" value="" />
      </div>
      <div class="cartshop">
        <a href="javascript:void(0);" class="nowshop">立即购买</a>
        <a href="javascript:void(0);" class="shoporder">加入购物车</a>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",
  data () {
    return {
      disb:'display:block',
      disn:'display:none',
      show:true,
      goodNo: 'F00006',      //商品ID
      goodPrice: 0,         //商品价格
      sum: 0,                //总价
      cartNum: 1,            //商品数量
      collecttype: 0,    //收藏状态
      collotColor: '',
      jietijiashow: false,   //是否显示阶梯价
      dinghuoclass: '',
      yidangclass: '',
      erdangclass: '',
      sandangcalss: '',
      yidang:false,
      erdang:false,
      sandang:false,
      iscur: 0,                //被选中单位序号index
      jietijialength: 0,        //阶梯价层数
      min1:0,
      max1:0,
      price1:0,
      min2:0,
      max2:0,
      price2:0,
      min3:0,
      price3:0,
      dataValue:{
        goodImgUrl: "http://dianxiaohuo-st-image.oss-cn-beijing.aliyuncs.com/1995/20171017/m_4473dd9f30484b9e9fd24a1da9bf9eef.jpg",
        salePrice: 6000,
        specMemo: '16G',
        isCollect: 0,
        unitList: [{
          unitType: 0,
          unit: '部',
          tieredPricing: {
            minPrice: 10,
						details: [{
							min: 2,
							price: 100
						}
            ],
						maxPrice: 300
          },
          salePrice: 1000,
					startNum: 1,
					goodPrice: 1000
        },
        {
					unitType: 1,
					unit: "件",
					tieredPricing: {
						minPrice: 10,
						details: [{
							min: 3,
							max: 5,
							price: 10
						},
						{
							min: 6,
							price: 20
						}
						],
						maxPrice: 300
					},
					salePrice: 6000,
					startNum: 1,
					goodPrice: 6000
				},
        {
					unitType: 1,
					unit: "个",
					tieredPricing: {
						minPrice: 10,
						details: [{
							min: 3,
							max: 5,
							price: 10
						},
						{
							min: 6,
							max: 9,
							price: 20
						},
						{
							min: 10,
							price: 30
						}
						],
						maxPrice: 300
					},
					salePrice: 6000,
					startNum: 1,
					goodPrice: 6000
				}
        ],
        goodPrice: 6000.87567,
				saleNum: 13,
				goodNo: "iphone5",
				cartNum: 0,
				unitType: 1,
				unit: "件",
				goodName: "IPhone5",
				desPath: "",
				tieredPricing: {
					minPrice: 10,
					details: [{
						min: 3,
						max: 10,
						price: 30
					},
					{
						min: 11,
						max: 20,
						price: 20
					},
					{
						min: 21,
						price: 10
					}
					],
					maxPrice: 300
				},
				minPrice: 6000,
				stockGoodCount: 9990,
				parameterList: [{
					name: "商品编码",
					value: "iphone5"
				},
				{
					name: "商品单位",
					value: "部"
				},
				{
					name: "品牌名称",
					value: "Apple"
				},
				{
					name: "规格参数",
					value: "16G"
				}]
      }
    }
  },
  computed: {
    
  },
  mounted: function(){
     //初始化阶梯价显示状态
    this.$nextTick(function(){  
      this.jietijialength = this.dataValue.unitList[this.iscur].tieredPricing.details.length;//阶梯价层数写入
      this.jietijiaL();
      if(localStorage.getItem('collecttype')){
        this.collecttype = localStorage.getItem('collecttype');
        if(this.collecttype == '1'){
            this.collotColor = 'collotColor';
            this.collecttype = '0';
          }else{
            this.collecttype = '1';
            this.collotColor = '';
          }    
      }else{
        if(this.collecttype == '1'){
          this.collotColor = 'collotColor';
          this.collecttype = '0';
        }else{
          this.collecttype = '1';
          this.collotColor = '';
        }    
      }                           
      if( this.dataValue.unitList[0].tieredPricing.details == undefined ){
        this.jietijiashow = false;
      }else{
        this.jietijiashow = true;
      }
      this.judge();
    })
  },
  methods:{
    //根据阶梯价层数和商品数量高亮价格
    jietijiaHighlight: function(){
      var m = this.cartNum;
      if(this.jietijialength==1){
        if( m < this.min1 ){
          this.dinghuoclass = 'pdgactive';
          this.goodPrice = this.dataValue.unitList[this.iscur].goodPrice;    //商品价格写入
          this.sum = this.goodPrice * this.cartNum; //总价计算
        }else if( m >= this.min1 ){
          this.dinghuoclass = '';
          this.yidangclass = 'pdgactive';
          this.goodPrice = this.price1;    //商品价格写入
          this.sum = this.goodPrice * this.cartNum; //总价计算
        }
      }
      if(this.jietijialength==2){
        if( m < this.min1 ){
          this.dinghuoclass = 'pdgactive';
          this.goodPrice = this.dataValue.unitList[this.iscur].goodPrice;    //商品价格写入
          this.sum = this.goodPrice * this.cartNum; //总价计算
        }else if( m >= this.min1 && m <= this.max1){
          this.dinghuoclass = '';
          this.yidangclass = 'pdgactive';
          this.goodPrice = this.price1;    //商品价格写入
          this.sum = this.goodPrice * this.cartNum; //总价计算
        }else if( m >= this.min2){
          this.yidangclass = '';
          this.erdangclass = 'pdgactive';
          this.goodPrice = this.price2;    //商品价格写入
          this.sum = this.goodPrice * this.cartNum; //总价计算
        }
      }
      if(this.jietijialength==3){
        if( m < this.min1 ){
          this.dinghuoclass = 'pdgactive';
          this.goodPrice = this.dataValue.unitList[this.iscur].goodPrice;    //商品价格写入
          this.sum = this.goodPrice * this.cartNum; //总价计算
        }else if( m >= this.min1 && m <= this.max1){
          this.dinghuoclass = '';
          this.yidangclass = 'pdgactive';
          this.goodPrice = this.price1;    //商品价格写入
          this.sum = this.goodPrice * this.cartNum; //总价计算
        }else if( m >= this.min2 && m <= this.max2){
          this.yidangclass = '';
          this.erdangclass = 'pdgactive';
          this.goodPrice = this.price2;    //商品价格写入
          this.sum = this.goodPrice * this.cartNum; //总价计算
        }else if( m >= this.min3 ){
          this.erdangclass = '';
          this.sandangcalss = 'pdgactive';
          this.goodPrice = this.price3;    //商品价格写入
          this.sum = this.goodPrice * this.cartNum; //总价计算
        }
      }
    },
    //根据阶梯价层数定义变量
    jietijiabianliang :function(){
      this.min1 = this.dataValue.unitList[this.iscur].tieredPricing.details[0].min;
      this.max1 = this.dataValue.unitList[this.iscur].tieredPricing.details[0].max;
      this.price1 = this.dataValue.unitList[this.iscur].tieredPricing.details[0].price;
      if(this.dataValue.unitList[this.iscur].tieredPricing.details[1]){
        this.min2 = this.dataValue.unitList[this.iscur].tieredPricing.details[1].min;
        this.max2 = this.dataValue.unitList[this.iscur].tieredPricing.details[1].max;
        this.price2 = this.dataValue.unitList[this.iscur].tieredPricing.details[1].price;
      }
      
      if(this.dataValue.unitList[this.iscur].tieredPricing.details[2]){
        this.min3 = this.dataValue.unitList[this.iscur].tieredPricing.details[2].min;
        this.price3 = this.dataValue.unitList[this.iscur].tieredPricing.details[2].price;
      }
    },
    //根据阶梯价层数显示阶梯价
    jietijiaL: function(){
      if ( this.jietijialength == 1 ){
        this.yidang = true;
        this.erdang = false;
        this.sandang = false;
      }
      if ( this.jietijialength == 2 ){
        this.yidang = true;
        this.erdang = true;
        this.sandang = false;
      }
      if ( this.jietijialength == 3) {
        this.yidang = true;
        this.erdang = true;
        this.sandang = true;
      }
    },
    //阶梯价判断
    judge: function(){
      this.jietijiabianliang();
      // this.cartNum = this.cartNum;
      this.goodPrice = this.dataValue.goodPrice * this.cartNum;
      this.jietijiaHighlight();
    }, 
    //单位选择
    danweichange: function(index){
      this.dinghuoclass = '';
      this.yidangclass = '';
      this.erdangclass = '';
      this.sandangcalss = '';
      this.iscur=index;
      this.jietijialength = this.dataValue.unitList[this.iscur].tieredPricing.details.length;
      this.goodPrice = this.dataValue.unitList[this.iscur].goodPrice;     //商品价格写入
      this.sum = this.goodPrice * this.cartNum; //总价计算
      this.judge();
      this.jietijiaL();
    },
    //增加数量
    add: function(){
      this.jietijiabianliang();    
      this.cartNum = this.cartNum+1;
      this.goodPrice = this.dataValue.goodPrice * this.cartNum;
      this.jietijiaHighlight();
    },
    //减少数量
    reduce: function(){
      this.jietijiabianliang();    
      this.cartNum = this.cartNum-1;
      if( this.cartNum <=1 ){
        this.cartNum = 1;
        this.goodPrice = this.dataValue.unitList[this.iscur].goodPrice;    //商品价格写入
        this.sum = this.goodPrice * this.cartNum; //总价计算
      }
      let m = this.cartNum;
      if( m <= this.max2 && m >= this.min2 ){
        this.erdangclass = 'pdgactive';
        this.sandangcalss = '';
        this.goodPrice = this.price2;    //商品价格写入
        this.sum = this.goodPrice * this.cartNum; //总价计算
      }else if( m <= this.max1 && m >= this.min1 ){
        this.yidangclass = 'pdgactive';
        this.erdangclass = '';
        this.goodPrice = this.price1;    //商品价格写入
        this.sum = this.goodPrice * this.cartNum; //总价计算
      }else if( m < this.min1){
        this.dinghuoclass = 'pdgactive';
        this.yidangclass = '';
      }   
    },
    //点击收藏
    collect: function(){
      if( this.collecttype == '0' || this.collecttype == 0 ){
        this.collecttype = 1;
        this.collotColor = 'collotColor';
        let a = this.collecttype;
        window.localStorage.setItem('collecttype',a);
      }else {
        this.collecttype = 0;
        this.collotColor = '';
        let a = this.collecttype;
        window.localStorage.setItem('collecttype',a);
      }
    }
  }
};
</script>
<style>

</style>
