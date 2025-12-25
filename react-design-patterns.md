# Complete React Design Patterns Guide (2025)

## All 23 Patterns with Code Examples for Senior React Developers

**Comprehensive reference for  SSDE interviews. Includes your enterprise patterns (Factory, Service Layer, Feature Modules) + modern React 19 patterns.**

***

## Table of Contents

1. [Fundamental Patterns (1-5)](#fundamental-patterns)
2. [State \& Logic Sharing (6-10)](#state-logic-sharing)
3. [Enterprise Architecture (11-18)](#enterprise-architecture)
4. [Performance Patterns (19-21)](#performance-patterns)
5. [Modern 2025 Patterns (22-23)](#modern-2025)
6. [Pattern Comparison Matrix](#pattern-matrix)

***

## Fundamental Patterns

### 1. Container/Presentational

**Separate logic from UI rendering**

```jsx
// Container (Smart - data/logic)
const UserContainer = () => {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);
  
  useEffect(() => {
    userService.getUsers().then(({ data }) => {
      setUsers(data);
      setLoading(false);
    });
  }, []);
  
  return <UserList users={users} loading={loading} />;
};

// Presentational (Dumb - pure UI)
const UserList = ({ users, loading }) => (
  <div className="user-list">
    {loading ? (
      <Spinner />
    ) : (
      users.map(user => <UserCard key={user.id} user={user} />)
    )}
  </div>
);
```


### 2. Higher-Order Components (HOC)

**Reusable logic wrapper**

```jsx
const withAuth = (WrappedComponent) => {
  return function AuthenticatedComponent(props) {
    const { user, loading } = useAuth();
    
    if (loading) return <LoadingSpinner />;
    if (!user) return <Redirect to="/login" />;
    
    return <WrappedComponent {...props} user={user} />;
  };
};

// Usage
const ProtectedDashboard = withAuth(Dashboard);
```


### 3. Render Props

**Share code via render function**

```jsx
const WindowSize = ({ render }) => {
  const [size, setSize] = useState({ width: 0, height: 0 });
  
  useEffect(() => {
    const updateSize = () => setSize({ 
      width: window.innerWidth, 
      height: window.innerHeight 
    });
    window.addEventListener('resize', updateSize);
    updateSize();
    return () => window.removeEventListener('resize', updateSize);
  }, []);
  
  return render(size);
};

// Usage
<WindowSize render={({ width, height }) => (
  <div>Width: {width}px, Height: {height}px</div>
)} />
```


### 4. Compound Components

**Related components share implicit state**

```jsx
const SelectContext = createContext();

const Select = ({ children, value, onChange }) => {
  const [isOpen, setIsOpen] = useState(false);
  return (
    <SelectContext.Provider value={{ value, onChange, isOpen, setIsOpen }}>
      <div className="select-container">{children}</div>
    </SelectContext.Provider>
  );
};

const SelectTrigger = ({ children }) => {
  const { isOpen, setIsOpen } = useContext(SelectContext);
  return (
    <button onClick={() => setIsOpen(!isOpen)} className="select-trigger">
      {children}
    </button>
  );
};

const SelectOption = ({ value: optionValue, children }) => {
  const { value, onChange, setIsOpen } = useContext(SelectContext);
  return (
    <div 
      className={`option ${value === optionValue ? 'selected' : ''}`}
      onClick={() => {
        onChange(optionValue);
        setIsOpen(false);
      }}
    >
      {children}
    </div>
  );
};

// Usage
<Select value={selected} onChange={setSelected}>
  <SelectTrigger>Select...</SelectTrigger>
  <div className="options">
    <SelectOption value="react">React</SelectOption>
    <SelectOption value="vue">Vue</SelectOption>
  </div>
</Select>
```


### 5. Provider Pattern (Context API)

**Global state without prop drilling**

```jsx
const ThemeContext = createContext();

const ThemeProvider = ({ children }) => {
  const [theme, setTheme] = useState('light');
  const toggleTheme = () => setTheme(t => t === 'light' ? 'dark' : 'light');
  
  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
};

const ThemedButton = () => {
  const { theme, toggleTheme } = useContext(ThemeContext);
  return (
    <button 
      className={`btn btn-${theme}`}
      onClick={toggleTheme}
    >
      Toggle Theme
    </button>
  );
};
```


***

## State \& Logic Sharing

### 6. Custom Hooks

**Extract reusable stateful logic**

```jsx
function useLocalStorage(key, initialValue) {
  const [value, setValue] = useState(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch {
      return initialValue;
    }
  });

  const setStoredValue = useCallback((newValue) => {
    try {
      setValue(newValue);
      window.localStorage.setItem(key, JSON.stringify(newValue));
    } catch {
      console.error('localStorage error');
    }
  }, [key]);

  return [value, setStoredValue];
}

// Usage
const [cart, setCart] = useLocalStorage('cart', []);
```


### 7. Hooks Pattern (Logic Extraction)

**Replace HOCs/Render Props with hooks**

```jsx
// Custom fetch hook
function useApi(url, options = {}) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  const fetchData = useCallback(async () => {
    try {
      setLoading(true);
      const response = await fetch(url, options);
      if (!response.ok) throw new Error('API error');
      const result = await response.json();
      setData(result);
    } catch (err) {
      setError(err.message);
    } finally {
      setLoading(false);
    }
  }, [url, options]);

  useEffect(() => {
    fetchData();
  }, [fetchData]);

  return { data, loading, error, refetch: fetchData };
}

// Usage
const { data: users, loading, error } = useApi('/api/users');
```


### 8. Controlled Components

**React manages form state**

```jsx
function UserForm() {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    role: 'user'
  });

  const handleChange = (e) => {
    const { name, value, type, checked } = e.target;
    setFormData(prev => ({
      ...prev,
      [name]: type === 'checkbox' ? checked : value
    }));
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    userService.createUser(formData);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        name="name"
        value={formData.name}
        onChange={handleChange}
        placeholder="Name"
      />
      <input
        name="email"
        type="email"
        value={formData.email}
        onChange={handleChange}
        placeholder="Email"
      />
      <select name="role" value={formData.role} onChange={handleChange}>
        <option value="user">User</option>
        <option value="admin">Admin</option>
      </select>
      <button type="submit">Create User</button>
    </form>
  );
}
```


***

## Enterprise Architecture (Your App Patterns)

### 9. Factory Pattern (Axios Clients)

**Centralized API client creation**

```jsx
// api/factory.js
import axios from 'axios';

class ApiFactory {
  static create(baseURL, config = {}) {
    const client = axios.create({
      baseURL,
      timeout: 10000,
      headers: { 'Content-Type': 'application/json' },
      ...config
    });

    // Request interceptor
    client.interceptors.request.use(
      (config) => {
        const token = localStorage.getItem('authToken');
        if (token) {
          config.headers.Authorization = `Bearer ${token}`;
        }
        return config;
      },
      (error) => Promise.reject(error)
    );

    // Response interceptor
    client.interceptors.response.use(
      (response) => response,
      (error) => {
        if (error.response?.status === 401) {
          // Handle unauthorized
          window.location.href = '/login';
        }
        return Promise.reject(error);
      }
    );

    return client;
  }
}

// API Clients
export const userApi = ApiFactory.create(`${process.env.REACT_APP_API_URL}/users`);
export const productApi = ApiFactory.create(`${process.env.REACT_APP_API_URL}/products`);
export const authApi = ApiFactory.create(`${process.env.REACT_APP_API_URL}/auth`);
```


### 10. Service Layer Pattern

**Business logic abstraction**

```jsx
// services/userService.js
import { userApi } from '@/api/factory';

class UserService {
  constructor(api) {
    this.api = api;
  }

  async getAll(params = {}) {
    const response = await this.api.get('/users', { params });
    return response.data;
  }

  async getById(id) {
    const response = await this.api.get(`/users/${id}`);
    return response.data;
  }

  async create(userData) {
    const response = await this.api.post('/users', userData);
    return response.data;
  }

  async update(id, userData) {
    const response = await this.api.put(`/users/${id}`, userData);
    return response.data;
  }

  async delete(id) {
    const response = await this.api.delete(`/users/${id}`);
    return response.data;
  }

  async search(query) {
    const response = await this.api.get('/users/search', { params: { q: query } });
    return response.data;
  }
}

export const userService = new UserService(userApi);
```


### 11. Feature-based Modules

**Domain-driven organization**

```
src/
├── features/
│   ├── auth/
│   │   ├── components/ LoginForm.jsx RegisterForm.jsx
│   │   ├── services/ authService.js
│   │   ├── hooks/ useAuth.js useLogin.js
│   │   ├── types/ auth.types.ts
│   │   ├── store/ authSlice.js
│   │   └── index.js
│   ├── users/
│   │   ├── components/ UserList.jsx UserProfile.jsx UserForm.jsx
│   │   ├── services/ userService.js
│   │   ├── hooks/ useUsers.js useUser.js
│   │   ├── store/ userSlice.js
│   │   └── index.js
│   └── dashboard/
│       ├── components/ Dashboard.jsx MetricsCard.jsx
│       ├── services/ dashboardService.js
│       ├── hooks/ useDashboard.js
│       └── index.js
```

```jsx
// features/users/index.js - Feature barrel
export { default as UserList } from './components/UserList';
export { default as UserProfile } from './components/UserProfile';
export { default as UserForm } from './components/UserForm';
export { userService } from './services/userService';
export { useUsers, useUser } from './hooks/useUsers';
export { userReducer } from './store/userSlice';

// App Usage
import { UserList, useUsers, userService } from '@/features/users';
```


### 12. Repository Pattern

**Unified data access layer**

```jsx
// repositories/userRepository.js
import { userService } from '@/features/users/services/userService';

class UserRepository {
  constructor(service) {
    this.service = service;
  }

  async getAll(params = {}) {
    const users = await this.service.search(params.query || '');
    return this.transformCollection(users);
  }

  async getById(id) {
    const user = await this.service.getById(id);
    return this.transform(user);
  }

  transform(user) {
    return {
      id: user.id,
      name: user.fullName,
      email: user.email,
      role: user.role,
      avatar: user.profileImageUrl
    };
  }

  transformCollection(users) {
    return users.map(this.transform.bind(this));
  }
}

export const userRepository = new UserRepository(userService);
```


***

## Performance Patterns

### 13. Lazy Loading + Suspense

**Code splitting**

```jsx
const LazyUserProfile = lazy(() => import('@/features/users/UserProfile'));
const LazyDashboard = lazy(() => import('@/features/dashboard'));

function AppRoutes() {
  return (
    <Suspense fallback={<FullPageLoader />}>
      <Routes>
        <Route path="/dashboard" element={<LazyDashboard />} />
        <Route path="/profile/:id" element={<LazyUserProfile />} />
      </Routes>
    </Suspense>
  );
}
```


### 14. Error Boundaries

**Catch component tree errors**

```jsx
class FeatureErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false, error: null };
  }

  static getDerivedStateFromError(error) {
    return { hasError: true, error };
  }

  componentDidCatch(error, errorInfo) {
    console.error('Feature error:', error, errorInfo);
    // Send to monitoring service
  }

  render() {
    if (this.state.hasError) {
      return (
        <div className="error-boundary">
          <h2>Something went wrong in this feature</h2>
          <button onClick={() => this.setState({ hasError: false, error: null })}>
            Try again
          </button>
        </div>
      );
    }

    return this.props.children;
  }
}

// Usage
<FeatureErrorBoundary>
  <UserList />
</FeatureErrorBoundary>
```


### 15. ForwardRef + useImperativeHandle

**Expose parent methods**

```jsx
const VideoPlayer = forwardRef((props, ref) => {
  const videoRef = useRef(null);

  useImperativeHandle(ref, () => ({
    play: () => videoRef.current?.play(),
    pause: () => videoRef.current?.pause(),
    seek: (time) => videoRef.current.currentTime = time
  }), []);

  return <video ref={videoRef} {...props} />;
});

// Parent usage
const playerRef = useRef();
<VideoPlayer ref={playerRef} />
// playerRef.current.play();
```


***

## Layout \& UI Patterns

### 16. Atomic Design

**Component hierarchy**

```jsx
// Atoms
const Button = ({ children, variant = 'primary', ...props }) => (
  <button className={`btn btn-${variant}`} {...props}>
    {children}
  </button>
);

// Molecules
const SearchBox = ({ onSearch }) => {
  const [query, setQuery] = useState('');
  return (
    <div className="search-box">
      <input 
        value={query} 
        onChange={e => setQuery(e.target.value)}
        placeholder="Search..."
      />
      <Button onClick={() => onSearch(query)}>Search</Button>
    </div>
  );
};

// Organisms
const Header = () => (
  <header className="header">
    <Logo />
    <Navigation />
    <SearchBox onSearch={handleSearch} />
    <UserMenu />
  </header>
);
```


***

## Modern 2025 Patterns

### 17. Server Components (React 19)

**Zero-client JS data fetching**

```jsx
// app/users/page.js (Server Component)
async function UsersPage() {
  const users = await userService.getAll(); // Direct DB/API
  
  return (
    <div>
      <h1>Users</h1>
      <UserList users={users} /> {/* Client Component */}
    </div>
  );
}
```


### 18. Prop Getters/Combination

**Dynamic child props**

```jsx
const Table = ({ children, selectedRowId }) => {
  return React.Children.map(children, child => 
    React.cloneElement(child, { 
      isSelected: child.props.rowId === selectedRowId 
    })
  );
};

const TableRow = ({ rowId, children, isSelected }) => (
  <tr className={isSelected ? 'selected' : ''}>
    {children}
  </tr>
);

// Usage
<Table selectedRowId="1">
  <TableRow rowId="1">Row 1</TableRow>
  <TableRow rowId="2">Row 2</TableRow>
</Table>
```


***

## Pattern Comparison Matrix

| Pattern | Use Case | Complexity | Team Size | Bundle Impact | Example |
| :-- | :-- | :-- | :-- | :-- | :-- |
| **Custom Hooks** | Logic reuse | 🟢 Low | Solo-Small | None | `useLocalStorage` |
| **Compound** | UI groups | 🟡 Medium | Small-Medium | None | Tabs, Select |
| **Feature Modules** | Domain org | 🟡 Medium | Large | None | `features/users/` |
| **Factory + Service** | API layer | 🟡 Medium | Medium | Minimal | Axios clients |
| **HOC** | Cross-cutting | 🟠 High | Small | Minimal | `withAuth` |
| **Module Federation** | Micro-FEs | 🔴 High | Large | Shared | Team apps |
| **Server Components** | Data fetch | 🟢 Low | Medium | Reduced | Next.js 15 |

## Implementation Priority for Your App

```
1. Feature Modules (Structure) ✅
2. Factory + Service Layer (API) ✅  
3. Custom Hooks (Logic) ✅
4. Compound Components (UI)
5. Repository (Data)
6. Server Components (Next.js)
```

** SSDE Interview Ready**: Demonstrate your exact architecture - Factory → Service → Feature Module → Custom Hook → Compound Component flow.

