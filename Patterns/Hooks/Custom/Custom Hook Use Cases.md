Do's and Don'ts of custom hooks

Use custom hooks to encapsulate related logic, for instance, many related states and/or logic to handle said state, refactor out the state and actions to a hook that can expose an interface to your consuming component.

For example, say your component cosumes an API endpoint:

This example encapsulates logic for a specific api request

use-endpoint.js
```
import { useState } from 'react'
import { myApi } from '.../my-api'

const useEndpoint = ({ endpoint, onSuccess, onError }) => {
	const [ data, setData ] = useState(null)
	const [ isLoading, setIsLoading ] = useState(false)
	const [ error, setError ] = useState(null)

	const handleSuccess = useCallback(({ data }) => {
		setData(responseData)
		setError(null)

		onSuccess() // trigger side-effects in your component
	}, [onSuccess])

	const handleFailure = useCallback(({ error }) => {
		setData(null)
		setError(error)

		onFailure() // trigger side-effects in your component
	}, [onFailure])

	const makeRequest = async useCallback((params, data) => {
		setIsLoading(true)
		await myApi.get(endpoint) // don't return, set state instead
			.then(handleSuccess)
			.catch(handleFailure)
			.finally(() => setIsLoading(false))
	}, [endpoint, handleSuccess, handleFailure])

	return [{ data, isLoading, error}, makeRequest]
}

export default useEndpoint
```