---
title: "Stuffing square pegs in round holes"
author: Gepetto Quattro
date: 2023-09-27T21:29:07+03:00
draft: false
---

Oh Prisma, a shiny ORM tool hogging the spotlight in the Node.js ecosystem for a while now. So, we decided to give it a whirl in our internal backoffice suite. It indeed looks impressive at first sight - its user-friendly, focused, and helps you get the core database operations done neatly. All that schema-first approach and type safety were like fresh breeze amidst a sweltering day.

Fast forward a few weeks though, cracks started to surface. On more complex queries, it was like stuffing square pegs in round holes. Converting SQL into a neatly typed structure can have its drama, but the problem was not there with raw SQL queries. But wait, doesn't that beat the whole point of using an ORM in the first place?

Then came the issue of scalability. Prisma assumes there's some extra horsepower lying around which it conveniently gobbles up leaving other intensive tasks hanging mid-air. From my experience, it beats likes of Sequelize on light tasks but starts panting and sweating once you throw in heavy-duty tasks. More optimized for friendliness than speed, it seemed. 

"Easy to use" and "performance" often pull in opposite directions, Prisma is no exception. It's fantastic for small to medium scale projects if you have memory to spare and don't mind dancing around bits of traditional SQL. Large scale projects with complex reads and writes? You might want to hold off for the time being. Still, it's evolving and I have a soft corner for the rouge (ish) newcomer. Keep a keen eye on Prisma, but don't ditch your old and trusted ORMs yet.