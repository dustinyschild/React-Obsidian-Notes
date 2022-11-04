A basic example of how to handle an api request using a custom hook:

use-endpoint.js
```
import { useState, useCallback } from 'react'
import { myApi } from '.../my-api'

const useEndpoint = ({ endpoint, onSuccess, onError }) => {
	const [ data, setData ] = useState(null)
	const [ isLoading, setIsLoading ] = useState(false)
	const [ error, setError ] = useState(null)

	const handleSuccess = useCallback((response) => {
		setData(response.data.json())
		setError(null)

		onSuccess() // trigger side-effects in your component

		return response
	}, [onSuccess])

	const handleFailure = useCallback((response) => {
		setData(null)
		setError(response.error)

		onFailure() // trigger side-effects in your component

		return response
	}, [onFailure])

	const makeRequest = async useCallback((params, data) => {
		setIsLoading(true)
		await myApi.get(endpoint) // don't return, set state instead
			.then(handleSuccess)
			.catch(handleFailure)
			.finally((response) => setIsLoading(false))
	}, [endpoint, handleSuccess, handleFailure])

	return [{ data, isLoading, error}, makeRequest]
}

export default useEndpoint
```

But this functionality is limited. What if we need to handle multiple requests to the same api?

If the number of requests is known, or if our component is consuming multipe api endpoints we can compose them like so:

**Note: This example is contrived. More than likely your consuming component will determine the implementation specifics.**

use-multiple-endpoints.js
```
import { useReducer, useCallback } from 'react'
import useEndpoint from './use-endpoint'
import myReducer from './my-reducer'

const apiOneConfig = {
	endpoint: '/endpoint-one'
	onSuccess: console.log
	onError: console.error
}

const apiTwoConfig = {
	endpoint: '/endpoint-two'
	onSuccess: console.log
	onError: console.error
}

const useMultipleEndpoints = () => {
	const [apiOne, makeApiOneRequest] = useEndpoint(apiOneConfig)
	const [apiTwo, makeApiTwoRequest] = useEndpoint(apiTwoConfig)

	const fetchAll = async useCallback((/* your params */) => {
		// structure this however your requests need to be handled
		await makeApiOneRequest()
		await makeApiTwoRequest()
	}, [makeApiOneRequest, makeApiTwoRequest])

	return [[apiOne, apiTwo], fetchAll]
}

export default useMultipleEndpoints
```


What if we need to call the same endpoint N times?
Note: in this case wei

use-mapped-endpoint.js
```

```