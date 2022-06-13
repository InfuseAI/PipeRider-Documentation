---
description: Compare the results of previous reports.
---

# Compare Reports

Once you have generated multiple reports it may be desirable to compare reports to view how your data has changed over time.

The `compare-report` feature allows you to compare two different reports by generating a comparison report.

```
piperider compare-report
```

You will be prompted to select two reports for the comparison.

```
#Output
Please select the 2 reports to compare ( SPACE to select, and ENTER to confirm ):
   o mydataproj->SYMBOL               #pass=  0 #fail=0   #row=505      #column=11  2022-06-09T21:29:19.309687Z
   o mydataproj->SYMBOL               #pass=  0 #fail=0   #row=505      #column=11  2022-06-09T21:17:29.178259Z
   X mydataproj->PRICE                #pass=  0 #fail=0   #row=157881   #column=11  2022-06-09T21:29:19.309687Z
   X mydataproj->PRICE                #pass=  0 #fail=0   #row=157881   #column=11  2022-06-09T21:17:29.178259Z
   o mydataproj->ACTION               #pass=  0 #fail=0   #row=1960     #column=4   2022-06-09T21:29:19.309687Z
 > o mydataproj->ACTION               #pass=  0 #fail=0   #row=1960     #column=4   2022-06-09T21:17:29.178259Z
```

PipeRider will generate an HTML report of the comparison.

```
Selected reports:
  Base:  /path/to/dataproject/.piperider/outputs/dataproject-20220609212919/PRICE.json
  Input: /path/to/dataproject/.piperider/outputs/dataproject-20220609211729/PRICE.json

Comparison report: /path/to/dataproject/.piperider/comparisons/dataproject-PRICE-20220609214450/index.html
```

Open the HTML report in your browser to review any changes.
