<div align='center'>
  <h1> The Hot Spot (Celebrity) Problem </h1>
</div>

# About

This is a problem of high throughput that happens when there are too many requests to a given shard. For example, a viral video on YouTube, a trending topic on Twitter such as the Superbowl, or a public figure/celebrity profile on Instagram.

- Solution for a viral video: temporarily store the video in a CDN.
- Solution for Superbowl or profile page: provisioning an individual compute instance (to handle peak traffic) with a Hot Standby (to ensure high availability) and sufficient Read Capacity Units (to avoid Throttling), along with an autoscale feature (for automation).

A smart caching solution can detect the source of higher traffic and cache responses from there.
