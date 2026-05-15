两个大题
# Plan Cost Management
Plan Cost Management is the process that establishes the policies, procedures, and documentation for **planning, managing, expending, and controlling** project costs
The key benefit of this process is that it provides **guidance** and **direction** on how the project costs will be managed throughout the project.

## Output
cost management plan

> Pecision VS Accuracy


![](assets/5%20Cost%20Management/file-20260515095601056.png)
# Estimate Costs
The process of developing an approximation of the **monetary** resources needed to complete project activities
The key benefit of this process is that it **determines the amount of cost** required to complete project work

Cost estimates are a **prediction** that is based on the **information** known
## TT
### Three-point estimating
#exam 考三角的
Most likely (cM)
Optimistic (cO)
Pessimistic (cP)

1) Triangular Distribution cE=(cO+cM+cP)/3
2) Beta Distribution cE=(cO+4cM+cP)/6
### Bottom-up estimating
a method of estimating a component of work

The cost of individual **work packages or activities** is estimated to the **greatest level** of specified detail. 
The detailed cost is then **summarized** or “rolled up” to higher levels for subsequent reporting and tracking purposes.
![](assets/5%20Cost%20Management/file-20260515101449232.png)
## Output
1. Activity Cost Estimates
	1. Activity cost estimates are **quantitative assessments**
	2. can be presented in summary form or in detail.
2. Basis of Estimates
## TT
### Software project cost estimating
1. Lines Of Code (LOC) 
2. Function point (FP) 
3. Expert estimation method 
4. A practical software cost estimationprocess

Software project scale measurement unit
- LOC
	- Advantage:
		- Intuitive and accurate
		- the easy-to-calculate
	- Disadvantage:
		- no accepted measure of lines of code in standard definition
		- difficult to accurately estimate
- FP
	- Count the external and internal functions of the system. (**Unadjusted Function Point Count**)
	- According to the **technical complexity factor**, they are adjusted to produce the product scale measurement results.
#### FP计算
#exam 要考计算
FP =UFC`*`TCF  
- UFC(Unadjusted Function Point Count) 
- TFC(Technical Complexity Factor)

> UFC
> 根据complexity X 数量加起来

![](assets/5%20Cost%20Management/file-20260515145502794.png)![](assets/5%20Cost%20Management/file-20260515145509548.png)

> TCF

![](assets/5%20Cost%20Management/file-20260515145620551.png)

TCF=0.65+0.01(sum(Fi))：Fi:0-5,TCF:0.65~1.35

sum(Fi)=`1+5+0+3+1+0...`=22
TCF=0.65+0.01(sum(Fi))=0.65+0.01`*`22=0.87

#### Expert estimation method-Delphi
#exam 选择题
给专家做问卷，填表格，让他们评估
- Minimum ai 
- The most likely value mi 
- Maximum bi

Calculating the average estimate of each expert.
Ei =(ai+4mi + bi)/6 and 
total average E=(E1+E2+…+En)/n (n represents n experts)
#### A practical software cost estimationprocess:
1. Task decomposition(activity): T1, T2,…,Ti,…,Tn
2. Estimate the cost of each task Ci
	- Ci= Ei * Human (workload of the task Ei)
	- For example: if a software project workload is 3 person-months, and the company's human cost parameter is 20,000 yuan / person-months, the cost of the project is $ 60,000
3. Direct cost of the project =C1+C2+…+Ci+…+Cn
	- Development workload: Effort(Dev)
	- Management and quality work : Effort(Mgn)=a`*`Effort(Dev)
	- Direct cost = Effort(Dev) + a`*`Effort(Dev)
4. Indirect cost estimation
	- Indirect costs = Direct costs `*`Indirect cost factor
5. Total project cost = direct cost + indirect cost
	- = workload * human cost parameters （1+Indirect costcoefficient)
	- = workload * Cost coefficient
	- For example: The workload of a project is 40 months, Cost coefficient of 20 thousand yuan/man-month, the total estimatedcost of the project is 40 * 20=800 thousand.
# Determine Budget
The process of **aggregating** the **estimated costs** of individual activities or work packages to establish an authorized **cost baseline**.

## Output
cost baseline
- is the approved version of the **time-phased project budget**, **excluding** any **management reserves**
S-curve
# Control Costs
## TT
### Earned Value Management （EVM）
#exam 大题，计算
花销超了怎么解决

PV(Planned value)计划花的钱
EV(Earned value)工作价值
AC(Actual cost)实际花销
SV(Schedule variance)
CV(Cost variance)
SPI(Schedule Performance Index)
CPI(Cost Performance Index)

BAC(Budget at completion)
planned % complete

PV=BAC * planned
EV=BAC * actual

SV=EV-PV (>0，超前完成)
CV=EV-AC (>0，干的多花的少)
SPI=EV/PV (<1，干得少)
CPI=EV/AC (<1， 花的多)

> Example

100页文章，500元，限时10天
第六天时：
PV=300
EV=100（做了2页）
AC=600（实际花了600）

### Forecasting
Budget at completion (BAC) 完工预算 
Estimate at completion (EAC)完工估算
Estimate to complete (ETC) 完工尚需估算（到结束的时候还需要的预算）

EAC=AC+ETC

#exam 必考，根据文字描述选择公式，不考第三个
work performed at the budgeted rate
- EAC = AC+ (BAC–EV)
work performed at the present CPI
- EAC = BAC/CPI
work considering both SPI and CPI factors
- EAC = AC+ [(BAC-EV)/(CPI×SPI)]
VAC = BAC– EAC
### To-Complete Performance Index (TCPI)
TCPI=(BAC – EV)/(BAC – AC)
TCPI=(BAC – EV)/(EAC – AC)