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

--------------------------------------------------------------------------------------------------------------


## State Management Setup : Redux Tookit 


!. setup store with redux persistStore: 


```js
import { configureStore } from "@reduxjs/toolkit";
import { combineReducers } from "redux";
import { persistReducer, persistStore } from "redux-persist";
import storage from "redux-persist/lib/storage";
import dashboardReducer from "./slices/dashboardSlice";
import userReducer from "./slices/userSlice";

const rootReducer = combineReducers({
  user: userReducer,
  dashboard: dashboardReducer,
});

const persistConfig = {
  key: "root",
  storage,
};

const persistedReducer = persistReducer(persistConfig, rootReducer);

const store = configureStore({
  reducer: persistedReducer,
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware({
      serializableCheck: false,
    }),
});

export const persistor = persistStore(store);
export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;

export default store;



```










