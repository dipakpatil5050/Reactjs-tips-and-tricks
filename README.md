---

# React JS Tips and Tricks

This document provides essential tips and best practices for working with React, covering environment variables, state management using Redux Toolkit, and setting up an API interceptor.

---

## 1. Accessing `.env` Variables in React Frontend App

### **Create React App (CRA)**

For CRA, environment variables should be prefixed with `REACT_APP_` to be accessible in the React code.

**Example:**

**.env** file

```bash
REACT_APP_API_URL="my_sample_api"
```

In your React component:

```js
const baseUrl = process.env.REACT_APP_API_URL;
```

---

### **Vite**

For Vite, environment variables should be prefixed with `VITE_`.

**Example:**

**.env** file

```bash
VITE_MY_API="my_sample_api"
```

In your React component:

```js
const baseUrl = import.meta.env.VITE_MY_API;
```

---

## 2. State Management Setup: **Redux Toolkit**

State management is crucial for larger React applications. Below are the steps for setting up Redux with **Redux Toolkit** and **Redux Persist**.

### **Install Required Packages**

```bash
npm i @reduxjs/toolkit redux-persist react-redux
```

---

### **1. Setup Store with Redux Persist**

Configure the Redux store and integrate **redux-persist** for persisting the state across page reloads.

```ts
// store/store.tsx

import { configureStore } from "@reduxjs/toolkit";
import { combineReducers } from "redux";
import { persistReducer, persistStore } from "redux-persist";
import storage from "redux-persist/lib/storage";  // LocalStorage as storage
import dashboardReducer from "./slices/dashboardSlice";
import userReducer from "./slices/userSlice";

// Combine all reducers
const rootReducer = combineReducers({
  user: userReducer,
  dashboard: dashboardReducer,
});

// Configure redux-persist
const persistConfig = {
  key: "root",
  storage,
};

const persistedReducer = persistReducer(persistConfig, rootReducer);

// Create Redux store
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

---

### **2. Wrap Root `App.js` with Redux Provider**

To provide access to the Redux store in your entire application, wrap the root component with the **`Provider`** and **`PersistGate`** components.

```ts
// App.tsx

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

---

### **3. Create Store/Slices Folder**

Next, we create the **slices** to manage different pieces of state. Here's an example of a **userSlice** for managing user authentication.

```ts
// store/slices/userSlice.ts

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

---

### **4. Create Custom Redux Hooks**

To make it easier to access the Redux store and dispatch actions, create custom hooks.

```ts
// store/Hooks/reduxHooks.ts

import { useDispatch, useSelector } from "react-redux";
import { AppDispatch, RootState } from "../store";

// Create custom hooks
export const useAppDispatch = useDispatch.withTypes<AppDispatch>();
export const useAppSelector = useSelector.withTypes<RootState>();
```

---

## 3. API Interceptor Setup

Interceptors allow you to modify HTTP requests and responses globally. Below is an example of setting up an API interceptor using **Axios** for handling authorization tokens and error handling.

### **Setup Axios Interceptor**

```ts
import { VITE_SERVER_URL } from "@/config/env";
import store from "@/store/store";
import axios from "axios";
import toast from "react-hot-toast";

export const http = axios.create({
  baseURL: VITE_SERVER_URL,
  headers: {
    "Content-Type": "application/json",
  },
});

// Request Interceptor
http.interceptors.request.use(
  async (config) => {
    const state = store.getState();
    const { token } = state.user;

    if (token) {
      config.headers.Authorization = `Bearer ${token}`;
    }
    config.headers["Content-Type"] = "application/json";
    
    return config;
  },
  (error) => {
    return Promise.reject(error);
  }
);

// Response Interceptor
http.interceptors.response.use(
  (response) => {
    // Handle successful response
    return response;
  },
  async (error) => {
    // Handle error response
    if (error.response && error.response.status === 401) {
      // Unauthorized access, token might be expired
      toast.error("Session expired");
    }
    return Promise.reject(error);
  }
);
```

#Production grade Folder Structure :

```
├───api
│   └───Auth
├───assets
│   ├───Icons
│   │   └───favicon
│   └───images
│       └───logo
├───components
│   ├───dashboard
│   ├───layouts
│   ├───ui
│   └───widgets
│       └───charts
├───config
├───constant
├───context
├───hooks
├───lib
├───pages
│   ├───dashboard
│   └───User
├───routes
│   ├───hooks
│   └───modules
├───services
├───store
│   ├───hooks
│   └───slices
├───types
└───utils

```


---

## Conclusion

This document covers some essential tips and tricks for working with React, including:

1. **Accessing environment variables** in React apps.
2. **Setting up Redux Toolkit** for state management along with **Redux Persist** for persisting state.
3. **Setting up Axios interceptors** to manage authorization and error handling globally.

By following these best practices, you can streamline your React application development, making it more maintainable, efficient, and scalable.

---
