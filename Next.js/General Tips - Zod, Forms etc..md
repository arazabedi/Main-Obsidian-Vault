Zod
```ts
.shape to log the schema 
```

If your logs aren't logging it's because they're in the terminal (server-side)

```ts

const users = (await prisma.user.findMany({})).reverse()
// type of users is an object array with the properties seen below

// This:
type User = typeof users[number]

// Is identical to:
	interface User {
		name: string;
		email: string;
		gender: string;
		preferredGender: string;
		responses: number[]; // Assuming responses are numerical for simplicity
	}
```

```shell
npx depcheck --oneline // outputs all unused dependencies space spearated so you can uninstall easily
```

If you are using a component within another component and it has a runtime error. You can use `error.tsx` to display what you want while that error exists. For example, I had a form that took email and on submit was supposed to render matchmaker matches (the child component was conditionally rendered). On the email not being found by the child component, that component had an error. Now on the parent component we were sent to error.tsx, where I just put the same exact form except it didn't have the child component.

