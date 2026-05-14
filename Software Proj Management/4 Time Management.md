# Importance and Complexity of Time Management
**Completing** the project **on time** is one of the **biggest challenges**
The **problem of schedule** is the main cause of project **conflict**
Time is the **least flexible** factor in project planning
![](assets/4%20Time%20Management/file-20260512150058197.png)
# Plan schedule management
Plan Schedule Management is the process of **establishing** the **policies, procedures, and documentation** for **planning, developing, managing, executing, and controlling** the project schedule. 
The key benefit of this process is that it provides guidance and direction on **how the project schedule will be managed throughout the project**.

Schedule Management Plan:
A **component of the project management plan** that establishes the criteria and the activities for **developing, monitoring, and controlling the schedule**.

ITTO应该不重要
## Inputs
1. Project management plan
2. Project charter
3. Enterprise environmentalfactors
4. Organizational processassets
## Tools & Techniques
1. Expertjudgment
2. Analytical techniques
3. Meetings
## Outputs
1. Schedule managementplan
# Define Activities
Define Activities is the process of **identifying** and **documenting** the **specific actions** to be performed to produce the project **deliverables**. 
The key benefit of this process is to **break down work packages into activities** that provide a basis for estimating, scheduling,executing, monitoring, andcontrolling the project work.

**Work packages** are typically decomposed into **smaller components** called **activities** that represent the work effort required to complete the work packages.

> WBS->Work packages->Activity Decomposition->Activity

## Inputs
1. Schedule management plan
2. Scope baseline
3. Enterprise environmental factors
4. Organizational process assets
## Tools & Techniques
1. Decomposition
2. Rolling wave planning
3. Expert judgment
## Outputs
1. Activity List 
2. Activity Attributes 
3. Milestone List
	1. A **milestone** is a **significant point** or event inaproject.
	2. **Milestones** are similar to regular **schedule activities**, with the same structure and attributes, but they have **zero duration** because milestones represent a **moment** in time.
# Sequence Activities
Sequence Activities is the process of **identifying** and **documenting** the **relationships** among the project **activities**.
The key benefit of this process is that it defines the **logical sequence** of work to obtain the greatest efficiency given all project constraints.

## Input
2,3,4框起来了
1. Schedule management plan
2. Activity list
3. Activity attributes
4. Milestone list
5. Project scope statement
6. Enterprise environmental factors
7. Organizational process assets
## TT
1. **Precedence diagramming method**(PDM)
	1. Finish-to-start (FS) 前一个结束，后一个才能开始
	2. Finish-to-finish (FF) 
	3. Start-to-start (SS) 
	4. Start-to-finish (SF)
2. Dependency determination
	- Mandatory/discretionary:
		- Mandatory: often involved physical limitations
		- discretionary: based on knowledge of best practices within a particular application area
	- external/internal:
		- external: relationship betweenproject activities and non-project activities
		- internal: Involve a precedence relationship between project activities
3. Leads and lags
	- Lag: 前一个活动完成后，必须停止一段时间，才能开始下一个（记作SS +15 Days）
## Output
#exam 要考Project schedule network diagram
1. Project Schedule NetworkDiagramsn 
2. Project Documents Updates
![](assets/4%20Time%20Management/file-20260512195027987.png)
# Estimate Activity Durations
The process of estimating the **number** of **work periods** needed to complete individual activities with estimated resources
The key benefit of this process is that it provides the amount of time each activity will take to complete, which is a **major** **input** into the Develop Schedule process.
## Estimate Activity Resources
The process of estimating the **type** and **quantities** of **material, human resources, equipment, or supplies** required to perform each activity.
The key benefit of this process is that it **identifies** the **type, quantity, and characteristics** of **resources** required to complete the activity which allows more accurate cost and duration estimates.
## Input
1. Schedule management plan
2. Activity list
3. Activity attributes
4. Activity resource requirements
5. Resource calendars
6. Project scope statement
7. Risk register
8. Resource breakdown structure
9. Enterprise environmental factors
10. Organizational process assets
## TT
#exam 要考选择题，Three-point estimating默认用beta
2,3,4,6框起来了
1. Expert judgment 
2. Analogous estimating 
	- 使用historical data from a similar activity or project来估计
	- A **gross** value estimating approach, 
	- when there is a **limited amount** of detailed information
3. Parametric estimating 
	- `Duration time = quantity of work × labor hours per unit of work`
4. Three-point estimating 
	- Most likely (tM)，Optimistic (tO)，Pessimistic (tP)
	- Triangular Distribution: `tE=(tO+tM+tP)/3`
	- Beta Distribution: `tE=(tO+4tM+tP)/6`
5. Group decision-making techniques
6. Reserve analysis
	- Reserve analysis is used to determine the **amount** of **contingency** and **management** **reserve** needed for the project
		- Contingency reserves: The **estimated duration** within the **schedule baseline**, which is allocated for **identified risks** that are **accepted**, it is associated with the “**known-unknowns**”. should be **clearly identified** in the **schedule documentation**
		- Management reserves: A **specified amount of the project budget withheld for management control purposes** and are reserved for **unforeseen work** that is within scope of the project. Management reserves are intended to address the “**unknown-unknowns**” that can affect a project. **not included** in the **schedule baseline**
## Output
1. Activity Duration Estimates
	- Activity duration estimates mayincludesomeindication of the range of possible results:
		- 2 weeks ±2 days,
2. Project Documents Updates
# Develop Schedule
The process of **analyzing activity sequences, durations, resource requirements**, and schedule constraints to **create** the project **schedule model**.

The key benefit of this process is that by entering schedule **activities, durations, resources, resource availabilities**, and **logical relationships** into the scheduling tool, it generates a **schedule model** with **planned dates** for completing project activities.

## Input
略
## TT
#exam 要考critical path的大题
1. Schedule network analysis 
	- A technique that generates the project schedule model. 
	- It employs several other techniques, such as **critical path method, critical chain method, resource optimization techniques， and modeling** techniques to **calculate** the **early** and **late** **start** and **finish dates** for the uncompleted portions of project activities.
2. Critical path method 
3. Critical chain method
4. Resource optimization techniques
5. Schedule compression

### Critical path method
A method used to estimate the **minimum project duration** and determine the **amount of scheduling flexibility** on the logical network paths within the schedule model.

![](assets/4%20Time%20Management/file-20260512210440217.png)

`critical path`：最长的路径（耗时最多）

> 计算Total float

1. 计算critical path，给其中的每个任务float=0
2. 找到第二长的path，每个任务float=critical path-当前path的duration
3. 再找到下一条的，同理计算。如果有交叉的节点，float保留最小的结果
![](assets/4%20Time%20Management/file-20260512211349156.png)

![](assets/4%20Time%20Management/file-20260512211415110.png)
> 初始状态：
> 如果前一个任务在第6天结束，下一个任务的最早开始时间是7

![](assets/4%20Time%20Management/file-20260512211503767.png)
> 其他path的最晚时间需要回推：
> 从后往前计算
![](assets/4%20Time%20Management/file-20260512211653293.png)

### Critical chain method
A method that allows the project team to **place buffers** on any project schedule path to account for limited resources and project uncertainties.

It is developed from the **critical path method** approach and considers the **effects of resource allocation, resource optimization, resource leveling**, and **activity duration uncertainty** on the critical path determined using the critical path method

#### Project buffer vs Feeding buffers
Feeding buffers只能加在进入critical chain的时候
 Project buffer 会影响critical chain，影响整个项目的完成时间
![](assets/4%20Time%20Management/file-20260512215637746.png)
### Resource optimization techniques
Used to **adjust the start and finish dates** of activities to **adjust planned resource** use to be equal to or less than resource availability.
> Resource leveling vs Resource smoothing

Resource leveling
- A technique in which start and finish dates are adjusted **based on resource constraints** with the goal of balancing demand for resources with the available supply.
- often cause the original **critical path to change**, usually to **increase**.

> Example

![](assets/4%20Time%20Management/file-20260514161105678.png)
![](assets/4%20Time%20Management/file-20260514161114305.png)

Resource smoothing
- 