# validadorCNPJ
Validador de CNPJ desenvolvido em Python.

This is a CNPJ (Brazil's Legal Person Registry) Validator. The format of such document is always: XX.YYY.ZZZ/AAAA-BB. And the digits are ALWAYS numbers.
However, there's a calculation to be done which will return the last two digits after the hyphen.
If the calculated and then newly generated CNPJ matches the one who was provided by the user, the CNPJ is valid.

This is how the calculation works
1. Remove all special characters ('.', '/', '-')
2. Get all the elements except the last two digits -> [:-2] in Python
3. Now we are going to multiply EACH element by an integer and this integer will decrease after each loop. This is how it looks like:

Example CNPJ: 04.252.011/0001-10

The integer starts at 5 and goes all the way down up until 2. Then it becomes 9 and goes all the way down AGAIN up until 2.
042520110001
0 * 5
4 * 4
2 * 3
5 * 2 # After 2, if  you have more elements, you will need to go back to 9
2 * 9
0 * 8
1 * 7
1 * 6
0 * 5
0 * 4
0 * 3
1 * 2

The result of each and all of the multiplications must be added.
The result of this example would be 67.
Now there's a formula to use:
11 - (total % 11)
According to the example, it will become:
11 - (67 % 11)
If the result of this expression is bigger than 9, the first digit after the hyphen will be zero.
If it's smaller than 9, it will keep its value 
In the example we are using, it will be:
11 - (65 % 11) = 1
So, the first digit is 1, because 1 is lower than 9.

Now we are going to add this digit to our [:-2] CNPJ, so now we have 13 elements:
0425201100011

The rule has changed a bit now, we are going to perform those multiplications AGAIN, but now we are starting at 6:
0 * 6
4 * 5
2 * 4
5 * 3 # After 2, if  you have more elements, you will need to go back to 9
2 * 2
0 * 9
etc.

After we repeat the same process, we are going to do the formula again:
11 - (67 % 11) = 11
Since the result is bigger than 9, our second digit after the hyphen will be ZERO.
So our newly generated CNPJ should be:
04252011000110

Does it match our input?
04.252.011/0001-10
Removing special characters: 04252011000110
04252011000110 == 04252011000110 TRUE

So it's a valid CNPJ and that's how we check it.

ATTENTION!!
There's a validation that needs to be done and it's in my code but it's always a heads-up:
sequences of the same number validate as well but they are not considered valid CNPJs.
So any 11111111111111, 22222222222222, etc. must be considered invalid.
