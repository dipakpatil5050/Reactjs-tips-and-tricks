
## Tenstack Query : Server side state Management 






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
