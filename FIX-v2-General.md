##火币网 FIX API简介
###FIX协议介绍
FIX协议是适用于实时证券、金融电子交易开发的数据通信标准在各类参与者之间，包括投资经理、经纪人，买方、卖方建立起实时的电子化通讯协议。它提供了一个ssl加密消息传递的可靠数据流，为用户提供了一个可靠和快速的消息传输机制。火币网FIX协议实现了Fix4.4版本，完整支持了Fix4.4的市场行情和交易。
###火币网FIX 信息格式
一条FIX协议信息的基本格式是：《标准头》+《信息正文域》+《标准尾》每条信息都是由一系列带有〈标记(Tag)〉=〈值(Value)〉的域组成的。在每个域之间通过"<>"分开。除了一些特殊规定外，信息中的域可按照任意顺序排列。所有域在都以"定界符(|)"表示终止。

```
8=FIX.4.4|9=144|35=V|34=2|49=234abcxyzc|52=20140411-13:49:41.295|56=EXCHANGE|262=md1397224181279| 263=1|264=100|146=1|55=BTC/CNY|267=6|269=4|269=5|269=7|269=8|269=9|269=B|10=249|
```