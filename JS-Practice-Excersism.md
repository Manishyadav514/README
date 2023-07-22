# Javascript-Practice-Exercism
This Readme file contains solved exercises for Javascript questions available on Exercism website.

## Exercise: Lucian's Luscious Lasagna | Learning Exercise
![image](https://user-images.githubusercontent.com/60292480/177029095-618e3782-c6cc-4766-90ef-318e083d0d3e.png)
![image](https://user-images.githubusercontent.com/60292480/177029102-e6d4369a-b763-43d4-bb1c-c5a61cf8c6ce.png)

#### Iteration 1
```javascript
const PREPARATION_MINUTES_PER_LAYER = 2;
export const EXPECTED_MINUTES_IN_OVEN = 40

export function remainingMinutesInOven(actualMinutesInOven) {
  return EXPECTED_MINUTES_IN_OVEN-actualMinutesInOven;
}
export function preparationTimeInMinutes(numberOfLayers) {
  return numberOfLayers*2;
}
export function totalTimeInMinutes(numberOfLayers, actualMinutesInOven) {
  return numberOfLayers*2+ actualMinutesInOven;
}

```


## Exercise: Freelancer Rates | Learning Exercise
![image](https://user-images.githubusercontent.com/60292480/177030347-5e71bd35-66e2-455f-8422-77a1c26b1c69.png)

#### Iteration 1
```javascript
export function dayRate(ratePerHour) {
  return 8*ratePerHour;
}
export function daysInBudget(budget, ratePerHour) {
  return Math.floor(budget/(ratePerHour*8))
  ;
}
export function priceWithMonthlyDiscount(ratePerHour, numDays, discount) {
  const billingDaysPerMonth = 22;
  const numberOfMonths = Math.floor(numDays / billingDaysPerMonth);
  const dailyRate = dayRate(ratePerHour);
  const monthlyRate = billingDaysPerMonth * dailyRate * (1 - discount);
  const daysLeftAtFullRate = numDays - (numberOfMonths * billingDaysPerMonth);
  const priceForFullMonths = numberOfMonths * monthlyRate;
  const priceForRemainingDays = daysLeftAtFullRate * dailyRate;
  return Math.ceil(priceForFullMonths + priceForRemainingDays);
}
```


## Exercise: Scrabble Score | Medium
![image](https://user-images.githubusercontent.com/60292480/177027200-f703c1ab-40d2-41b0-bff8-c5f1ba6786e1.png)

#### Iteration 1:
```javascript
export const score = (string) => {
      string = string.toLowerCase();
      let sum = 0;
      const string1 = "aeioulnrst"
      const string2="dg"
      const string3="bcmp"
      const  string4="fhvwy"
      const string5="k"
      const string6="jx"
      const string7="qz"
  for (let i=0; i<string.length; i++ ){
           (string1.includes(string[i]))?sum = sum+1:string2.includes(string[i])?sum = sum+2:string3.includes(string[i])?sum = sum+3:string4.includes(string[i])?sum = sum+4:string5.includes(string[i])?sum = sum+5:string6.includes(string[i])?sum = sum+8:sum = sum+10
  }      return(sum);
};
```

#### Iteration 2:
```javascript
export const score = (string) => {
      string = string.toLowerCase();
      let sum = 0;
      const string1 = "aeioulnrst"
      const string2="dg"
      const string3="bcmp"
      const  string4="fhvwy"
      const string5="k"
      const string6="jx"
      const string7="qz"
  for (let i=0; i<string.length; i++ ){
      if(string1.includes(string[i])){
          sum = sum+1;
      }
      else if(string2.includes(string[i])){
          sum = sum+2;
      }
      else if(string3.includes(string[i])){
          sum = sum+3;
      }      
      else if(string4.includes(string[i])){
          sum = sum+4;
      }      
      else if(string5.includes(string[i])){
          sum = sum+5;
      }
            else if(string6.includes(string[i])){
          sum = sum+8;
      }
            else if(string7.includes(string[i])){
          sum = sum+10;
      }
  }      return(sum);
};
```


