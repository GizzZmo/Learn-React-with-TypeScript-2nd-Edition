# Chapter 7: Working with Forms

This chapter explores different approaches to handling forms in React, from basic controlled components to advanced form libraries.

## What You'll Learn

- Controlled vs uncontrolled components
- Form validation strategies
- Using native HTML5 validation
- React Hook Form library
- Form integration with React Router
- Best practices for form handling

## Examples Overview

Each example demonstrates a different approach to form handling. All are complete React applications.

---

## Example 1: Uncontrolled Fields

**Location:** `uncontrolled-fields/`

Forms where form data is handled by the DOM itself, accessed via refs.

### Running the Example

```bash
cd uncontrolled-fields
npm install
npm start
```

### Key Concepts

```typescript
import { useRef, FormEvent } from 'react';

function ContactForm() {
  const nameRef = useRef<HTMLInputElement>(null);
  const emailRef = useRef<HTMLInputElement>(null);
  
  const handleSubmit = (e: FormEvent) => {
    e.preventDefault();
    console.log({
      name: nameRef.current?.value,
      email: emailRef.current?.value
    });
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input ref={nameRef} name="name" />
      <input ref={emailRef} name="email" type="email" />
      <button type="submit">Submit</button>
    </form>
  );
}
```

**Pros:** Simple, less code, good for simple forms  
**Cons:** Less control, harder to validate, not React-idiomatic

---

## Example 2: Controlled Fields

**Location:** `controlled-fields/`

Forms where React state controls all input values.

### Running the Example

```bash
cd controlled-fields
npm install
npm start
```

### Key Concepts

```typescript
import { useState, FormEvent, ChangeEvent } from 'react';

function ContactForm() {
  const [name, setName] = useState('');
  const [email, setEmail] = useState('');
  
  const handleSubmit = (e: FormEvent) => {
    e.preventDefault();
    console.log({ name, email });
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input 
        value={name}
        onChange={(e: ChangeEvent<HTMLInputElement>) => setName(e.target.value)}
      />
      <input 
        value={email}
        onChange={(e: ChangeEvent<HTMLInputElement>) => setEmail(e.target.value)}
        type="email"
      />
      <button type="submit">Submit</button>
    </form>
  );
}
```

**Pros:** Full control, easy validation, React-idiomatic  
**Cons:** More code, state updates on every keystroke

---

## Example 3: Native Validation

**Location:** `native-validation/`

Using HTML5's built-in form validation.

### Running the Example

```bash
cd native-validation
npm install
npm start
```

### Key Concepts

```typescript
function ContactForm() {
  return (
    <form>
      <input 
        name="name"
        required
        minLength={3}
        maxLength={50}
      />
      <input 
        name="email"
        type="email"
        required
        pattern="[a-z0-9._%+-]+@[a-z0-9.-]+\.[a-z]{2,}$"
      />
      <input 
        name="age"
        type="number"
        min={18}
        max={100}
      />
      <button type="submit">Submit</button>
    </form>
  );
}
```

**HTML5 Validation Attributes:**
- `required` - Field must be filled
- `minLength/maxLength` - String length constraints
- `min/max` - Number range constraints
- `pattern` - Regex validation
- `type` - Email, URL, number, etc.

**Pros:** No JavaScript needed, browser-native, accessible  
**Cons:** Limited customization, inconsistent styling across browsers

---

## Example 4: React Hook Form

**Location:** `react-hook-form/`

Using the popular React Hook Form library for performant, flexible forms.

### Running the Example

```bash
cd react-hook-form
npm install
npm start
```

### Key Concepts

```typescript
import { useForm } from 'react-hook-form';

interface FormData {
  name: string;
  email: string;
  age: number;
}

function ContactForm() {
  const { 
    register, 
    handleSubmit, 
    formState: { errors } 
  } = useForm<FormData>();
  
  const onSubmit = (data: FormData) => {
    console.log(data);
  };
  
  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input 
        {...register('name', { 
          required: 'Name is required',
          minLength: { value: 3, message: 'Min 3 characters' }
        })}
      />
      {errors.name && <span>{errors.name.message}</span>}
      
      <input 
        {...register('email', {
          required: 'Email is required',
          pattern: {
            value: /^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,}$/i,
            message: 'Invalid email'
          }
        })}
        type="email"
      />
      {errors.email && <span>{errors.email.message}</span>}
      
      <button type="submit">Submit</button>
    </form>
  );
}
```

**Advanced Features:**

**Default Values:**
```typescript
const { register } = useForm({
  defaultValues: {
    name: 'John Doe',
    email: 'john@example.com'
  }
});
```

**Watch Values:**
```typescript
const { watch } = useForm();
const watchName = watch('name');
// Re-renders on name change
```

**Custom Validation:**
```typescript
register('password', {
  validate: (value) => 
    value.length >= 8 || 'Password must be 8+ characters'
});
```

**Pros:** Performant, flexible, great TypeScript support, easy validation  
**Cons:** Additional dependency, learning curve

---

## Example 5: React Router Form

**Location:** `react-router-form/`

Forms integrated with React Router's data APIs.

### Running the Example

```bash
cd react-router-form
npm install
npm start
```

### Key Concepts

```typescript
import { Form, useActionData } from 'react-router-dom';

// Route configuration
{
  path: '/contact',
  element: <ContactPage />,
  action: async ({ request }) => {
    const formData = await request.formData();
    const data = {
      name: formData.get('name'),
      email: formData.get('email')
    };
    
    // Process form
    await submitContact(data);
    return { success: true };
  }
}

// Component
function ContactPage() {
  const actionData = useActionData();
  
  return (
    <Form method="post">
      <input name="name" required />
      <input name="email" type="email" required />
      <button type="submit">Submit</button>
      
      {actionData?.success && <p>Form submitted!</p>}
    </Form>
  );
}
```

**Pros:** Integrates with routing, progressive enhancement, handles navigation  
**Cons:** Tied to React Router, requires route configuration

---

## Comparison of Approaches

| Feature | Uncontrolled | Controlled | Native | React Hook Form | Router Form |
|---------|-------------|-----------|--------|-----------------|-------------|
| Setup | Simple | Medium | Simple | Medium | Medium |
| Validation | Manual | Manual | Built-in | Easy | Easy |
| Performance | Best | Good | Best | Best | Good |
| TypeScript | Good | Great | Good | Excellent | Good |
| Learning Curve | Easy | Easy | Easy | Medium | Medium |
| Best For | Simple forms | React apps | Quick forms | Complex forms | Router apps |

---

## Form Validation Patterns

### Manual Validation

```typescript
const [errors, setErrors] = useState<Record<string, string>>({});

const validate = (data: FormData) => {
  const newErrors: Record<string, string> = {};
  
  if (!data.name) newErrors.name = 'Name is required';
  if (!data.email) newErrors.email = 'Email is required';
  else if (!/\S+@\S+\.\S+/.test(data.email)) {
    newErrors.email = 'Email is invalid';
  }
  
  return newErrors;
};

const handleSubmit = (e: FormEvent) => {
  e.preventDefault();
  const formErrors = validate({ name, email });
  
  if (Object.keys(formErrors).length === 0) {
    // Submit form
  } else {
    setErrors(formErrors);
  }
};
```

### Async Validation

```typescript
// Check if email exists
const validateEmail = async (email: string) => {
  const response = await fetch(`/api/check-email?email=${email}`);
  const { exists } = await response.json();
  return !exists || 'Email already registered';
};

// React Hook Form
register('email', { 
  validate: {
    available: async (email) => await validateEmail(email)
  }
});
```

---

## TypeScript Types for Forms

```typescript
// Form data interface
interface ContactFormData {
  name: string;
  email: string;
  message: string;
  subscribe: boolean;
}

// Event types
import { FormEvent, ChangeEvent } from 'react';

const handleSubmit = (e: FormEvent<HTMLFormElement>) => {
  e.preventDefault();
};

const handleChange = (e: ChangeEvent<HTMLInputElement>) => {
  setValue(e.target.value);
};

// Select change event
const handleSelectChange = (e: ChangeEvent<HTMLSelectElement>) => {
  setOption(e.target.value);
};

// Textarea change event  
const handleTextareaChange = (e: ChangeEvent<HTMLTextAreaElement>) => {
  setMessage(e.target.value);
};
```

---

## Key Takeaways

✅ Multiple valid approaches to form handling  
✅ Controlled components are most common in React  
✅ React Hook Form offers best performance for complex forms  
✅ HTML5 validation works for simple cases  
✅ React Router forms integrate well with navigation  
✅ TypeScript provides excellent form type safety  

---

## Next Steps

- **Chapter 8**: Manage form data with state management
- **Chapter 9**: Submit forms to REST APIs
- **Chapter 11**: Create reusable form components

---

## Resources

- **React Hook Form**: https://react-hook-form.com/
- **React Router Forms**: https://reactrouter.com/en/main/guides/form
- **HTML5 Validation**: https://developer.mozilla.org/en-US/docs/Learn/Forms/Form_validation
- **React Forms**: https://react.dev/reference/react-dom/components/form
