#### case when

case 具有两种格式

##### 简单case语句

~~~sql
SELECT CASE v.REPAYMETHOD
WHEN 'MonthlyInterest' THEN '按月付息,到期还本'
WHEN 'EqualInstallment' THEN '按月等额本息'
WHEN 'EqualPrincipal' THEN '按月等额本金'
WHEN 'BulletRepayment' THEN '一次性还本付息'
WHEN 'SeasonInterest' THEN '按季付息到期还本'
WHEN 'HalfYearInterest' THEN '每半年付息'
ELSE '没有相关的还款方式'
END
FROM TB_INVEST v;
~~~

##### case搜索函数

~~~sql
CONCAT(
		CASE
		WHEN P.YEARS = 0 THEN
			''
		ELSE
			CONCAT(P.YEARS, '年')
		END,
		CASE
	WHEN P.MONTHS = 0 THEN
		''
	ELSE
		CONCAT(P.MONTHS, '月')
	END,
	CASE
WHEN P.DAYS = 0 THEN
	''
ELSE
	CONCAT(P.DAYS, '日')
END
	)
~~~

