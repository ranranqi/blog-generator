---
title: Session
date: 2018-11-30 14:06:33
tags: http
---
# Session是什么？
 Session实质上就是一块内存，服务器通过Cookie给用户一个sessionID，然后sessionID对应服务器里的一小块内存，每次用户访问服务器时，服务器就通过sessionID去读取对应的那一块内存(即session)，然后就知道了用户的隐私信息如用户名ID，email账号等等。

  sessionID就是一个随机数，无法被篡改，因此用户的隐私得到了安全保证。

## Cookie与Session的关系
  session是基于cookie实现的，因为session必须将sessionID放在cookie里然后发送给用户。所以session是依赖于cookie的。
