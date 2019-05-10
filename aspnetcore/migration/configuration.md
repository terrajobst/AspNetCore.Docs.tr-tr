---
title: ASP.NET Core yapılandırma geçirme
author: ardalis
description: Yapılandırma, bir ASP.NET Core MVC projesini ASP.NET MVC projesinde geçirmeyi öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: migration/configuration
ms.openlocfilehash: c379f1f64dc5ab8aeb48055124e86e4e60d93785
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64900995"
---
# <a name="migrate-configuration-to-aspnet-core"></a>ASP.NET Core yapılandırma geçirme

Tarafından [Steve Smith](https://ardalis.com/) ve [Scott Addie](https://scottaddie.com)

Önceki makalede biz başlangıcından [bir ASP.NET MVC projesi için ASP.NET Core MVC geçirme](xref:migration/mvc). Bu makalede, biz yapılandırma geçirin.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/migration/configuration/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="setup-configuration"></a>Kurulum yapılandırması

ASP.NET Core bundan böyle *Global.asax* ve *web.config* ASP.NET'in önceki sürümlerinde kullanılan dosya. ASP.NET önceki sürümlerinde, uygulama başlangıç mantığı yerleştirildiği bir `Application_StartUp` yöntemi içinde *Global.asax*. Daha sonra ASP.NET MVC, bir *Startup.cs* dosyası; proje kök dizininde dahil ve uygulama başlatıldığında çağrılır. ASP.NET Core geliştirmiştir bu yaklaşım tamamen tüm başlangıç mantığı yerleştirerek *Startup.cs* dosya.

*Web.config* dosya içinde ASP.NET Core de değiştirilmiştir. Kendi yapılandırma artık yapılandırılabilir, açıklanan uygulaması başlangıç yordamını bir parçası olarak *Startup.cs*. Yapılandırma hala XML dosyalarını kullanmasına, ancak genellikle ASP.NET Core projeleri yapılandırma değerlerini JSON biçimli bir dosya içinde gibi yerleştireceğiniz *appsettings.json*. ASP.NET Core'nın yapılandırma sistemi sağlayabilen ortam değişkenlerini de bir kolayca erişebileceği bir [daha güvenli ve sağlam konumu](xref:security/app-secrets) ortama özgü değerler için. Bu, özellikle bağlantı dizelerini ve kaynak denetimine iade olmamalıdır API anahtarları gibi gizli dizileri için geçerlidir. Bkz: [yapılandırma](xref:fundamentals/configuration/index) ASP.NET core'da yapılandırma hakkında daha fazla bilgi edinmek için.

Bu makalede kısmen geçirilen ASP.NET Core projesinden ile başlıyoruz [önceki makalede](xref:migration/mvc). Yapılandırma kurulumu, aşağıdaki oluşturucusu ve özelliğini eklemek için *Startup.cs* proje kök dizininde bulunan dosya:

[!code-csharp[](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-16)]

Unutmayın, bu noktada, *Startup.cs* dosyasını olmaz derlemek, biz yine de aşağıdakileri eklemeniz gerektiği `using` deyimi:

```csharp
using Microsoft.Extensions.Configuration;
```

Ekleme bir *appsettings.json* uygun öğe şablonu kullanarak proje kök dosya:

![AppSettings JSON ekleyin](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a>Web.config dosyasından yapılandırma ayarlarını geçirme

ASP.NET MVC Projemizin gerekli veritabanı bağlantı dizesine dahil *web.config*, `<connectionStrings>` öğesi. ASP.NET Core Projemizin bu bilgileri depolamak için kullanacağız *appsettings.json* dosya. Açık *appsettings.json*, aşağıdakileri içerir, not edin:

[!code-json[](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]

Yukarıda gösterilen vurgulanan satırında, veritabanı adını değiştirin **_CHANGE_ME** veritabanınızın adı.

## <a name="summary"></a>Özet

ASP.NET Core tüm başlangıç mantıksal uygulama için bir tek, gerekli hizmetlerini ve bağımlılıklarını tanımlanan yapılandırılmış ve dosyasına yerleştirir. Değiştirir *web.config* dosyası ortam değişkenlerini yanı sıra JSON gibi dosya biçimlerinde çeşitli yararlanabilen esnek yapılandırma özelliği.
