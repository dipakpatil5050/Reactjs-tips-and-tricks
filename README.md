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


#### Install Packages 

```bash
npm i @reduxjs/toolkit redux-persist react-redux
```


1. setup store with redux persistStore: 

store.tsx
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


2.  wrap root app.js with redux Provider :

App.tsx
```js

import { Toaster } from "@/components/ui/toaster";
import { Provider } from "react-redux";
import { PersistGate } from "redux-persist/integration/react";
import AppRouter from "./routes/Routes";
import store, { persistor } from "./store/store";


const App = () => (
  <Provider store={store}>
    <PersistGate loading={<p>Loading...</p>} persistor={persistor}>
              <Toaster />
              <AppRouter />
    </PersistGate>
  </Provider>
);

export default App;

```


3. create store/slices folder in src

userSlice.ts
   ```js
   
import { createSlice } from "@reduxjs/toolkit";

const initialState = {
  user: null,
  isAuthenticated: false,
  token: null,
  error: null,
  loading: false,
  role: null,
};

const userSlice = createSlice({
  name: "user",
  initialState,
  reducers: {
    setloginSuccess(state, action) {
      state.user = action.payload.user;
      state.token = action.payload.token;
      state.isAuthenticated = true;
      state.loading = false;
    },
    setloginFailure(state, action) {
      state.error = action.payload;
      state.loading = false;
  },
},
});

export const {
  setloginSuccess,
  setloginFailure,
} = userSlice.actions;
export default userSlice.reducer;
   ```

4. create Hooks folder in store 

reduxHooks.ts

```js

import { useDispatch, useSelector } from "react-redux";
import { AppDispatch, RootState } from "../store";

export const useAppDispatch = useDispatch.withTypes<AppDispatch>();
export const useAppSelector = useSelector.withTypes<RootState>();


```
