---
title: "How to Setup Slack and Drone Slack Blame"
date: 2020-11-22T23:24:20+08:00
categories: ['drone']
tags: ['slack', 'drone', 'drone-slack-blame']
description: "How to create a Slack App and use it with the drone-slack-blame plugin after Slack deprecated legacy tokens, including OAuth scope setup and Drone configuration."
---

Slack now has deprecated legacy tokens, instead, Slack encourages us to create [`Slack Apps`](https://api.slack.com/apps) to do our job, so I created a note to record how I set up Slack app and use it on [`drone-slack-blame`](http://plugins.drone.io/drone-plugins/drone-slack-blame/) plugin.



1. Create a app: https://api.slack.com/apps?new_app=1

	> ![image](/images/droplr/76PLxe.png)
	
2. Add features and functionality

	> ![](/images/droplr/dikQXb.png)

	> ![](/images/droplr/T4fYcK.png)

	Edit App display name

	> ![](/images/droplr/Yu2jv6.png)

3. Go to OAuth & Permission, then edit the scope of bots:

	> ![](/images/droplr/ymdhsQ.png)
   
	What I need is to make Drone (drone-slack-blame) to notify the users with the build status, and send messages to channels as well, so I need `users:read` permission to map Github users and Slack users (you can set `mapping` in drone-slack-blame section in drone.yml), and Drone Bot requires `chat:write` to send private messages, and `chat:write:public` to send messages to public channels.

4. Install App to workspace

	> ![](/images/droplr/uU9Zuc.png)

	then you should see `Bot User OAuth Access Token` is present.

5. Now you can use the token in Drone, here is the example of my drone setting:

	<script src="https://gist.github.com/kerkerj/c70eacccf813018756a67859e0249b83.js"></script>

	Remember to set user mappings, first argument is your GitHub name, and the second one is Slack display name. You can check more in the [source code](https://github.com/drone-plugins/drone-slack-blame).

6. How it works?

	The image below is for private messages:

	> ![](/images/droplr/AvXeIw.png)

	And it sends message to a public channel as well:

	> ![](/images/droplr/e8DRl6.png)


Last but not least, you can check https://api.slack.com/apps/ to configure your Slack app, I have to say that Slack administration pages are like a huge maze... it always takes me some time to find out what I want.