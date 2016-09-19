# Node.JS vs PHP + Express vs Laravel

If your work colleagues still tries to argue on using PHP over Node.js for whatever reason, show them these results and tell them to shut up. ;) I will see when I have the time to add Go to the test bed. I'm pretty sure Go will win overall but I have no plans on learning the language and environment in the near future. I'll get back to it when I feel ready.
`Node.js FTW`



#### Test machine:
- MacBook Pro (13 inch, late 2012)
- 2.9GHz Intel Core i7
- RAM 16GB 1333MHz DDR3
- HDD 1TB

- Server: nginx/1.10.0

- PHP 7.0.7
- Laravel 5.3.0


- Node 6.5.0
- Express 4.13.4

#### Test Condition #1
- Duration: 5s
- Threads: 10
- Connections: 5000

```php
Node.js
wrk -d5s -t10 -c5000 http://localhost:3000

Laravel
wrk -d5s -t10 -c5000 http://laravel.dev
```

```php
Simple GET request that returns "Hello World"
/**
 * Test results:
 * | Rank  | Subject                      | Requests/sec  | Data/sec   | Avg. Response |
 * -------------------------------------------------------------------------------------
 * |   1   | Pure Node.js(Multi Thread)   | 20,873.34     | 3.09MB     | 11.40ms       |
 * |   2   | Pure Node.js(Single Thread)  | 10,375.05     | 1.53MB     | 22.70ms       |
 * |   3   | Express(JS, Multi Thread)    | 3,426.94      | 779.76KB   | 68.92ms       |
 * |   4   | Pure PHP(Multi Thread)       | 3,147.95      | 667.17KB   | 23.28ms       |
 * |   5   | Express(JS, Single Thread)   | 2,386.69      | 543.06KB   | 97.97ms       |
 * |   6   | Laravel(PHP, Multi Thread)   | 26.23         | 101.44KB   | 392.64ms      |
 * -------------------------------------------------------------------------------------
 */
```

```php
Simple GET request that returns prime numbers between 0 and 1000
/**
 * Test results:
 * | Rank  | Subject                      | Requests/sec  | Data/sec   | Avg. Response |
 * -------------------------------------------------------------------------------------
 * |   1   | Pure Node.js(Multi Thread)   | 14,290.14     | 10.75MB    | 16.57ms       |
 * |   2   | Pure Node.js(Single Thread)  | 7,902.13      | 5.95MB     | 19.13ms       |
 * |   3   | Express(JS, Multi Thread)    | 3,534.16      | 2.95MB     | 66.85ms       |
 * |   4   | Pure PHP(Multi Thread)       | 3,189.39      | 1.43MB     | 29.21ms       |
 * |   5   | Express(JS, Single Thread)   | 2,206.12      | 1.84MB     | 106.68ms      |
 * |   6   | Laravel(PHP, Multi Thread)   | 44.67         | 60.28KB    | 370.60ms      |
 * -------------------------------------------------------------------------------------
 */
```

#### Test Condition #2
- Duration: 30s
- Threads: 10
- Connections: 5000

```php
Node.js
wrk -d30s -t10 -c5000 http://localhost:3000

Laravel
wrk -d30s -t10 -c5000 http://laravel.dev
```

```php
Simple GET request that returns "Hello World"
/**
 * Test results:
 * | Rank  | Subject                      | Requests/sec  | Data/sec   | Avg. Response |
 * -------------------------------------------------------------------------------------
 * |   1   | Pure Node.js(Multi Thread)   | 24,559.01     | 3.63MB     | 9.76ms        |
 * |   2   | Pure Node.js(Single Thread)  | 11,629.34     | 1.72MB     | 20.63ms       |
 * |   3   | Express(JS, Multi Thread)    | 4,429.92      | 0.98MB     | 54.23ms       |
 * |   4   | Express(JS, Single Thread)   | 2,689.57      | 611.98KB   | 89.16ms       |
 * |   5   | Pure PHP(Multi Thread)       | 549.22        | 116.91KB   | 27.73ms       |
 * |   6   | Laravel(PHP, Multi Thread)   | 35.35         | 226.28KB   | 144.51ms      |
 * -------------------------------------------------------------------------------------
 */
```

```php
Simple GET request that returns prime numbers between 0 and 1000
/**
 * Test results:
 * | Rank  | Subject                      | Requests/sec  | Data/sec   | Avg. Response |
 * -------------------------------------------------------------------------------------
 * |   1   | Pure Node.js(Multi Thread)   | 16,802.80     | 12.64MB    | 14.30ms       |
 * |   2   | Pure Node.js(Single Thread)  | 8,727.14      | 6.57MB     | 26.92ms       |
 * |   3   | Express(JS, Multi Thread)    | 4,240.04      | 3.54MB     | 56.68ms       |
 * |   4   | Express(JS, Single Thread)   | 2,404.82      | 2.01MB     | 99.70ms       |
 * |   5   | Pure PHP(Multi Thread)       | 550.48        | 251.90KB   | 27.81ms       |
 * |   6   | Laravel(PHP, Multi Thread)   | 33.48         | 216.63KB   | 417.37ms      |
 * -------------------------------------------------------------------------------------
 */
```

## Test Source Code

####Pure Node.js(multi- and single-thread)

```javascript
// Hello World
var server = http.createServer(function (request, response) {
        if (request.method == "GET") {
            response.writeHead(200, {'Content-Type': 'text/html'});
            response.write('Hello worlds');
            response.end();
        }
    });
server.listen(3000);


// Prime Numbers
var server = http.createServer(function (request, response) {
        if (request.method == "GET") {
            response.writeHead(200, {'Content-Type': 'text/html'});
            // Notice the JSON.stringify process which is included in the operation
            response.write(JSON.stringify(getPrimes(1000)));
            response.end();
        }
    });
server.listen(3000);
```

####Express(multi- and single-threaded)
```javascript
router.get('/', function (req, res, next) {
    res.send('Hello World');
});


// Get prime numbers
router.get('/', function (req, res, next) {
    res.send(getPrimes(1000);
});
```

#### JavaScript Prime Number Function
```javascript
const getPrimes = (max) => {
    var sieve = [], i, j, primes = [];
    for (i = 2; i <= max; ++i) {
        if (!sieve[i]) {
            primes.push(i);
            for (j = i << 1; j <= max; j += i) {
                sieve[j] = true;
            }
        }
    }
    return primes;
};
```


#### Pure PHP
```php
// Hello world
echo "Hello world";

// Prime numbers
print_r(getPrimeNumbers(1000));
```


#### Laravel
```php
Route::get('/', function () {
    return 'Hello World';
});

Route::get('/', function () {
    return getPrimeNumber(1000);
});
```

#### PHP Prime Number Function
```php
function getPrimeNumbers($n)
{
    $sieve = [];
    for($i = 1; $i <= $n; $i++) {
        $sieve[$i] = $i;
    }

    $i =2;
    while($i * $i <= $n) {
        if(isset($sieve[$i])) {
            $k = $i;
            while ($k * $i <= $n) {
                unset($sieve[$k * $i]);
                $k++;
            }
        }
        $i++;
    }
    return $sieve;
}
```




