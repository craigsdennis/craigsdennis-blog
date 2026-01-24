---
title: 'TypeScript is Like a Greek Salad'
description: 'Why TypeScript reminds me of the perfect Greek salad - fresh ingredients, clear structure, and surprisingly delightful when you embrace all the parts.'
pubDate: 'Jan 24 2026'
heroImage: '../../assets/typescript-is-like-a-greek-salad-header.webp'
---

Bear with me here. I was eating a Greek salad yesterday while debugging some gnarly TypeScript errors, and it hit me - **TypeScript is basically a Greek salad**.

Before you close this tab, hear me out.

## The Ingredients Are Simple

A proper Greek salad (or *horiatiki* if you want to be authentic about it) has a handful of core ingredients:

- Tomatoes
- Cucumbers
- Red onions
- Feta cheese
- Kalamata olives
- Olive oil
- Oregano

That's it. No lettuce. No croutons. No weird additions.

TypeScript is the same way. At its core, it's just **JavaScript with types**. The ingredients are simple:

```typescript
// JavaScript you already know
const user = {
  name: "Alice",
  age: 30
};

// Plus types
interface User {
  name: string;
  age: number;
}
```

You don't need to learn a whole new language. You're still writing JavaScript - you're just being clear about what you're working with.

## Everything Has Its Place

In a Greek salad, the feta sits on top, the olives are scattered throughout, and everything is drizzled with olive oil. There's a structure to it. You know what you're getting.

TypeScript brings that same clarity to your code. Every variable, every function parameter, every return value has a clearly defined type:

```typescript
interface Salad {
  tomatoes: number;
  cucumbers: number;
  feta: string;
  olives: string[];
}

function makeSalad(ingredients: Salad): string {
  return `A salad with ${ingredients.tomatoes} tomatoes and ${ingredients.feta} feta`;
}
```

You can look at this code and immediately know what goes where. No surprises. No guessing.

## It Prevents Disasters

Ever bitten into what you thought was feta, only to discover it was a chunk of raw onion? That's what JavaScript without TypeScript feels like sometimes.

You think you're passing a number, but you're actually passing a string. You assume an object has a property, but it's undefined. These bugs only show up at runtime, usually in production, usually on a Friday afternoon.

TypeScript catches these disasters before they happen:

```typescript
function addOlives(count: number): void {
  console.log(`Adding ${count} olives`);
}

addOlives("twelve"); // Error: Argument of type 'string' is not assignable to parameter of type 'number'
```

Your editor tells you immediately: "Hey, that's not what this function expects." No need to run the code. No need to deploy. Just fix it and move on.

## The Type System is Your Friend

Some people look at a Greek salad and complain about the raw onions. "Too strong," they say. "Too sharp."

Those people are wrong. The onions are what make the salad **sing**. They provide bite, contrast, sharpness. Without them, it's just sad vegetables.

TypeScript's type system gets the same bad rap from developers who haven't given it a real chance. "Too strict," they complain. "Too much ceremony."

But once you embrace it, you realize the types aren't fighting you - they're **helping** you. They're documentation that stays up to date. They're autocomplete that actually works. They're refactoring that you can trust.

```typescript
type SaladSize = "small" | "medium" | "large";

function orderSalad(size: SaladSize, extras?: string[]): void {
  console.log(`Ordering a ${size} salad`);
  if (extras) {
    console.log(`With extras: ${extras.join(", ")}`);
  }
}

// Your editor now autocompletes the size options
orderSalad("medium", ["extra feta", "no onions"]);

// And catches mistakes
orderSalad("gigantic"); // Error: Argument of type '"gigantic"' is not assignable to parameter of type 'SaladSize'
```

## It Scales Beautifully

A Greek salad works whether you're making one serving or feeding a family reunion. The recipe scales. The proportions stay consistent.

TypeScript does the same for your codebase. A small project with a few files? Types help. A massive application with hundreds of developers? Types are **essential**.

The magic is in the tooling. TypeScript's language server powers your editor. It knows every type in your entire codebase. It can jump to definitions, find all references, and rename symbols across thousands of files without breaking anything.

```typescript
// Define a type once
interface MenuItem {
  id: string;
  name: string;
  price: number;
  available: boolean;
}

// Use it everywhere
function displayMenu(items: MenuItem[]): void {
  items.forEach(item => {
    console.log(`${item.name}: $${item.price}`);
  });
}

// Refactor with confidence
// If you change MenuItem, TypeScript will tell you everywhere it breaks
```

## It's Better Fresh

A Greek salad is best when the ingredients are fresh. Ripe tomatoes, crisp cucumbers, good olive oil. You can't fake quality.

TypeScript is the same way. It works best when you write types for your actual data structures, not when you slap `any` on everything and call it a day.

```typescript
// Don't do this
function processData(data: any): any {
  return data.something.whatever;
}

// Do this
interface UserData {
  id: string;
  profile: {
    name: string;
    email: string;
  };
}

function processData(data: UserData): string {
  return data.profile.name;
}
```

Fresh types give you fresh guarantees. Stale types (or no types) leave you wondering what's going to break.

## You Can Customize It

A traditional Greek salad follows the recipe, but that doesn't mean you can't adjust things. Want extra feta? Go for it. Hate onions? Leave them out.

TypeScript is surprisingly flexible. You can start with loose types and tighten them over time. You can use utility types to transform existing types. You can even tell TypeScript to back off when you know better:

```typescript
// Strict by default
let salad: Salad = {
  tomatoes: 3,
  cucumbers: 2,
  feta: "traditional",
  olives: ["kalamata"]
};

// But flexible when needed
type OptionalSalad = Partial<Salad>;
type ReadonlySalad = Readonly<Salad>;
type SaladWithoutFeta = Omit<Salad, "feta">;

// Pick the flavor that works for you
```

## It Takes Practice

Nobody makes a perfect Greek salad on the first try. You might add too much onion. You might forget the oregano. You might use the wrong olives.

TypeScript is the same way. Your first attempts will be awkward. You'll fight with the compiler. You'll Google "how to fix TypeScript error" more times than you'd like to admit.

But stick with it. After a while, you stop thinking about the types and just write them naturally. Your editor becomes an extension of your brain. Your bugs decrease. Your confidence increases.

## The Result is Worth It

A well-made Greek salad is a thing of beauty. Simple ingredients, expertly combined, creating something greater than the sum of its parts.

TypeScript does the same for your codebase. It takes JavaScript - already a powerful, flexible language - and adds just enough structure to make it **reliable**. You get better tooling, fewer bugs, and code that's easier to understand and maintain.

## Get Started

If you haven't tried TypeScript yet, start small:

1. Rename a `.js` file to `.ts`
2. Add types to your function parameters
3. Let your editor show you what's possible
4. Watch as your code becomes more predictable

You don't need to convert your entire codebase overnight. Just like you don't need to eat Greek salad every day. But once you've had a good one, you'll understand why people keep coming back.

And hey, maybe next time you're debugging, grab a Greek salad. The tomatoes won't fix your code, but the fresh perspective might.

---

*The header image for this post was generated using [Flux Schnell](https://replicate.com/black-forest-labs/flux-schnell) on Replicate. Because nothing says "TypeScript and Greek Salad" quite like AI-generated food photography with code snippets floating around.*
