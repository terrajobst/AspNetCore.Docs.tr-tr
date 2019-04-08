---
title: ASP.NET core'da ASP.NET machineKey değiştirin
author: rick-anderson
description: Yeni ve daha güvenli veri koruma sisteminde kullanımına olanak tanımak için ASP.NET machineKey nasıl değiştirileceğini keşfedin.
ms.author: riande
ms.date: 04/06/2019
uid: security/data-protection/compatibility/replacing-machinekey
ms.openlocfilehash: ff36382d22a218a228b42a31ae4f8ad2eb2d5b5f
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068290"
---
# <a name="replace-the-aspnet-machinekey-in-aspnet-core"></a>ASP.NET core'da ASP.NET machineKey değiştirin

<a name="compatibility-replacing-machinekey"></a>

Uygulamasını `<machineKey>` ASP.NET öğesinde [değiştirilebilen](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/). Bu, ASP.NET şifreleme rutinleri için çoğu çağrıları yeni veri koruma sistemi dahil olmak üzere bir değiştirme veri koruma mekanizması yönlendirilmesini sağlar.

## <a name="package-installation"></a>Paket yüklemesi

> [!NOTE]
> Yeni veri koruma sisteminde yalnızca .NET 4.5.1 targeting mevcut bir ASP.NET uygulamasına veya üzeri yüklenebilir. Yükleme uygulaması, .NET 4.5 hedefliyse başarısız veya azaltın.

Yeni veri koruma sisteminde mevcut bir ASP.NET 4.5.1+ projeye yüklemek için ' % s'paketi Microsoft.AspNetCore.DataProtection.SystemWeb yükleyin. Veri koruma sistemi kullanarak örneği oluşturulmaz [varsayılan yapılandırmayı](xref:security/data-protection/configuration/default-settings) ayarları.

Paket yükleme sırasında bir satıra ekler. *Web.config* ASP.NET için kullanılacağını söyler [en şifreleme işlemleri](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/)form kimlik doğrulaması, Görünüm durumu ve çağrılar dahil olmak üzere MachineKey.Protect. Eklenen satırın aşağıdaki gibidir.

```xml
<machineKey compatibilityMode="Framework45" dataProtectorType="..." />
```

>[!TIP]
> Yeni veri koruma sisteminde gibi alanlarını inceleyerek etkin olup olmadığını söyleyebilir `__VIEWSTATE`, aşağıdaki örnekte olduğu gibi "CfDJ8" ile başlatılması. "CfDJ8" Sihirli "09 F0 C9 F0" üstbilgisinin veri koruma sistemi tarafından korunan bir yükü tanımlayan base64 gösterimidir.

```html
<input type="hidden" name="__VIEWSTATE" id="__VIEWSTATE" value="CfDJ8AWPr2EQPTBGs3L2GCZOpk..." />
```

## <a name="package-configuration"></a>Paketi yapılandırması

Veri koruma sisteminde varsayılan sıfır kurulum yapılandırma ile oluşturulur. Varsayılan olarak yerel dosya sistemine anahtarları kalıcı olduğundan, ancak bu bir sunucu grubunda dağıtılan uygulamalar için çalışmaz. Bu sorunu çözmek için alt DataProtectionStartup bir tür oluşturarak yapılandırması sağlayabilir ve onun Createservicereplicalisteners() yöntemini geçersiz kılar.

Burada anahtarları kalıcı hem de bekleme sırasında nasıl şifrelenmeden yapılandırılan özel veri koruma başlangıç türü örneği aşağıda verilmiştir. Ayrıca varsayılan uygulama yalıtımı İlkesi, kendi uygulama adı sağlayarak geçersiz kılar.

```csharp
using System;
using System.IO;
using Microsoft.AspNetCore.DataProtection;
using Microsoft.AspNetCore.DataProtection.SystemWeb;
using Microsoft.Extensions.DependencyInjection;

namespace DataProtectionDemo
{
    public class MyDataProtectionStartup : DataProtectionStartup
    {
        public override void ConfigureServices(IServiceCollection services)
        {
            services.AddDataProtection()
                .SetApplicationName("my-app")
                .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\myapp-keys\"))
                .ProtectKeysWithCertificate("thumbprint");
        }
    }
}
```

>[!TIP]
> Ayrıca `<machineKey applicationName="my-app" ... />` SetApplicationName açık çağrı yerine. Tüm yapılandırma istedikleri ayarlanması, uygulama adı DataProtectionStartup türetilmiş bir tür oluşturmak için geliştirici zorlama önlemek için bir kolaylık mekanizması budur.

Bu özel yapılandırmasını etkinleştirmek için Web.config dosyasına geri dönün ve Ara `<appSettings>` paket yükleme yapılandırma dosyasına eklenen öğe. Bunu, aşağıdaki biçimlendirme gibi görünür:

```xml
<appSettings>
  <!--
  If you want to customize the behavior of the ASP.NET Core Data Protection stack, set the
  "aspnet:dataProtectionStartupType" switch below to be the fully-qualified name of a
  type which subclasses Microsoft.AspNetCore.DataProtection.SystemWeb.DataProtectionStartup.
  -->
  <add key="aspnet:dataProtectionStartupType" value="" />
</appSettings>
```

Yeni oluşturduğunuz DataProtectionStartup türetilen tür bütünleştirilmiş kodla nitelenen adı boş değeriyle doldurun. Uygulamanın adını DataProtectionDemo ise, bu gibi görünür aşağıda.

```xml
<add key="aspnet:dataProtectionStartupType"
     value="DataProtectionDemo.MyDataProtectionStartup, DataProtectionDemo" />
```

Yeni yapılandırılmış veri koruma sisteminde artık uygulama içinde kullanılmaya hazırdır.
