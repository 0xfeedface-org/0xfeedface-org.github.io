---
layout: post
title: Drupal 7 Issues
created: 1324054049
---
Since upgrading to Drupal 7, I've had a couple of issues to solve. One issue I haven't solved, yet, is why RSS aggregators (like Google Reader) aren't being updated with new posts. I have cron set up to run every 15 minutes on my server. The RSS feed URL is the same. The RSS feed shows the new posts, but aggregators aren't picking up the new posts.

Let me know if there are any other issues I should be aware of.

<strong>UPDATE 2011-12-16</strong>: Google's reasoning for new posts not showing up: <cite>"if a feed reuses GUID tags, Reader will treat them as changes to the original item and not as distinct, new entries. You'll be able to find them by scrolling back in the feed to the original entry."</cite>
