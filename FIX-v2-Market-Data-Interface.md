##3. Market Data Interface(行情接口)
####3.1. MarketDataRequest(V)-行情订阅请求消息
用户需发送一个订阅行情数据请求，MarketDataRequest(V)。服务端根据用户订阅的事件给用户推送实时行情信息。MarketDataRequest(V)

行情订阅分为三种类型订阅:
1. Order book request （买卖盘订阅 ）   (NoMDEntryTypes&lt;267&gt;=2,MDEntryType&lt;269&gt; is 0,1)
2. Trade request   （实时交易订阅） (NoMDEntryTypes&lt;267&gt;=1,MDEntryType&lt;269&gt; is 2)
3. 24 hour ticker request （24 th ticker订阅）(NoMDEntryTypes&lt;267&gt;=6,MDEntryType&lt;269&gt; is 4,5,7,8,9,B)<table class="table table-bordered">
	<thead>
		<tr>
			<th>消息名称</th>
			<th>字段名称</th>
			<th width='20%'>是否必填</th>
			<th>字段说明</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<th rowspan="7">MarketDataRequest(V)</th>
			<th>&lt;NoRelatedSym&gt;</th>
			<td>Y</td>
			<td>重复组 NoRelatedSym，一直为1</td>
		</tr>
		<tr>
			<th>Symbol</th>
			<td>Y</td>
			<td>btc/cny 或ltc/cny（火币自定义）</td>
		</tr>
		<tr>
			<th>MDReqID</th>
			<td>Y</td>
			<td>指配给该请求的唯一ID，或前一个MarketDataRequest的ID以关闭订阅</td>
		</tr>
		<tr>
			<th>SubscriptionRequestType</th>
			<td>Y</td>
			<td>0 = 快照模式<br>
				1 = 快照+更新（订阅）<br>
				2 = 禁用上一个快照 + 更新请求（取消订阅）
			</td>
		</tr>
		<tr>
			<th>MarketDepth</th>
			<td>Y</td>
			<td>只在订单更新请求时有用，其他情况忽略。<br>
				0 = 由服务器端确定每次推送的深度<br>
				N = (1-150)每次推送N个深度的数据
			</td>
		</tr>
		<tr>
			<th>&lt;NoMDEntryTypes&gt;</th>
			<td>Y</td>
			<td>重复组NoMDEntryTypes。请求中实体类别的数量</td>
		</tr>
		<tr>
			<th>MDEntryType</th>
			<td>Y</td>
			<td>注册websocket行情深度数据(marketDepthTop)时必须包含MDEntryType.BID、MDEntryType.OFFER		注册websocket交易明细数据(tradeDetail)时必须包含MDEntryType.TRADE		注册websocket市场概况数据(marketOverview)时必须包含MDEntryType.OPENING_PRICE、MDEntryType.CLOSING_PRICE、MDEntryType.TRADING_SESSION_HIGH_PRICE、MDEntryType.TRADING_SESSION_LOW_PRICE、MDEntryType.TRADING_SESSION_VWAP_PRICE、MDEntryType.TRADE_VOLUME</p></td>
		</tr>
	</tbody>
</table>
####3.2. MarketDataSnapshotFullRefresh(W)-行情刷新消息
发送了MarketDataRequest(V)订阅行情消息，服务将实时推送行情数据MarketDataSnapshotFullRefresh(W)消息，实时发送行数据。
<table class="table table-bordered">
	<thead>
		<tr>
			<th>消息名称</th>
			<th>字段名称</th>
			<th>是否必填</th>
			<th>字段说明</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<th rowspan="8">MarketDataSnapshotFullRefresh(W)</th>
			<th>OrigTime</th>
			<td>Y</td>
			<td>消息响应时间</td>
		</tr>
		<tr>
			<th>Symbol</th>
			<td>Y</td>
			<td>btc/cny 或ltc/cny</td>
		</tr>
		<tr>
			<th>MDReqID</th>
			<td>N</td>
			<td>请求ID</td>
		</tr>
		<tr>
			<th>&lt;NoMDEntries&gt;</th>
			<td>Y</td>
			<td>行情Group组</td>
		</tr>
		<tr>
			<th>WV</th>
			<td>Y</td>
			<td>0=Bid卖方<br>
				1=Offer买方<br>
				2=Trade成交<br>
				4=OpeningPrice<br>
				5=ClosingPrice<br>
				6=Trading Session High Price&lt;44&gt;(最高价)<br>
				7= Trading Session Low Price&lt;44&gt;<br>（最低价）<br>
				8= Trading Session VWAP Price&lt;44&gt;
			</td>
		</tr>
		<tr>
			<th>MDEntryPx</th>
			<td>N</td>
			<td>成交价格</td>
		</tr>
		<tr>
			<th>MDEntrySize</th>
			<td>N</td>
			<td>交易数量</td>
		</tr>
		<tr>
			<th>Side</th>
			<td>N</td>
			<td>买卖方<br>
				1=Buy<br>
				2=Sell
			</td>
		</tr>
	</tbody>
</table>

