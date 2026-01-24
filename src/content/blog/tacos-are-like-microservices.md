---
title: 'Tacos Are Like Microservices'
description: 'Why the best tacos and the best microservices share the same secret: modular design, independent components, and the freedom to mix and match.'
pubDate: 'Jan 24 2026'
heroImage: '../../assets/tacos-are-like-microservices-header.webp'
---

If you've ever built a microservices architecture, you already understand tacos. And if you've ever assembled the perfect taco, you're halfway to understanding distributed systems. Let me explain.

## The Monolith vs. The Taco Bar

Think about a traditional burrito. Everything is wrapped up in one giant tortilla - rice, beans, meat, cheese, salsa, guacamole, all mashed together. It's delicious, sure, but you're committed to the whole package. Want to replace just the beans? Too bad. Want to add an extra serving of guacamole without doubling the rice? Tough luck.

That's your monolith. Everything is coupled together, deployed as one unit, and you can't change one thing without touching the whole enchilada.

Now picture a taco bar. You've got separate containers for:

- **Tortillas** - Your delivery mechanism
- **Proteins** - Carne asada, carnitas, chicken, or tofu
- **Toppings** - Onions, cilantro, jalapeños
- **Sauces** - Salsa verde, salsa roja, hot sauce
- **Extras** - Lime, radishes, guacamole

Each component exists independently. You can swap the chicken for carnitas without touching the salsa. You can run out of cilantro without shutting down the whole operation. You can scale up guacamole production independently because everyone knows guac is the first to go.

This is microservices architecture.

## Each Taco is Independently Deployable

When you order three tacos, each one can be different. One al pastor, one carnitas, one vegetarian. Each taco is an independent unit of value that can be assembled, customized, and delivered without affecting the others.

Similarly, each microservice can be:

- **Developed independently** - The carnitas team doesn't need to coordinate with the salsa team
- **Deployed independently** - Update the salsa service without redeploying the tortilla service
- **Scaled independently** - Run more guacamole instances during happy hour
- **Failed independently** - If you run out of cilantro, the rest of the taco bar keeps operating

## Single Responsibility Principle

A good taco ingredient does one thing really well. The tortilla holds everything. The protein provides substance. The salsa adds flavor and heat. The cilantro adds freshness.

Nobody expects the tortilla to also be the hot sauce. That would be chaos.

Good microservices follow the same principle. Each service has a clear, focused responsibility:

- **Authentication service** - Handles login, not payment processing
- **Payment service** - Processes payments, not user profiles
- **Notification service** - Sends emails and texts, not business logic

When services try to do too much, you end up with the software equivalent of a tortilla that's also trying to be cilantro. It's confusing and it doesn't work.

## API Contracts Are Like Taco Assembly Rules

At a good taco stand, there's an implicit contract. You know what to expect:

- Tortillas come warm and pliable
- Protein is pre-cooked and ready to serve
- Toppings are chopped to the right size
- Everything is served in containers that make sense

If the carnitas suddenly arrived frozen or the tortillas came shredded, the whole system breaks down.

Microservices need the same clarity. Each service exposes an API contract that other services can depend on:

```javascript
// Taco Assembly API
POST /api/tacos
{
  "tortilla": "corn",
  "protein": "carnitas",
  "toppings": ["onion", "cilantro"],
  "salsa": "verde",
  "extras": ["lime", "radish"]
}
```

As long as the contract stays stable, you can completely rewrite the carnitas service from Python to Go, and nobody downstream needs to know or care.

## Orchestration and Choreography

There are two ways to assemble tacos:

### Orchestration

A single person (the orchestrator) controls the entire process:

1. Grab tortilla
2. Add protein
3. Add toppings
4. Add salsa
5. Serve

This is like a service mesh or API gateway that coordinates all your microservices. One central authority manages the flow.

### Choreography

Each station knows what to do when triggered:

- Tortilla station: When order arrives, warm tortilla and pass it along
- Protein station: When tortilla arrives, add protein and pass it along
- Topping station: When taco arrives, add requested toppings
- Final station: When complete taco arrives, serve it

This is event-driven architecture. Services react to events and messages without a central coordinator. The taco emerges through the collaborative dance of independent services.

Both approaches work, depending on your needs.

## Failure Handling and Graceful Degradation

What happens when you run out of cilantro?

At a bad taco stand, they shut down. "Sorry, no tacos today."

At a good taco stand, they adapt. "We're out of cilantro, but everything else is available. Would you still like your tacos?"

Your microservices should be equally resilient:

- **Circuit breakers** - If the recommendation service is down, show recent posts instead
- **Fallbacks** - If the image service fails, show a placeholder
- **Graceful degradation** - Core functionality works even when non-critical services fail

The entire system doesn't collapse because one ingredient is missing.

## Distributed Systems Are Messy

Here's where the metaphor gets real. Running a taco bar is hard:

- **Consistency** - Did all stations get the order update?
- **Timing** - Tortillas get cold, proteins dry out
- **Coordination** - What if one station is backed up?
- **Communication** - How do stations signal they're out of ingredients?

Distributed systems face the same challenges:

- **CAP theorem** - You can't have perfect consistency, availability, and partition tolerance
- **Network latency** - Services communicate over networks that can be slow or unreliable
- **Data consistency** - Keeping data in sync across services is hard
- **Observability** - Understanding what's happening across services is complex

The benefit? When done well, you get flexibility, scalability, and resilience. Worth it for the right use case.

## When to Use a Burrito (Monolith) Instead

Not every meal needs to be tacos. Sometimes a burrito makes more sense:

- **Small teams** - If you're three people, managing microservices is overkill
- **Simple domains** - Not every app needs distributed complexity
- **Early stages** - Start with a monolith, split into services as you grow
- **Strong consistency requirements** - When you absolutely need transactions and consistency

Don't build a distributed taco bar when you're making one burrito for yourself.

## Takeaway

The best tacos are:

- Made from independent, high-quality ingredients
- Assembled fresh to order
- Customizable to taste
- Resilient to missing components
- Served by people who understand the system

The best microservices are:

- Built as independent, focused services
- Deployed and scaled dynamically
- Composed to meet specific needs
- Resilient to partial failures
- Maintained by teams who understand the tradeoffs

Next time you're at a taco bar, take a moment to appreciate the distributed architecture in front of you. And next time you're designing a system, ask yourself: should this be tacos or a burrito?

---

*Now if you'll excuse me, all this talk about tacos has made me hungry.*
