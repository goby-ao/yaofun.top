---
date: <% tp.file.creation_date() %>
title: <% tp.file.title %>
tags: <% tp.system.suggester(item => item, Object.keys(app.metadataCache.getTags()).map(x => x.replace("#", ""))) %>
---
