# Chapter 9: Interacting with RESTful APIs

This chapter demonstrates different approaches to fetching and managing data from REST APIs in React applications.

## What You'll Learn

- Fetching data with useEffect
- Error handling and loading states
- Using React Query for data management
- Integrating with React Router loaders
- Caching and background updates
- Mutations and optimistic updates

## Examples Overview

Each example shows a different approach to working with REST APIs, progressing from basic to advanced techniques.

---

## Example 1: useEffect Fetch

**Location:** `useEffect-fetch/`

Basic data fetching using useEffect and fetch API.

### Running the Example

```bash
cd useEffect-fetch
npm install
npm start
```

### Key Concepts

```typescript
import { useState, useEffect } from 'react';

interface Post {
  id: number;
  title: string;
  body: string;
}

function Posts() {
  const [posts, setPosts] = useState<Post[]>([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);
  
  useEffect(() => {
    async function fetchPosts() {
      try {
        setLoading(true);
        const response = await fetch('https://jsonplaceholder.typicode.com/posts');
        
        if (!response.ok) {
          throw new Error('Failed to fetch');
        }
        
        const data = await response.json();
        setPosts(data);
      } catch (err) {
        setError(err instanceof Error ? err.message : 'An error occurred');
      } finally {
        setLoading(false);
      }
    }
    
    fetchPosts();
  }, []);
  
  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  
  return (
    <ul>
      {posts.map(post => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
}
```

### Problems with this Approach

❌ Manual loading and error state management  
❌ No caching  
❌ Refetching requires reloading the page  
❌ Race conditions possible  
❌ No background updates  

### When to Use

✅ Very simple, one-time fetch  
✅ Learning React  
✅ No dependencies allowed  

---

## Example 2: React Query

**Location:** `react-query/`

Using React Query (TanStack Query) for powerful data management.

### Running the Example

```bash
cd react-query
npm install
npm start
```

### Key Concepts

**1. Install React Query:**
```bash
npm install @tanstack/react-query
```

**2. Setup Query Client:**
```typescript
// App.tsx
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import { ReactQueryDevtools } from '@tanstack/react-query-devtools';

const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      staleTime: 1000 * 60 * 5, // 5 minutes
      cacheTime: 1000 * 60 * 10, // 10 minutes
    }
  }
});

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <Posts />
      <ReactQueryDevtools initialIsOpen={false} />
    </QueryClientProvider>
  );
}
```

**3. Fetch Data with useQuery:**
```typescript
import { useQuery } from '@tanstack/react-query';

async function fetchPosts(): Promise<Post[]> {
  const response = await fetch('https://api.example.com/posts');
  if (!response.ok) throw new Error('Failed to fetch');
  return response.json();
}

function Posts() {
  const { data, isLoading, error, refetch } = useQuery({
    queryKey: ['posts'],
    queryFn: fetchPosts
  });
  
  if (isLoading) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;
  
  return (
    <div>
      <button onClick={() => refetch()}>Refresh</button>
      <ul>
        {data?.map(post => (
          <li key={post.id}>{post.title}</li>
        ))}
      </ul>
    </div>
  );
}
```

**4. Query with Parameters:**
```typescript
function Post({ id }: { id: number }) {
  const { data: post } = useQuery({
    queryKey: ['posts', id],
    queryFn: () => fetchPost(id)
  });
  
  return <div>{post?.title}</div>;
}
```

**5. Mutations:**
```typescript
import { useMutation, useQueryClient } from '@tanstack/react-query';

async function createPost(newPost: NewPost): Promise<Post> {
  const response = await fetch('https://api.example.com/posts', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(newPost)
  });
  return response.json();
}

function CreatePost() {
  const queryClient = useQueryClient();
  
  const mutation = useMutation({
    mutationFn: createPost,
    onSuccess: () => {
      // Invalidate and refetch
      queryClient.invalidateQueries({ queryKey: ['posts'] });
    }
  });
  
  const handleSubmit = (data: NewPost) => {
    mutation.mutate(data);
  };
  
  return (
    <form onSubmit={handleSubmit}>
      {/* form fields */}
      <button type="submit" disabled={mutation.isPending}>
        {mutation.isPending ? 'Creating...' : 'Create Post'}
      </button>
      {mutation.isError && <p>Error: {mutation.error.message}</p>}
    </form>
  );
}
```

**6. Optimistic Updates:**
```typescript
const mutation = useMutation({
  mutationFn: updatePost,
  onMutate: async (newPost) => {
    // Cancel outgoing refetches
    await queryClient.cancelQueries({ queryKey: ['posts', newPost.id] });
    
    // Snapshot previous value
    const previousPost = queryClient.getQueryData(['posts', newPost.id]);
    
    // Optimistically update
    queryClient.setQueryData(['posts', newPost.id], newPost);
    
    return { previousPost };
  },
  onError: (err, newPost, context) => {
    // Rollback on error
    queryClient.setQueryData(['posts', newPost.id], context?.previousPost);
  },
  onSettled: (data, error, variables) => {
    // Always refetch after error or success
    queryClient.invalidateQueries({ queryKey: ['posts', variables.id] });
  }
});
```

### React Query Features

✅ Automatic caching  
✅ Background refetching  
✅ Stale-while-revalidate  
✅ Pagination support  
✅ Infinite scroll  
✅ Request deduplication  
✅ Retry logic  
✅ DevTools  

---

## Example 3: React Router

**Location:** `react-router/`

Fetching data using React Router loaders.

### Running the Example

```bash
cd react-router
npm install
npm start
```

### Key Concepts

```typescript
import { createBrowserRouter, useLoaderData } from 'react-router-dom';

// Loader function
async function postsLoader() {
  const response = await fetch('https://api.example.com/posts');
  if (!response.ok) throw new Error('Failed to fetch');
  return response.json();
}

// Route configuration
const router = createBrowserRouter([
  {
    path: '/posts',
    element: <PostsPage />,
    loader: postsLoader,
    errorElement: <ErrorPage />
  },
  {
    path: '/posts/:id',
    element: <PostPage />,
    loader: async ({ params }) => {
      const response = await fetch(`https://api.example.com/posts/${params.id}`);
      return response.json();
    }
  }
]);

// Component
function PostsPage() {
  const posts = useLoaderData() as Post[];
  
  return (
    <ul>
      {posts.map(post => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
}
```

### Advantages

✅ Data loads before component renders  
✅ No loading states in component  
✅ Integrates with routing  
✅ Progressive enhancement  

### Disadvantages

❌ No built-in caching  
❌ Refetch on every navigation  
❌ Limited to route components  

---

## Example 4: React Query + React Router

**Location:** `react-query-and-react-router/`

Combining React Query with React Router for the best of both worlds.

### Running the Example

```bash
cd react-query-and-react-router
npm install
npm start
```

### Key Concepts

```typescript
import { createBrowserRouter } from 'react-router-dom';
import { QueryClient } from '@tanstack/react-query';

const queryClient = new QueryClient();

// Query function
const postsQuery = {
  queryKey: ['posts'],
  queryFn: async () => {
    const response = await fetch('https://api.example.com/posts');
    return response.json();
  }
};

// Loader uses query client
const router = createBrowserRouter([
  {
    path: '/posts',
    element: <PostsPage />,
    loader: async () => {
      return await queryClient.ensureQueryData(postsQuery);
    }
  }
]);

// Component uses useQuery
function PostsPage() {
  const { data: posts } = useQuery(postsQuery);
  
  return (
    <ul>
      {posts?.map(post => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
}
```

This approach provides:
✅ Fast initial load (loader)  
✅ Caching (React Query)  
✅ Background updates (React Query)  
✅ Best user experience  

---

## Common Patterns

### Axios Instead of Fetch

```bash
npm install axios
```

```typescript
import axios from 'axios';

const api = axios.create({
  baseURL: 'https://api.example.com',
  headers: {
    'Content-Type': 'application/json'
  }
});

// Interceptors for auth
api.interceptors.request.use((config) => {
  const token = localStorage.getItem('token');
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});

// Usage
async function fetchPosts() {
  const { data } = await api.get<Post[]>('/posts');
  return data;
}
```

### Error Boundary

```typescript
import { Component, ReactNode } from 'react';

interface Props {
  children: ReactNode;
}

interface State {
  hasError: boolean;
  error?: Error;
}

class ErrorBoundary extends Component<Props, State> {
  state: State = { hasError: false };
  
  static getDerivedStateFromError(error: Error) {
    return { hasError: true, error };
  }
  
  render() {
    if (this.state.hasError) {
      return (
        <div>
          <h1>Something went wrong</h1>
          <p>{this.state.error?.message}</p>
        </div>
      );
    }
    
    return this.props.children;
  }
}
```

### Pagination

```typescript
function Posts() {
  const [page, setPage] = useState(1);
  
  const { data, isLoading } = useQuery({
    queryKey: ['posts', page],
    queryFn: () => fetchPosts(page)
  });
  
  return (
    <div>
      <ul>{/* render posts */}</ul>
      <button onClick={() => setPage(p => p - 1)} disabled={page === 1}>
        Previous
      </button>
      <button onClick={() => setPage(p => p + 1)}>
        Next
      </button>
    </div>
  );
}
```

### Infinite Scroll

```typescript
import { useInfiniteQuery } from '@tanstack/react-query';

function Posts() {
  const {
    data,
    fetchNextPage,
    hasNextPage,
    isFetchingNextPage
  } = useInfiniteQuery({
    queryKey: ['posts'],
    queryFn: ({ pageParam = 1 }) => fetchPosts(pageParam),
    getNextPageParam: (lastPage, pages) => lastPage.nextPage
  });
  
  return (
    <div>
      {data?.pages.map((page) => (
        page.posts.map((post) => (
          <div key={post.id}>{post.title}</div>
        ))
      ))}
      
      {hasNextPage && (
        <button onClick={() => fetchNextPage()} disabled={isFetchingNextPage}>
          {isFetchingNextPage ? 'Loading...' : 'Load More'}
        </button>
      )}
    </div>
  );
}
```

---

## Key Takeaways

✅ useEffect fetch is basic but limited  
✅ React Query provides excellent data management  
✅ React Router loaders enable fast initial loads  
✅ Combining both gives best results  
✅ Always handle loading and error states  
✅ TypeScript ensures type safety for API data  

---

## Next Steps

- **Chapter 10**: Work with GraphQL APIs
- **Chapter 11**: Create reusable data-fetching hooks
- **Chapter 12**: Test components that fetch data

---

## Resources

- **React Query Docs**: https://tanstack.com/query/latest
- **React Router Loaders**: https://reactrouter.com/en/main/route/loader
- **Axios**: https://axios-http.com/
- **Fetch API**: https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API
