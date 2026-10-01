অবশ্যই। আমি এগুলোকে **real interview live-coding style**-এ সাজিয়ে দিচ্ছি—প্রতিটায় থাকবে:

* **Interview Question**
* **What interviewer expects**
* **Concept/Description**
* **Code**
* **Expected Output**
* **How to explain in interview**
* **Possible follow-up question**

## 1. JavaScript — Reverse a String

**Question:**
Write a JavaScript function to reverse a string without using a built-in reverse function.

**Description:**
Interviewer এখানে basic JavaScript logic, loop এবং string manipulation দেখতে চায়।

```javascript
function reverseString(str) {
  let result = "";

  for (let i = str.length - 1; i >= 0; i--) {
    result += str[i];
  }

  return result;
}

console.log(reverseString("hello"));
```

**Output:**

```text
olleh
```

**Interview-এ বলবে:**

> "I start from the last character of the string and move backward. Each character is added to a new string."

**Follow-up:**

* Can you do it using `split()`, `reverse()`, and `join()`?
* What is the time complexity?

---

# 2. JavaScript — Check Palindrome

**Question:**
Check whether a string is a palindrome.

**Description:**
Palindrome means সামনে থেকে এবং পিছন থেকে একই।

```javascript
function isPalindrome(str) {
  let reversed = "";

  for (let i = str.length - 1; i >= 0; i--) {
    reversed += str[i];
  }

  return str === reversed;
}

console.log(isPalindrome("madam"));
console.log(isPalindrome("hello"));
```

**Output:**

```text
true
false
```

**Interview explanation:**

> "I reverse the string and compare it with the original string. If both are equal, it is a palindrome."

---

# 3. JavaScript — Find Largest Number

**Question:**
Find the largest number from an array without using `Math.max()`.

```javascript
function findLargest(numbers) {
  let largest = numbers[0];

  for (let i = 1; i < numbers.length; i++) {
    if (numbers[i] > largest) {
      largest = numbers[i];
    }
  }

  return largest;
}

console.log(findLargest([10, 5, 30, 20, 8]));
```

**Output:**

```text
30
```

**Concept:**
প্রথম element-কে temporarily largest ধরে নিয়ে একে একে compare করছি।

---

# 4. JavaScript — Remove Duplicate Values

**Question:**
Remove duplicate values from an array.

```javascript
function removeDuplicates(arr) {
  return [...new Set(arr)];
}

console.log(removeDuplicates([1, 2, 2, 3, 4, 4, 5]));
```

**Output:**

```text
[1, 2, 3, 4, 5]
```

**Interview explanation:**

> "JavaScript Set stores only unique values, so I convert the array into a Set and then spread it back into an array."

**Follow-up:**
"Can you do it without Set?"

```javascript
function removeDuplicates(arr) {
  let result = [];

  for (let item of arr) {
    if (!result.includes(item)) {
      result.push(item);
    }
  }

  return result;
}
```

---

# 5. JavaScript — Count Character Frequency

**Question:**
Count how many times each character appears in a string.

```javascript
function characterFrequency(str) {
  const frequency = {};

  for (let char of str) {
    if (frequency[char]) {
      frequency[char]++;
    } else {
      frequency[char] = 1;
    }
  }

  return frequency;
}

console.log(characterFrequency("hello"));
```

**Output:**

```javascript
{
  h: 1,
  e: 1,
  l: 2,
  o: 1
}
```

**Concept:**
এখানে object-কে frequency counter হিসেবে ব্যবহার করা হয়েছে।

---

# 6. JavaScript — Find Second Largest

**Question:**
Find the second largest number from an array.

```javascript
function secondLargest(arr) {
  let largest = -Infinity;
  let second = -Infinity;

  for (let num of arr) {
    if (num > largest) {
      second = largest;
      largest = num;
    } else if (num > second && num !== largest) {
      second = num;
    }
  }

  return second;
}

console.log(secondLargest([10, 50, 20, 40, 30]));
```

**Output:**

```text
40
```

---

# 7. JavaScript — Two Sum

**Question:**
Given an array and a target, find two numbers whose sum equals the target.

```javascript
function twoSum(arr, target) {
  const map = {};

  for (let i = 0; i < arr.length; i++) {
    const needed = target - arr[i];

    if (map[needed] !== undefined) {
      return [map[needed], i];
    }

    map[arr[i]] = i;
  }

  return [];
}

console.log(twoSum([2, 7, 11, 15], 9));
```

**Output:**

```text
[0, 1]
```

**Interview explanation:**

> "I store previously visited numbers in a hash map. For each number, I calculate the required complementary value and check whether it already exists."

এটা **খুব common live coding question**।

---

# 8. JavaScript — Anagram Check

**Question:**
Check whether two strings are anagrams.

Example:

```text
listen
silent
```

```javascript
function isAnagram(str1, str2) {
  const a = str1.toLowerCase().split("").sort().join("");
  const b = str2.toLowerCase().split("").sort().join("");

  return a === b;
}

console.log(isAnagram("listen", "silent"));
```

**Output:**

```text
true
```

---

# 9. JavaScript — Fibonacci

**Question:**
Generate Fibonacci numbers.

```javascript
function fibonacci(n) {
  let a = 0;
  let b = 1;

  for (let i = 0; i < n; i++) {
    console.log(a);

    let next = a + b;
    a = b;
    b = next;
  }
}

fibonacci(7);
```

**Output:**

```text
0
1
1
2
3
5
8
```

---

# 10. JavaScript — Debounce

**Question:**
Implement a debounce function.

**Description:**
Search box-এ user typing করার সময় প্রতিটি keystroke-এর জন্য API request না পাঠিয়ে কিছু সময় wait করার জন্য debounce ব্যবহার করা হয়।

```javascript
function debounce(func, delay) {
  let timer;

  return function (...args) {
    clearTimeout(timer);

    timer = setTimeout(() => {
      func(...args);
    }, delay);
  };
}

const search = debounce((value) => {
  console.log("Searching:", value);
}, 500);

search("react");
```

**Interview explanation:**

> "Debounce delays function execution until the user stops triggering the event for a specified amount of time."

---

# 11. React — Counter

**Question:**
Create a counter using React `useState`.

```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <h2>{count}</h2>

      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>

      <button onClick={() => setCount(count - 1)}>
        Decrement
      </button>
    </div>
  );
}

export default Counter;
```

**What interviewer checks:**

* `useState`
* Event handling
* State update
* JSX

**Explain:**

> "`useState` creates a state variable called count. When I call setCount, React updates the state and re-renders the component."

---

# 12. React — Todo App

**Question:**
Create a basic Todo application.

```jsx
import { useState } from "react";

function TodoApp() {
  const [task, setTask] = useState("");
  const [todos, setTodos] = useState([]);

  const addTodo = () => {
    if (!task.trim()) return;

    setTodos([...todos, task]);
    setTask("");
  };

  return (
    <div>
      <input
        value={task}
        onChange={(e) => setTask(e.target.value)}
        placeholder="Enter task"
      />

      <button onClick={addTodo}>
        Add
      </button>

      <ul>
        {todos.map((todo, index) => (
          <li key={index}>
            {todo}
          </li>
        ))}
      </ul>
    </div>
  );
}

export default TodoApp;
```

**Important interview concept:**

```jsx
key={index}
```

React list render করার সময় `key` ব্যবহার করে কোন item change/add/remove হয়েছে সেটা efficiently identify করে।

---

# 13. React — Search and Filter

**Question:**
Create a search box that filters a list of products.

```jsx
import { useState } from "react";

function Products() {
  const [search, setSearch] = useState("");

  const products = [
    "Laptop",
    "Mobile",
    "Keyboard",
    "Mouse",
    "Monitor"
  ];

  const filteredProducts = products.filter((product) =>
    product.toLowerCase().includes(search.toLowerCase())
  );

  return (
    <div>
      <input
        value={search}
        onChange={(e) => setSearch(e.target.value)}
        placeholder="Search product"
      />

      {filteredProducts.map((product) => (
        <p key={product}>{product}</p>
      ))}
    </div>
  );
}

export default Products;
```

**Interview concept:**

এখানে:

```javascript
filter()
```

ব্যবহার করে matching product বের করা হয়েছে।

---

# 14. React — API Fetch

**Question:**
Fetch data from an API and display it.

```jsx
import { useEffect, useState } from "react";

function Users() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch("https://jsonplaceholder.typicode.com/users")
      .then((res) => res.json())
      .then((data) => {
        setUsers(data);
        setLoading(false);
      });
  }, []);

  if (loading) {
    return <p>Loading...</p>;
  }

  return (
    <div>
      {users.map((user) => (
        <p key={user.id}>
          {user.name}
        </p>
      ))}
    </div>
  );
}

export default Users;
```

**Why `[]`?**

```jsx
useEffect(() => {
   // API call
}, []);
```

Empty dependency array থাকার কারণে component প্রথমবার render হওয়ার পরে effect run করবে।

---

# 15. Node.js + Express — Basic Server

**Question:**
Create a basic Express server with a GET endpoint.

```javascript
const express = require("express");

const app = express();

app.use(express.json());

app.get("/", (req, res) => {
  res.json({
    message: "Server is running"
  });
});

app.listen(5000, () => {
  console.log("Server running on port 5000");
});
```

**Interview explanation:**

> "Express is used to create the HTTP server and API routes. `express.json()` allows the server to parse JSON request bodies."

---

# 16. Express — POST API

**Question:**
Create a POST API to receive user data.

```javascript
app.post("/users", (req, res) => {
  const { name, email } = req.body;

  res.status(201).json({
    message: "User created",
    user: {
      name,
      email
    }
  });
});
```

Request:

```json
{
  "name": "Shanto",
  "email": "shanto@example.com"
}
```

Response:

```json
{
  "message": "User created",
  "user": {
    "name": "Shanto",
    "email": "shanto@example.com"
  }
}
```

---

# 17. Express — Middleware

**Question:**
Write a middleware that logs every request.

```javascript
const logger = (req, res, next) => {
  console.log(
    `${req.method} ${req.url}`
  );

  next();
};

app.use(logger);
```

**Most important point:**

```javascript
next();
```

`next()` না দিলে request পরবর্তী middleware/route-এ যাবে না।

---

# 18. Express — Authentication Middleware

**Question:**
Create middleware to check authentication.

```javascript
const authMiddleware = (req, res, next) => {
  const token = req.headers.authorization;

  if (!token) {
    return res.status(401).json({
      message: "Unauthorized"
    });
  }

  next();
};

app.get("/profile", authMiddleware, (req, res) => {
  res.json({
    message: "Welcome to profile"
  });
});
```

**Interview explanation:**

> "The middleware checks whether an authorization token exists before allowing the request to reach the protected route."

---

# 19. MongoDB + Mongoose — User Schema

**Question:**
Create a Mongoose User model.

```javascript
const mongoose = require("mongoose");

const userSchema = new mongoose.Schema(
  {
    name: {
      type: String,
      required: true
    },

    email: {
      type: String,
      required: true,
      unique: true
    },

    password: {
      type: String,
      required: true
    }
  },
  {
    timestamps: true
  }
);

const User = mongoose.model("User", userSchema);

module.exports = User;
```

**Concept:**

Schema defines:

* Field
* Data type
* Validation
* Constraints

---

# 20. MongoDB — Create User

**Question:**
Create a user using Mongoose.

```javascript
app.post("/users", async (req, res) => {
  try {
    const user = await User.create(req.body);

    res.status(201).json(user);
  } catch (error) {
    res.status(500).json({
      message: error.message
    });
  }
});
```

---

# 21. MongoDB — Get All Users

```javascript
app.get("/users", async (req, res) => {
  try {
    const users = await User.find();

    res.json(users);
  } catch (error) {
    res.status(500).json({
      message: error.message
    });
  }
});
```

---

# 22. MongoDB — Get Single User

```javascript
app.get("/users/:id", async (req, res) => {
  try {
    const user = await User.findById(req.params.id);

    if (!user) {
      return res.status(404).json({
        message: "User not found"
      });
    }

    res.json(user);
  } catch (error) {
    res.status(500).json({
      message: error.message
    });
  }
});
```

এখানে interviewer দেখতে পারে তুমি **route parameter** বুঝো কিনা:

```javascript
req.params.id
```

---

# 23. MongoDB — Update User

```javascript
app.put("/users/:id", async (req, res) => {
  try {
    const user = await User.findByIdAndUpdate(
      req.params.id,
      req.body,
      {
        new: true
      }
    );

    res.json(user);
  } catch (error) {
    res.status(500).json({
      message: error.message
    });
  }
});
```

---

# 24. MongoDB — Delete User

```javascript
app.delete("/users/:id", async (req, res) => {
  try {
    await User.findByIdAndDelete(req.params.id);

    res.json({
      message: "User deleted successfully"
    });
  } catch (error) {
    res.status(500).json({
      message: error.message
    });
  }
});
```

এই 4টা একসাথে মনে রাখো:

```text
POST    → Create
GET     → Read
PUT     → Update
DELETE  → Delete
```

---

# 25. Full Stack — Simple Login

**Question:**
Create a basic login API using Express, MongoDB and bcrypt.

```javascript
const bcrypt = require("bcrypt");

app.post("/login", async (req, res) => {
  try {
    const { email, password } = req.body;

    const user = await User.findOne({ email });

    if (!user) {
      return res.status(404).json({
        message: "User not found"
      });
    }

    const isMatch = await bcrypt.compare(
      password,
      user.password
    );

    if (!isMatch) {
      return res.status(401).json({
        message: "Invalid credentials"
      });
    }

    res.json({
      message: "Login successful"
    });

  } catch (error) {
    res.status(500).json({
      message: error.message
    });
  }
});
```

**Interview-এ explain করবে:**

> "First I find the user by email. Then I compare the plain password with the hashed password using bcrypt. If they match, authentication succeeds."

---

# 26. Full Stack — React + API

**Question:**
Fetch users from your Express backend and display them in React.

```jsx
import { useEffect, useState } from "react";

function Users() {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    fetch("http://localhost:5000/users")
      .then((res) => res.json())
      .then((data) => {
        setUsers(data);
      });
  }, []);

  return (
    <div>
      {users.map((user) => (
        <div key={user._id}>
          <h3>{user.name}</h3>
          <p>{user.email}</p>
        </div>
      ))}
    </div>
  );
}

export default Users;
```

এটা **Frontend → Backend** connection-এর basic example।

---

# 27. React — Form Handling

**Question:**
Create a registration form.

```jsx
import { useState } from "react";

function Register() {
  const [form, setForm] = useState({
    name: "",
    email: "",
    password: ""
  });

  const handleChange = (e) => {
    setForm({
      ...form,
      [e.target.name]: e.target.value
    });
  };

  const handleSubmit = (e) => {
    e.preventDefault();

    console.log(form);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        name="name"
        value={form.name}
        onChange={handleChange}
        placeholder="Name"
      />

      <input
        name="email"
        value={form.email}
        onChange={handleChange}
        placeholder="Email"
      />

      <input
        name="password"
        type="password"
        value={form.password}
        onChange={handleChange}
        placeholder="Password"
      />

      <button type="submit">
        Register
      </button>
    </form>
  );
}

export default Register;
```

এখানে খুব important concept:

```javascript
[e.target.name]: e.target.value
```

একটা handler দিয়ে multiple input manage করা হচ্ছে।

---

# ⭐ Interview-এর জন্য সবচেয়ে আগে এগুলো Practice করো

তোমার **MERN Full Stack interview** target হলে আমি priority এভাবে রাখব:

| Priority | Topic                            |
| -------- | -------------------------------- |
| 🔥🔥🔥   | Array/String JavaScript problems |
| 🔥🔥🔥   | `map`, `filter`, `reduce`        |
| 🔥🔥🔥   | React Counter                    |
| 🔥🔥🔥   | React Todo                       |
| 🔥🔥🔥   | React API Fetch                  |
| 🔥🔥🔥   | Form Handling                    |
| 🔥🔥🔥   | Express CRUD                     |
| 🔥🔥🔥   | MongoDB CRUD                     |
| 🔥🔥     | Authentication                   |
| 🔥🔥     | JWT                              |
| 🔥🔥     | Middleware                       |
| 🔥🔥     | Search + Pagination              |
| 🔥       | File Upload                      |
| 🔥       | Payment integration              |

**সবচেয়ে গুরুত্বপূর্ণ:** interview-এ শুধু code মুখস্থ করে লিখলে হবে না। Code করার সময় প্রতিটা step explain করবে—**“আমি এটা কেন করছি”**। Interviewer সাধারণত syntax-এর পাশাপাশি তোমার problem-solving approach-ও দেখতে চায়।
