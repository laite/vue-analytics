[![npm version](https://badge.fury.io/js/vue-analytics.svg)](https://badge.fury.io/js/vue-analytics) [![npm](https://img.shields.io/npm/dt/vue-analytics.svg)](https://www.npmjs.com/package/vue-analytics)


# vue-analytics
Vue plugin to handle basic Google Analytics tracking: pages and events.

Here the documentation about [pageview](https://developers.google.com/analytics/devguides/collection/analyticsjs/pages) and [events](https://developers.google.com/analytics/devguides/collection/analyticsjs/events)

## Installation

```shell
npm install vue-analytics
```

## Usage

```js
import Vue from 'vue'
import VueAnalytics from 'vue-analytics'

// your Google Analytcs domain ID
const id = 'UA-XXX-X'

Vue.use(VueAnalytics, { id })

```


## Tracking methods

It's only possible to track events and pages.

`trackEvent` and `trackPage` methods are available in the Vue instance 

```js
/**
 * Page tracking
 * @param  {String} page
 * @param  {String} title
 * @param  {String} location
 */
Vue.$ga.trackPage('/home')

/**
 * Event tracking
 * @param  {String} category
 * @param  {String} action
 * @param  {String} [label='']
 * @param  {Number} [value=0]
 */
Vue.$ga.trackEvent('share', 'click', 'facebook')
```

and also in the component scope itself

```js
export default {	
	mounted () {
		this.$ga.trackPage('/home')
	},
	
	methods: {
		onShareButtonClick () {
			this.$ga.trackEvent('share', 'click', 'facebook')
		}
	}
}
```

## Auto-tracking

Auto-tracking is enabled by default and it will load the Google Analytics script and start tracking every route change.

To be able to work properly the route object needs to have a `name` and a `path`

```js
import Vue from 'vue'
import VueAnalytics from 'vue-analytics'

const router = new VueRouter({
	routes: [
		{
			name: 'home',
			path: '/',
			component: {
				template: '<div>home page!</div>'
			}
		},
		{
			name: 'about',
			path: '/about',
			component: {
				template: '<div>about page!</div>'
			}
		}
	]
})

// your Google Analytcs domain ID
const id = 'UA-XXX-X'

Vue.use(VueAnalytics, { id, router })

```

**If you only need to track your routes, this is everything you need to do!**

#### Disable auto-tracking

```js
Vue.use(VueAnalytics, {
	id: 'UA-XXX-X',
	autoTracking: false
})
```

#### Ignore routes

Auto-tracking tracks every route in you router instance, but if needed, it's possible to pass an array of route names that we don't want to track


```js
Vue.use(VueAnalytics, {
	id: 'UA-XXX-X',
	router: router
	ignoreRoutes: ['home']
})
```

## Go manual!

You can disable auto-tracking and the auto-loading of the Google Analytics script just setting `manual` to `true`

```js
Vue.use(VueAnalytics, {
	manual: true
})
```

With this setup, the plugin only exposes the tracking methods, so you need to manually load the script tag from Google, attach the domain ID to the Google snippet and tracking pages manually.

I don't know why you want to do this...but you can!

## Debug

There is already a [Google Analytics extension](https://chrome.google.com/webstore/detail/google-analytics-debugger/jnkmfdileelhofjcijamephohjechhna) for Chrome that allows you to read what's going on.
This logger will just tell you what type of tracking is fired and which parameters.
Enable/disable logs. Default value is false.

```js
Vue.use(VueAnalytics, { 
	debug: true 
})
```

# Issues and features requests
Please drop an issue, if you find something that doesn't work, or a feature request at https://github.com/MatteoGabriele/vue-analytics/issues

Follow me on twitter [@matteo_gabriele](https://twitter.com/matteo_gabriele)
