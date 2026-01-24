---
title: 'TypeScript and Bearded Dragons: A Love Story'
description: 'What my pet bearded dragon taught me about TypeScript, type safety, and why both make better companions than you might think.'
pubDate: 'Jan 24 2026'
heroImage: '../../assets/typescript-and-bearded-dragons-header.webp'
---

I never thought I'd be writing about TypeScript and bearded dragons in the same post, but here we are. My bearded dragon, Smaug, has been sitting on his basking rock judging my code for three years now, and I've realized these two unlikely companions have more in common than you'd think.

## The Type Safety Parallel

Here's the thing about bearded dragons: they're surprisingly predictable. Smaug wakes up, basks, eats crickets, basks some more, and occasionally does a head bob to assert dominance over... well, nothing really. There's a pattern, a structure, a *type system* to his behavior.

TypeScript offers that same kind of predictability for your code. Just like I know Smaug needs his UVB light at a specific temperature range (95-105°F for the basking spot, thanks), TypeScript ensures your functions receive the right types of data at the right time.

```typescript
interface BeardedDragon {
  name: string;
  age: number;
  temperament: 'chill' | 'judgmental' | 'hangry';
  baskingSpotTemp: number;
}

function checkBaskingSpot(dragon: BeardedDragon): boolean {
  return dragon.baskingSpotTemp >= 95 && dragon.baskingSpotTemp <= 105;
}

const smaug: BeardedDragon = {
  name: 'Smaug',
  age: 3,
  temperament: 'judgmental',
  baskingSpotTemp: 98
};

checkBaskingSpot(smaug); // ✓ TypeScript keeps Smaug happy
```

Without TypeScript, you might accidentally pass a string where you meant a number, just like you might accidentally set the basking spot to 150°F and... well, let's not think about that.

## Runtime Errors vs. Compile-Time Checks

One of the worst feelings in reptile keeping is discovering something went wrong *after* it's already happened. The timer on the heat lamp failed. The cricket escaped. The water bowl is somehow upside down (seriously, how?).

JavaScript is like that. You don't know there's a problem until your code runs and explodes in production:

```javascript
// JavaScript - no warnings until runtime
function feedDragon(dragon, food) {
  return dragon.eat(food);
}

feedDragon(smaug, 'pizza'); // Runtime error: Dragons can't eat pizza!
```

TypeScript catches these problems *before* they become disasters:

```typescript
type DragonFood = 'crickets' | 'dubia roaches' | 'greens' | 'squash';

function feedDragon(dragon: BeardedDragon, food: DragonFood): void {
  console.log(`${dragon.name} is eating ${food}`);
}

feedDragon(smaug, 'pizza'); 
// ✗ Compile error: Argument of type '"pizza"' is not assignable to parameter of type 'DragonFood'
```

Your IDE yells at you immediately. No waiting. No surprises. No unhappy dragon.

## The Comfort of Constraints

New reptile keepers often think more freedom means better care. "I'll just wing it with temperatures!" "Who needs a feeding schedule?" This rarely ends well.

Similarly, some developers see TypeScript's type system as restrictive. All those type annotations, interfaces, and compiler complaints feel like bureaucracy. But constraints aren't limitations - they're *guardrails*.

Smaug thrives with structure. TypeScript helps your code thrive the same way.

### Union Types: The Temperament Spectrum

Bearded dragons have moods. Smaug cycles through his:

```typescript
type DragonMood = 
  | 'peaceful' 
  | 'alert' 
  | 'hungry' 
  | 'dark-beard-angry';

function handleDragonMood(mood: DragonMood): string {
  switch (mood) {
    case 'peaceful':
      return 'Let him vibe';
    case 'alert':
      return 'Something interesting is happening';
    case 'hungry':
      return 'Time for crickets';
    case 'dark-beard-angry':
      return 'Give him space immediately';
  }
}
```

TypeScript ensures you handle every possible mood. Miss one, and the compiler complains. It's like having a care guide that never lets you forget the important stuff.

## Generics: Reusable Care Routines

Both bearded dragons and code benefit from reusable patterns. You wouldn't create a completely different feeding routine for every single meal - you'd have a template:

```typescript
interface Pet<T> {
  name: string;
  species: string;
  diet: T[];
}

interface DragonDiet {
  foodType: DragonFood;
  quantity: number;
}

const smaug: Pet<DragonDiet> = {
  name: 'Smaug',
  species: 'Pogona vitticeps',
  diet: [
    { foodType: 'crickets', quantity: 10 },
    { foodType: 'greens', quantity: 1 }
  ]
};

function feedPet<T>(pet: Pet<T>): void {
  console.log(`Feeding ${pet.name} their diet`);
  pet.diet.forEach(meal => console.log(meal));
}

feedPet(smaug);
```

Generics let you write flexible, reusable code that still maintains type safety. It's like having a care routine that works for different reptiles while respecting their unique needs.

## The TypeScript Migration Journey

Getting a bearded dragon is a commitment. You don't just bring one home and hope for the best - you research, prepare, and gradually build the right habitat.

Migrating to TypeScript is similar. You don't need to convert everything overnight. Start small:

### Step 1: Enable TypeScript Checking

Add `// @ts-check` to existing JavaScript files:

```javascript
// @ts-check

/**
 * @param {BeardedDragon} dragon
 * @param {number} temp
 */
function adjustHeat(dragon, temp) {
  // TypeScript will now check this!
}
```

### Step 2: Rename Files Gradually

Pick one module. Change `.js` to `.ts`. Add types. See how it feels. Just like adding one decoration to a terrarium at a time.

### Step 3: Add Strict Mode When Ready

```json
{
  "compilerOptions": {
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true
  }
}
```

This is like upgrading from a basic heat lamp to a temperature-controlled setup. More work upfront, better results long-term.

## Why Both Matter

Smaug doesn't care that I use TypeScript. He cares that his habitat is consistent, his food is reliable, and his routine is predictable. TypeScript helps me write code with that same reliability.

Every time I fix a type error before it becomes a runtime bug, that's time I can spend on features that matter. Or, you know, watching Smaug chase crickets. That's important work too.

## Final Thoughts

If you're on the fence about TypeScript, think about it like this: you wouldn't trust someone who says "I'll just guess what temperature my reptile needs." So why trust code that guesses what types your functions need?

TypeScript isn't perfect. Neither are bearded dragons (Smaug once got scared of his own reflection). But both offer structure, reliability, and fewer surprises. In programming and pet care, that's worth its weight in crickets.

Now if you'll excuse me, Smaug is giving me the "where are my greens" stare, and TypeScript won't help me with that.

---

*No bearded dragons were harmed in the writing of this post. One bearded dragon was, however, mildly inconvenienced by having to wait an extra five minutes for lunch.*
