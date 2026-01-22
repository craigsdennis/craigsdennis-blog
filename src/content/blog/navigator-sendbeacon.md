---
title: 'The Secret Power of navigator.sendBeacon'
description: 'How I discovered navigator.sendBeacon and used it to build non-obtrusive user tracking in LinkedOut.dev without disrupting the user experience.'
pubDate: 'Jan 22 2026'
heroImage: '../../assets/navigator-sendbeacon-header.webp'
---

I recently stumbled upon a web API that's been hiding in plain sight since 2014, and it completely changed how I think about user analytics: `navigator.sendBeacon`. Let me tell you why this little function is a game-changer for tracking user behavior without being annoying about it.

## The Problem with Traditional Analytics

Here's the thing: when you want to track what users are doing on your site—clicks, page views, form submissions—you typically fire off an HTTP request. But what happens when a user clicks a link or closes a tab? Your carefully crafted analytics request gets **canceled** before it reaches your server.

The traditional workaround? Make the request synchronous and block the page until it completes. Gross. This freezes the browser, creates a terrible user experience, and makes you look like you don't care about performance.

## Enter navigator.sendBeacon

The [`navigator.sendBeacon()`](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/sendBeacon) API was built specifically for this use case. Here's what makes it brilliant:

```javascript
// That's it. That's the whole API.
navigator.sendBeacon('/analytics', JSON.stringify({
  event: 'button_click',
  timestamp: Date.now(),
  metadata: { button_id: 'signup' }
}));
```

When you call `sendBeacon()`, the browser:
1. **Queues the request** to be sent asynchronously
2. **Guarantees delivery** even if the page is closing or navigating away
3. **Doesn't block** the user experience—not even for a millisecond
4. **Prioritizes user actions** over your analytics ping

It's like having a postal service for your analytics data. Drop it in the mailbox and forget about it—the browser handles the rest.

## How I Used It in LinkedOut.dev

I built [LinkedOut.dev](https://linkedout.dev) as a tool to help developers create better LinkedIn posts. Since user behavior is key to improving the tool, I needed to track:

- Which templates users click on
- How long they spend editing
- Whether they copy the final result
- When they abandon the flow

Here's how I implemented it with `sendBeacon`:

```javascript
// Track template selection
function trackTemplateSelect(templateId) {
  navigator.sendBeacon('/api/analytics', JSON.stringify({
    event: 'template_select',
    templateId,
    sessionId: getSessionId(),
    timestamp: Date.now()
  }));
}

// Track when users leave the page (visibility change)
document.addEventListener('visibilitychange', () => {
  if (document.visibilityState === 'hidden') {
    navigator.sendBeacon('/api/analytics', JSON.stringify({
      event: 'session_end',
      duration: Date.now() - sessionStart,
      sessionId: getSessionId()
    }));
  }
});

// Track copy action
copyButton.addEventListener('click', () => {
  navigator.sendBeacon('/api/analytics', JSON.stringify({
    event: 'copy_result',
    sessionId: getSessionId()
  }));
  
  // User interaction isn't blocked
  navigator.clipboard.writeText(content);
});
```

The beauty? **Zero impact on user experience.** When someone clicks a template, the tracking happens in the background while the UI updates instantly. When they close the tab, I still get the session data.

## Browser Support and Fallbacks

The best part? Browser support is [excellent](https://caniuse.com/beacon):
- ✅ Chrome 39+
- ✅ Firefox 31+
- ✅ Safari 11.1+
- ✅ Edge 14+

For older browsers, you can fall back to a standard `fetch` with `keepalive`:

```javascript
function trackEvent(url, data) {
  if (navigator.sendBeacon) {
    navigator.sendBeacon(url, JSON.stringify(data));
  } else {
    // Fallback for older browsers
    fetch(url, {
      method: 'POST',
      body: JSON.stringify(data),
      keepalive: true // Similar guarantee
    }).catch(() => {
      // Fire and forget
    });
  }
}
```

## Important Gotchas

A few things to keep in mind:

1. **Request size limit**: Most browsers limit beacon requests to 64 KB. Don't try to send your entire app state.

2. **POST only**: `sendBeacon` always sends a POST request. Your server needs to handle it accordingly.

3. **No response handling**: This is fire-and-forget. You can't read the response or handle errors.

4. **Content-Type matters**: If you send a string, it defaults to `text/plain`. Send a `Blob` or `FormData` for other content types:

```javascript
const blob = new Blob(
  [JSON.stringify(data)],
  { type: 'application/json' }
);
navigator.sendBeacon('/api/analytics', blob);
```

## Why This Matters

In a world where users expect instant responses and hate janky websites, `navigator.sendBeacon` lets you collect the data you need without compromising the experience. It's respectful, it's performant, and it just works.

I've been writing JavaScript for years and somehow missed this API. If you're doing any kind of user analytics, event tracking, or error reporting—especially on page unload—you owe it to yourself to check out `sendBeacon`.

## Resources

- [MDN: Navigator.sendBeacon()](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/sendBeacon)
- [W3C Beacon Specification](https://www.w3.org/TR/beacon/)
- [Can I Use: sendBeacon](https://caniuse.com/beacon)
- [LinkedOut.dev](https://linkedout.dev) - See it in action!

---

Have you used `sendBeacon` in your projects? Found other creative uses for it? I'd love to hear about it. Drop me a line or share your experiences!
