
> 策略名称

双均线DDI策略

> 策略作者

littleDreamXX

> 策略描述

- 策略思路：双均线确定方向，用DDI指标，确定进场时点，比例止损和回撤止盈
- 数据周期：15M
- 数据合约：指数合约
- 交易合约：主力合约
- 适合品种：沪银/沪镍/热卷/铁矿石/焦煤/焦炭/鸡蛋/郑醇/PP/螺纹/橡胶/锰硅/郑煤等
- 官方网站：www.quant.la

- 主图指标线：
  VAR2^^MA(C,PARAM2);
  VAR3^^MA(VAR2,PARAM1);

- 副图指标线：
  VAR9:MA(VAR8,2*PARAM1);
  VAR10:MA(VAR9,PARAM1);
  VAR8:VAR6-VAR7;

![IMG](https://www.fmz.com/upload/asset/e83641b0567b242687792b105de8e211.png)

> 策略参数



|参数|默认值|描述|
|----|----|----|
|PARAM1|60|均线2的参数|
|PARAM2|300|均线1的参数|
|PARAM3|true|价格系数|


> 源码 (麦语言)

``` pascal
(*backtest
start: 2017-01-01 00:00:00
end: 2017-02-20 00:00:00
period: 15m
exchanges: [{"eid":"Futures_CTP","currency":"FUTURES","balance":20000}]
*)

VAR1:=MAX(1,INTPART(MONEYTOT/(O*UNIT*0.1)));

VAR2^^MA(C,PARAM2);
VAR3^^MA(VAR2,PARAM1);

VAR4:=IFELSE((HIGH+LOW)<=(REF(HIGH,1)+REF(LOW,1)),0,MAX(ABS(HIGH-REF(HIGH,1)),ABS(LOW-REF(LOW,1)))),NODRAW;
VAR5:=IFELSE((HIGH+LOW)>=(REF(HIGH,1)+REF(LOW,1)),0,MAX(ABS(HIGH-REF(HIGH,1)),ABS(LOW-REF(LOW,1)))),NODRAW;
VAR6:=SUM(VAR4,PARAM1)/(SUM(VAR4,PARAM1)+SUM(VAR5,PARAM1)),NODRAW;
VAR7:=SUM(VAR5,PARAM1)/(SUM(VAR5,PARAM1)+SUM(VAR4,PARAM1)),NODRAW;
VAR8:VAR6-VAR7;
VAR9:MA(VAR8,2*PARAM1);
VAR10:MA(VAR9,PARAM1);

BUYK:=BARPOS>PARAM2 AND C>VAR2 AND VAR2>VAR3 AND VAR8>0 AND VAR9>VAR10;
SELLK:=BARPOS>PARAM2 AND C<VAR2 AND VAR2<VAR3 AND VAR8<0 AND VAR9<VAR10;

SELLY:=C<VAR2 AND C>BKPRICE*(1+0.01*PARAM3);
BUYY:=C>VAR2 AND C<SKPRICE*(1-0.01*PARAM3);

SELLS:=C<BKPRICE*(1-PARAM3*0.01);
BUYS:=C>SKPRICE*(1+PARAM3*0.01);

BKVOL=0 AND BUYK,BK(VAR1);
SKVOL=0 AND SELLK,SK(VAR1);

SELLS,SP(BKVOL);
BUYS,BP(SKVOL);

SELLY,SP(BKVOL);
BUYY,BP(SKVOL);

// DRAWCOLORKLINE(BKVOL=0 AND SKVOL=0,COLORWHITE,0);
// DRAWCOLORKLINE(SKVOL>0,COLORGREEN,0);
// DRAWCOLORKLINE(BKVOL>0,COLORRED,0);
// 累计盈亏..OFFSETPROFIT1,COLORWHITE,BOLD;
// PROFITSUM..OFFSETPROFIT1,COLORWHITE,BOLD;
//TRVAR10E_OTHER('AUTO');
```

> 策略出处

https://www.fmz.com/strategy/128133

> 更新时间

2018-12-05 11:47:14
