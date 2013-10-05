#include <assert.h>

#include <numeric>
#include <algorithm>
#include <functional>


using namespace std;
//+----------------------------------------------------------------------------+
//| Spoils of the Egyptians Lecture 1										   |
//+----------------------------------------------------------------------------+

// (n + 1)a = na + a; //depends on distributivity

int multiply0(int n, int a)
{
	if (n == 1) return a;
	return multiply0(n - 1, a) + a;
}

int half(int n)
{
	return n >> 1;
}

bool odd(int n)
{
	return n & 1;
}

bool even(int n)
{
	return !odd(n);
}

int max(int n, int m)
{
	return n > m ? n : m;
}

// for result 15, count of additions == 6

int multiply1(int n, int a)
{
	if (n == 1) return a;
	int result = multiply1(half(n), a + a);			// log(n)
	if (odd(n)) result += a;						// v(n)
	return result;
}

// iter version:

// iter version:

int multiply_iter_impl(int a, int x, int y)
{
	for(a = odd(y) ? a + x : a; y = half(y); a += (x + x));
	return a;
}

int multiply_iter_com(int x, int y)									// x >= 1 && y >=1
{
	if(x == 1 || y == 1) return max(x, y);							// commutative
	return multiply_iter_impl(x, x, y - 1);
}

int multiply_iter_non_com(int x, int y)								// x >= 1 && y >=1
{
	if(x == 1) return x;											// non commutative
	return multiply_iter_impl(y, y, x - 1);
}

// log(n) + v(n) - 1
// v(n) - number of 1s in the binary representation of n.
//(population count) - общее количество.

// But for some numbers thar rule is not optimal:

// count of additions == 5

int multiply15(int n, int a)
{
	int result3 = a + a + a;
	int result6 = result3 + result3;
	int result12 = result6 + result6;
	return result12 + result3;
}
