---
title: 'RESTful APIs'
date: 2025-01-20
draft: false
description: 'A practical guide to building secure, scalable RESTful APIs with Node.js and MongoDB'
slug: 'api'
tags: ['Backend Development', 'Node.js', 'REST API', 'MongoDB', 'Security']
---

Let's talk about building RESTful APIs that don't fall apart in production. After spending months building and rebuilding backend services, I've learned that creating an API is easy—creating one that's secure, maintainable, and performs well under pressure is where the real challenge lies.

## What Even Is a RESTful API?

Before diving into the implementation details, let's demystify this term. An API (Application Programming Interface) is essentially a contract between different pieces of software, allowing them to communicate. REST (Representational State Transfer) is just a set of conventions that makes these APIs predictable and easy to work with.

Think of it like ordering at a restaurant. You (the client) look at a menu (API documentation), place an order using specific terms the waiter understands (HTTP methods), and receive your food in a predictable format (JSON response). The kitchen doesn't need to know who you are between visits (stateless), and the menu items are organized logically (resources).

## The REST Principles That Actually Matter

After building several APIs, I've found these five principles make the biggest difference:

### 1. Resources Are King

Instead of thinking about actions, think about resources. Don't create endpoints like:

```
POST /createNewUser
GET /getAllProjects
POST /updateUserEmail
```

Instead, organize around resources:

```
POST   /users          (create user)
GET    /projects       (get all projects)
PATCH  /users/123      (update user)
```

This mental shift from "what action am I performing" to "what resource am I manipulating" makes your API intuitive.

### 2. HTTP Methods Have Meaning

Each HTTP method has a specific purpose:

- **GET**: Reading data (never changes anything)
- **POST**: Creating new resources
- **PUT**: Replacing entire resources
- **PATCH**: Updating parts of resources
- **DELETE**: Removing resources

Here's a mistake I made early on: using POST for everything because "it works." Sure, it works, but when another developer (or future you) tries to understand the API, they'll curse your name.

### 3. Statelessness Isn't Optional

Every request must contain all information needed to understand it. The server shouldn't remember previous requests. This sounds simple until you're tempted to store user session data on the server "just this once." Don't. Use JWTs or similar tokens to maintain state on the client side.

## The MVC Pattern: Keeping Your Sanity

When I first started, my route files were a disaster—database queries mixed with business logic mixed with response formatting. Enter the MVC pattern:

```javascript
// Model (models/userModel.js)
// Handles data and business logic
const User = {
	async create(userData) {
		// Validation logic
		if (!userData.email) throw new Error('Email required');
		// Database interaction
		return await db.users.insert(userData);
	},
};

// Controller (controllers/userController.js)
// Handles requests and responses
const createUser = async (req, res) => {
	try {
		const user = await User.create(req.body);
		res.status(201).json({ status: 'success', data: user });
	} catch (error) {
		res.status(400).json({ status: 'error', message: error.message });
	}
};

// Routes (routes/userRoutes.js)
// Defines endpoints
router.post('/users', createUser);
```

**Fat models, thin controllers** became my mantra. Push business logic into models, keep controllers focused on handling HTTP concerns.

## MongoDB & Mongoose: The Dynamic Duo

Coming from SQL, MongoDB felt like freedom—no rigid schemas, nested documents, easy scaling. But with great power comes great responsibility. Here's what I learned:

### Schema Design Matters (Even in NoSQL)

Just because MongoDB is schemaless doesn't mean your data should be. Mongoose schemas saved me from myself:

```javascript
const projectSchema = new mongoose.Schema({
	name: {
		type: String,
		required: [true, 'Project must have a name'],
		trim: true,
		maxlength: [100, 'Name too long'],
	},
	status: {
		type: String,
		enum: ['active', 'completed', 'archived'],
		default: 'active',
	},
	team: [
		{
			type: mongoose.Schema.ObjectId,
			ref: 'User',
		},
	],
});
```

### The Reference vs Embed Dilemma

This drove me crazy initially. When should you embed documents vs reference them?

**Embed when:**

- Data is accessed together (user profile + preferences)
- One-to-few relationships (blog post + comments)
- Data won't grow unbounded

**Reference when:**

- Data needs independent queries (users in a project)
- Many-to-many relationships
- Large documents that change frequently

I learned this the hard way when I embedded all project tickets inside project documents. Worked great until projects had thousands of tickets and every query returned megabytes of data.

## Security: Where Things Get Serious

Security isn't optional, and "I'll add it later" is a dangerous game. Here's my security checklist that's saved me multiple times:

### Password Security: Beyond Bcrypt

While everyone talks about bcrypt, I switched to Argon2 after diving into the research. It's memory-hard, making GPU attacks impractical:

```javascript
const argon2 = require('argon2');

// Hashing
const hash = await argon2.hash(password, {
	type: argon2.argon2id,
	memoryCost: 2 ** 16,
	timeCost: 3,
	parallelism: 1,
});

// Verifying
const valid = await argon2.verify(hash, password);
```

### JWT Implementation That Actually Works

JWTs are powerful but easy to mess up. Store them in httpOnly cookies, not localStorage:

```javascript
const token = jwt.sign({ userId: user._id }, process.env.JWT_SECRET, {
	expiresIn: '7d',
});

res.cookie('jwt', token, {
	httpOnly: true,
	secure: process.env.NODE_ENV === 'production',
	sameSite: 'strict',
	maxAge: 7 * 24 * 60 * 60 * 1000,
});
```

### The Security Measures That Saved Me

1. **Rate Limiting**: Stopped a DDoS attempt cold

```javascript
const limiter = rateLimit({
	max: 100,
	windowMs: 15 * 60 * 1000,
	message: 'Too many requests',
});
app.use('/api', limiter);
```

2. **Input Sanitization**: Prevented NoSQL injection

```javascript
const mongoSanitize = require('express-mongo-sanitize');
app.use(mongoSanitize());
```

3. **Security Headers**: Basic but effective

```javascript
const helmet = require('helmet');
app.use(helmet());
```

## Performance Optimizations That Matter

### Indexing: The Low-Hanging Fruit

Adding indexes to frequently queried fields improved response times by 10x:

```javascript
projectSchema.index({ status: 1, createdAt: -1 });
projectSchema.index({ 'team.userId': 1 });
```

But remember: indexes use memory and slow down writes. Profile your queries first:

```javascript
const explanation = await Project.find({ status: 'active' }).explain(
	'executionStats'
);
console.log(explanation.executionStats);
```

### The Aggregation Pipeline Magic

Instead of fetching all documents and processing in Node.js, use MongoDB's aggregation pipeline:

```javascript
const stats = await Project.aggregate([
	{ $match: { status: 'active' } },
	{ $unwind: '$team' },
	{
		$group: {
			_id: '$team',
			projectCount: { $sum: 1 },
			avgCompletion: { $avg: '$completionRate' },
		},
	},
	{ $sort: { projectCount: -1 } },
]);
```

### Caching Strategy (My TODO That Became Reality)

I procrastinated on caching until our API started timing out. Redis solved it:

```javascript
const cached = await redis.get(`project:${id}`);
if (cached) return JSON.parse(cached);

const project = await Project.findById(id);
await redis.setex(`project:${id}`, 3600, JSON.stringify(project));
return project;
```

## Lessons from Production Disasters

### The Cascading Delete Nightmare

I once deleted a user and watched in horror as all their projects, tickets, and comments vanished. Now I use soft deletes:

```javascript
userSchema.add({
	isActive: { type: Boolean, default: true },
});

userSchema.pre(/^find/, function () {
	this.find({ isActive: { $ne: false } });
});
```

### The Factory Function Pattern That Saved My Controllers

Repeating try-catch blocks everywhere was painful. Factory functions cleaned it up:

```javascript
const catchAsync = (fn) => {
	return (req, res, next) => {
		fn(req, res, next).catch(next);
	};
};

// Clean controller without try-catch
const getProject = catchAsync(async (req, res, next) => {
	const project = await Project.findById(req.params.id);
	if (!project) {
		return next(new AppError('Project not found', 404));
	}
	res.json({ status: 'success', data: project });
});
```

### Client-Side Rendering for Dynamic Content

Loading all comments with the initial page load was killing performance. Moving to client-side rendering for comments cut initial load time by 40%:

```javascript
// Instead of server-side rendering all comments
// Load them dynamically when needed
async function loadComments(ticketId) {
	const response = await fetch(`/api/tickets/${ticketId}/comments`);
	const comments = await response.json();
	renderComments(comments.data);
}
```

## The Tools That Make Life Easier

After trying different setups, here's my go-to stack:

- **Express.js**: Still the most flexible Node.js framework
- **MongoDB + Mongoose**: Perfect for rapidly evolving schemas
- **Pug**: For server-side rendering (yes, better than Handlebars)
- **Morgan**: HTTP request logger that's saved debugging hours
- **PM2**: Process manager for production
- **Postman**: API testing and documentation

## Looking Back: What I'd Do Differently

1. **Start with TypeScript**: The type safety would've prevented countless runtime errors
2. **Write tests first**: TDD feels slow initially but saves time long-term
3. **Document as you go**: Your future self will thank you
4. **Use DataLoader pattern**: Solves N+1 query problems elegantly
5. **Implement observability early**: Logs, metrics, and traces from day one

## Final Thoughts

Building RESTful APIs is a journey of continuous learning. Every production issue teaches you something new, every security breach you read about adds to your checklist, and every performance bottleneck makes you a better developer.

The key is starting simple and iterating. Don't try to implement every best practice from day one—you'll never ship. Start with the basics: clean separation of concerns, basic security, and consistent patterns. Then improve incrementally.

Remember: the best API is not the one with the most features or the cleverest abstractions. It's the one that's still running smoothly at 3 AM when you're fast asleep, handling thousands of requests without breaking a sweat.

---

_Currently working on: Implementing GraphQL alongside REST, exploring Deno as an alternative to Node.js, and diving deeper into microservices architecture. The learning never stops._
