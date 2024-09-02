/*
Group 8
Names: Satya Vangoor, Frida Salgado, Sristi Halder, Evan White
Celia Hurtado
Date: 06/20/2024
Purpose: Data Project Graphs
*/

clear all
set more off

cd "/Users/satyavangoor/Desktop/EDE Summer Institute/Group Data Project/Stata and Data"
use currentPopulationSurvey.dta, clear

//keep all the variables necessary
keep inctot year hhtenure ownershp race educ 

//create a new race variable to simplify data set
gen raceSimplified = "Other"
replace raceSimplified = "White" if race == 100
replace raceSimplified = "Black" if race == 200
replace raceSimplified = "AAPI" if race == 650
replace raceSimplified = "Native American" if race == 300


//indicator variable for if someone is a certain race and owns their home
gen whiteOwnership = .
replace whiteOwnership = 0 if raceSimplified == "White" 
replace whiteOwnership = 1 if (raceSimplified == "White" & ownershp == 10)

gen blkOwnership = .
replace blkOwnership = 0 if raceSimplified == "Black" 
replace blkOwnership = 1 if (raceSimplified == "Black" & ownershp == 10)

gen aapiOwnership = .
replace aapiOwnership = 0 if raceSimplified == "AAPI" 
replace aapiOwnership = 1 if (raceSimplified == "AAPI" & ownershp == 10)

gen nativeOwnership = .
replace nativeOwnership = 0 if raceSimplified == "Native American" 
replace nativeOwnership = 1 if (raceSimplified == "Native American" & ownershp == 10)

gen otherOwnership = .
replace otherOwnership = 0 if raceSimplified == "Other" 
replace otherOwnership = 1 if (raceSimplified == "Other" & ownershp == 10)

//create a new education variable to simplify data set
gen educSimplified = "."
replace educSimplified = "Some High School" if educ == 40 | educ == 50 | educ == 60 | educ == 71
replace educSimplified = "High School Diploma or Equivalent" if educ == 73
replace educSimplified = "Some College" if educ == 110 | educ == 121 | educ == 90 | educ == 80 | educ == 100 | educ == 122 | educ == 81
replace educSimplified = "Associate's Degree" if educ == 91 | educ == 92
replace educSimplified = "Bachelor's Degree" if educ == 111
replace educSimplified = "Graduate Degree" if educ == 123 | educ == 124 | educ == 125 
drop if educSimplified == "."

/*gen educNum = (educSimplified == "Some High School")
replace educNum = 2 if educSimplified == "High School Diploma or Equivalent"
replace educNum = 3 if educSimplified == "Some College"
replace educNum = 4 if educSimplified == "Associate's Degree"
replace educNum = 5 if educSimplified == "Bachelor's Degree"
replace educNum = 6 if educSimplified == "Graduate Degree"
regress inctot educNum
*/

gen order=1 if educSimplified=="Some High School"
replace order=2 if educSimplified=="High School Diploma or Equivalent"
replace order=3 if educSimplified=="Some College"
replace order = 4 if educSimplified == "Associate's Degree"
replace order = 5 if educSimplified == "Bachelor's Degree"
replace order = 6 if educSimplified == "Graduate Degree"

//create a horizontal bar graph showing home ownership by race across education levels
graph hbar whiteOwnership blkOwnership aapiOwnership nativeOwnership otherOwnership, over(educSimplified, sort(order)) bar(1, color(sand)) bar(2, color(ltblue)) bar(3, color(lavender)) bar(4, color(erose)) bar(5, color(eltgreen)) legend(rows(3) position(7) label(1 "White") label(2 "Black") label(3 "AAPI") label(4 "Native American") label(5 "Other")region(color("244 239 238"))) aspect(.5) title("Home Ownership Across Race and Education Level") graphregion(color("244 239 238"))

graph export raceHomeOwnershipByEduc.pdf, replace

tabstat whiteOwnership blkOwnership aapiOwnership nativeOwnership otherOwnership, by (educSimplified)

gen someHighSchool = (order == 1)
gen highSchool = (order ==2)
gen someCollege = (order == 3)
gen associatesDegree = (order == 4)
gen bachelorsDegree = (order == 5)
gen gradDegree = (order == 6)

gen raceOrder = 1 if raceSimplified == "White"
replace raceOrder = 2 if raceSimplified == "Black"
replace raceOrder = 3 if raceSimplified =="AAPI"
replace raceOrder = 4 if raceSimplified == "Native American"
replace raceOrder = 5 if raceSimplified == "Other"

graph hbar someHighSchool highSchool someCollege associatesDegree bachelorsDegree gradDegree, over(raceSimplified, sort(raceOrder)) bar(1, color(sand)) bar(2, color(ltblue)) bar(3, color(lavender)) bar(4, color(erose)) bar(5, color(eltgreen)) bar(6, color(ltkhaki)) legend(rows(3) position(7) label(1 "Some High School") label(2 "High School Degree or Equivalent") label(3 "Some College") label(4 "Associate's Degree") label(5 "Bachelor's Degree") label(6 "Graduate Degree")region(color("244 239 238"))) aspect(.5) title("Education Levels across Race") graphregion(color("244 239 238"))

// Regression analysis to assess the relationship between education, race, and income
regress inctot i.raceSimplified i.educSimplified
margins, at(raceSimplified=(1 2 3 4 5))

// Save the cleaned and processed data
save "cleanedData_06.20.dta", replace

graph export educationLevelbyRace.pdf, replace

save "cleanedData_06.20.dta", replace
