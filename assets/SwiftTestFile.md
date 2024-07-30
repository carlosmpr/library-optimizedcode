---
  title: 'SwiftTestFile.swift'
  description: 'component description'
  pubDate: 'July 30, 2024'
  author: 'Carlos Polanco'
  ---
  
  
  
  # SwiftTestFile.swift
  # Code Explanation

The provided code is written in Swift and consists of two functions and some code to calculate Body Mass Index (BMI) and provide a health recommendation based on the calculated BMI.

### `calculateBMI` Function
- **Purpose**: This function calculates the BMI using the formula BMI = weight / (height * height).
- **Parameters**:
  - `weight`: Represents the weight of a person in kilograms.
  - `height`: Represents the height of a person in meters.
- **Return**: The function returns the calculated BMI as a `Double`.

### `getHealthRecommendation` Function
- **Purpose**: This function provides a health recommendation based on the calculated BMI.
- **Parameters**:
  - `bmi`: Represents the Body Mass Index of a person.
- **Return**: The function returns a health recommendation as a `String` based on the BMI value.
- **Recommendations**:
  - If BMI is less than 18.5, it suggests the person is underweight and should eat more nutritious food.
  - If BMI is between 18.5 and 24.9, it indicates normal weight and encourages the person to keep up the good work.
  - If BMI is between 25 and 29.9, it suggests the person is overweight and should consider a balanced diet and exercise.
  - For BMI values outside the above ranges, it recommends consulting a healthcare provider due to obesity.

### Example Usage
```swift
let weight = 70.0  // weight in kilograms
let height = 1.75  // height in meters

let bmi = calculateBMI(weight: weight, height: height)
let recommendation = getHealthRecommendation(bmi: bmi)

print("BMI: \(bmi)")
print("Recommendation: \(recommendation)")
```

### Output
```
BMI: 22.86
Recommendation: Normal weight - Keep up the good work!
```

This code snippet efficiently calculates BMI and provides personalized health recommendations based on the calculated BMI value.
  
  ## Component Code
  ```jsx
  import Foundation

func calculateBMI(weight: Double, height: Double) -> Double {
    return weight / (height * height)
}

func getHealthRecommendation(bmi: Double) -> String {
    switch bmi {
    case ..<18.5:
        return "Underweight - You should eat more nutritious food."
    case 18.5..<24.9:
        return "Normal weight - Keep up the good work!"
    case 25..<29.9:
        return "Overweight - Consider a balanced diet and exercise."
    default:
        return "Obesity - Consult a healthcare provider."
    }
}

let weight = 70.0  // weight in kilograms
let height = 1.75  // height in meters

let bmi = calculateBMI(weight: weight, height: height)
let recommendation = getHealthRecommendation(bmi: bmi)

print("BMI: \(bmi)")
print("Recommendation: \(recommendation)")
  ```
  