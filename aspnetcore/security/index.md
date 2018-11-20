---
title: ASP.NET Core güvenliğine genel bakış
author: tdykstra
description: ASP.NET Core kimlik doğrulaması, yetkilendirme ve güvenlik temel bilgileri öğrenin.
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/24/2018
uid: security/index
ms.openlocfilehash: 579e472e01efd08bbafe949e37a3b655a42a5b46
ms.sourcegitcommit: 04b55a5ce9d649ff2df926157ec28ae47afe79e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52156925"
---
# <a name="overview-of-aspnet-core-security"></a>ASP.NET Core güvenliğine genel bakış

ASP.NET Core geliştiricilerinin kolayca yapılandırmak ve uygulamalarını için güvenliği yönetmek etkinleştirir. ASP.NET Core kimlik doğrulaması, yetkilendirme, veri koruma, SSL zorlama, uygulama gizli anahtarlarının, istek sahteciliğinden koruma koruması ve CORS yönetim yönetmeye yönelik özellikler içerir. Bu güvenlik özellikleri, güçlü derleme henüz ASP.NET Core uygulamalarının güvenliğini sağlama imkan tanır.

## <a name="aspnet-core-security-features"></a>ASP.NET Core güvenlik özellikleri

Birçok araca ve kitaplığa yerleşik kimlik sağlayıcıları dahil olmak üzere uygulamalarınızın güvenliğini sağlamak için ASP.NET Core sağlar ancak Facebook, Twitter ve LinkedIn gibi 3. taraf kimlik Hizmetleri kullanabilirsiniz. ASP.NET Core ile bir şekilde depolayın ve kod göstermek zorunda kalmadan gizli bilgiler kullanın olan, uygulama gizli anahtarlarının kolayca yönetebilirsiniz.

## <a name="authentication-vs-authorization"></a>Kimlik doğrulaması vs. Yetkilendirme

Kimlik doğrulaması, bir kullanıcı bu bir işletim sistemi, veritabanı, uygulama veya kaynak depolanan karşılaştırılır kimlik bilgilerini sağlayan bir işlemdir. Eşleşirlerse, kullanıcıların kimliğini başarıyla doğrulamak ve, sahip olduğunuz eylemler gerçekleştirebilir bir yetkilendirme sürecinde. Yetkilendirme, bir kullanıcı yapmak için izin verilenden belirleyen işlemini gösterir.

Kimlik doğrulaması düşünmek için başka bir yetkilendirme (sunucu, veritabanı veya uygulama) bu alanı içinde hangi nesneleri için kullanıcının gerçekleştirebileceği eylemleri olsa da sunucu, veritabanı, uygulama veya kaynağa gibi bir alan girin. bir yolu olarak dikkate alınması gereken yoludur.

## <a name="common-vulnerabilities-in-software"></a>Ortak yazılımdaki

ASP.NET Core ve EF yardımcı olan özellikler içeriyor uygulamanızı güvenli hale getirme ve güvenlik ihlallerini önleyin. Aşağıdaki listede yer alan bağlantıları, web uygulamalarında en yaygın güvenlik açıklarından önlemek için yapılan teknikleri belgeleri alır:

* [Siteler arası betik saldırıları](xref:security/cross-site-scripting)
* [SQL ekleme saldırıları](/ef/core/querying/raw-sql)
* [Siteler arası istek sahteciliği (CSRF)](xref:security/anti-request-forgery)
* [Açık yeniden yönlendirme saldırılarını](xref:security/preventing-open-redirects)

Bilmeniz gereken daha fazla güvenlik açıkları vardır. Daha fazla bilgi için bkz: diğer makaleleri **güvenlik ve kimlik** İçindekiler bölümünde.
