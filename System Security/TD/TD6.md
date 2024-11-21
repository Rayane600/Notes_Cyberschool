# Password Storage
## Q1.
- Transformation function.
- Storage of hashed passwords.
- Function that proves identity
## Q2.
- Online attacks are if the attacker has access to the UI and try guessing the passwords until it succeeds  (can be rate limited)
- Offline attacks mean that the attacker has access to the transformation function and to the table of hashed passwords and try different guesses until the passwords are found (not rate limited)

## Q3. 
password salting prevents the attacker from using pre computed hashes.

# Shadow file
## Q4.
We need to store passwords in a root protected file to reduce risks of unauthorized access to the hash table of the passwords.

## Q5.
in the first scenario the password has expired and the account will be inactive after Oct 16 if the password isn't changed.
in the second scenario the password has expired, The account remains active but the passwords needs to be changed to regain access.

## Q6.
- they have the same hash function , different salts and different passwords maybe.
## Q7.
`sudo su sab1`:
1.  Requires the password of the current user (sudo privileges).
2. ???
3. ???
`su sab1`:
1. requires password of sab1.
2. Will fail.
3. Will fail.
## Q8.
a-
possible combinations :
$62⁶ = 56.800.235.584$
Time required:
$\frac{56.800.235.584}{10} = 5.680.023.558.4 seconds$
b-
Time required:
$\frac{56.800.235.584}{500.000.000} = 113.6seconds$
c-
possible combinations :
$62⁸ = 218.340.105.584.896seconds$
Time required:
$\frac{218.340.105.584.896}{500.000.000} = 436.680 seconds$
## Q9.

$Total attempts=1,000,000passwords/second×60×60×24×365=31,536,000,000,000attempts.$
$2×31,536,000,000,000=63,072,000,000,000combinations.$

$alphabet^L≥63,072,000,000,000$

1. Lowercase Letters:
	$26^L≥63,072,000,000,000$
	$L≥\frac{\log(63,072,000,000,000)}{log(26)}​≈9.75$
	Minimum length = 10
2. Lowercase, uppercase letters, and digits:
	$62^L≥63,072,000,000,000$
	$L≥\frac{\log(63,072,000,000,000)}{log(62)}​≈7.7$
	Minimum length = 8
3. 92 different characters
	$92^L≥63,072,000,000,000$
	$L≥\frac{\log(63,072,000,000,000)}{log(92)}​≈7.03$
	Minimum length = 7

## Q10.
The second one is more secure since it will fail if the authentication fails the user will be denied the first one will always grant access

## Q11.
1. too short

