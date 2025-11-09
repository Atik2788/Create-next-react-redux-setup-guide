# 🚀 React + Redux Toolkit + Axios + Tailwind + Router – Full Project Setup

This repository contains a **complete, scalable React project setup** that integrates:

- ⚡ Vite (for blazing-fast development)
- 🎨 Tailwind CSS (for styling)
- 🌐 React Router (for navigation)
- 🧩 Redux Toolkit + RTK Query (for state & API management)
- 🔌 Axios (for HTTP requests and interceptors)

---

## 🧠 Project Overview

Building a new React project can be confusing for beginners —  
Which one first? Tailwind, Router, Redux, or Axios?

This repo provides a **step-by-step setup guide** so you can start clean every time without repeating the same boilerplate setup.

---

## 🪜 Setup Steps

### 1️⃣ Create project with Vite
```bash
npm create vite@latest my-app --template react
cd my-app
npm i
2️⃣ Install dependencies
bash
Copy code
npm i react-router-dom @reduxjs/toolkit react-redux axios
npm i -D tailwindcss postcss autoprefixer
3️⃣ Configure Tailwind
bash
Copy code
npx tailwindcss init -p
Edit tailwind.config.js:

js
Copy code
content: ["./index.html", "./src/**/*.{js,ts,jsx,tsx}"]
Add this to index.css:

css
Copy code
@tailwind base;
@tailwind components;
@tailwind utilities;
4️⃣ Setup Router
src/main.tsx

tsx
Copy code
import React from "react";
import ReactDOM from "react-dom/client";
import { BrowserRouter } from "react-router-dom";
import { Provider } from "react-redux";
import { store } from "./redux/store";
import App from "./App";

ReactDOM.createRoot(document.getElementById("root")!).render(
  <React.StrictMode>
    <Provider store={store}>
      <BrowserRouter>
        <App />
      </BrowserRouter>
    </Provider>
  </React.StrictMode>
);
src/App.tsx

tsx
Copy code
import { Routes, Route } from "react-router-dom";
import Home from "./pages/Home";
import Login from "./pages/Login";

function App() {
  return (
    <Routes>
      <Route path="/" element={<Home />} />
      <Route path="/login" element={<Login />} />
    </Routes>
  );
}

export default App;
5️⃣ Configure Redux Store
src/redux/store.ts

ts
Copy code
import { configureStore } from "@reduxjs/toolkit";
import { baseApi } from "./baseApi";
import { setupListeners } from "@reduxjs/toolkit/query";

export const store = configureStore({
  reducer: {
    [baseApi.reducerPath]: baseApi.reducer,
  },
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware().concat(baseApi.middleware),
});

setupListeners(store.dispatch);

export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;
6️⃣ Setup Axios Base Query
src/redux/axiosBaseQuery.ts

ts
Copy code
import { axiosInstance } from "@/lib/axios"
import type { BaseQueryFn } from "@reduxjs/toolkit/query"
import type { AxiosError, AxiosRequestConfig } from "axios"

const axiosBaseQuery =
  (): BaseQueryFn<
    {
      url: string
      method?: AxiosRequestConfig['method']
      data?: AxiosRequestConfig['data']
      params?: AxiosRequestConfig['params']
      headers?: AxiosRequestConfig['headers']
    },
    unknown,
    unknown
  > =>
  async ({ url, method, data, params, headers }) => {
    try {
      const result = await axiosInstance({ url, method, data, params, headers })
      return { data: result.data }
    } catch (axiosError) {
      const err = axiosError as AxiosError
      return {
        error: {
          status: err.response?.status,
          data: err.response?.data || err.message,
        },
      }
    }
  }

export default axiosBaseQuery
7️⃣ Base API (RTK Query)
src/redux/baseApi.ts

ts
Copy code
import { createApi } from "@reduxjs/toolkit/query/react";
import axiosBaseQuery from "./axiosBaseQuery";

export const baseApi = createApi({
  reducerPath: "baseApi",
  baseQuery: axiosBaseQuery(),
  endpoints: () => ({}),
});
8️⃣ Feature API (Auth)
src/redux/features/authApi.ts

ts
Copy code
import { baseApi } from "@/redux/baseApi";

const authApi = baseApi.injectEndpoints({
  endpoints: (builder) => ({
    login: builder.mutation({
      query: (userInfo) => ({
        url: "/auth/login",
        method: "POST",
        data: userInfo,
      }),
    }),
    register: builder.mutation({
      query: (userInfo) => ({
        url: "/users/register",
        method: "POST",
        data: userInfo,
      }),
    }),
  }),
});

export const { useRegisterMutation, useLoginMutation } = authApi;
9️⃣ Typed Hooks
src/redux/hooks.ts

ts
Copy code
import { useDispatch, useSelector } from "react-redux";
import type { RootState, AppDispatch } from "./store";

export const useAppDispatch = useDispatch.withTypes<AppDispatch>();
export const useAppSelector = useSelector.withTypes<RootState>();
🔟 Folder Structure
css
Copy code
src/
├── App.tsx
├── main.tsx
├── pages/
│   ├── Home.tsx
│   └── Login.tsx
├── redux/
│   ├── store.ts
│   ├── baseApi.ts
│   ├── axiosBaseQuery.ts
│   ├── hooks.ts
│   └── features/
│       └── authApi.ts
└── lib/
    └── axios.ts
✅ Summary
Routing setup

Redux store configured

Axios integrated via RTK Query

Modular API system for scalability

🔗 GitHub Repo: github.com/Atik2788/react-redux-setup-guide

💡 Feel free to fork this repo or use it as a starting template for your next project!

#ReactJS #ReduxToolkit #RTKQuery #Axios #TailwindCSS #JavaScript #FrontendDevelopment #Vite #WebDevelopment #TypeScript
