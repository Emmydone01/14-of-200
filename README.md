def compare_methods(f, f_prime, a, b, x0, tol=1e-6, max_iter=100):
    def bisection(f, a, b, tol, max_iter):
        steps = 0
        if f(a) * f(b) >= 0:
            return None, steps  # Root not bracketed
        while (b - a) / 2.0 > tol and steps < max_iter:
            midpoint = (a + b) / 2.0
            if f(midpoint) == 0:
                return midpoint, steps + 1
            elif f(a) * f(midpoint) < 0:
                b = midpoint
            else:
                a = midpoint
            steps += 1
        return (a + b) / 2.0, steps

    def newton_raphson(f, f_prime, x0, tol, max_iter):
        steps = 0
        x = x0
        for _ in range(max_iter):
            fx = f(x)
            if abs(fx) < tol:
                return x, steps
            fpx = f_prime(x)
            if fpx == 0:
                return None, steps
            x = x - fx / fpx
            steps += 1
        return x, steps

    root_bisect, steps_bisect = bisection(f, a, b, tol, max_iter)
    root_newton, steps_newton = newton_raphson(f, f_prime, x0, tol, max_iter)

    return {
        "bisection": {"root": root_bisect, "steps": steps_bisect},
        "newton_raphson": {"root": root_newton, "steps": steps_newton}
    }

# Example usage:
import math

f = lambda x: x**3 - x - 2
f_prime = lambda x: 3*x**2 - 1

result = compare_methods(f, f_prime, a=1, b=2, x0=1.5)
print(result)
