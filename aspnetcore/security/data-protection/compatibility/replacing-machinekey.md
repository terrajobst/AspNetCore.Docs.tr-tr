---
title: "Değiştirme `<machineKey>` ASP.NET"
author: rick-anderson
description: "Değiştirme `<machineKey>` ASP.NET"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/compatibility/replacing-machinekey
ms.openlocfilehash: 865c949a526e07d41ac4627fdf0411d5422adc16
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="replacing-machinekey-in-aspnet"></a>Değiştirme `<machineKey>` ASP.NET

<a name="compatibility-replacing-machinekey"></a>

Uygulaması `<machineKey>` ASP.NET öğesinde [değiştirilebilen](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/). Bu yeni veri koruma sisteminde dahil olmak üzere bir değiştirme veri koruma mekanizması yönlendirilecek çoğu çağrıları ASP.NET şifreleme yordamlar sağlar.

## <a name="package-installation"></a>Paket yükleme

> [!NOTE]
> Yeni veri koruma sisteminde yalnızca .NET 4.5.1'in hedefleme mevcut bir ASP.NET uygulamasına veya daha yüksek yüklenebilir. Yükleme uygulamayı .NET 4.5 hedefliyorsa başarısız veya azaltın.

Varolan bir ASP.NET 4.5.1+ projeye yeni veri koruma sistemi yüklemek için Microsoft.AspNetCore.DataProtection.SystemWeb paketini yükleyin. Bu veri koruması sistem kullanarak örneği oluşturulmaz [varsayılan yapılandırması](xref:security/data-protection/configuration/default-settings) ayarlar.

Paketini yüklediğinizde, bir satıra ekler *Web.config* kullanmak için ASP.NET söyler [en şifreleme işlemlerinin](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/)form kimlik doğrulaması, Görünüm durumu ve çağrıları dahil olmak üzere MachineKey.Protect. Eklenen satırın aşağıdaki gibidir.

```xml
<machineKey compatibilityMode="Framework45" dataProtectorType="..." />
```

>[!TIP]
> Yeni veri koruma sisteminde gibi alanları inceleyerek etkin olup olmadığını anlayabilirsiniz `__VIEWSTATE`, hangi başlamalıdır "İle CfDJ8" aşağıdaki örnekte olduğu gibi. "CfDJ8" veri koruma sistemi tarafından korunan bir yükü tanımlayan Sihirli "09 F0 C9 F0" başlığı base64 gösterimidir.

```html
<input type="hidden" name="__VIEWSTATE" id="__VIEWSTATE" value="CfDJ8AWPr2EQPTBGs3L2GCZOpk..." />
```

## <a name="package-configuration"></a>Paket yapılandırması

Veri koruma sisteminde varsayılan sıfır Kurulum yapılandırmayla örneği. Varsayılan olarak yerel dosya sistemine anahtarları kalıcı olduğundan, ancak, bu grubu içinde dağıtılan uygulamalar için çalışmaz. Bu sorunu gidermek için hangi alt sınıfların DataProtectionStartup türü oluşturarak yapılandırması sağlayabilir ve onun ConfigureServices yöntemini geçersiz kılar.

Burada anahtarları kalıcı hem de bekleyen şifrelenmeden nasıl yapılandırılmış bir özel veri koruma başlangıç türü örneği aşağıdadır. Ayrıca varsayılan uygulama yalıtımı İlkesi kendi uygulama adı sağlayarak geçersiz kılar.

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
> Aynı zamanda `<machineKey applicationName="my-app" ... />` SetApplicationName için açık bir çağrı yerine. Tüm yapılandırma istedikleri ayarlanması, uygulama adı DataProtectionStartup türetilmiş bir türün oluşturmak için geliştiricinin zorlama önlemek için bir kolaylık mekanizması budur.

Bu özel yapılandırma etkinleştirmek için Web.config dosyasına geri dönün ve Ara `<appSettings>` yapılandırma dosyasına eklenen paketini yükle öğesi. Aşağıdaki biçimlendirmede gibi görünür:

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

Boş değer, yeni oluşturduğunuz DataProtectionStartup türetilen tür bütünleştirilmiş kod tam adı ile doldurun. Uygulamanın adını DataProtectionDemo ise, bu gibi görünür aşağıda.

```xml
<add key="aspnet:dataProtectionStartupType"
     value="DataProtectionDemo.MyDataProtectionStartup, DataProtectionDemo" />
```

Yeni yapılandırılmış veri koruma sisteminde artık uygulama kullanılmaya hazırdır.
