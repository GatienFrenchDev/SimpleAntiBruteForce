# SimpleAntiBruteForce - PHP Library

SimpleAntiBruteForce is a lightweight PHP library designed to mitigate brute force attacks on PHP forms effectively.

## Overview

Many existing libraries lack a straightforward and robust system for managing brute force attempts, prompting the creation of SimpleAntiBruteForce.

This library provides a reliable solution without unnecessary complexity and is designed to optimize resource usage by automatically clearing old records from the database over time.

## Setup the library

Setting up **SimpleAntiBruteForce** is very simple.

- Simply create a new MySQL table in your project with the following schema:

```sql
CREATE TABLE user_failed_logins (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    email VARCHAR(255) NOT NULL,
    attempted_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    ip_address VARCHAR(45) NOT NULL
);
```

- And define your MySQL credentials in the library file. You can also adjust the default settings of maximum attempts and time interval by modifying the designated variables in the file.

## Usage

### IP Check
Before verifying passwords, ensure that the IP address is not blocked by calling the `::isAuthorized()` method.

```php
use SimpleAntiBruteForce;
// ...
if(!SimpleAntiBruteForce::isAuthorized($ip, $email)){
    http_response_code(429);
    die("Too many connection attempts... Retry later");
}
```
### Record Failed Attempts
When a login attempt fails, record it using the `::addFailedAttempt()` method.

```php
use SimpleAntiBruteForce;
// ...
if (password_verify($password, $user_details["hash_password"])) {
    header("Location: /dashboard");
} else {
    SimpleAntiBruteForce::addFailedAttempt($ip, $email);
}
```

## Contact
For questions or support, feel free to contact me via email at <contact@gatiendev.fr>.
