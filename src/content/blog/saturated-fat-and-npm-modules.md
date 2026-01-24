---
title: 'Saturated Fat and npm Modules: A Delicious Lesson in Dependencies'
description: 'What nutritional science can teach us about managing dependencies, avoiding bloat, and understanding what we really need in our stack.'
pubDate: 'Jan 24 2026'
heroImage: '../../assets/saturated-fat-and-npm-modules-header.webp'
---

Here's a weird thought I had while staring at my `package.json`: managing npm dependencies is a lot like navigating dietary advice about saturated fat.

Bear with me.

For decades, saturated fat was the villain of nutrition science. Avoid it at all costs! Replace it with margarine and vegetable oils! Then science evolved, we learned nuance exists, and now we understand that the full picture is way more complex than "saturated fat = bad." Context matters. Sources matter. The whole diet matters.

Sound familiar? It should, because we've been doing the exact same thing with npm modules.

## The "Fat-Free" Era of npm

Remember when the advice was basically "install everything you need"? Need left-pad? Install it. Need a date formatter? Install moment.js (all 67KB of it, minified). The npm ecosystem exploded with tiny, single-purpose modules, and we gobbled them up like fat-free cookies in the '90s.

The thinking was simple: **dependencies are good**. They're modular, they're reusable, they let you focus on your actual problem instead of reinventing wheels. Don't Repeat Yourself, right?

Then we started noticing problems:

- Our `node_modules` folders became black holes of disk space
- Build times ballooned
- Security vulnerabilities in dependencies we didn't even know we had
- Dependency trees deeper than my backlog of coding tutorials I'll "definitely watch later"

The pendulum swung hard. Suddenly dependencies were the enemy. "Zero dependencies" became a badge of honor. Bundle size optimization became an obsession.

## The Problem with Extremes

Here's where the saturated fat comparison really clicks: **absolutism doesn't work**.

Just like you can't just say "avoid all saturated fat" (coconut oil and bacon are not the same thing), you can't say "avoid all dependencies" or "install whatever you need without thinking."

The truth is more nuanced:

### Some Dependencies Are Essential

Just like your body needs some fats to function, your project genuinely needs some dependencies. You're probably not going to write your own:

- **React or Vue** - Unless you enjoy pain
- **Express or Fastify** - HTTP servers are solved problems
- **Lodash** (selectively) - Some utilities are just battle-tested and reliable
- **Testing frameworks** - Jest, Vitest, Mocha - pick your poison, don't write your own

These are your **essential fats** - they provide structure, they've been around long enough to prove their worth, and reimplementing them is a waste of your time.

### Some Dependencies Are Empty Calories

Then there are the packages that give you 3 lines of code you could've written yourself:

```javascript
// Do you really need a package for this?
const isOdd = (num) => num % 2 !== 0;

// Or this?
const arrayFirst = (arr) => arr[0];
```

These are your **empty calorie dependencies** - they add weight without meaningful nutrition. They increase your attack surface, bloat your bundle, and add maintenance overhead for functionality you could've implemented in 30 seconds.

### Some Dependencies Are Context-Dependent

The interesting category. Is [axios](https://axios-http.com/) necessary when `fetch` is built-in now? It depends:

- **If you need interceptors, automatic transforms, and better error handling** - Yes, axios is your friend
- **If you're making basic GET requests** - Native fetch is probably fine

Is [day.js](https://day.js.org/) better than [date-fns](https://date-fns.org/) or [Temporal](https://tc39.es/proposal-temporal/docs/)? Depends on your bundle size requirements, your browser support needs, and how complex your date manipulation is.

This is like the saturated fat question: **context and quality matter**. A grass-fed steak is different from a pile of processed cheese. A well-maintained library with active community support is different from an abandoned npm package last updated in 2017.

## A Balanced Diet for Your Dependencies

So how do you strike the balance? Here's my approach:

### 1. Check the Nutrition Label

Before installing, ask:

- **How big is it?** Use [Bundlephobia](https://bundlephobia.com/) to check bundle size impact
- **Is it maintained?** Check GitHub activity, last publish date, open issues
- **What are its dependencies?** A package with 47 subdependencies might not be worth it
- **Can I do this myself easily?** If it's < 20 lines, maybe write it

### 2. Use Selective Imports

Don't import the whole kitchen when you need a spoon:

```javascript
// 😱 Imports ALL of lodash
import _ from 'lodash';

// 😊 Imports just what you need
import debounce from 'lodash/debounce';
```

Tree-shaking helps, but being explicit is better.

### 3. Audit Regularly

Just like you might check your bloodwork, audit your dependencies:

```bash
npm outdated
npm audit
npx depcheck
```

Remove what you're not using. Update what's maintained. Replace what's abandoned.

### 4. Know Your Macros

Every project has different needs:

- **Shipping to browsers?** Bundle size is critical. Be ruthless.
- **Backend service?** You have more breathing room. Prioritize developer experience.
- **Internal tool?** Install whatever makes you productive.

There's no one-size-fits-all diet, and there's no one-size-fits-all dependency strategy.

## The Real Lesson

Both nutrition science and dependency management teach us the same thing: **nuance beats dogma**.

Don't blindly follow "zero dependencies" any more than you'd follow "zero fat." Don't install everything without thinking any more than you'd eat a stick of butter.

Instead:

- **Understand what you're consuming**
- **Know why you need it**
- **Consider the trade-offs**
- **Adjust based on your specific context**

Your `package.json` is like your diet. Make it sustainable, make it fit your needs, and don't feel guilty about including high-quality ingredients that genuinely make your life better.

---

Now if you'll excuse me, I'm going to have some coffee with actual cream in it while I remove three dependencies I don't actually need. Balance, baby.

