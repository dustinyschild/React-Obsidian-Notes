This looks ugly in the React devtools. However, I prefer composing multiple contexts over creating a `GlobalContext`-ish pattern due to the nature of forced rerenders caused by Context API (see todo: insert link to explanation on forced Consumer rerendering).

The best implementation I've come across uses an HOC to inject the necessary providers

**IMPORTANT: This pattern does not inject props into context wrappers. It is assumed that all contextual configuration is handled within the Context itself**

TODO: Test this code in an app
with-wrappers.jsx
```
// Wrappers MUST be an array
const composeComponents = (components) => 
	components.reduce((Providers, Wrapper) => {
		return (
			<Providers>
				<Wrapper />
			</Providers>
		)
	}, 
	({ children }) => children // return just children by default
)

const withWrappers = (wrappers) => (Component) => {
	return composeComponents([...wrappers, Component])
}

export default withWrappers
```

App.jsx
```
import withWrappers from './with-wrappers'

const App = () => {
	return (
		{/* Your mark up */}
	)
}

// pass as many wrappers as you need, keep in mind order matters!!
export default withWrappers([AuthContextProvider, ThemeProvider, etc...])(App)
```


