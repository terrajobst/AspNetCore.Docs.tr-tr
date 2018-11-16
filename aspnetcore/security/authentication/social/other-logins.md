---
title: Dış OAuth kimlik doğrulama sağlayıcıları
author: rick-anderson
description: ASP.NET Core uygulamaları ile çalışan dış OAuth kimlik doğrulama sağlayıcıları keşfedin.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/otherlogins
ms.openlocfilehash: b69c366ec1bf12ccf434991fc8a79eaf8c09da3d
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708471"
---
# <a name="external-oauth-authentication-providers"></a>Dış OAuth kimlik doğrulama sağlayıcıları

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Pranav Rastogi'nin](https://github.com/rustd), ve [Valeriy Novytskyy](https://github.com/01binary)

Aşağıdaki liste, ASP.NET Core uygulamaları ile çalışan yaygın dış OAuth kimlik doğrulama sağlayıcılarını içerir. Tarafından korunan olanlar gibi üçüncü taraf NuGet paketlerini [aspnet contrib](https://www.nuget.org/packages?q=owners%3Aaspnet-contrib+title%3AOAuth), ASP.NET Core ekibi tarafından gerçekleştirilen kimlik doğrulama sağlayıcıları tamamlamak için kullanılabilir.

* [LinkedIn](https://www.linkedin.com/developer/apps) ([yönergeleri](https://developer.linkedin.com/docs/oauth2))

* [Instagram](https://www.instagram.com/developer/register/) ([yönergeleri](https://www.instagram.com/developer/authentication/))

* [Reddit](https://www.reddit.com/login?dest=https%3A%2F%2Fwww.reddit.com%2Fprefs%2Fapps) ([yönergeleri](https://github.com/reddit/reddit/wiki/OAuth2-Quick-Start-Example))

* [Github](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fsettings%2Fapplications%2Fnew) ([yönergeleri](https://developer.github.com/v3/oauth/))

* [Yahoo](https://login.yahoo.com/config/login?src=devnet&.done=http%3A%2F%2Fdeveloper.yahoo.com%2Fapps%2Fcreate%2F) ([yönergeleri](https://developer.yahoo.com/bbauth/user.html))

* [Tumblr](https://www.tumblr.com/oauth/apps) ([yönergeleri](https://www.tumblr.com/docs/api/v2#auth))

* [Pinterest](https://www.pinterest.com/login/?next=http%3A%2F%2Fdevsite%2Fapps%2F) ([yönergeleri](https://developers.pinterest.com/docs/api/overview/?))

* [Pocket](https://getpocket.com/developer/apps/new) ([yönergeleri](https://getpocket.com/developer/docs/authentication))

* [Flickr](https://www.flickr.com/services/apps/create) ([yönergeleri](https://www.flickr.com/services/api/auth.oauth.html))

* [Dribble](https://dribbble.com/signup) ([yönergeleri](http://developer.dribbble.com/v1/oauth/))

* [Vimeo](https://vimeo.com/join) ([yönergeleri](https://developer.vimeo.com/api/authentication))

* [SoundCloud](https://soundcloud.com/you/apps/new) ([yönergeleri](https://developers.soundcloud.com/blog/we-love-oauth-2))

* [VK](https://vk.com/apps?act=manage) ([yönergeleri](https://vk.com/pages?oid=-17680044&p=Authorizing_Sites))

[!INCLUDE[Multiple authentication providers](includes/chain-auth-providers.md)]

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]
