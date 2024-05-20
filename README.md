# 🔐 SimpleAntiBruteForce

Small library to manage brute force attempts on a PHP form.

Didn't find a library that offered a correct and simple system.

To use it, you only need to create a MySQL table on your project following this data model:

`user_failed_logins(id: int, email: varchar, attempted_at:int, ip_adress:varchar)`

# Usage 

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