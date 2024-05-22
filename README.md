# SimpleAntiBruteForce - PHP Library

Small library to manage brute force attempts on a PHP form.

I couldn't find a library offering a correct and simple system so I made one myself

To use it, you only need to create a MySQL table on your project following this data model:

```sql
CREATE TABLE user_failed_logins (
    id INT AUTO_INCREMENT PRIMARY KEY,
    email VARCHAR(255) NOT NULL,
    attempted_at INT NOT NULL,
    ip_address VARCHAR(45) NOT NULL
);
```

The default constants are defined as follows: **maximum 10 attempts in the last 5 minutes**.

You can change these values by changing the two variables defined at the top of the file: `$MAX_FAILED_ATTEMPT` and `$INTERVAL_IN_S`.

The library is designed so that the database empties itself over time.

# Usage 

- Define MySQL credentials ($DB_HOST, $DB_NAME, $DB_USERNAME, $DB_PASSWORD) at the top of the file.

- Call the `::isAuthorized()` method before verifying the entered password.

Exemple :
```php
// checking that the IP is not blocked
if(!SimpleAntiBruteForce::isAuthorized($ip, $email)){
    http_response_code(429);
    die("Too many connection attempts... Retry later");
}

```

- Call the `::addFailedAttempt()` method when a connection fails in order to remember it.
```php
// if the password entered is the correct one
if (password_verify($password, $user_details["hash_password"])) {
    header("Location: /dashboard");
}
// if the password entered is incorrect
else {
    // we record using the library that a connection attempt has failed
    SimpleAntiBruteForce::addFailedAttempt($ip, $email);
}
```


For any questions, contact me by email.
