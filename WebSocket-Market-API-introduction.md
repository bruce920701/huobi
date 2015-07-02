##2.行情API使用说明
###2.1接口说明

Socket.IO实时行情接口分为三类：第一类服务能力接口，第二类实时数据接口，第三类历史数据接口，所支持的消息如下：

####1)	服务能力接口：

	reqSymbolList：请求交易代码列表。<br>
	reqSymbolDetail：请求交易代码详细信息。<br>
	reqMsgSubscribe：推送消息订阅。<br>
	reqMsgUnsubscribe：推送消息取消订阅。

####2)	实时数据消息： 

	marketDetail：推送盘口数据。<br>
	tradeDetail：推送交易明细数据。<br>
	marketDepthTop：推送top行情深度数据：150条。<br>
	marketDepthTopShort：推送短的top行情深度数据：20条。<br>
	marketDepth：推送行情深度数据。<br>
	marketDepthTopDiff：推送top行情深度差量数据。<br>
	marketDepthDiff：推送行情深度差量数据。<br>
	lastKLine：推送最后一个k线数据。<br>
	lastTimeLine：推送最后一个分时数据。<br>
	marketOverview：推送市场概况数据。<br>
	marketStatic：推送市场统计信息数据：暂不支持。


####3)	历史数据消息：


	reqTimeLine：请求历史分时线。<br>
	reqKLine：请求历史k线。<br>
	reqMarketDepthTop：请求top行情深度。<br>
	reqMarketDepth：请求行情深度。<br>
	reqTradeDetailTop：请求top交易明细。<br>
	reqMarketDetail：请求盘口信息。


###2.2 接口常量定义

	接口中各个字段的定义请参见《火币行情服务器-开放接口设计.xlsx》中的《常量、JSON字段定义》，其中包含JSON字段定义，消息名称，错误代码，以及其他一些可能用到的常量。<br>
	<a href="https://news.huobi.com/download/socket_doc.zip">下载地址</a>


###2.3 行情API演示

	Node.js的演示程序，请参见：huobi-hq-push-demo.js。<br>
	请先安装node，然后安装socket.io-client: npm install socket.io-client@0.9.16.<br>
	最后在node命令行下执行:node huobi-hq-push-demo.js<br>
	huobi-hq-push-demo.js<br>
	<a href="https://news.huobi.com/download/socket_demo_js.zip">下载地址</a>


