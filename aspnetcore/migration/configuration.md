---
title: "Geçirme yapılandırma"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/configuration
ms.openlocfilehash: 51665059d1d803cbe57bc9a884a0e91eac9e7cb4
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="migrating-configuration"></a>Geçirme yapılandırma

Tarafından [Steve Smith](https://ardalis.com/) ve [Scott Addie](https://scottaddie.com)

Önceki makalede, biz başlangıcından [ASP.NET Core MVC için ASP.NET MVC projesinde geçirme](mvc.md). Bu makalede, yapılandırmasını geçir.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="setup-configuration"></a>Kurulum yapılandırma

ASP.NET Core artık kullanan *Global.asax* ve *web.config* ASP.NET önceki sürümlerinde kullanılan dosyaları. Uygulama başlangıç mantığı yerleştirilen ASP.NET önceki sürümlerde bir `Application_StartUp` yöntemi içinde *Global.asax*. Daha sonra ASP.NET MVC, bir *haline* dosya; proje kök dizininde dahil ve uygulama başlatıldığında çağrıldı. ASP.NET Core geliştirmiştir bu yaklaşım tamamen tüm başlangıç mantığı koyarak *haline* dosya.

*Web.config* dosya ASP.NET Core de değiştirilmiştir. Yapılandırma kendisini artık yapılandırılabilir, açıklanan uygulaması başlangıç yordamını bir parçası olarak *haline*. Yapılandırma hala XML dosyalarını kullanan, ancak genellikle ASP.NET Core projeleri yapılandırma değerlerini bir JSON biçimli dosyasında gibi yerleştirir *appsettings.json*. ASP.NET Core'nın yapılandırma sistemi ortama özgü değerleri için daha güvenli ve sağlam bir konum sağlayabilir ortam değişkenlerini de kolayca erişebilirsiniz. Bu, özellikle bağlantı dizeleri ve kaynak denetimine iade döndürmemelidir API anahtarları gibi gizli anahtarları için geçerlidir. Bkz: [yapılandırma](xref:fundamentals/configuration/index) ASP.NET çekirdek yapılandırması hakkında daha fazla bilgi edinmek için.

Bu makalede, biz kısmen geçirilen ASP.NET Core projeden ile başlıyorsanız [önceki makaleyi](mvc.md). Yapılandırma kurulumu, aşağıdaki oluşturucusu ve özelliğini eklemek için *haline* proje kök dizininde bulunan dosyası:

[!code-csharp[Main](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-21)]

Unutmayın, bu noktada, *haline* dosyasını olmaz derleyin, biz aşağıdaki eklemek gerek duyduğunuz `using` deyimi:

```csharp
using Microsoft.Extensions.Configuration;
```

Ekleme bir *appsettings.json* uygun öğeyi şablonunu kullanarak projenin kök dosyaya:

![AppSettings JSON ekleme](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a>Web.config yapılandırma ayarlarını geçirme

ASP.NET MVC Projemizin gerekli veritabanı bağlantı dizesine dahil *web.config*, `<connectionStrings>` öğesi. ASP.NET Core Projemizin Biz bu bilgileri depolamak için kalacaklarını *appsettings.json* dosya. Açık *appsettings.json*, aşağıdakileri içeren, not edin:

[!code-json[Main](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]


Vurgulanan satırda, yukarıda gösterilen veritabanından adını değiştirmek **_CHANGE_ME** veritabanınızın adını.

## <a name="summary"></a>Özet

ASP.NET Core tüm başlangıç mantığı uygulama için bir tek, bağımlılıklar ve gerekli hizmetleri tanımlanan yapılandırılmış ve dosyasına yerleştirir. Yerini *web.config* dosya bir esnek yapılandırma özelliğiyle dosya biçimlerinde, ortam değişkenleri yanı sıra, JSON gibi çeşitli yararlanabilirsiniz.
