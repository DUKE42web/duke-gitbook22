# PICK Mapped Type

In this post you’ll learn how to use [Pick in TypeScript](https://www.typescriptlang.org/docs/handbook/utility-types.html#picktype-keys) to help clean up your codebase and make your type management far easier and cleaner!

What is `Pick`? Pick is what’s known as a Mapped Type, and I’ve covered other Mapped Types in my [TypeScript Masterclass course](https://ultimatecourses.com/learn/typescript-masterclass).

Here’s the syntax for `Pick`:

We pass in an existing `Type` and then declare the `Keys` we wish to “pick” from the `Type`. Make sense? The `Keys` _have to_ exist on the `Type` for you to be able to “pick” them, otherwise you’ll see a TypeScript error.

> 🛹 The opposite behaviour would be to use [TypeScript’s Omit type](https://www.typescriptlang.org/docs/handbook/utility-types.html#omittype-keys) instead.

Mapped Types allow us to dynamically construct new types from existing ones, creating a new type with new meaning. It allows us to create new types on the fly, without having to declare new types or interfaces.

For example, here is a common authentication service I’ve been working on lately, written in [Angular](https://ultimatecourses.com/courses/angular) and Firebase:

```
@Injectable({
  providedIn: 'root',
})
export class AuthService {
  constructor() {}

  async create({ name, email, password }: any) {...}

  async login({ email, password }: any) {...}

  async resetPassword({ email }: any) {...}
}
```

You’ll note that we are essentially using the same properties to “create”, “login” and “reset” a password. We use a mixture of `name`, `email`, `password`.

How could we type this? 🤔

Using the `type` keyword we could separately construct each property we expect and the using an [Intersection Type](https://www.typescriptlang.org/docs/handbook/unions-and-intersections.html#intersection-types) to combine the types into new ones:

```typescript
type Name = { name: string };
type Email = { email: string };
type Password = { password: string };

type CreateUser = Name & Email & Password; // { name: string, email: string, password: string }
type LoginUser = Email & Password; // { email: string, password: string }
type ResetPassword = Email; // { email: string }
```

We could then do this to add the types in the correct places:

```typescript
@Injectable({
  providedIn: 'root',
})
export class AuthService {
  constructor() {}

  async create({ name, email, password }: CreateUser) {...}

  async login({ email, password }: LoginUser) {...}

  async resetPassword({ email }: ResetPassword) {...}
}
```

This is okay for such simple usage, but you can imagine expanding your codebase with more complex features and suddenly things begin spiralling out of control into a `type` world of pain.

This is where `Pick` comes into play. I aim for minimal code. No code is good code!

As we’ve looked at the syntax already, `Pick` accepts an existing `Type` and then the properties (`Keys`) that you’d like to “pick” from it.

First, let’s create a `Credentials` type via an `interface`, which will replace our stack of `type` declarations and introduce some `Pick` magic:

```typescript
export interface Credentials {
  name: string;
  email: string;
  password: string;
}
```

First of all, this is far easier to read and manage.

Secondly, I prefer using an `interface` over a `type` unless I’m [composing more complex types](https://ultimatecourses.com/blog/typescript-interfaces-vs-types), which I don’t think “credentials” are particularly complex.

Using `Pick`, we can tell TypeScript _just to pick_ those particular `Keys` from our existing `Type` (which could be an `interface` or `type`).

This allows us to cleverly construct a new type from using `Pick`, but only use the relevant properties (ignoring the others):

```typescript
export interface Credentials {
  name: string;
  email: string;
  password: string;
}

@Injectable({
  providedIn: 'root',
})
export class AuthService {
  constructor() {}

  async create({ name, email, password }: Credentials) {...}

  async login({ email, password }: Pick<Credentials, 'email' | 'password'>) {...}

  async resetPassword({ email }: Pick<Credentials, 'email'>) {...}
}
```

Far cleaner, and all the types are kept in a single place. If you’re likely to reuse them, you can of course break them out into `type` keywords as well which I’ll show you momentarily.

> 🚨 Pro tip: Be careful not to confuse the `'email' | 'password'` with a [Union Type](https://www.typescriptlang.org/docs/handbook/unions-and-intersections.html#union-types). In a Mapped Type such as Pick, this `|` syntax is more “and” instead of “or”.

y

Ready to go beyond ForEach? Get confident with advanced methods - Reduce, Find, Filter, Every, Some and Map.

As an extra bonus, we'll also send you some extra goodies across a few extra emails.

If we were to in future change the `Credentials` interface, the new definitions will flow through the rest of the code, rather than us updating each Intersection Type manually.

`Pick` is a great utility [Mapped Type](https://www.typescriptlang.org/docs/handbook/2/mapped-types.html) that makes things lots easier for us, I highly encourage you to use it to your advantage to keep your codebase clean!

> 🕵️♂️ You can also check out the brief [Pick documentation](https://www.typescriptlang.org/docs/handbook/utility-types.html#picktype-keys) if you’d like to as well.

And promised, here’s how you can create new types with the resulting `Pick` types, which would allow you to use them elsewhere without repeating the same code.

```
export interface Credentials {
  name: string;
  email: string;
  password: string;
}

type LoginUser = Pick<Credentials, 'email' | 'password'>;
type ResetPassword = Pick<Credentials, 'email'>;
```

Either way, this is far easier to see! We also get added intellisense benefits with using an `interface` instead of a `type`.
