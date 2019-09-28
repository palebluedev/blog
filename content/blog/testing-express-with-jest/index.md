---
title: Lessons learned whilst creating this blog.
date: "2019-09-28T00:00:00.001Z"
description: "A list of lessons learned"
---
 - Test in another browser (a bug it seems is causing brave to endlessly refresh in develop)
 - When setting up a project always declare somewhere the node version that is going to be used
 - Github pages will only deploy your pages when a new push has been made to github pages, so if you were pointing at master /docs and changed it in settings to gh-pages, you'll need to do another push for the changes to be visible. Otherwise you'll just get a 404.