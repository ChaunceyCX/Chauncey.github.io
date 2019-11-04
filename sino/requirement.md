# 需求文档的一些标注

## 名词及联系

1. 渠道 是什么渠道
2. 手续费是对于什么操作的手续费
3. 协议？？？
4. 渠道跟影像文件是什么关系，影像文件是关于什么的
5. 费用类型是什么是的费用类型
6. 副渠道？？？

## 接口对接

### 协议类型和渠道类型有点混乱 , riskCode和productCode混乱, 产品,生效时间,失效时间没有相关字段

### 中介协议查询(数据字段映射)

请求类型: POST

入参: 
- comCode (机构码):prpdcompany--COMCODE [string]
- ~~agentType (渠道类型): prpsq_channel--channel_type~~(channel_type有三个值:1,2,3,兼业,专业,个人)
- agentType (渠道类型): prpsq_agreement:type
- agentCode(渠道代码): prpsq--id ???
- agentName(渠道名称): prpsq--name
- queryDate(查询日期): 

出参:

- head(返回头): SalesHeadBean
    - status(查询状态)
    - appCode(请求返回码)
    - appMessage(返回信息)
  
- retAgentAgreementInfo(返回体,List) (RetAgentAgreementInfoBean)
    - agentCode(渠道代码:id)
    - agentName(渠道名称:name)
    - comCode(归属机构代码:prpdcompany--COMCODE)
    - agentType(渠道类型) ???
    - startDate---queryDate
    - endDate---查询完出接口时时刻
    - BDInfo(销售人员信息,List---prpsq_channel_person_relation,prpsq_channel,prpsr_user): RetBokerInfoBean
    - BDCode(销售人员代码)---prpsr_user:id
    - BDName(销售人员姓名)---prpsr_user: username
    - BDMobile(销售人员手机号)---prpsr_user: phone
    - BDTeamCode(销售人员团队代码)---prpsr_user_group_links,prpsu_team:team_id
    - BDTeamName(销售人员团队名称)---prpsr_user_group_links,prpsu_team:team_name
    - BDProfitCode(利润中心代码)---prpsu_team_profit_center_links,prpsr_profits_center:id
    - BDProfitName(利润中心名称)---prpsu_team_profit_center_links,prpsr_profits_center:name
    - agreement(协议信息,List)---prpsq_agreeement
      - agreementNo(协议代码):---id
      - agreementCode(渠道协议代码):---channel_agreement_id
      - agreemetName: ---name
      - type(smallint):---type
      - priority_number(int):---priority_number
      - startDate:---start_date
      - endDate:---end_date
      - agreementFee(渠道信息,List,Agreement) :---prpsq_agreement_product
        - agreementNo : ---agreement_id
        - riskCode :---product_id
        - productCode :---product_id ???
        - fee_limit_rate: fee_limit_rate

### 佣金查询

入参:

- riskCode (Y,产品): ---prpsq_agreement_product : product_id
- productCode()
- planCode
- agentCode(Y,渠道代码): ---prpsq_agreement_product : agreement_id

出参:

- code
- message
- data(List<CommissionDetailVo>)
  - agentCode: ---prpsq_agreement: id
  - feeType : ---prpsq_agreement: cost_type
  - agentFlag : ???
  - commRate : ---prpsq_agreement: ???
  - limitRate : ---prpsq_agreement_product: fee_limit_rate
  - effDate: ???
  - invaildDate :  ???
  - agentType : ---prpsq_agreement: type