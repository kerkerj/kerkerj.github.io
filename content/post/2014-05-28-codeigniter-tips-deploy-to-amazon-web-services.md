---
title: '[Codeigniter] Tips deploy to Amazon Web Services'
description: "CodeIgniter 部署到 AWS 的注意事項：使用 RDS 需關閉 pconnect 持久連線避免超過連線上限、注意 RDS 時區為 UTC，以及使用 Load Balancer 的考量。"
date: 2014-05-28
categories: 
---


1. If you use Amazon RDS, especially micro plan,  
	remember to set `$config['pconnect'] = FALSE` in `application/config/database.php`.  
  	Because RDS has Max_connection limits.  
2. If you use RDS, remeber that the system timezone in RDS is +0 timezone.  
	In your application, you should care about this thing.  
3. If you will use loadbalancer, remember to ....(???)  
