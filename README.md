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
    - [基金更新]（#fundupdate）
    - [互联网更新](#inetupdate)
    - [银行更新](#bankupdate)
    - [总资产更新](#capitalupdate)
    - [静态数据更新](#staticupdate)
    - [周得分更新](#pointupdate)
    - [清理工作](#clean)

####每日更新入口
#####基金更新
- 
