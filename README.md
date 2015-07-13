## 每日00：05定时更新
- [配置日志文件](#confLogger)
    - 创建日志文件，路径：
      - linux:'/koolla/log/crawler-%s.log'   
      - win:'crawler-%s.log'
    - 基本配置 设置等级等
- [爬虫入口](#crawlerEntry)
    - 创建 基金爬虫，银行爬虫，互联网爬虫
    - 多线程运行
    - 基金爬虫和互联网爬虫爬到的内容写入csv文件
- [每日更新入口](#updatedaily)
    - [基金](#fundupdate)
    - [互联网](#inetupdate)
    - [银行](#bankupdate)
    - [总资产](#capitalupdate)
    - [静态数据](#staticupdate)
    - [周得分](#pointupdate)
    - [清理工作](#clean)

####每日更新入口
#####基金
- 从csv文件中获取数据 fundName， netValue， monthlyRise  计算净值总和
- 更新基金  
    - game_fund 更新条目或者创建新条目（由基金名字获取）
    - game_daily_fund 更新或者创建
- 更新基金订单
    - 获取订单list 状态为0，2，3 
    - 计算昨日收益 份额乘以净值差
    - game_order `cost, earning`
    - game_accountdetailfund  `create`
    - game_user `capital_sum`
    - game_accountdetailcapital
    - 更新已经卖出的订单收益
- 更新订单状态
    - 获取订单记录列表 `triggerDay == date.today()`
    - game_orderrecord `triggerDay(=(2000,1,1))`
    - game_acebankuser `capital_sum`
    - game_order `cost, yestEarning, earning` 这里先昨日收益先减去买入基金的手续费
    - game_orderrecord `amount(-trading_fee) create(BUY_FEE)`
    - game_order `shares(+add_shares)  !!status`
    - game_accountdetailfund
    - game_addtransactionmessage
    - game_order `cost, yestEarning, earning` 这里把差价加到昨日收益里面
    - game_accountdetailfund
    - game_acebankuser


####买入操作
- 基金 参数：产品类型，产品ID， 用户ID， 交易数量
  - 获取买入的产品，并储存到data中
  - 检查合法性（只与银行有关）
  - 生成订单
    - 如果已经有该类订单（除去状态为已经卖出的），则更新订单状态和订单更新时间，修改cost，并保存。
    - 如果没有，则生成改订单，这里cost和capital都设置为交易数量
  - 修改用户信息 修改cash
  - 创建账户详细买入基金的信息 产品从产品表中获取
  - 增加积分，发送信息（创建新的信息条目）
- 互联网产品 具体步骤类似于基金 需要注意每日收益和七日年化收益
- 银行产品 注意预期收益
