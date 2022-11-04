This is my preferred pattern for using the React Context API when state does not need to be updated by Consumers.

Consider this pattern for persisting data across the application (Like redux, but not required to be global).

my-context.jsx
```
import { createContext, useContext, useState } from 'react'

export const MyContext = createContext('default value')

export const MyContextProvider = ({ children }) => {
	const [vaules, setValues] = useState({})

	return (
		<MyContext.Provider value={{values, setValues}}>
			{children}
		</MyContext.Provider>
	)
}
```

use-my-context.jsx
```
import { MyContext } from './my-context'

export const useMyContext = () => {
	const {values, setValues} = useContext(MyContext)

	const interfacedAction = () => {
		// do a thing with setValues
	}

	return {
		values,
		interfacedAction // should actions be wrapped in an `actions` object?
	}
}

```

