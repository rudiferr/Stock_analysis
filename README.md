# "stock_analysis" Week 2 Challenge
## Overview of the Project
### Tools Used
* Microsoft Excel
* Visual Basic for Applications
The purpose of this project was to take an initial codebase and refactor it for scalability. The initial code created for the module work allowed for analysis a small sample size of stocks, but to be able to make an efficient analysis algorithm it needs to be able to be efficients regardless of sample size; i.e. be able to absorb a large sample size without proportionally increasing its runtime. Nested loops were originally used to scan through the data multiiple tmies, and the method was able to accomplish the task for relatively well. But the method to be scalable, a runtime of about 0.855 seconds was going to be a problem.
![Screenshot of workbook analysis runtime prior to refactoring](https://github.com/rudiferr/stock-analysis/blob/main/Resources/2017_runtime_original.png)

## Results
The code was refactored to utilize arrays and a different method of condtional loops that would only require the dataset to be analyzed once. By having the code determine which of the four conditonials it would need to meet and applying the proper protocol, the algorithm wouldn't need to execute one single-protocol throughout the entirety of the workbook before it could loop to the top to attempt a second protocol, and consiquentially for any additional number of conditionals. 
```
    'Get the number of rows to loop over
    RowCount = Cells(Rows.Count, "A").End(xlUp).Row

    '1a) Create a ticker Index
    Dim tickerIndex As Integer
    tickerIndex = 0
    
    '1b) Create three output arrays
    Dim tickerVolumes(12) As Long
    Dim tickerStartingPrices(12), tickerEndingPrices(12) As Single

    ''2a) Create a for loop to initialize the tickerVolumes to zero.
    For i = 0 To 11
        tickerVolumes(i) = 0
    Next i
    
    ''2b) Loop over all the rows in the spreadsheet.
    For i = 2 To RowCount

        '3a) Increase volume for current ticker
        tickerVolumes(tickerIndex) = tickerVolumes(tickerIndex) + Cells(i, 8).Value

        '3b) Check if the current row is the first row with the selected tickerIndex.
        If Cells(i, 1).Value = tickers(tickerIndex) And Cells(i - 1, 1).Value <> tickers(tickerIndex) Then
            tickerStartingPrices(tickerIndex) = Cells(i, 6).Value
        End If
        
        '3c) check if the current row is the last row with the selected ticker
         'If the next row’s ticker doesn’t match, increase the tickerIndex.
        If Cells(i, 1).Value = tickers(tickerIndex) And Cells(i + 1, 1).Value <> tickers(tickerIndex) Then
            tickerEndingPrices(tickerIndex) = Cells(i, 6).Value
        End If
            
            '3d Increase the tickerIndex.
        If Cells(i + 1, 1).Value <> tickers(tickerIndex) Then
           tickerIndex = tickerIndex + 1
        End If
    Next i
    
    '4) Loop through your arrays to output the Ticker, Total Daily Volume, and Return.
    For i = 0 To 11
        Worksheets("All Stocks Analysis").Activate
        Cells(4 + i, 1).Value = tickers(i)
        Cells(4 + i, 2).Value = tickerVolumes(i)
        Cells(4 + i, 3).Value = tickerEndingPrices(i) / tickerStartingPrices(i) - 1
    Next i
```

As a result, the runtime decreased to 0.086 seconds to analyze both years of stock data, which was a 90% decrease in runtime.

![Screenshot of 2017 workbook analysis runtime after refactoring](https://github.com/rudiferr/stock-analysis/blob/main/Resources/2017_runtime_refactored.png)
![Screenshot of 2017 workbook analysis runtime after refactoring](https://github.com/rudiferr/stock-analysis/blob/main/Resources/2018_runtime_refactored.png)

## Summary
Dramatically decreasing runtime will always be a major advantage and a reason why code should be refactored, but the method as to how the code should be refactored will always be up for debate depending on the size of your sample, quality of data in your sample, and numerous other factors. Different principles of mathematics have been applied to sorting algorithms to determine what are the most efficient ways to sort datasets that contain a variety of different attributes, and I'm sure if we were given a different workbook with different datapoints to analyze our method of analysis would have to be rethought and refactored again.
