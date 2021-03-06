# PSelect

A SQL-ish DSL in PowerShell to assist in aggregating collections of data.


### Example - FromPipeline

```powershell
Get-Process | PSelect {
    Field ProcessName
    Field WorkingSet -as Count -Count
    Field WorkingSet -Sum
    
    GroupBy ProcessName

    SortData
}

ProcessName                           Count WorkingSet
-----------                           ----- ----------
ApplicationFrameHost                      1   27492352
AppVShNotify                              1    7405568
armsvc                                    1    5464064
audiodg                                   1   18952192
BleServicesCtrl                           1    9011200
bmservice                                 1   46051328
chrome                                   25 1875648512
Code                                      7  357859328
CodeHelper                                1   13967360
conhost                                   7   38752256
csrss                                     2   16859136
dasHost                                   1   10018816
DataExchangeHost                          1   12623872
dllhost                                   3   26742784
...
```

### Example - FromCsv

```powershell
PSelect {
    Field category
    Field raisedAmt -as AvgRaisedAmt -Average -Unit Currency
    Field raisedAmt -as TotalRaisedAmt -Sum -Unit Currency
    Field raisedAmt -as MaxRaisedAmt -Maximum -Format "{0:N}"
    Field raisedAmt -as MinRaisedAmt -Min -Format "{0,13:C}"
    Field raisedAmt -as SDRaisedAmd -StdDev -Format "{0:f}"
    Field raisedAmt -as Rounds -Count
    
    GroupBy category
    
    FromCsv TechCrunchcontinentalUSA.csv

    SortData

} | Format-Table -AutoSize

category   AvgRaisedAmt   TotalRaisedAmt     MaxRaisedAmt   MinRaisedAmt  SDRaisedAmd Rounds
--------   ------------   --------------     ------------   ------------  ----------- ------
           $15,554,166.67 $373,300,000.00    150,000,000.00   $900,000.00 29888793.60     24
biotech    $19,312,500.00 $77,250,000.00     37,000,000.00    $250,000.00 13920954.30      4
cleantech  $18,492,857.14 $258,900,000.00    57,000,000.00  $1,000,000.00 14340525.21     14
consulting $6,427,000.00  $32,135,000.00     13,000,000.00     $10,000.00 5576451.92       5
hardware   $21,141,025.64 $824,500,000.00    130,000,000.00   $100,000.00 25244059.65     39
mobile     $6,729,583.33  $323,020,000.00    25,000,000.00     $20,000.00 5752943.80      48
other      $7,490,625.00  $119,850,000.00    29,000,000.00    $300,000.00 6911092.05      16
software   $9,979,823.53  $1,017,942,000.00  60,000,000.00     $10,000.00 10046611.33    102
web        $9,739,300.29  $11,765,074,750.00 300,000,000.00     $6,000.00 19034496.45   1208
```