
## Tenstack Query : Server side state Management 




# Query
### Fetch Get API Date 
```js
import { useQuery } from 'react-query';

const { data, isLoading, isError } = useQuery('todos', async () => {
  const response = await fetch('/api/todos');
  const data = await response.json();
  return data;
});

```



## useMutation 
### useMutation example to Post data via RTQ

```js
const { mutate, isLoading, isError } = useMutation(async (text) => {
  const response = await fetch('/api/todos', {
    method: 'POST',
    body: JSON.stringify({ text }),
    headers: {
      'Content-Type': 'application/json',
    },
  });
  const data = await response.json();
  return data;
});

```



## Root provider 

`
```js

import { QueryClient, QueryClientProvider } from 'react-query';

const queryClient = new QueryClient();

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      {/* Your app code here */}
    </QueryClientProvider>
  );
}
```
