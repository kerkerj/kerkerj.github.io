---
title: "How to Setup Slack and Drone Slack Blame"
date: 2020-11-22T23:24:20+08:00
categories: ['drone']
tags: ['slack', 'drone', 'drone-slack-blame']
keywords: ['slack', 'drone', 'drone-slack-blame']
clearReading: true
comments: true
showTags: true
showPagination: true
showSocial: true
showDate: true
showMeta: true
showActions: true
#disqusIdentifier: ""
#thumbnailImage: image-1.png
#thumbnailImagePosition: bottom
#autoThumbnailImage: yes
#metaAlignment: center
#coverImage: image-2.png
#coverCaption: "A beautiful sunrise"
#coverMeta: out
#coverSize: full
#coverImage: image-2.png
#gallery:
#    - image-3.jpg "New York"
#    - image-4.png "Paris"
#    - http://i.imgur.com/o9r19kD.jpg "Dubai"
#    - https://example.com/orignal.jpg https://example.com/thumbnail.jpg "Sidney"
---

Slack now has deprecated legacy tokens, instead, Slack encourages us to create [`Slack Apps`](https://api.slack.com/apps) to do our job, so I created a note to record how I set up Slack app and use it on [`drone-slack-blame`](http://plugins.drone.io/drone-plugins/drone-slack-blame/) plugin.

<!--more-->


1. Create a app: https://api.slack.com/apps?new_app=1

	> ![image](http://d.kerkerj.in/76PLxe+)
	
2. Add features and functionality

	> ![](http://d.kerkerj.in/dikQXb+)

	> ![](http://d.kerkerj.in/T4fYcK+)

	Edit App display name

	> ![](http://d.kerkerj.in/Yu2jv6+)

3. Go to OAuth & Permission, then edit the scope of bots:

	> ![](http://d.kerkerj.in/ymdhsQ+)
   
	What I need is to make Drone (drone-slack-blame) to notify the users with the build status, and send messages to channels as well, so I need `users:read` permission to map Github users and Slack users (you can set `mapping` in drone-slack-blame section in drone.yml), and Drone Bot requires `chat:write` to send private messages, and `chat:write:public` to send messages to public channels.

4. Install App to workspace

	> ![](http://d.kerkerj.in/uU9Zuc+)

	then you should see `Bot User OAuth Access Token` is present.

5. Now you can use the token in Drone, here is the example of my drone setting:

	<script src="https://gist.github.com/kerkerj/c70eacccf813018756a67859e0249b83.js"></script>

	Remember to set user mappings, first argument is your GitHub name, and the second one is Slack display name. You can check more in the [source code](https://github.com/drone-plugins/drone-slack-blame).

6. How it works?

	The image below is for private messages:

	> ![](http://d.kerkerj.in/AvXeIw+)

	And it sends message to a public channel as well:

	> ![](http://d.kerkerj.in/e8DRl6+)


Last but not least, you can check https://api.slack.com/apps/ to configure your Slack app, I have to say that Slack administration pages are like a huge maze... it always takes me some time to find out what I want.