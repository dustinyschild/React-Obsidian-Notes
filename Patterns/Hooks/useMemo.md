When to use:
	useMemo is very good when a state is derived but not a direct reference to the original data.
	For example, using array find to get a specific element from the array, unless the underlying data hanges this can be cached.
	***Side note: Keep in mind the cost of the function; Only consider using memoization if your computation is O(n^2) or higher***

When NOT to use:
	Direct references to data or any any nested data therein. Opt for destructuring wherever possible in these scenarios.

References:
- https://blog.logrocket.com/rethinking-hooks-memoization/