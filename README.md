# Raect Js Tips and Tricks 


## How to access .env variables in React Frontend App


### CRA

.env = REACT_APP_API_URL="my sample api"

```bash
const baseUrl = process.env.REACT_APP_API_URL
```


### Vite 
.env = VITE_MY_API="my sample api"

```bash
const baseUrl = import.meta.env.VITE_MY_API

```


