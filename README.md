## Refrence Slicer
Max_Toc_Maximum = DATATABLE(
    "Reference Slicer", STRING,
    {
        {"Max"},
        {"Toc"},{"Maximum"}
    }
)

## Group Inventory Quality (IQ) in Lakhs
Group Table PieChart = 
VAR _SelectedValue = SELECTEDVALUE(Max_Toc_Maximum[Reference Slicer])
VAR _Max = CALCULATE(sum('Group Table Pie chart'[Stock Value in Lakhs]),FILTER('Group Table Pie chart','Group Table Pie chart'[Max_TOC_Maximum]="Max"))
VAR _TOC = CALCULATE(sum('Group Table Pie chart'[Stock Value in Lakhs]),FILTER('Group Table Pie chart','Group Table Pie chart'[Max_TOC_Maximum]="TOC"))
VAR _Maximum = CALCULATE(sum('Group Table Pie chart'[Stock Value in Lakhs]),FILTER('Group Table Pie chart','Group Table Pie chart'[Max_TOC_Maximum]="Maximum"))

Return
if(_SelectedValue="Max",_Max,if(_SelectedValue="Toc",_TOC,_Maximum)) 

##  Inventory Excess Over Operating Level in Lakhs
Group Excess_WF_Chart Stockable = 
VAR _SelectedValue = SELECTEDVALUE(Max_Toc_Maximum[Reference Slicer])
VAR _MaxStockable = CALCULATE(sum('Group Table Excess'[Value]),filter('Group Table Excess','Group Table Excess'[Max_Toc_Maximum]="Max" && 'Group Table Excess'[Group_Max_Stck_Nonstck]="Stockable"))/100000
VAR _TocStockable = CALCULATE(sum('Group Table Excess'[Value]),filter('Group Table Excess','Group Table Excess'[Max_Toc_Maximum]="Toc" && 'Group Table Excess'[Group_Toc_Stck_Nonstck]="Stockable"))/100000
VAR _MaximumStockable = CALCULATE(sum('Group Table Excess'[Value]),filter('Group Table Excess','Group Table Excess'[Max_Toc_Maximum]="Maximum" && 'Group Table Excess'[Group_Maximum_Max_Toc_Stck_Nonstck]="Stockable"))/100000
Return
if(_SelectedValue="Max" ,_MaxStockable,if(_SelectedValue="Toc",_TocStockable,if(_SelectedValue="Maximum",_MaximumStockable)))

## Inventory Lower than Operating Level in Lakhs
Group New Short Max Value = 
Var _selectedvalue= SELECTEDVALUE('Max_Toc_Maximum'[Reference Slicer])
Var _TOC= CALCULATE(
    SUMX('Group Table Main', 'Group Table Main'[Rate] * 'Group Table Main'[Net_Short_Toc]))/100000
Var _MAX= CALCULATE(
   SUMX('Group Table Main', 'Group Table Main'[Rate] * 'Group Table Main'[Net_Short_Max]))/100000
Var _MAXIMUM= CALCULATE(
    SUMX('Group Table Main', 'Group Table Main'[Rate] * 'Group Table Main'[Net_Short_Maximum_Max_Toc]))/100000
Return
If(_selectedvalue="Max", _MAX,if(_selectedvalue="Toc",_TOC,_MAXIMUM))


## Inventory - Price Cut View in Lakhs
Group_PieChart Stock Value = 
VAR _SelectedValue = SELECTEDVALUE(Max_Toc_Maximum[Reference Slicer])
VAR _Max = CALCULATE(sum('Group Table Pie chart'[Stock Value in Lakhs]),FILTER('Group Table Pie chart','Group Table Pie chart'[Max_TOC_Maximum]="Max"&& 'Group Table Pie chart'[Stock Value in Lakhs]>0))
VAR _TOC = CALCULATE(sum('Group Table Pie chart'[Stock Value in Lakhs]),FILTER('Group Table Pie chart','Group Table Pie chart'[Max_TOC_Maximum]="TOC"&& 'Group Table Pie chart'[Stock Value in Lakhs]>0))
VAR _Maximum = CALCULATE(sum('Group Table Pie chart'[Stock Value in Lakhs]),FILTER('Group Table Pie chart','Group Table Pie chart'[Max_TOC_Maximum]="Maximum"&& 'Group Table Pie chart'[Stock Value in Lakhs]>0))

Return
if(_SelectedValue="Max",_Max,if(_SelectedValue="Toc",_TOC,_Maximum))

## Movement Code View in Lakhs
Group_PieChart Stock Value = 
VAR _SelectedValue = SELECTEDVALUE(Max_Toc_Maximum[Reference Slicer])
VAR _Max = CALCULATE(sum('Group Table Pie chart'[Stock Value in Lakhs]),FILTER('Group Table Pie chart','Group Table Pie chart'[Max_TOC_Maximum]="Max"&& 'Group Table Pie chart'[Stock Value in Lakhs]>0))
VAR _TOC = CALCULATE(sum('Group Table Pie chart'[Stock Value in Lakhs]),FILTER('Group Table Pie chart','Group Table Pie chart'[Max_TOC_Maximum]="TOC"&& 'Group Table Pie chart'[Stock Value in Lakhs]>0))
VAR _Maximum = CALCULATE(sum('Group Table Pie chart'[Stock Value in Lakhs]),FILTER('Group Table Pie chart','Group Table Pie chart'[Max_TOC_Maximum]="Maximum"&& 'Group Table Pie chart'[Stock Value in Lakhs]>0))

Return
if(_SelectedValue="Max",_Max,if(_SelectedValue="Toc",_TOC,_Maximum))

## Inventory Days
Full Inventory Days = 
IFERROR(
    DIVIDE(
        SUM('Group Table Main'[Stock Value Lakhs]) * 30,
        SUM('Group Table Main'[group sales value lk])
    ),
    0
)

