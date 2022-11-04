This is my preferred pattern for using the React Context API when state needs to be updated by Consumers (e.g. Forms).

This is a useful pattern when we don't want to expose `setState` to consuming components. So the location of setState is moved to the custom hook rather than the Context.Provider.

BEWARE - Avoid nesting of the same Context.Provider!!
	*Todo: explore the consequences of this*
		Hypothesis; One of 2 things will happen:
			1. The nearest Provider ancestor will be used
			2. React will throw an error

unmanaged-context.jsx
```
import { createContext, useContext, useState } from 'react'
import useMyContext from './use-my-context'

export const MyContext = createContext('default value')

const UnmanangedContextProvider = ({ children }) => {
	const { values } = MyCustomContextHook

	return (
		<MyContext.Provider value={{values}}> // only the values state is exposed
			{children}
		</MyContext.Provider>
	)
}

export default MyContextProvider
```

use-my-context.jsx
```
const useMyContext = () => {
	const {values, setValues} = useContext(MyContext)

	const interfacedAction = () => {
		// do a thing with setValues
	}

	return {
		values,
		interfacedAction // should actions be wrapped in an `actions` object?
	}
}

export default useMyContext
```


(This could also be applied to a page level)
App.jsx
```
import { MyContextProvider } from './my-context'
import ChildComponent from './ChildComponent'

const App = () => (
	<MyContextProvider>
		<ChildComponent />
	</MyContextProvider>
)
```


ChildComponent.jsx
```
import { useContext } from 'react'
import { MyContext } from './my-context'

const ChildComponent = () => {
	const { values } = useContext(MyContext)

	const formattedCode = JSON.stringify(data, undefined, 2)

	return (
		<pre>
			<code>
				{formattedCode}
			</code>
		</pre>
	)
}

export default ChildComponent
```