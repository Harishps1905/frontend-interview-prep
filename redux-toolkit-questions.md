
# Redux Toolkit + Thunk + RTK Query Interview Questions (2025)

## Senior Developer Level - SSDE Prep

**Redux Toolkit (RTK) is the official standard. Expect questions on `createSlice`, `createAsyncThunk`, RTK Query caching, and when to use each vs Context/Zustand.**[^1][^2][^3]

## Redux Toolkit Fundamentals (1-15)

**1. What is Redux Toolkit? Why use it over vanilla Redux?**
RTK is the official, opinionated toolset: `configureStore()` (devTools/middleware auto), `createSlice()` (Immer + action creators), `createAsyncThunk()` (async boilerplate). Reduces 100+ lines to 20. **Answer**: Less boilerplate, built-in Immer, TypeScript support, RTK Query.[^4][^2]

**2. What does `createSlice` do?**
Combines reducer + actions: `createSlice({ name, initialState, reducers })` auto-generates action types/creators. Immer enables "mutative" syntax.

```js
const usersSlice = createSlice({
  name: 'users',
  initialState: { list: [] },
  reducers: {
    addUser: (state, action) => { state.list.push(action.payload); }
  }
});
```

**Answer**: Reduces boilerplate, auto action types, Immer mutability.[^3]

**3. `configureStore` vs `createStore`?**
`configureStore` auto-adds Redux Thunk, DevTools, immutable middleware. Single reducer injection.

```js
export const store = configureStore({
  reducer: { users: usersReducer }
}); // Auto DevTools + thunk
```

**Answer**: Production-ready defaults.[^5]

**4. Role of Immer in RTK?**
Enables "mutative" code (`state.list.push(item)`) while producing immutable updates. Draft proxy magic.
**Answer**: Write mutable code, get immutable results.[^3]

**5. `extraReducers` vs `reducers`?**
`reducers`: Synchronous actions. `extraReducers`: Async thunk cases (`pending/fulfilled/rejected`).

```js
extraReducers: (builder) => {
  builder.addCase(fetchUsers.fulfilled, (state, action) => {
    state.list = action.payload;
  });
}
```

**Answer**: Handle thunk lifecycles.[^6]

## createAsyncThunk Deep Dive (16-25)

**6. What is `createAsyncThunk`? Lifecycle?**
Generates thunk with `pending/fulfilled/rejected` actions. PayloadCreator receives `(arg, thunkAPI)`.

```js
export const fetchUsers = createAsyncThunk(
  'users/fetch',
  async (page = 1, { rejectWithValue }) => {
    const res = await userApi.get(`/users?page=${page}`);
    if (!res.ok) return rejectWithValue(res.error);
    return res.data;
  }
);
```

**Answer**: Async action with loading/error states.[^6]

**7. `thunkAPI` methods?**
`getState()`, `dispatch()`, `rejectWithValue()`, `requestId`, `signal` (abort).
**Answer**: Access store, custom error payloads, AbortController.[^6]

**8. `pending/fulfilled/rejected` handling?**

```js
extraReducers: (builder) => {
  builder
    .addCase(fetchUsers.pending, (state) => {
      state.loading = true;
      state.error = null;
    })
    .addCase(fetchUsers.fulfilled, (state, action) => {
      state.loading = false;
      state.list = action.payload;
    })
    .addCase(fetchUsers.rejected, (state, action) => {
      state.loading = false;
      state.error = action.payload;
    });
}
```

**Answer**: Full request lifecycle management.[^7]

**9. `createAsyncThunk` error handling?**
`rejectWithValue(err)` for custom payloads, `error.response.data` accessible in rejected case.
**Answer**: Structured error payloads vs throwing.[^6]

**10. Conditional thunk dispatch?**
`if (getUsersLoading(state)) return;` inside thunk or `skip: true` option.
**Answer**: Prevent duplicate requests.[^6]

## RTK Query Mastery (26-40)

**11. RTK Query vs `createAsyncThunk`?**
**RTK Query**: Auto-caching, auto-refetch, tags, optimistic updates, generated hooks. **Thunk**: Custom async (file uploads, complex workflows). Use RTK Query 90% time.
**Answer**: RTK Query eliminates thunk boilerplate + adds caching.[^8][^1]

**12. `createApi` structure?**

```js
export const api = createApi({
  reducerPath: 'api',
  baseQuery: fetchBaseQuery({ baseUrl: '/api' }),
  tagTypes: ['User', 'Post'],
  endpoints: (builder) => ({
    getUsers: builder.query({ query: () => 'users', providesTags: ['User'] }),
    addUser: builder.mutation({ query: (user) => ({ url: 'users', method: 'POST', body: user }), invalidatesTags: ['User'] })
  })
});
```

**Answer**: Declarative API layer.[^1]

**13. `providesTags` / `invalidatesTags`?**
`providesTags: ['User']` - marks data as "User" type. `invalidatesTags: ['User']` - refetches all "User" queries.
**Answer**: Automatic cache invalidation.[^1]

**14. Auto-generated hooks?**
`useGetUsersQuery()`, `useLazyGetUsersQuery()`, `useAddUserMutation()`.

```js
const { data, isLoading, error } = useGetUsersQuery();
const [addUser, { isLoading }] = useAddUserMutation();
```

**Answer**: Typed, cached, lifecycle-aware.[^1]

**15. `baseQuery` customization?**

```js
const baseQuery = fetchBaseQuery({
  baseUrl: '/api',
  prepareHeaders: (headers, { getState }) => {
    const token = (getState()).auth.token;
    if (token) headers.set('authorization', `Bearer ${token}`);
    return headers;
  }
});
```

**Answer**: Auth, logging, retry logic.[^1]

**16. Cache lifetime control?**
`keepUnusedDataFor: 60` (seconds), `refetchOnMountOrArgChange: true`.
**Answer**: Prevent stale data, control memory.[^1]

**17. Optimistic updates in RTK Query?**

```js
addPost: builder.mutation({
  query: (newPost) => ({ url: 'posts', method: 'POST', body: newPost }),
  async onQueryStarted({ draft: newPost }, { dispatch, queryFulfilled }) {
    const patchResult = dispatch(api.util.updateQueryData('getPosts', undefined, (draft) => {
      draft.push(newPost);
    }));
    try {
      await queryFulfilled;
    } catch {
      patchResult.undo();
    }
  }
})
```

**Answer**: Rollback on failure.[^1]

## Advanced Patterns (41-50)

**18. Redux Thunk vs RTK Query?**
**Thunk**: Any async (uploads, analytics). **RTK Query**: REST/GraphQL caching. RTK Query uses thunks internally.
**Answer**: RTK Query for data fetching, thunk for side effects.[^9]

**19. Normalized state with RTK?**

```js
const usersAdapter = createEntityAdapter();
const usersSlice = createSlice({
  name: 'users',
  initialState: usersAdapter.getInitialState(),
  reducers: {
    usersReceived: usersAdapter.setAll
  }
});
```

**Answer**: `createEntityAdapter` for ids → entities.[^2]

**20. RTK Query + Server-Side Rendering?**
Prefetch: `dispatch(api.endpoints.getUsers.initiate())`, hydrate: `hydrationData`.
**Answer**: `prefetch` + `hydration` for SSR.[^9]

**21. `skip` / `forceRefetch` options?**

```js
const { data } = useGetUsersQuery(undefined, { skip: !isLoggedIn });
```

**Answer**: Conditional queries, manual refresh.[^1]

**22. Multiple API slices?**

```js
const store = configureStore({
  reducer: {
    [usersApi.reducerPath]: usersApi.reducer,
    [postsApi.reducerPath]: postsApi.reducer
  },
  middleware: (getDefaultMiddleware) => 
    getDefaultMiddleware().concat(usersApi.middleware, postsApi.middleware)
});
```

**Answer**: Modular API management.[^1]

**23. RTK Query cache invalidation strategies?**

- `invalidatesTags`: All matching queries
- `providesTags`: Granular (current page)
- `refetchOnFocus/Mount`
**Answer**: Tag-based automatic refetching.[^1]


## Integration \& Best Practices

**24. Store setup with RTK Query?**

```js
export const store = configureStore({
  reducer: { 
    [api.reducerPath]: api.reducer,
    ui: uiSlice.reducer 
  },
  middleware: (getDefault) => 
    getDefault().concat(api.middleware)
});
```

**Answer**: Single `configureStore` with multiple reducers.[^1]

**25. When to use Context vs RTK?**
**Context**: Theme, UI state. **RTK**: App data, async ops.
**Answer**: Context for transient UI, RTK for persistent data.[^3]

## SSDE Priority Questions

1. **RTK Query vs Thunk** - Always prefer RTK Query for APIs[^8]
2. **`createSlice` + `extraReducers`** - Handle async lifecycles[^6]
3. **Cache invalidation** - `providesTags/invalidatesTags` mastery[^1]
4. **Optimistic updates** - Rollback patterns[^1]
5. **Normalized state** - `createEntityAdapter`[^2]

**Practice**: Build users CRUD with RTK Query + optimistic updates + pagination. Explain why RTK Query > thunks 90% time.[^3][^1]
