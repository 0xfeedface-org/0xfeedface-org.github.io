---
layout: post
title: Scanning for Vulnerabilities
created: 1326746019
---
Security is priority for Wayfair. We follow strict coding standards to ensure secure coding practices. Even with all the precautions Wayfair engineers take, mistakes are made. We're humans, after all. I've had the fun task of creating a handy web interface our developers can use to run automated vulnerability scans on their code.

Drupal provides an extensible, easy-to-use framework. I decided to write my interface as a Drupal 7 module.

The module is easy to use. After installing the module, you set certain configuration variables and you add servers and sites. We use haproxy and nginx to do load balancing. The servers are the individual servers configured in haproxy and get passed as a cookie to the client's browser. The site is the site you want to scan. You will need to create a scan group--a group of servers. I created the concept of "scan groups" in case a developer needs to test the same code on multiple servers.

Once you have your servers and server groups configured, you assign each respective server group to each developer. The developer can now run scans against his or her server.

The <a href="https://github.com/wayfair/drupalsec/tree/dev" target="_blank">project</a> is located at <a href="https://github.com/wayfair" target="_blank">Wayfair's</a> public GitHub account. Feel free to clone it, fork it, and contribute to it. The project uses Skipfish as the scanner, but can be easily extended to support more scanners.
