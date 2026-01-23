---
title: 'Finding Delicious Tacos with APIs'
description: 'Explore how to use location-based APIs like Yelp Places and Foursquare to build taco-hunting apps that help you discover the best Mexican food in your area.'
pubDate: 'Jan 23 2026'
heroImage: '../../assets/finding-tacos-with-apis-header.webp'
---

Let's be real - finding amazing tacos is one of life's most important quests. And what better way to combine two of my favorite things (tacos and APIs) than building an app that helps people discover the best Mexican food in their area?

Location-based APIs have made it incredibly easy to build apps that help people find great local businesses. Whether you're building a food discovery app, a travel companion, or just a personal taco tracker, APIs like Yelp Places and Foursquare give you access to millions of verified business listings, reviews, and real-time data.

## Why Use Location APIs for Food Discovery?

Before we dive into the code, let's talk about why these APIs are perfect for building food discovery apps:

- **Comprehensive data** - Millions of restaurants, complete with addresses, hours, photos, and menus
- **User reviews** - Real feedback from actual customers who've been there
- **Rich metadata** - Categories, price levels, amenities, delivery options
- **Real-time info** - Current hours, busy times, and availability
- **Global coverage** - Works across cities and countries

Plus, you don't have to maintain any of this data yourself. The hard work of verifying addresses, updating hours, and aggregating reviews is already done.

## Yelp Places API - The Taco Hunter's Best Friend

The [Yelp Places API](https://docs.developer.yelp.com/docs/fusion-intro) is one of my go-to choices for food discovery. With over 200 million reviews and data on millions of businesses worldwide, it's incredibly powerful.

### Getting Started

First, sign up for a free account at [Yelp for Developers](https://www.yelp.com/developers/v3/manage_app) and grab your API key. Authentication is straightforward - just include your API key in the `Authorization` header:

```javascript
const YELP_API_KEY = process.env.YELP_API_KEY;

const headers = {
  'Authorization': `Bearer ${YELP_API_KEY}`
};
```

### Finding Taco Spots Nearby

Here's how to search for tacos near a specific location:

```javascript
async function findTacosNearby(latitude, longitude) {
  const url = new URL('https://api.yelp.com/v3/businesses/search');
  url.searchParams.set('term', 'tacos');
  url.searchParams.set('latitude', latitude);
  url.searchParams.set('longitude', longitude);
  url.searchParams.set('limit', '20');
  url.searchParams.set('sort_by', 'rating'); // or 'distance', 'review_count'
  
  const response = await fetch(url, {
    headers: {
      'Authorization': `Bearer ${YELP_API_KEY}`
    }
  });
  
  const data = await response.json();
  return data.businesses;
}
```

The response includes everything you need:

```javascript
{
  "businesses": [
    {
      "id": "taqueria-el-buen-sabor-san-francisco",
      "name": "Taqueria El Buen Sabor",
      "rating": 4.5,
      "review_count": 892,
      "price": "$$",
      "phone": "+14155551234",
      "coordinates": {
        "latitude": 37.7749,
        "longitude": -122.4194
      },
      "location": {
        "address1": "123 Mission St",
        "city": "San Francisco",
        "state": "CA",
        "zip_code": "94103"
      },
      "categories": [
        { "alias": "mexican", "title": "Mexican" },
        { "alias": "tacos", "title": "Tacos" }
      ],
      "image_url": "https://s3-media2.fl.yelpcdn.com/...",
      "url": "https://www.yelp.com/biz/..."
    }
  ]
}
```

### Autocomplete for Better UX

The Yelp API also includes an autocomplete endpoint that's perfect for search boxes:

```javascript
async function autocompleteTacoSearch(text, latitude, longitude) {
  const url = new URL('https://api.yelp.com/v3/autocomplete');
  url.searchParams.set('text', text);
  url.searchParams.set('latitude', latitude);
  url.searchParams.set('longitude', longitude);
  
  const response = await fetch(url, { headers });
  const data = await response.json();
  
  return {
    terms: data.terms,      // e.g., ["tacos al pastor", "taco truck"]
    businesses: data.businesses,
    categories: data.categories
  };
}
```

This makes it super easy to build a search experience where users start typing "tac" and immediately see suggestions for "tacos", "taco truck", "tacos al pastor", and nearby taco restaurants.

### Getting Reviews and Details

Once users find a place they like, you can grab detailed info and reviews:

```javascript
async function getTacoSpotDetails(businessId) {
  // Get business details
  const detailsUrl = `https://api.yelp.com/v3/businesses/${businessId}`;
  const details = await fetch(detailsUrl, { headers }).then(r => r.json());
  
  // Get reviews (up to 3 excerpts)
  const reviewsUrl = `https://api.yelp.com/v3/businesses/${businessId}/reviews`;
  const reviews = await fetch(reviewsUrl, { headers }).then(r => r.json());
  
  return {
    ...details,
    reviews: reviews.reviews,
    hours: details.hours,
    photos: details.photos
  };
}
```

### Filtering for Delivery

During those late-night taco cravings when you don't want to leave the couch, you can search specifically for places that deliver:

```javascript
async function findTacosWithDelivery(latitude, longitude) {
  const url = `https://api.yelp.com/v3/transactions/delivery/search`;
  const params = new URLSearchParams({
    latitude,
    longitude,
    term: 'tacos',
    limit: '20'
  });
  
  const response = await fetch(`${url}?${params}`, { headers });
  return await response.json();
}
```

## Foursquare Places API - Another Solid Option

The [Foursquare Places API](https://docs.foursquare.com/developer/reference/places-api-overview) is another excellent choice, especially if you want detailed place attributes and check-in data.

### Authentication

Foursquare uses a simple API key authentication:

```javascript
const FOURSQUARE_API_KEY = process.env.FOURSQUARE_API_KEY;

const headers = {
  'Authorization': FOURSQUARE_API_KEY
};
```

### Searching for Tacos

Here's how to find taco spots with Foursquare:

```javascript
async function searchTacosFoursquare(latitude, longitude) {
  const url = new URL('https://api.foursquare.com/v3/places/search');
  url.searchParams.set('query', 'tacos');
  url.searchParams.set('ll', `${latitude},${longitude}`);
  url.searchParams.set('limit', '20');
  url.searchParams.set('categories', '13303'); // Mexican Restaurant category
  
  const response = await fetch(url, {
    headers: {
      'Authorization': FOURSQUARE_API_KEY
    }
  });
  
  const data = await response.json();
  return data.results;
}
```

Foursquare's response includes rich venue information:

```javascript
{
  "results": [
    {
      "fsq_id": "4b058a0cf964a520c1a522e3",
      "name": "Tacos El Gordo",
      "distance": 245,
      "location": {
        "address": "456 Valencia St",
        "locality": "San Francisco",
        "region": "CA",
        "postcode": "94110"
      },
      "categories": [
        {
          "id": 13303,
          "name": "Mexican Restaurant"
        }
      ],
      "rating": 8.5,
      "stats": {
        "total_photos": 127,
        "total_tips": 45
      }
    }
  ]
}
```

### Getting Place Tips

One cool feature Foursquare has is "tips" - short recommendations from users:

```javascript
async function getPlaceTips(fsqId) {
  const url = `https://api.foursquare.com/v3/places/${fsqId}/tips`;
  
  const response = await fetch(url, {
    headers: { 'Authorization': FOURSQUARE_API_KEY }
  });
  
  const data = await response.json();
  return data;
}
```

## Building a Complete Taco Finder

Let's put it all together into a simple taco-finding function:

```javascript
async function findBestTacos(latitude, longitude, options = {}) {
  const {
    delivery = false,
    maxDistance = 5000, // meters
    minRating = 4.0,
    priceLevel = null
  } = options;
  
  // Search for tacos
  let tacoSpots;
  
  if (delivery) {
    tacoSpots = await findTacosWithDelivery(latitude, longitude);
  } else {
    tacoSpots = await findTacosNearby(latitude, longitude);
  }
  
  // Filter by criteria
  const filtered = tacoSpots.filter(spot => {
    if (spot.rating < minRating) return false;
    if (spot.distance > maxDistance) return false;
    if (priceLevel && spot.price !== priceLevel) return false;
    return true;
  });
  
  // Sort by rating, then review count
  filtered.sort((a, b) => {
    if (b.rating !== a.rating) {
      return b.rating - a.rating;
    }
    return b.review_count - a.review_count;
  });
  
  return filtered;
}

// Usage
const bestTacos = await findBestTacos(37.7749, -122.4194, {
  delivery: false,
  maxDistance: 2000,
  minRating: 4.2,
  priceLevel: '$$'
});

console.log(`Found ${bestTacos.length} amazing taco spots!`);
```

## Pro Tips for Building Food Apps

Here are some lessons I've learned building with these APIs:

### Respect Rate Limits

Both Yelp and Foursquare have rate limits. Yelp's free tier allows 500 API calls per day. Implement caching to make your quota go further:

```javascript
const cache = new Map();
const CACHE_DURATION = 1000 * 60 * 15; // 15 minutes

async function cachedSearch(latitude, longitude) {
  const key = `${latitude},${longitude}`;
  const cached = cache.get(key);
  
  if (cached && Date.now() - cached.timestamp < CACHE_DURATION) {
    return cached.data;
  }
  
  const data = await findTacosNearby(latitude, longitude);
  cache.set(key, { data, timestamp: Date.now() });
  
  return data;
}
```

### Combine Multiple APIs

Don't put all your eggs in one basket. Use multiple APIs to get the best coverage:

```javascript
async function findTacosEverywhere(lat, lng) {
  const [yelpResults, foursquareResults] = await Promise.all([
    findTacosNearby(lat, lng),
    searchTacosFoursquare(lat, lng)
  ]);
  
  // Deduplicate and merge results
  return mergeAndDeduplicate(yelpResults, foursquareResults);
}
```

### Handle Errors Gracefully

APIs can fail. Always have a fallback:

```javascript
async function searchWithFallback(lat, lng) {
  try {
    return await findTacosNearby(lat, lng);
  } catch (error) {
    console.error('Yelp API failed:', error);
    
    try {
      return await searchTacosFoursquare(lat, lng);
    } catch (fallbackError) {
      console.error('All APIs failed:', fallbackError);
      return [];
    }
  }
}
```

### Display Attribution

Both APIs require proper attribution. Make sure to display:

- Yelp logo and "Powered by Yelp" text with links back to Yelp
- Foursquare logo and credit when using their data

Check their [display requirements](https://www.yelp.com/developers/display_requirements) and usage policies.

## What's Next?

Now that you know how to find tacos with APIs, here are some ideas to level up your taco-hunting app:

- **Add filters** - Vegetarian options, outdoor seating, late-night hours
- **Save favorites** - Let users bookmark their go-to taco spots
- **Route planning** - Create a taco crawl route visiting multiple places
- **Social features** - Share taco discoveries with friends
- **Photo uploads** - Let users contribute their own taco photos
- **Push notifications** - Alert users when they're near a highly-rated taco spot

## Wrapping Up

Building location-based apps is incredibly fun, and food discovery is one of the most satisfying use cases. Whether you choose Yelp, Foursquare, or both, these APIs give you everything you need to help people find amazing tacos (or any other food) in their area.

The best part? You're not just building an app - you're helping people discover their next favorite taco spot. And that's a noble mission.

Now if you'll excuse me, all this taco talk has made me hungry. Time to test my own app.

---

*Ready to start building? Sign up for [Yelp Places API](https://www.yelp.com/developers/v3/manage_app) or [Foursquare Places API](https://location.foursquare.com/developer/) and start hunting for tacos!*
