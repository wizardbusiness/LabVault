```ad-ad-yaml

	# Web Application Configuration
	application:
	  name: MyWebApp
	  version: 1.2.3
	  port: 8080
	  debug: true
	
	database:
	  host: localhost
	  port: 5432
	  name: myapp_db
	  username: admin
	  password: secret123
	
	features:
	  - user_authentication
	  - file_upload
	  - email_notifications
	
	servers:
	  production:
	    url: https://prod.example.com
	    timeout: 30
	  staging:
	    url: https://staging.example.com
	    timeout: 60
	
	logging:
	  level: info
	  file: /var/log/myapp.log
```

#### **Key YAML features shown:**

- ##### **Indentation** (spaces, not tabs) shows hierarchy
- ##### **Nested structures** represent data relationships
- ##### **Colons** separate keys from values
- ##### **Hyphens** create lists
- ##### **Hash marks (#)** indicate comments
