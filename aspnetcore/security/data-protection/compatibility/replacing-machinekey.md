---
title: ASP.NET core'da ASP.NET machineKey değiştirin
author: rick-anderson
description: Yeni ve daha güvenli veri koruma sisteminde kullanımına olanak tanımak için ASP.NET machineKey nasıl değiştirileceğini keşfedin.
ms.author: riande
ms.date: 04/06/2019
uid: security/data-protection/compatibility/replacing-machinekey
ms.openlocfilehash: 2317cb50cfe63226baf336ebfc5d681d1cebe5c6
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2019
ms.locfileid: "59468572"
---
# <a name="replace-the-aspnet-machinekey-in-aspnet-core"></a><span data-ttu-id="de374-103">ASP.NET core'da ASP.NET machineKey değiştirin</span><span class="sxs-lookup"><span data-stu-id="de374-103">Replace the ASP.NET machineKey in ASP.NET Core</span></span>

<a name="compatibility-replacing-machinekey"></a>

<span data-ttu-id="de374-104">Uygulamasını `<machineKey>` ASP.NET öğesinde [değiştirilebilen](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/).</span><span class="sxs-lookup"><span data-stu-id="de374-104">The implementation of the `<machineKey>` element in ASP.NET [is replaceable](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/).</span></span> <span data-ttu-id="de374-105">Bu, ASP.NET şifreleme rutinleri için çoğu çağrıları yeni veri koruma sistemi dahil olmak üzere bir değiştirme veri koruma mekanizması yönlendirilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="de374-105">This allows most calls to ASP.NET cryptographic routines to be routed through a replacement data protection mechanism, including the new data protection system.</span></span>

## <a name="package-installation"></a><span data-ttu-id="de374-106">Paket yüklemesi</span><span class="sxs-lookup"><span data-stu-id="de374-106">Package installation</span></span>

> [!NOTE]
> <span data-ttu-id="de374-107">Yeni veri koruma sisteminde yalnızca .NET 4.5.1 targeting mevcut bir ASP.NET uygulamasına veya üzeri yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="de374-107">The new data protection system can only be installed into an existing ASP.NET application targeting .NET 4.5.1 or later.</span></span> <span data-ttu-id="de374-108">Yükleme uygulaması, .NET 4.5 hedefliyse başarısız veya azaltın.</span><span class="sxs-lookup"><span data-stu-id="de374-108">Installation will fail if the application targets .NET 4.5 or lower.</span></span>

<span data-ttu-id="de374-109">Yeni veri koruma sisteminde mevcut bir ASP.NET 4.5.1+ projeye yüklemek için ' % s'paketi Microsoft.AspNetCore.DataProtection.SystemWeb yükleyin.</span><span class="sxs-lookup"><span data-stu-id="de374-109">To install the new data protection system into an existing ASP.NET 4.5.1+ project, install the package Microsoft.AspNetCore.DataProtection.SystemWeb.</span></span> <span data-ttu-id="de374-110">Veri koruma sistemi kullanarak örneği oluşturulmaz [varsayılan yapılandırmayı](xref:security/data-protection/configuration/default-settings) ayarları.</span><span class="sxs-lookup"><span data-stu-id="de374-110">This will instantiate the data protection system using the [default configuration](xref:security/data-protection/configuration/default-settings) settings.</span></span>

<span data-ttu-id="de374-111">Paket yükleme sırasında bir satıra ekler. *Web.config* ASP.NET için kullanılacağını söyler [en şifreleme işlemleri](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/)form kimlik doğrulaması, Görünüm durumu ve çağrılar dahil olmak üzere MachineKey.Protect.</span><span class="sxs-lookup"><span data-stu-id="de374-111">When you install the package, it inserts a line into *Web.config* that tells ASP.NET to use it for [most cryptographic operations](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/), including forms authentication, view state, and calls to MachineKey.Protect.</span></span> <span data-ttu-id="de374-112">Eklenen satırın aşağıdaki gibidir.</span><span class="sxs-lookup"><span data-stu-id="de374-112">The line that's inserted reads as follows.</span></span>

```xml
<machineKey compatibilityMode="Framework45" dataProtectorType="..." />
```

>[!TIP]
> <span data-ttu-id="de374-113">Yeni veri koruma sisteminde gibi alanlarını inceleyerek etkin olup olmadığını söyleyebilir `__VIEWSTATE`, aşağıdaki örnekte olduğu gibi "CfDJ8" ile başlatılması.</span><span class="sxs-lookup"><span data-stu-id="de374-113">You can tell if the new data protection system is active by inspecting fields like `__VIEWSTATE`, which should begin with "CfDJ8" as in the example below.</span></span> <span data-ttu-id="de374-114">"CfDJ8" Sihirli "09 F0 C9 F0" üstbilgisinin veri koruma sistemi tarafından korunan bir yükü tanımlayan base64 gösterimidir.</span><span class="sxs-lookup"><span data-stu-id="de374-114">"CfDJ8" is the base64 representation of the magic "09 F0 C9 F0" header that identifies a payload protected by the data protection system.</span></span>

```html
<input type="hidden" name="__VIEWSTATE" id="__VIEWSTATE" value="CfDJ8AWPr2EQPTBGs3L2GCZOpk...">
```

## <a name="package-configuration"></a><span data-ttu-id="de374-115">Paketi yapılandırması</span><span class="sxs-lookup"><span data-stu-id="de374-115">Package configuration</span></span>

<span data-ttu-id="de374-116">Veri koruma sisteminde varsayılan sıfır kurulum yapılandırma ile oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="de374-116">The data protection system is instantiated with a default zero-setup configuration.</span></span> <span data-ttu-id="de374-117">Varsayılan olarak yerel dosya sistemine anahtarları kalıcı olduğundan, ancak bu bir sunucu grubunda dağıtılan uygulamalar için çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="de374-117">However, since by default keys are persisted to the local file system, this won't work for applications which are deployed in a farm.</span></span> <span data-ttu-id="de374-118">Bu sorunu çözmek için alt DataProtectionStartup bir tür oluşturarak yapılandırması sağlayabilir ve onun Createservicereplicalisteners() yöntemini geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="de374-118">To resolve this, you can provide configuration by creating a type which subclasses DataProtectionStartup and overrides its ConfigureServices method.</span></span>

<span data-ttu-id="de374-119">Burada anahtarları kalıcı hem de bekleme sırasında nasıl şifrelenmeden yapılandırılan özel veri koruma başlangıç türü örneği aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="de374-119">Below is an example of a custom data protection startup type which configured both where keys are persisted and how they're encrypted at rest.</span></span> <span data-ttu-id="de374-120">Ayrıca varsayılan uygulama yalıtımı İlkesi, kendi uygulama adı sağlayarak geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="de374-120">It also overrides the default app isolation policy by providing its own application name.</span></span>

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
> <span data-ttu-id="de374-121">Ayrıca `<machineKey applicationName="my-app" ... />` SetApplicationName açık çağrı yerine.</span><span class="sxs-lookup"><span data-stu-id="de374-121">You can also use `<machineKey applicationName="my-app" ... />` in place of an explicit call to SetApplicationName.</span></span> <span data-ttu-id="de374-122">Tüm yapılandırma istedikleri ayarlanması, uygulama adı DataProtectionStartup türetilmiş bir tür oluşturmak için geliştirici zorlama önlemek için bir kolaylık mekanizması budur.</span><span class="sxs-lookup"><span data-stu-id="de374-122">This is a convenience mechanism to avoid forcing the developer to create a DataProtectionStartup-derived type if all they wanted to configure was setting the application name.</span></span>

<span data-ttu-id="de374-123">Bu özel yapılandırmasını etkinleştirmek için Web.config dosyasına geri dönün ve Ara `<appSettings>` paket yükleme yapılandırma dosyasına eklenen öğe.</span><span class="sxs-lookup"><span data-stu-id="de374-123">To enable this custom configuration, go back to Web.config and look for the `<appSettings>` element that the package install added to the config file.</span></span> <span data-ttu-id="de374-124">Bunu, aşağıdaki biçimlendirme gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="de374-124">It will look like the following markup:</span></span>

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

<span data-ttu-id="de374-125">Yeni oluşturduğunuz DataProtectionStartup türetilen tür bütünleştirilmiş kodla nitelenen adı boş değeriyle doldurun.</span><span class="sxs-lookup"><span data-stu-id="de374-125">Fill in the blank value with the assembly-qualified name of the DataProtectionStartup-derived type you just created.</span></span> <span data-ttu-id="de374-126">Uygulamanın adını DataProtectionDemo ise, bu gibi görünür aşağıda.</span><span class="sxs-lookup"><span data-stu-id="de374-126">If the name of the application is DataProtectionDemo, this would look like the below.</span></span>

```xml
<add key="aspnet:dataProtectionStartupType"
     value="DataProtectionDemo.MyDataProtectionStartup, DataProtectionDemo" />
```

<span data-ttu-id="de374-127">Yeni yapılandırılmış veri koruma sisteminde artık uygulama içinde kullanılmaya hazırdır.</span><span class="sxs-lookup"><span data-stu-id="de374-127">The newly-configured data protection system is now ready for use inside the application.</span></span>
