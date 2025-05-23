# Js Syntax Shortcuts and Essentials

### 1. Ternary Operator

const isAdult = age >= 18 ? "Yes" : "No";

### 2. Default Parameters

function greet(name = "Guest") {
return `Hello, ${name}`;
}

### 3. Arrow Functions

const add = (a, b) => a + b;

### 4. Destructuring Objects

const { name, age } = person;

### 5. Destructuring Arrays

const [first, second] = colors;

### 6. Template Literals

const message = `Hi, ${user.name}!`;

### 7. Spread Operator

const newArray = [...oldArray, 4];

### 8. Rest Parameters

function sum(...numbers) {
return numbers.reduce((a, b) => a + b);
}

### 9. Optional Chaining

const street = user?.address?.street;

### 10. Nullish Coalescing

const username = inputName ?? "Anonymous";
