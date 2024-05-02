# FlyingWhale-Airline
Background 
FlyingWhale Airline, a prominent (fictional) international airline, is seeking to enhance its business intelligence capabilities by 
analyzing Customer Flight Activity and Customer Loyalty History. The airline is committed to optimizing customer experience, 
understanding travel patterns, and maximizing the effectiveness of its loyalty programs.
Data: You have access to two key datasets:
1. Customer Flight Activity:
• Loyalty Number: A unique identifier for each customer's loyalty account.
• Year and Month: Period details for analysis.
• Flights Booked: Number of flights booked by the member during the period.
• Flights with Companions: Number of flights booked with additional passengers.
• Total Flights: Combined total of Flights Booked and Flights with Companions.
• Distance: Flight distance traveled in kilometers during the period.
• Points Accumulated: Loyalty points earned in the period.
• Points Redeemed: Loyalty points redeemed during the period.
• Dollar Cost Points Redeemed: Dollar equivalent for points redeemed in Canadian Dollars (CDN).
2. Customer Loyalty History:
• Loyalty Number: A unique identifier for each customer's loyalty account.
• Demographics: Country, Province, City, Postal Code, Gender, Education, Salary, Marital Status.
• Loyalty Card: Current loyalty card status
• Customer Lifetime Value (CLV): Total invoice value for all flights ever booked by the member.
• Enrollment Details: Enrollment Type (Standard / 2018 Promotion), Enrollment Year, Enrollment Month.
• Cancellation Details: Cancellation Year and Month if applicable.
Business Scenarios: 
1. Flight Activity Analysis:
• Analyze monthly and yearly flight booking patterns.
• Tips: 
▪ Make sure months are named properly
▪ Make sure months are sorted properly
• Explore the correlation between flight distances and loyalty points accumulated.
There is a positive correlation between flight distances and loyalty points accumulated. 
• Include a trend line and a max line





• Assess the impact of companion bookings on loyalty points redeemed


Visual tells that the impact of companion bookings on loyalty points redeemed is huge, ie 198967.

• What is the number of companions where members are redeeming the most points?
Flights with companion 3 are redeeming the most points, ie 2195087.

2. Loyalty Segmentation:
• Segment customers based on loyalty card status.
Based on loyality card status ,20% are “Aurora”, 45% are “Star” and 34% are “Nova”.

• Show Total number of flights by Loyalty Card across months
In July (Star card) there were maximum number of flights booked ie, 112219


• Analyze the demographics and behaviors of customers
From the visuals, we can say that customers with “Star and Nova loyalty cards” have booked the greatest number of flights specially in the month of “July.”

• Depict Number of loyalty members by marital status
From the visulas , we can depict that max Loyalty members are “Married”
These are 9735.





• Show flights booked by loyalty card and broken up by gender
I can interpret that “star loyalty card holders” have booked maximum number of flights.


• Show median distance travelled by different loyalty card tiers
o	Aurora => median distance travelled is 543
o	Star => median distance travelled is 536
o	Nova => median distance travelled is 491

• Use the Narrative visual to autogenerate insights. Make sure to remove insights that may not be useful.

July in Loyalty Card Star made up 11.67% of Sum of Flights Booked. 
At 4,264, Married had the highest Count of Loyalty Number and was 266.01% higher than Divorced, which had the lowest Count of Loyalty Number at 1,165. 
Married had 4,264 Count of Loyalty Number, Divorced had 1,165, and Single had 2,208. 

• Identify trends in Customer Lifetime Value (CLV) across loyalty segments.
Median of Loyality car for three different tiers is different. 
CLV for Aurora card holders is huge than any other.



• Answer the question: 
Which credit card tier on average has customers with the highest Customer Lifetime Value?
“Aurora” has the highest CLV.

















3. Enrollment and Cancellation Trends:
• Analyze the reasons and patterns behind membership cancellations.

• Tips
▪ Create a table Customer Loyalty Cancellation for loyalty members that have cancelled. 
Customer Loyalty Cancellation2 = SELECTCOLUMNS(
        'Customer Loyalty History',
        "loyalty Number",'Customer Loyalty History'[Loyalty Number],
        "Enrollement Month",'Customer Loyalty History'[Enrollment Month],
        "Enrollment Year",'Customer Loyalty History'[Enrollment Year],
        "Cancellation Month",'Customer Loyalty History'[Cancellation Month],
        "Cancellation Year",'Customer Loyalty History'[Cancellation Year]
        )



▪ Create two new columns in the new table Enrollment Duration (e.g. 2 years 1 month) and 
Enrollment Duration (Months) (e.g. 25)

 Done in the table 
1. 
Enrollment Duration (Months) = DATEDIFF(
    'Customer Loyalty Cancellation2'[Enrollment Date],
    'Customer Loyalty Cancellation2'[cancellation Date],MONTH
)
2. 
Enrollement Duration(years,months) = IF(
    ISBLANK('Customer Loyalty Cancellation2'[Enrollment Duration (Months)]),BLANK(),
    var Years = INT('Customer Loyalty Cancellation2'[Enrollment Duration (Months)]/12)
    var Months = MOD('Customer Loyalty Cancellation2'[Enrollment Duration (Months)],12)

RETURN
    IF(Years>0 && Months>0,
        Years & " Years " & Months&" Months",
            IF(Years>0,
                Years & " Years ",Months &" Months"
            )
    )
)

▪ Create two new columns in Customer Loyalty History table Enrollment Duration (Till Date) and 
Enrollment Duration (Till Date) Months

1. 
Enrollment Duration(Till Date) Months = 
    DATEDIFF(
        'Customer Loyalty Cancellation2'[Enrollment Date],TODAY(),MONTH
        )
2. 

Enrollment Duration(Till Date)MonthsYears = 
VAR Years = INT(DIVIDE('Customer Loyalty Cancellation2'[Enrollment Duration(Till Date) Months],12))
VAR Months = MOD('Customer Loyalty Cancellation2'[Enrollment Duration(Till Date) Months],12)
RETURN
    IF(Years>0 && Months>0,
        Years & " Years " & Months & " Months",
            IF(Years>0, Years & " Years ",
                Months & " Months"
            )
    )



• These columns should count the time a member has been enrolled till today or till 
they cancelled whatever comes first

Enrollment Duration(TillDate)or(TillCancelled) = 
    VAR EnrolledMonths=
        IF(
            ISBLANK(CustomerLoyaltyCancellation[cancellation Date(Months)]),
            DATEDIFF(CustomerLoyaltyCancellation[Enrollement Date],TODAY(),MONTH),
            DATEDIFF(CustomerLoyaltyCancellation[Enrollement Date],CustomerLoyaltyCancellation[cancellation Date(Months)],MONTH)
        )
    VAR Years= 
        INT(DIVIDE(EnrolledMonths,12))
    VAR Months =
        MOD(EnrolledMonths,12)
    RETURN
        IF(Years>0 && Months>0,
            Years &" Years " & Months & " Months",
            IF(
                Years>0,
                Years & " Years ",
                Months & " Months"
            )
        )

• Answer the following:
▪ Provide information for average duration of enrollment among cancelled members by province. 
Done 
Which province sees members cancelling the fastest? Bonus: Depict this information on a map
British Columbia saw the member cancel their member ship the fastest.

▪ Most popular months for cancellations
December i.e. 213
▪ Cancellations by education and marital status. Which demographic is cancelling the most?
Education “Bachelors” , married are people with highest count of cancellation.

▪ Which loyalty card members have the lowest enrollment duration among cancellations

Arora has 449
• Recommend strategies for improving enrollment and retention.
page 3 of dashboard

Deliverables:
• Power BI Dashboards and Reports for the scenarios mentioned.
• A comprehensive presentation summarizing key findings and recommendations.
Evaluation Criteria: Students will be evaluated based on the 
• depth of their analysis
• the clarity of visualizations
• strategic nature of their recommendations
• professional nature of presentation
The goal is to showcase your ability to derive actionable insights from Customer Flight Activity and Customer Loyalty History data 
for FlyingWhale Airline. 
