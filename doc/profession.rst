.. _profession:

TqSdk 专业版
=================================================
TqSdk 中大部分功能是供用户免费使用的, 同时我们也提供了 TqSdk 专业版的增值功能供用户选择

如果想使用 TqSdk 专业版功能，可以登录 `个人中心 <https://account.shinnytech.com/>`_ 申请15天试用或正式购买

更稳定的行情服务器
-------------------------------------------------
在每次的行情服务器升级当中，我们会优先选择连接到免费版行情服务器中的百分之十左右的用户进行升级，然后在稳定后逐步扩大免费版的升级范围

专业版的行情服务器会在免费版全部升级成功且没有问题之后再进行升级，因此对于 TqSdk 的专业版用户来说，会有更稳定行情服务器连接

更多的实盘交易账户数
-------------------------------------------------
对于 TqSdk 免费版，每个信易账户支持最多绑定一个实盘账户，而天勤量化专业版支持最多一个信易账户绑定3个实盘账户

信易账户会在用户使用实盘账户时自动进行绑定，直到该信易账户没有能绑定实盘账户的名额(自动绑定功能需要 TqSdk 版本> 1.8.3)

如果需要注册信易账户或者修改您的信易账户绑定的实盘账户，请点击 `登录用户管理中心 <https://account.shinnytech.com/>`_

登录成功后显示如下，在下方红框处,用户可以自行解绑/绑定实盘账户，其中解绑操作每天限定一次

.. figure:: images/user_web_management.png

如需一个信易账户支持更多的实盘账户，请联系工作人员进行批量购买 `个人中心 <https://account.shinnytech.com/>`_

策略回测功能
-------------------------------------------------
:ref:`backtest` 是 TqSdk 专业版中的功能，能让用户在不改变代码的情况下去回测自己的策略在历史行情的表现，并且提供对应的web界面来统计用户的回测表现

.. figure:: images/web_gui_backtest.png

对于 TqSdk 免费版本的用户，每天可以进行3次回测，同时也可以申请模拟账户后模拟运行来检验策略 :ref:`sim_trading`

股票行情
-------------------------------------------------
TqSdk 免费版本提供全部的期货、商品/金融期权和上证50、沪深300和中证500的实时行情

购买或申请 TqSdk 专业版试用后可提供A股股票的实时和历史行情，TqSdk 中股票示例代码参考如下::

	SSE.600000 - 上交所浦发银行股票编码
	SZSE.000001 - 深交所平安银行股票编码
	SSE.000016 - 上证50指数
	SSE.000300 - 沪深300指数
	SSE.000905 - 中证500指数
	SSE.510050 - 上交所上证50etf
	SSE.510300 - 上交所沪深300etf
	SZSE.159919 - 深交所沪深300etf
	SSE.10002513 - 上交所上证50etf期权
	SSE.10002504 - 上交所沪深300etf期权
	SZSE.90000097 - 深交所沪深300etf期权


.. _profession_tqkqstock:

股票模拟交易
-------------------------------------------------
TqSdk 提供了 :py:class:`~tqsdk.account.TqKqStock` 方法供用户来进行股票的模拟交易

专业版用户可以长久的对同一账户进行模拟股票交易测试

需要注意股票模拟交易下，get_account，get_order，get_position 会返回对应股票交易模型下的 objs ，如 :py:class:`~tqsdk.objs.SecurityAccount`， :py:class:`~tqsdk.objs.SecurityOrder`，:py:class:`~tqsdk.objs.SecurityPosition`

参考代码如下::

    from tqsdk import TqApi, TqAuth, TqKqStock

    tq_kq_stock = TqKqStock()
    api = TqApi(account=tq_kq_stock, auth=TqAuth("信易账户", "账户密码"))
    quote = api.get_quote("SSE.688529")
    print(quote)
    # 下单限价单
    order = api.insert_order("SSE.688529", volume=200, direction="BUY", limit_price=quote.ask_price1)
    while order.status == 'ALIVE':
        api.wait_update()
        print(order)  # 打印委托单信息

    print(api.get_account())  # 打印快期股票模拟账户信息

    print(api.get_position("SSE.688529"))  # 打印持仓信息

    for trade in order.trade_records.values():
        print(trade)  # 打印委托单对应的成交信息
    api.close()


下载数据功能
-------------------------------------------------
数据下载工具 :py:meth:`~tqsdk.tools.downloader` 是 TqSdk 专业版中的功能

支持专业版用户下载目前 TqSdk 提供的全部期货、期权和股票类的历史数据，下载数据支持 tick 级别精度和任意 kline 周期

其他相关函数
-------------------------------------------------
 :py:meth:`~tqsdk.api.TqApi.query_symbol_ranking` 交易所每日成交持仓排名

 :py:meth:`~tqsdk.api.TqApi.get_kline_data_series` 以起始日期获取 Dataframe 格式的 kline 数据

 :py:meth:`~tqsdk.api.TqApi.get_trading_status` 获取指定合约的交易状态，帮助用户实现开盘/跨小节抢单

期权交易 & 交易所组合
-------------------------------------------------
TqSdk 中期权交易(商品期权、金融期权)和交易所官方组合也是 TqSdk 专业版中提供的功能

详细期权说明请点击 :ref:`option_trade`

TqSdk 中期权和交易所组合合约代码参考如下::

	DCE.m1807-C-2450 - 大商所豆粕期权
	CZCE.CF003C11000 - 郑商所棉花期权
	SHFE.au2004C308 - 上期所黄金期权
	CFFEX.IO2002-C-3550 - 中金所沪深300股指期权
	SSE.10002513 - 上交所上证50etf期权
	SSE.10002504 - 上交所沪深300etf期权
	SZSE.90000097 - 深交所沪深300etf期权
	CZCE.SPD SR901&SR903 - 郑商所 SR901&SR903 跨期合约
	DCE.SP a1709&a1801 - 大商所 a1709&a1801 跨期合约

工作时间内的天勤客服支持
-------------------------------------------------
如果您是 TqSdk 专业版的年费用户，那么我们将会单独为您建立一个讨论组，里面会有 TqSdk 的专门技术支持人员在工作时间内优先回答您的问题