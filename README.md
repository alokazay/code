
# Random Number Without Random Generator (Epic Bad Code)

This project demonstrates how to generate a random number between 1 and 100 without using a random number generator. The goal is to write code in an obfuscated, "bad code" style, perfect for fun coding challenges or competitions!

## Overview

In this code, we use the current system time (milliseconds) to simulate randomness. By applying a modulo operation, we can get a "random" number between 1 and 100.

To add some fun, the code has been obfuscated, and the core logic is encoded using `btoa` for extra obscurity!

## Usage

1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/repo-name.git
   ```

2. Open the project in your favorite code editor.

3. Run the code in a browser or Node.js environment.

4. To generate a random number between 1 and 100, execute the following code:

   ```javascript
   eval(atob('Y29uc29sZS5sb2coRGF0ZS5ub3coKSUxMDArMSk='));
   ```

This will output a random number in the console.

## How It Works

1. The code retrieves the current time in milliseconds using `new Date().getTime()`.
2. The time value is then divided by 100 using the modulo operator to get a remainder between 0 and 99.
3. Adding 1 ensures the final result is between 1 and 100.
4. The code is obfuscated by encoding the logic with `btoa`, and then decoded and executed using `eval` and `atob`.

## Warning

This code is written to be intentionally difficult to read and maintain. It is for fun and educational purposes only! Do **not** use this approach in production environments.

## License

This project is licensed under the MIT License. 
