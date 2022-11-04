- Create an instance for each API your app consumes
	- This allows you to set configuration on a per-api basis. Things like scope, client id/secret may need to be different to make successful requests to apis
	- Allows for exporting a named object that references the api
- Optional but recommended, define endpoints in the same file as your api instance object
	- This gives the opportunity to export named exports for specific endpoints
		- Q: Can/should these be bound to the axios object? This might be good for readability if it's possible without upsetting the typescript definitions

api-example.js
```
import axios from 'axios'



```