# NetPromoterScore-PowerBI
 Customer Experience Analysis

 ## Net Promoter Score

How likely is it that you would recommend [Organization X] to a friend/family?

Rating ranges between 0 (not likely) - 10 (Very likely).
Customers fall into 3 categories;

• Promoters (score 9-10) are loyal enthusiasts.  
• Passives (score 7-8) are satisfied but unenthusiastic, vulnerable to competitive offerings.  
• Detractors (score 0-6) are unhappy, can damage brand through negative word-of-mouth.  

![PBIDesktop_ieyw2Zlxw1](https://github.com/BrianGwayi/NetPromoterScore-PowerBI/assets/115585139/aab68aee-3284-4f86-a0e6-6376654c8670)

## Net Promoter Score Calculation

Subtract the percentage of Detractors from the percentage of Promoters, which can range from 
a low of -100 (if every customer is a Detractor) to a high of 100 (if every customer is a Promoter).

## Measures & Calculations

### Current Month - Number of Respodents

`Respodents = CALCULATE(  
    COUNT('Net-Promoter-Score'[ID]),  
    DATESMTD('Calendar'[Date]))`

### Current Month - Average Rating

`Avg-Rating = CALCULATE(  
    AVERAGE('Net-Promoter-Score'[NPS-Rating]),  
    DATESMTD('Calendar'[Date]))`

## Net Promoter Score Categories -  Count Current Month
### Promoters
`Promoters =  
    VAR Promoters = CALCULATE(  
        COUNT('Net-Promoter-Score'[ID]),  
        'Net-Promoter-Score'[NPS-Categories] = "Promoters",  
        DATESMTD('Calendar'[Date]))  
        RETURN Promoters`

### Passives
`Passives = 
    VAR Passives = CALCULATE(
        COUNT('Net-Promoter-Score'[ID]),
        'Net-Promoter-Score'[NPS-Categories] = "Passives",
        DATESMTD('Calendar'[Date]))
        RETURN Passives`

### Detractors
`Detractor =
VAR Detractors = CALCULATE(
        COUNT('Net-Promoter-Score'[ID]),
        'Net-Promoter-Score'[NPS-Categories] = "Detractors",
        DATESMTD('Calendar'[Date]))
    VAR Targets = 10
        RETURN Detractors`

## Net Promoter Score
`NPS-Score = 
    VAR promoters = 
        CALCULATE(
            COUNT('Net-Promoter-Score'[ID]),
            'Net-Promoter-Score'[NPS-Rating] >= 9,
            DATESMTD('calendar'[Date])
            )

    VAR detractors =
        CALCULATE(
            COUNT('Net-Promoter-Score'[ID]),
            'Net-Promoter-Score'[NPS-Rating] <= 6,
            DATESMTD('calendar'[Date])
        )

    VAR Respodents =
        CALCULATE(
            COUNT('Net-Promoter-Score'[ID]),
            DATESMTD('calendar'[Date])
        )

    VAR NetPromoters = Promoters - Detractors

RETURN
    DIVIDE(NetPromoters, Respodents) *100`

### Net Promoter Score Maximun Value
`Gauge-Max = 100`

### Icons and Color Coding

`Color-Code = IF([NPS-Score] < [NPS-Target], "#D64550", "#72D96F")`


Promoter-Icon = "☺"

Detractor-Icon = "☹"

Passive-Icon = "㊀"

Star-Rating-Bar = 

    VAR Icon_Total = 10
    var IconFill = ROUND([Avg-Rating], 1)
    var IconEmpty = Icon_Total - IconFill
    var IconFillSymbol = "★ "
    var IconEmptySymbol = "☆ "
    var DataBar = 
    REPT(IconFillSymbol, IconFill) &
    REPT(IconEmptySymbol, IconEmpty)

    Return
        DataBar


