#include <assert.h>

#include <numeric>
#include <algorithm>
#include <functional>

//+----------------------------------------------------------------------------+
//| Spoils of the Egyptians Lecture 2						   |
//| Обощим итератирвную версию алгоритма.				        |
//+----------------------------------------------------------------------------+

// semigroup: полугруппой называют множество с заданной на 
// нем ассоциативной бинарной операцией(S, *).
// additive: binary operation +
// Noncommutative : Commutative Semigroup

// monoid is a semigroup that contains an identity element id:
// a @ id = id @ a = a
// additive monoid is an additive semigroup where the identity element is 0:
// a + 0 = 0 + a = a

// group is a monoid with inverse operation:
// a @ inv(a) = inv(a) @ a = id
// additive group is an additive monoid with unary - (minus) such that
// a + -a = -a + a = 0

#define Integer								typename 
#define NonCommutativeAdditiveSemigroup		typename 
#define CommutativeAdditiveSemigroup		typename 
#define NonCommutativeAdditiveMonoid		typename 
#define MultiplicativeSemigroup				typename 
#define MultiplicativeMonoid				typename 
#define AdditiveGroup						typename 
#define MultiplicativeGroup					typename 

#define Regular								typename
#define SemigroupOperation					typename
#define MonoidOperation						typename

#define requires(...)						

#define precondition(expr) assert(expr)

//+----------------------------------------------------------------------------+
//| multiply_semigroup														   |
//+----------------------------------------------------------------------------+

template<NonCommutativeAdditiveSemigroup A, Integer N>
A multiply_accumulate_semigroup(A r, A a, N n)
{
	for(r = odd(n) ? r + a : r; n = half(n); r += (a + a));
	return r;
}

template<NonCommutativeAdditiveSemigroup A, Integer N>
A multiply_semigroup(A a, N n)
{
	precondition(n > 0);
	if (n == 1) return a;											
	return multiply_accumulate_semigroup(a, a, n - 1);
}

//+----------------------------------------------------------------------------+
//| multiply_monoid															   |
//+----------------------------------------------------------------------------+

template<NonCommutativeAdditiveMonoid A, Integer N>
A multiply_accumulate_monoid(A r, A a, N n)
{
	for(r = odd(n) ? r + a : r; n = half(n); r += (a + a));
	return r;
}

template<NonCommutativeAdditiveMonoid A, Integer N>
A multiply_monoid(A a, N n)
{
	precondition(n >= 0);
	if(n == 0) return A(0);											
	return multiply_accumulate_monoid(0, n, a);
}

//+----------------------------------------------------------------------------+
//| power_semigroup															   |
//+----------------------------------------------------------------------------+

template<MultiplicativeSemigroup A, Integer N>
A power_accumulate_semigroup(A r, A a, N n)
{
	precondition(n >= 0);
	if(n == 0) return r;		// почему не A(1)

	for(r = odd(n) ? r + a : r; n = half(n); r *= (a * a));
	return r;
}

template<MultiplicativeSemigroup A, Integer N>
A power_semigroup(A a, N n)
{
	precondition(n > 0);
	if(n == 1) return a;

	return power_accumulate_semigroup(a, a, n - 1);
}

//+----------------------------------------------------------------------------+
//| power_monoid										  					    |
//+----------------------------------------------------------------------------+

template<MultiplicativeMonoid A, Integer N>
A power_monoid(A a, N n)
{
	precondition(n >= 0);
	if(n == 0) return A(1);

	return power_semigroup(a, n);
}

//+----------------------------------------------------------------------------+
//| power_group											  					    |
//+----------------------------------------------------------------------------+

template<MultiplicativeGroup A>
A multiplicative_inverse(A a)
{
	return A(1) / a;
}

template<MultiplicativeSemigroup A, Integer N>
A power_group(A a, N n)
{
	if(n < 0)
	{
		n = -n;
		a = multiplicative_inverse(a);
	}
	return power_monoid(a, n);
}

//+----------------------------------------------------------------------------+
//| Generalize accumulate for an arbitary semigroup				  	     	    |
//+----------------------------------------------------------------------------+

//+----------------------------------------------------------------------------+
//| power_semigroup																|
//+----------------------------------------------------------------------------+

template<Regular A, Integer N, SemigroupOperation Op>
requires(Domain<Op, A>)
A power_accumulate_semigroup(A r, A a, N n, Op op)
{
	precondition(n >= 0);
	if(n == 0) return r;		

	for(r = odd(n) ? op(r, a) : r; n = half(n); r = op(r, op(a, a)));
	return r;
}

template<Regular A, Integer N, SemigroupOperation Op>
requires(Domain<Op, A>)
A power_semigroup(A a, N n, Op op)
{
	precondition(n > 0);

	if (n == 1) return a;
	return power_accumulate_semigroup(a, a, n - 1, op);
}

//+----------------------------------------------------------------------------+
//| power_monoid																|
//+----------------------------------------------------------------------------+

template<NonCommutativeAdditiveMonoid T>
T identity_element(std::plus<T>)
{
	return T(0);
}

template<MultiplicativeMonoid T>
T identity_element(std::multiplies<T>)
{
	return T(1);
}

template<AdditiveGroup T>
std::negate<T> inverse_operation(std::plus<T>)
{
	return std::negate<T>();
}

template<MultiplicativeGroup T>
struct reciprocal : public  std::unary_function<T, T>
{
	T operator()(const T& x)
	{
		return T(1) / x;
	}
};

template<MultiplicativeGroup T>
reciprocal<T> inverse_operation(std::multiplies<T>)
{
	return reciprocal<T>();
}

template<Regular A, Integer N, MonoidOperation Op>
//requires(Domain<Op, A>)
A power_monoid(A a, N n, Op op)
{
	precondition(n >= 0);

	if (n == 0) return identity_element(op);
	return power_semigroup(a, n, op);
}
