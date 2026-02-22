---
title: '[Codeigniter] Tips deploy to Amazon Web Services'
description: "Tips for deploying CodeIgniter to AWS: disable pconnect when using RDS to avoid exceeding connection limits, note that RDS timezone defaults to UTC, and considerations when using a Load Balancer."
date: 2014-05-28
categories: 
---


1. If you use Amazon RDS, especially micro plan,  
	remember to set `$config['pconnect'] = FALSE` in `application/config/database.php`.  
  	Because RDS has Max_connection limits.  
2. If you use RDS, remeber that the system timezone in RDS is +0 timezone.  
	In your application, you should care about this thing.  
3. If you will use loadbalancer, remember to ....(???)  
