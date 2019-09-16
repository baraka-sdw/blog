---
title: "Movie"
date: 2019-09-16T12:35:20+08:00
draft: false
---
111111
<head>
  <meta name="referrer" content="never">
   <link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/iMuFeng/bmdb@1.2.1/dist/Bmdb.min.css">
  <script src="https://cdn.jsdelivr.net/npm/jquery@3.3.1/dist/jquery.min.js"></script>
  <script src="https://cdn.jsdelivr.net/gh/iMuFeng/bmdb@1.2.1/dist/Bmdb.min.js" />
</head>
<script>
new Bmdb({
  type: 'movies',
  selector: '.container',
  secret: '7bf4205a0a62d00409f3cd70b0736e1a11a9a6a60f7231567f056819787b8096',
  noMoreText: '没有更多数据了',
  limit: 30
})
</script>