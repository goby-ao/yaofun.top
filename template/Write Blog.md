---
date: <% tp.file.creation_date("YYYY-MM-DD") %>
title: <% tp.file.title %>
draft: true
tags: ['<% tp.system.suggester(item => item, Object.keys(app.metadataCache.getTags()).map(x => x.replace("#", ""))) %>']
---
