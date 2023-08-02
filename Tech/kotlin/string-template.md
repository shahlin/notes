# String Template
1. Basic usage: Add a dollar sign before the variable name in a string
2. Example: `"My name is $name"`
3. Example: `val change = 4.00; println("Change is $$change") // Will output Change is $4.00`
4. Example: `val a = 1; val b = 2; println("Sum is ${a + b}")`
5. Example: `val emp = Employee(); println("Employee ID is ${employee.id}")`
    5.1. Object properties are considered expressions so they must be wrapped in curly braces
