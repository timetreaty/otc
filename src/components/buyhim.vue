<template>
	<div class="buyhim">
		<div class="left">
			<div class="main">
				<div class="title">购买他的{{datas.currencyType}}</div>
				<!--单价-->
				<div class="price">
					<div class="l_txt">单价</div>
					<div class="r_num">
						<div v-for="item in datas.list" >{{item.unitPrice}} {{payType[item.payType]}}</div>
					</div>
				</div>
				<!--交易限额-->
				<div class="quota">
					<div class="l_txt">交易限额</div>
					<div class="r_num">
						<div v-for="item in datas.list">{{item.minQuota}} - {{item.maxQuota}} {{payType[item.payType]}}</div>
					</div>
				</div>
					<div class="quota">
					<div class="l_txt">剩余数量</div>
					<div class="r_num">
						{{datas.usableDealNum &&  toDecimal(datas.usableDealNum)}}  {{datas.currencyType}}
					</div>					
				</div>
				<div class="content">
					<div class="left_div">
						<!--你想购买多少BIDT?-->
						<div class="tit">你想要购买多少{{datas.currencyType}}？</div>
						<div class="l_num">
							<!--左  输入框-->
							<div class="inp">
								<el-input v-model="l_inp" clearable placeholder="输入你想购买的数量" @input="left_inp">
							    	<template slot="append">{{datas.currencyType}}</template>
							  	</el-input>
							  	<!--限制-->
							  	<!--<div class="text">需≥{{quota}}BIDT</div>-->
							</div>
							<i class="el-icon-sort"></i>
						</div>
					</div>
					<div class="right_div">
						<!--下拉框-->
						<div class="tit">
							用什么支付?
							<el-select v-model="value_methods" clearable placeholder="请选择" @change="sele">
						    	<el-option
						      		v-for="item in options_methods"
						      		:key="item.value"
						      		:label="item.label"
						      		:value="item.value">
						    	</el-option>
						  	</el-select>
						</div>
						<!--右 输入框-->
						<el-input v-model="r_inp" clearable placeholder="输入你能支付的金额" @input="right_inp">
					    	<template slot="append">{{value_methods}}</template>
					  	</el-input>
					  	<div class="texts">需≥{{quota}} {{value_methods}}</div>
					  	<div class="text">
					  		<!--可获得-->
					  		<div class="num1">
					  			<div class="num1_l">本单交易 你可获得：</div>
					  			<div class="num1_r">{{l_inp}} {{datas.currencyType}}</div>
					  		</div>
					  		<!--需支付-->
					  		<div class="num1">
					  			<div class="num1_l">需支付：</div>
					  			<div class="num1_r">{{r_inp}} {{value_methods}}</div>
					  		</div>
					  		<!--30-->
					  		<span>请在下单后30分钟内完成支付</span>
					  		<!--立即购买按钮-->
					  		<div class="send_buy" @click="buy_send">立即购买</div>
					  	</div>
					</div>
				</div>
			</div>
			<!--交易须知-->
			<div class="know">
				<div class="know_title">交易须知</div>
				<!-- <li>请您详细了解对方的交易信息，并确认交易内容无误后，再点击立即出售按钮。</li>
				<li>请在下单后30分钟内完成支付，支付完成后务必点击【我已付款】，避免30分钟后超时订单自动取消造成您的财产损失。</li>
				<li>请在下单后尽快将您的收款账户发送给买家/卖家，务必使用站内聊天系统，避免资料外泄。</li>
				<li>在交易过程中请使用平台的聊天系统进行沟通，平台外的对话记录将无法作为交易纠纷的依据。</li>
				<li>请按照卖家收款方式完成线下汇款操作（选择银行卡支付的只能本人实名银行卡。</li>
				<li>银行卡转账请勿备注任何比特币、BTC、以太坊、ETH以及其他数字资产名称于转账备注，防止造成您的汇款被拦截，银行卡被冻结等问题。</li>
				<li>银行卡请使用及时到账的支付方式，1小时内未到账订单，卖家有权退回款项并取消订单。</li>
				<li>大于5w以上汇款，请分批支付保证能及时到账(&lt;5w汇款可及时到账)。</li> -->
				<li>1、下单前，请详细了解对方的交易信息，并确认交易内容无误后，再操作购买。</li>
				<li>2、下单后，请在30分钟内付款至对方指定账户，并在支付完成后在交易订单上点击【我已付款】，逾期系统将自动取消交易。</li>
				<li>3、完成付款后，若30分钟内未收到对方的应付交易资产，可联系TTC-OTC官方客服。</li>
				<li>4、在交易过程中，请务必使用站内聊天系统，避免资料外泄；同时平台外的对话记录将无法作为交易纠纷的依据。</li>
				<li>5、请按照卖家收款方式完成付款操作。</li>
				<li>6、若选择银行卡支付的，务必使用本人实名银行卡进行支付，并请使用及时到账的支付方式，1小时内未到账订单，卖家有权退回款项并取消订单；银行卡转账时请勿在转账备注填写任何比特币、BTC、以太坊、ETH等数字资产名称，防止出现您的汇款被拦截，银行卡被冻结等问题；大于5w以上汇款，请分批支付以确保能及时到账。</li>
				<li>7、下单后请履行契约精神，恶意抬价或者是反悔，被投诉将冻结账户。</li>
			</div>
		</div>
		<!--留言-->
		<div class="right">
			<div class="title">
				<div class="info">
					<img :src="datas.picUrl" class="head"/>
					<div class="names">
						{{datas.userName}}
						<div class="money"><img src="../../static/bzj.png"/>已缴保证金</div>
					</div>
				</div>
				<div class="num">
					成交 {{datas.payCount}}笔
					<div>成交率 {{datas.successRat}}%</div>
				</div>
			</div>
			<div class="text">广告留言</div>
			<div class="textcontent">{{datas.payTreaty}}</div>
		</div>
	</div>
</template>

<script>
	export default{
		data(){
			return{
				datas:'',
				payType: ["", "BIDT", "BTC", "ETH","CNY"],
				options_methods: [{
		          value: 'BTC',
		          label: 'BTC'
		        },{
		          value: 'ETH',
		          label: 'ETH'
		        },{
		          value: 'CNY',
		          label: 'CNY'
		        }],
		        value_methods: '',
						l_inp:'',
						toBackl_inp:"",
		        r_inp:'',
		        quota:'',
		        btn_sen:true
			}
		},
		created(){
			this.Getinfo()
		},
		watch : {
			l_inp (val) {
				this.toBackl_inp = val;
				// console.log(this.toBackl_inp);
				this.l_inp = this.toDecimal(val);
				return this.toDecimal(val);
			}
		},
		methods:{
			open(val) {
      this.$alert(val, "提示", {
        confirmButtonText: "确定",
        callback: action => {
          // this.$message({
          //   type: 'info',
          //   message: `action: ${ action }`
          // });
        }
      });
    },
			toDecimal(x1) {
      console.log(x1);
      if (isNaN(x1)) {
        let x = x1.split(" ")[0];
        var f = parseFloat(x);
        let x2 = x.split(".");
        f = x2[0] + "." + x2[1].slice(0, 8);
        return f;
      } else {
				
				let str = Math.ceil(x1 * 100000000) / 100000000;
				return str;

        // let f = x1.toString();
        // if (f.indexOf(".") == -1) {
        //   return f;
        // } else {
        //   let x2 = f.split(".");
				// 	// f = x2[0] + "." + x2[1].slice(0, 8);
				// 	f = x2[0] + "." + Math.floor(Number(x2[1].slice(0, 9)) / 10 );
        //   return f;
        // }
      }
    },
			//获取信息
			Getinfo(){
				let vm=this;
				$.ajax({
					type:"post",
					url:contextPath+"/api/push/deal/getPushDealOrderById",
					async:true,
					dataType:'json',
					data:{
						languageType: localStorage.otc_lang || "zh",
						token:localStorage.otc_token,
						orderId:vm.$route.query.id
					},
					success(res){
						vm.datas=res.data;
						if(res.data.list.length===1){
							vm.options_methods=[{value:vm.payType[res.data.list[0].payType],label:vm.payType[res.data.list[0].payType]}];
							vm.value_methods=vm.payType[res.data.list[0].payType];
//							vm.quota=Math.ceil(vm.datas.list[0].minQuota/vm.datas.list[0].unitPrice);
							vm.quota=vm.datas.list[0].minQuota;
							
						}else{
							vm.options_methods=[
								{value:vm.payType[res.data.list[0].payType],label:vm.payType[res.data.list[0].payType]},
								{value:vm.payType[res.data.list[1].payType],label:vm.payType[res.data.list[1].payType]}
							];
							vm.value_methods=vm.payType[res.data.list[0].payType];
//							vm.quota=Math.ceil(vm.datas.list[0].minQuota/vm.datas.list[0].unitPrice);
							vm.quota=vm.datas.list[0].minQuota;
						}
					}
				});
			},
			sele(){
				let vm=this;
				for (var i = 0; i < vm.datas.list.length; i++) {
					if(vm.value_methods===vm.payType[vm.datas.list[i].payType]){
//						vm.quota=Math.ceil(vm.datas.list[i].minQuota/vm.datas.list[i].unitPrice);
						vm.quota=vm.datas.list[i].minQuota;
					}
				}
				vm.left_inp()
			},
			left_inp(){
				let vm=this;
				for (var i = 0; i < vm.datas.list.length; i++) {
					if(vm.value_methods===vm.payType[vm.datas.list[i].payType]){
						if(vm.value_methods==='CNY'){
							vm.r_inp=(vm.datas.list[i].unitPrice*vm.l_inp).toFixed(2)
						}else{
							vm.r_inp=(vm.datas.list[i].unitPrice*vm.l_inp)
						}
					}
				}
			},
			right_inp(){
				let vm=this;
				for (var i = 0; i < vm.datas.list.length; i++) {
					if(vm.value_methods===vm.payType[vm.datas.list[i].payType]){
							vm.l_inp=parseFloat((vm.r_inp/vm.datas.list[i].unitPrice).toFixed(10))
					}
				}
			},
			//立即购买
			buy_send(){
				if(this.left_inp =="" || this.value_methods =="" || this.r_inp == "") {
					this.open("请填写完整！");
					return;
				}
				let vm=this;
//				payType: ["", "BIDT", "BTC", "ETH","CNY"],
//				if(vm.btn_sen!=false){
					vm.btn_sen=false;
					if(vm.value_methods==='BIDT'){
						vm.sends(1)
					}
					if(vm.value_methods==='BTC'){
						vm.sends(2)
					}
					if(vm.value_methods==='ETH'){
						vm.sends(3)
					}
					if(vm.value_methods==='CNY'){
						vm.sends(4)
					}
//				}
			},
			//购买下单
			sends(payType){
				console.log(1111111);
				console.log(this.toBackl_inp)
				console.log(2222222);
				let vm=this;
					$.ajax({
						type:"post",
						url:contextPath+"/api/push/deal/sellTTCOrderObtain",
						async:true,
						dataType:'json',
						data:{
							languageType: localStorage.otc_lang || "zh",
							token:localStorage.otc_token,
							orderId:vm.datas.uId,
							dealNum:vm.toBackl_inp,
							payType
						},
						success(res){
							if(res.state.code==='20000'){
								vm.btn_sen=true;
								mui.toast('购买下单成功！')
								vm.$router.push({
									path:'/PurchaseOrder',
									query:{id:res.data}
								})
							}
						}
					});
			}
		}
	}
</script>

<style lang="scss" scoped>
	.buyhim{
		box-sizing: border-box;
		padding: 0.65rem 1.5rem 0.5rem 1.5rem;
		display: flex;
		.left{
			.know{
				text-align: left;
				padding-left: 0.2rem;
				.know_title{
					font-size: 0.2rem;
					margin: 0.2rem 0;
					color: #282828;
					font-weight: 550;
				}
				li{
					list-style:none;
					font-size: 0.14rem;
					color: #333333;
					line-height: 0.3rem;
				}
			}
			.main{
				box-sizing: border-box;
				padding: 0.3rem 0.8rem 0.3rem 0.3rem;
				border-radius:0.06rem;
				box-shadow:2px 0px 8px rgba(171,171,171,1);
				display: flex;
				flex-direction: column;
				.title{
					text-align: left;
					font-size: 0.24rem;
					margin-bottom: 0.36rem;
				}
				.price,.quota{
					display: flex;
					align-items: baseline;
					.l_txt{
						width: 1.7rem;
						text-align: left;
						font-size: 0.2rem;
						color: #999999;
						line-height: 0.4rem;
					}
					.r_num{
						font-size: 0.2rem;
						text-align: left;
					}
				}
				.content{
					display: flex;
					.left_div{
						width:50%;
						.tit{
							font-size: 0.2rem;
							color: #43CB83;
							text-align: left;
							margin: 0.1rem 0;
							margin-bottom: 0.15rem;
							margin-top: 0.3rem;
							padding-left: 0.2rem;
						}
						.l_num{
							display: flex;
							align-items: flex-start;
							.inp{
								width: 100%;
								flex-direction: column;
								display: flex;
								.text{
									font-size: 0.16rem;
									color: #999999;
									text-align: left;
									padding-left: 0.2rem;
									margin-top: 0.1rem;
								}
							}
							i{
								color: #999999;
								margin: 0.07rem 0.2rem 0 0.2rem;
								font-size: 0.3rem;
								transform: rotate(90deg);
							}
						}
					}
					.right_div{
						width: 50%;
						display: flex;
						flex-direction: column;
						.tit{
							display: flex;
							font-size: 0.2rem;
							color: #43CB83;
							text-align: left;
							margin: 0.1rem 0;
							margin-top: 0.3rem;
							padding-left: 0.2rem;
							select{
								width: 1.08rem;
								height: 0.25rem;
								background:rgba(243,243,243,1);
								margin-left: 0.1rem;
							}
							.el-select{
								width: 120px;
								margin-top: -0.1rem;
								margin-left: 0.1rem;
							}
						}
						.texts{
							font-size: 0.16rem;
							color: #999999;
							text-align: left;
							padding-left: 0.2rem;
							margin-top: 0.1rem;
						}
						.text{
							text-align: right;
							display: flex;
							flex-direction: column;
							align-items: flex-end;
							margin-top: 0.4rem;
							.num1{
								display: flex;
								div{
									font-size: 0.16rem;
									color: #FF6600;
								}
								.num1_l{
									white-space: nowrap;
								}
								.num1_r{
									width: 2.5rem;
								}
							}
							span{
								font-size: 0.14rem;
								color: #999999;
								margin: 0.05rem 0;
							}
							.send_buy{
								cursor: pointer;
								width:2.2rem;
								height:0.4rem;
								font-size: 0.22rem;
								line-height: 0.4rem;
								text-align: center;
								background:rgba(68,158,212,1);
								border-radius:0.06rem;
								color: white;
								box-shadow:4px 0px 8px rgba(28,109,157,0.38);
							}
						}
					}
				}
			}
		}
		.right{
			height: 3rem;
			text-align: left;
			margin-left: 1.06rem;
			box-sizing: border-box;
			padding: 0.25rem 0.2rem;
			padding-bottom: 1rem;
			background:rgba(255,255,255,1);
			border-radius:0.06rem;
			box-shadow:2px 0px 8px rgba(171,171,171,1);
			.title{
				margin-bottom: 0.1rem;
				display: flex;
				align-items: center;
				.info{
					display: flex;
					align-items: center;
					.head{
						width: 0.4rem;
						height: 0.4rem;
						border-radius: 50%;
					}
					.names{
						margin-left: 5px;
						color: #3399FF;
						font-size: 0.2rem;
						.money{
							font-size: 0.12rem;
							color: #FF6600;
							display: flex;
							align-items: center;
							img{
								width: 0.12rem;
								height: 0.12rem;
							}
						}
					}
				}
				.num{
					border-left: 1px solid #999999;
					padding-left: 0.2rem;
					margin-left: 0.2rem;
					font-size: 0.16rem;
					div{
						color: #999999;
					}
				}
			}
			.text{
				text-align: left;
				font-size: 0.18rem;
				color: #999999;
				padding-bottom: 0.1rem;
				border-bottom: 1px solid #999999;
				width: 4.94rem;
				margin-bottom: 0.2rem;
			}
			.textcontent{
				font-size: 0.16rem;
			}
		}
	}
</style>