# DAX examples from work Power BI project

**Indentifying if the specific computer is the newest the user has**
```
Newest_PC = IF(
    CALCULATE (
    MAX ( Udskiftning_Reference[retirement_date] ), FILTER (Udskiftning_Reference , Udskiftning_Reference[Primary_User_Hash] = EARLIER ( Udskiftning_Reference[Primary_User_Hash] ) )
) = Udskiftning_Reference[retirement_date].[Date], 1, 0)
```

**Indentifying if the number of computers the user has**
```
Number_Of_PC = CALCULATE(
    COUNTA(Udskiftning_Reference[serial_number]),
    FILTER(ALL(Udskiftning_Reference), 
        EARLIER(Udskiftning_Reference[Primary_User_Hash])=Udskiftning_Reference[Primary_User_Hash]))
```

**Determining a text regarding what should happen with the specific computer**
```
Argument = 
    SWITCH(
        TRUE() ,
        Udskiftning_Reference[retirement_date].[Date] > TODAY() && Udskiftning_Reference[Newest_PC] = 1 , "Nyeste PC og ikke overskredet retirement" ,
        Udskiftning_Reference[retirement_date].[Date] > TODAY() && Udskiftning_Reference[Number_Of_PC] > 1 && Udskiftning_Reference[Newest_PC] = 0 , "Mere end 1 PC og ikke nyeste PC" ,
        Udskiftning_Reference[retirement_date].[Date] <= TODAY() && Udskiftning_Reference[Number_Of_PC] > 1 && Udskiftning_Reference[Newest_PC] = 1 , "Overskredet retirement og nyeste PC" ,
        Udskiftning_Reference[retirement_date].[Date] <= TODAY() && Udskiftning_Reference[Newest_PC] = 1 , "Overskredet retirement og ikke nyeste PC" ,
        Udskiftning_Reference[retirement_date].[Date] <= TODAY() && Udskiftning_Reference[Number_Of_PC] = 1, "Overskredet retirement og kun 1 PC" ,
        Udskiftning_Reference[In_Use]= 0 , "Bortskaffes fordi ikke i brug" ,
        Udskiftning_Reference[Primary_User_Hash]="" , "Mangler bruger",
        "Bortskaffes"
        )
```
