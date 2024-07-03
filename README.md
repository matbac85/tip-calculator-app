## Tip Calculator App

![Screenshot 1](desktop-preview.jpg)

### Application Description

The app allows users to compute tip amounts and total amounts per person for shared bills. Below is a detailed explanation of its functionality:

#### Features and Input Validation

1. **Bill Amount**:

   - Users enter the total bill amount. This value must be strictly positive.

2. **Tip Percentage**:

   - Users can select a predefined tip percentage using radio buttons (5%, 10%, 15%, 25%, 50%).
   - Alternatively, they can input a custom tip percentage in the provided field.

3. **Number of People**:
   - Users specify the integer number of people sharing the bill. This number must be strictly positive and cannot be a decimal.

#### Tools Used

- **Vue.js**: JavaScript framework used for building the user interface and managing data reactivity.
- **Computed Properties and Watchers**: Used to dynamically calculate tip amounts and total amounts per person based on changes in inputs.
- **HTML/CSS**: Used for structuring and styling the user interface in a responsive and aesthetic manner.

#### Functionality and Reset Button

- **Reset Button**: A button clears all form fields once they are filled or selected. The button remains disabled until all necessary fields are filled or selected.

#### Video Demonstration

You can watch a video demonstration of the app [link_to_video](https://www.webmobilefirst.com/screencasts/XnBmmECaq7/).

#### Try It Out

Explore the app live [here](https://matbac85.github.io/tip-calculator-app/).

#### Credits

This project was inspired by a design challenge from [Frontend Mentor](https://www.frontendmentor.io/).

### Summary

This application simplifies the process of splitting bills by automatically calculating tip amounts and total amounts per person. It ensures a user-friendly and responsive experience while adhering to input validation rules for accuracy and usability.
