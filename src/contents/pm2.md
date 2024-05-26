---
author: kuldeep
datetime: 2022-12-21
title: Package manager
slug: pm2
featured: false
draft: false
tags:
  - pm2
ogImage: ""
description: PM2 is a daemon process manager that will help you manage and keep your application online.
---

# PM2
PM2 is a powerful process manager for Node.js applications. It allows you to keep applications alive forever, reload them without downtime, facilitate common system admin tasks, and much more. Here's a comprehensive list of PM2 commands and their descriptions:

## Ecosystem file
```json
module.exports = {
  apps: [
    {
      name: "portal1",
      script: "server.js",
      instances: 1,
      autorestart: true,
      watch: true,
      max_memory_restart: "100M"
    },
	 {
      name: "portal2",
      script: "server.js",
      instances: 1,
      autorestart: true,
      watch: true,
      max_memory_restart: "100M"
    },
  ]
}
```

## Starting, Stopping, and Restarting Applications

- **Start an application**: `pm2 start app.js`
- **Start an application in cluster mode**: `pm2 start app.js -i max`
- **Start an application with a specific name**: `pm2 start app.js --name my-app`
- **Start an application with environment variables**: `pm2 start app.js --env production`
- **Start an application with a custom configuration file**: `pm2 start app.js --config pm2.config.js`
- **Stop an application**: `pm2 stop app_name`
- **Stop all applications**: `pm2 stop all`
- **Restart an application**: `pm2 restart app_name`
- **Restart all applications**: `pm2 restart all`
- **Reload an application**: `pm2 reload app_name`
- **Reload all applications**: `pm2 reload all`
- **Delete an application from PM2's process list**: `pm2 delete app_name`
- **Delete all applications from PM2's process list**: `pm2 delete all`

## Managing Application Logs

- **Show application logs**: `pm2 logs`
- **Show application logs for a specific application**: `pm2 logs app_name`
- **Show application error logs**: `pm2 logs --err`
- **Show application error logs for a specific application**: `pm2 logs app_name --err`
- **Flush logs**: `pm2 flush`

## Monitoring and Management

- **List all applications managed by PM2**: `pm2 list`
- **Show application details**: `pm2 show app_name`
- **Monitor CPU and memory usage of applications**: `pm2 monit`
- **Dump PM2 process list**: `pm2 dump`
- **Resurrect previously dumped processes**: `pm2 resurrect`
- **Save the current process list**: `pm2 save`
- **Generate a startup script to keep PM2 and your applications running**: `pm2 startup`
- **Startup script for systems using systemd**: `pm2 startup systemd`
- **Startup script for systems using launchd**: `pm2 startup launchd`
- **Startup script for systems using openrc**: `pm2 startup openrc`
- **Startup script for systems using upstart**: `pm2 startup upstart`
- **Startup script for systems using freebsd**: `pm2 startup freebsd`
- **Startup script for systems using systemv**: `pm2 startup systemv`
- **Startup script for systems using rcd**: `pm2 startup rcd`
- **Startup script for systems using no-daemon**: `pm2 startup no-daemon`
- **Startup script for systems using custom**: `pm2 startup custom`

## Cluster Mode

- **Start an application in cluster mode**: `pm2 start app.js -i max`
- **Start an application in cluster mode with a specific number of instances**: `pm2 start app.js -i 4`

## Ecosystem File

- **Start applications defined in an ecosystem file**: `pm2 start ecosystem.config.js`
- **Start applications defined in an ecosystem file in watch mode**: `pm2 start ecosystem.config.js --watch`

## Keymetrics Integration

- **Generate a public key for Keymetrics**: `pm2 set pm2:secret_key`
- **Link your PM2 instance to your Keymetrics account**: `pm2 link`
- **Unlink your PM2 instance from your Keymetrics account**: `pm2 unlink`

### Advanced Commands

- **Update PM2 to the latest version**: `pm2 update`
- **Update PM2 to a specific version**: `pm2 update version`
- **Reset PM2's configuration**: `pm2 reset`
- **Kill PM2 daemon**: `pm2 kill`
- **Gracefully stop PM2 daemon**: `pm2 gracefulReload`
- **Reload PM2 daemon**: `pm2 reload`
- **Show PM2 version**: `pm2 --version`

These commands cover a wide range of functionalities provided by PM2, from basic process management to advanced features like cluster mode, ecosystem file management, and integration with Keymetrics for monitoring and management.
