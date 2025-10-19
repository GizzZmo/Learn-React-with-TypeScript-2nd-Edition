# Chapter 10: Interacting with GraphQL APIs

This chapter explores working with GraphQL APIs in React, including querying data, mutations, and using popular GraphQL clients.

## What You'll Learn

- GraphQL fundamentals
- Writing queries and mutations
- Using Apollo Client
- Using React Query with GraphQL
- Type-safe GraphQL with TypeScript
- Caching strategies

## Examples Overview

---

## Understanding GraphQL Syntax

**Location:** `Understanding-GraphQL-Syntax/`

Learn GraphQL query language fundamentals before diving into React implementations.

### What is GraphQL?

GraphQL is a query language for APIs that allows clients to request exactly the data they need.

### Key Concepts

**Query - Fetch Data:**
```graphql
query GetPosts {
  posts {
    id
    title
    author {
      name
      email
    }
  }
}
```

**Query with Variables:**
```graphql
query GetPost($id: ID!) {
  post(id: $id) {
    id
    title
    body
    comments {
      id
      text
      author {
        name
      }
    }
  }
}
```

**Mutation - Modify Data:**
```graphql
mutation CreatePost($input: PostInput!) {
  createPost(input: $input) {
    id
    title
    author {
      name
    }
  }
}
```

**Fragments - Reuse Fields:**
```graphql
fragment PostFields on Post {
  id
  title
  body
  createdAt
}

query GetPosts {
  posts {
    ...PostFields
  }
}
```

### Advantages over REST

✅ Request exactly what you need  
✅ Single endpoint  
✅ Strongly typed  
✅ No over-fetching or under-fetching  
✅ Excellent tooling  

---

## Example 1: Using Apollo Client

**Location:** `Using-Apollo-Client/`

Apollo Client is the most popular GraphQL client for React.

### Running the Example

```bash
cd Using-Apollo-Client
npm install
npm start
```

### Key Concepts

**1. Install Apollo Client:**
```bash
npm install @apollo/client graphql
```

**2. Setup Apollo Client:**
```typescript
// App.tsx
import { ApolloClient, InMemoryCache, ApolloProvider } from '@apollo/client';

const client = new ApolloClient({
  uri: 'https://api.example.com/graphql',
  cache: new InMemoryCache(),
  headers: {
    authorization: `Bearer ${token}`
  }
});

function App() {
  return (
    <ApolloProvider client={client}>
      <Posts />
    </ApolloProvider>
  );
}
```

**3. Query Data:**
```typescript
import { useQuery, gql } from '@apollo/client';

const GET_POSTS = gql`
  query GetPosts {
    posts {
      id
      title
      author {
        name
      }
    }
  }
`;

interface Post {
  id: string;
  title: string;
  author: {
    name: string;
  };
}

interface GetPostsData {
  posts: Post[];
}

function Posts() {
  const { loading, error, data } = useQuery<GetPostsData>(GET_POSTS);
  
  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error.message}</p>;
  
  return (
    <ul>
      {data?.posts.map(post => (
        <li key={post.id}>
          {post.title} by {post.author.name}
        </li>
      ))}
    </ul>
  );
}
```

**4. Query with Variables:**
```typescript
const GET_POST = gql`
  query GetPost($id: ID!) {
    post(id: $id) {
      id
      title
      body
      author {
        name
        email
      }
    }
  }
`;

function Post({ id }: { id: string }) {
  const { loading, error, data } = useQuery(GET_POST, {
    variables: { id }
  });
  
  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error.message}</p>;
  
  return (
    <article>
      <h1>{data.post.title}</h1>
      <p>{data.post.body}</p>
      <p>By {data.post.author.name}</p>
    </article>
  );
}
```

**5. Mutations:**
```typescript
import { useMutation, gql } from '@apollo/client';

const CREATE_POST = gql`
  mutation CreatePost($input: PostInput!) {
    createPost(input: $input) {
      id
      title
      author {
        name
      }
    }
  }
`;

interface CreatePostInput {
  title: string;
  body: string;
}

function CreatePost() {
  const [createPost, { loading, error }] = useMutation(CREATE_POST);
  
  const handleSubmit = async (input: CreatePostInput) => {
    try {
      const { data } = await createPost({
        variables: { input },
        // Update cache after mutation
        refetchQueries: [{ query: GET_POSTS }]
      });
      console.log('Created:', data.createPost);
    } catch (err) {
      console.error('Error:', err);
    }
  };
  
  return (
    <form onSubmit={handleSubmit}>
      {/* form fields */}
      <button type="submit" disabled={loading}>
        {loading ? 'Creating...' : 'Create Post'}
      </button>
      {error && <p>Error: {error.message}</p>}
    </form>
  );
}
```

**6. Optimistic UI:**
```typescript
const [deletePost] = useMutation(DELETE_POST, {
  optimisticResponse: {
    deletePost: {
      __typename: 'Post',
      id: postId
    }
  },
  update: (cache, { data }) => {
    // Update cache immediately
    const existing = cache.readQuery({ query: GET_POSTS });
    cache.writeQuery({
      query: GET_POSTS,
      data: {
        posts: existing.posts.filter(p => p.id !== postId)
      }
    });
  }
});
```

**7. Polling:**
```typescript
const { data } = useQuery(GET_POSTS, {
  pollInterval: 5000  // Refetch every 5 seconds
});
```

**8. Lazy Query:**
```typescript
const [searchPosts, { loading, data }] = useLazyQuery(SEARCH_POSTS);

const handleSearch = (term: string) => {
  searchPosts({ variables: { term } });
};
```

### Apollo Client Features

✅ Intelligent caching  
✅ Automatic updates  
✅ Optimistic UI  
✅ Pagination  
✅ Error handling  
✅ DevTools  
✅ TypeScript support  

---

## Example 2: Using React Query

**Location:** `Using-React-Query/`

Using React Query (TanStack Query) with GraphQL as an alternative to Apollo.

### Running the Example

```bash
cd Using-React-Query
npm install
npm start
```

### Key Concepts

**1. Install Dependencies:**
```bash
npm install @tanstack/react-query graphql-request
```

**2. Setup GraphQL Client:**
```typescript
import { GraphQLClient } from 'graphql-request';

const client = new GraphQLClient('https://api.example.com/graphql', {
  headers: {
    authorization: `Bearer ${token}`
  }
});
```

**3. Query with React Query:**
```typescript
import { useQuery } from '@tanstack/react-query';
import { gql } from 'graphql-request';

const GET_POSTS = gql`
  query GetPosts {
    posts {
      id
      title
      author {
        name
      }
    }
  }
`;

function Posts() {
  const { data, isLoading, error } = useQuery({
    queryKey: ['posts'],
    queryFn: async () => client.request(GET_POSTS)
  });
  
  if (isLoading) return <p>Loading...</p>;
  if (error) return <p>Error: {error.message}</p>;
  
  return (
    <ul>
      {data?.posts.map(post => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
}
```

**4. Query with Variables:**
```typescript
const GET_POST = gql`
  query GetPost($id: ID!) {
    post(id: $id) {
      id
      title
      body
    }
  }
`;

function Post({ id }: { id: string }) {
  const { data } = useQuery({
    queryKey: ['post', id],
    queryFn: async () => client.request(GET_POST, { id })
  });
  
  return <article>{data?.post.title}</article>;
}
```

**5. Mutations:**
```typescript
import { useMutation, useQueryClient } from '@tanstack/react-query';

const CREATE_POST = gql`
  mutation CreatePost($input: PostInput!) {
    createPost(input: $input) {
      id
      title
    }
  }
`;

function CreatePost() {
  const queryClient = useQueryClient();
  
  const mutation = useMutation({
    mutationFn: (input: PostInput) => 
      client.request(CREATE_POST, { input }),
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['posts'] });
    }
  });
  
  return (
    <button onClick={() => mutation.mutate({ title: 'New Post', body: '...' })}>
      Create Post
    </button>
  );
}
```

### React Query + GraphQL Advantages

✅ Familiar React Query API  
✅ Lightweight (no Apollo overhead)  
✅ Easy to integrate  
✅ Flexible  

### Apollo vs React Query

| Feature | Apollo Client | React Query |
|---------|--------------|-------------|
| GraphQL-specific | Yes | No (general purpose) |
| Bundle Size | Larger | Smaller |
| Caching | GraphQL-aware | Generic |
| Learning Curve | Steeper | Gentler |
| DevTools | Excellent | Excellent |
| TypeScript | Good | Excellent |

---

## TypeScript with GraphQL

### Code Generation

Use GraphQL Code Generator for type-safe GraphQL:

```bash
npm install --save-dev @graphql-codegen/cli @graphql-codegen/typescript @graphql-codegen/typescript-operations @graphql-codegen/typescript-react-apollo
```

**codegen.yml:**
```yaml
schema: https://api.example.com/graphql
documents: './src/**/*.tsx'
generates:
  ./src/generated/graphql.ts:
    plugins:
      - typescript
      - typescript-operations
      - typescript-react-apollo
```

**Run:**
```bash
npx graphql-codegen
```

**Generated Hooks:**
```typescript
import { useGetPostsQuery, useCreatePostMutation } from './generated/graphql';

function Posts() {
  const { data, loading } = useGetPostsQuery();
  // Fully typed!
}
```

---

## Key Takeaways

✅ GraphQL allows precise data fetching  
✅ Apollo Client is feature-rich and popular  
✅ React Query offers a simpler alternative  
✅ Code generation provides type safety  
✅ Both clients support caching and mutations  
✅ Choose based on your needs and preferences  

---

## Next Steps

- **Chapter 11**: Create reusable GraphQL hooks
- **Chapter 12**: Test components with GraphQL

---

## Resources

- **GraphQL Docs**: https://graphql.org/
- **Apollo Client**: https://www.apollographql.com/docs/react/
- **GraphQL Request**: https://github.com/jasonkuhrt/graphql-request
- **GraphQL Code Generator**: https://www.graphql-code-generator.com/
- **GraphQL Playground**: https://github.com/graphql/graphql-playground
