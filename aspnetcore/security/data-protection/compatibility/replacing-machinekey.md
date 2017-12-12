---
title: "< MachineKey > ASP.NET değiştirme"
author: rick-anderson
description: "< MachineKey > ASP.NET değiştirme"
keywords: "ASP.NET Core, güvenlik, < machineKey > machineKey"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 5ac13589-3837-4b4d-8abe-81f843942120
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/compatibility/replacing-machinekey
ms.openlocfilehash: b5a1be5fee7489f266e8a676956f68b499c6f14f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="replacing-machinekey-in-aspnet"></a><span data-ttu-id="75f8d-104">Değiştirme `<machineKey>` ASP.NET</span><span class="sxs-lookup"><span data-stu-id="75f8d-104">Replacing `<machineKey>` in ASP.NET</span></span>

<a name="compatibility-replacing-machinekey"></a>

<span data-ttu-id="75f8d-105">Uygulaması `<machineKey>` ASP.NET öğesinde [değiştirilebilen](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/).</span><span class="sxs-lookup"><span data-stu-id="75f8d-105">The implementation of the `<machineKey>` element in ASP.NET [is replaceable](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/).</span></span> <span data-ttu-id="75f8d-106">Bu yeni veri koruma sisteminde dahil olmak üzere bir değiştirme veri koruma mekanizması yönlendirilecek çoğu çağrıları ASP.NET şifreleme yordamlar sağlar.</span><span class="sxs-lookup"><span data-stu-id="75f8d-106">This allows most calls to ASP.NET cryptographic routines to be routed through a replacement data protection mechanism, including the new data protection system.</span></span>

## <a name="package-installation"></a><span data-ttu-id="75f8d-107">Paket yükleme</span><span class="sxs-lookup"><span data-stu-id="75f8d-107">Package installation</span></span>

> [!NOTE]
> <span data-ttu-id="75f8d-108">Yeni veri koruma sisteminde yalnızca .NET 4.5.1'in hedefleme mevcut bir ASP.NET uygulamasına veya daha yüksek yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="75f8d-108">The new data protection system can only be installed into an existing ASP.NET application targeting .NET 4.5.1 or higher.</span></span> <span data-ttu-id="75f8d-109">Yükleme uygulamayı .NET 4.5 hedefliyorsa başarısız veya azaltın.</span><span class="sxs-lookup"><span data-stu-id="75f8d-109">Installation will fail if the application targets .NET 4.5 or lower.</span></span>

<span data-ttu-id="75f8d-110">Varolan bir ASP.NET 4.5.1+ projeye yeni veri koruma sistemi yüklemek için Microsoft.AspNetCore.DataProtection.SystemWeb paketini yükleyin.</span><span class="sxs-lookup"><span data-stu-id="75f8d-110">To install the new data protection system into an existing ASP.NET 4.5.1+ project, install the package Microsoft.AspNetCore.DataProtection.SystemWeb.</span></span> <span data-ttu-id="75f8d-111">Bu veri koruması sistem kullanarak örneği oluşturulmaz [varsayılan yapılandırması](xref:security/data-protection/configuration/default-settings) ayarlar.</span><span class="sxs-lookup"><span data-stu-id="75f8d-111">This will instantiate the data protection system using the [default configuration](xref:security/data-protection/configuration/default-settings) settings.</span></span>

<span data-ttu-id="75f8d-112">Paketini yüklediğinizde, bir satıra ekler *Web.config* kullanmak için ASP.NET söyler [en şifreleme işlemlerinin](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/)form kimlik doğrulaması, Görünüm durumu ve çağrıları dahil olmak üzere MachineKey.Protect.</span><span class="sxs-lookup"><span data-stu-id="75f8d-112">When you install the package, it inserts a line into *Web.config* that tells ASP.NET to use it for [most cryptographic operations](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/), including forms authentication, view state, and calls to MachineKey.Protect.</span></span> <span data-ttu-id="75f8d-113">Eklenen satırın aşağıdaki gibidir.</span><span class="sxs-lookup"><span data-stu-id="75f8d-113">The line that's inserted reads as follows.</span></span>

```xml
<machineKey compatibilityMode="Framework45" dataProtectorType="..." />
```

>[!TIP]
> <span data-ttu-id="75f8d-114">Yeni veri koruma sisteminde gibi alanları inceleyerek etkin olup olmadığını anlayabilirsiniz `__VIEWSTATE`, hangi başlamalıdır "İle CfDJ8" aşağıdaki örnekte olduğu gibi.</span><span class="sxs-lookup"><span data-stu-id="75f8d-114">You can tell if the new data protection system is active by inspecting fields like `__VIEWSTATE`, which should begin with "CfDJ8" as in the example below.</span></span> <span data-ttu-id="75f8d-115">"CfDJ8" veri koruma sistemi tarafından korunan bir yükü tanımlayan Sihirli "09 F0 C9 F0" başlığı base64 gösterimidir.</span><span class="sxs-lookup"><span data-stu-id="75f8d-115">"CfDJ8" is the base64 representation of the magic "09 F0 C9 F0" header that identifies a payload protected by the data protection system.</span></span>

```html
<input type="hidden" name="__VIEWSTATE" id="__VIEWSTATE" value="CfDJ8AWPr2EQPTBGs3L2GCZOpk..." />
```

## <a name="package-configuration"></a><span data-ttu-id="75f8d-116">Paket yapılandırması</span><span class="sxs-lookup"><span data-stu-id="75f8d-116">Package configuration</span></span>

<span data-ttu-id="75f8d-117">Veri koruma sisteminde varsayılan sıfır Kurulum yapılandırmayla örneği.</span><span class="sxs-lookup"><span data-stu-id="75f8d-117">The data protection system is instantiated with a default zero-setup configuration.</span></span> <span data-ttu-id="75f8d-118">Varsayılan olarak yerel dosya sistemine anahtarları kalıcı olduğundan, ancak, bu grubu içinde dağıtılan uygulamalar için çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="75f8d-118">However, since by default keys are persisted to the local file system, this won't work for applications which are deployed in a farm.</span></span> <span data-ttu-id="75f8d-119">Bu sorunu gidermek için hangi alt sınıfların DataProtectionStartup türü oluşturarak yapılandırması sağlayabilir ve onun ConfigureServices yöntemini geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="75f8d-119">To resolve this, you can provide configuration by creating a type which subclasses DataProtectionStartup and overrides its ConfigureServices method.</span></span>

<span data-ttu-id="75f8d-120">Burada anahtarları kalıcı hem de bekleyen şifrelenmeden nasıl yapılandırılmış bir özel veri koruma başlangıç türü örneği aşağıdadır.</span><span class="sxs-lookup"><span data-stu-id="75f8d-120">Below is an example of a custom data protection startup type which configured both where keys are persisted and how they're encrypted at rest.</span></span> <span data-ttu-id="75f8d-121">Ayrıca varsayılan uygulama yalıtımı İlkesi kendi uygulama adı sağlayarak geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="75f8d-121">It also overrides the default app isolation policy by providing its own application name.</span></span>

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
> <span data-ttu-id="75f8d-122">Aynı zamanda `<machineKey applicationName="my-app" ... />` SetApplicationName için açık bir çağrı yerine.</span><span class="sxs-lookup"><span data-stu-id="75f8d-122">You can also use `<machineKey applicationName="my-app" ... />` in place of an explicit call to SetApplicationName.</span></span> <span data-ttu-id="75f8d-123">Tüm yapılandırma istedikleri ayarlanması, uygulama adı DataProtectionStartup türetilmiş bir türün oluşturmak için geliştiricinin zorlama önlemek için bir kolaylık mekanizması budur.</span><span class="sxs-lookup"><span data-stu-id="75f8d-123">This is a convenience mechanism to avoid forcing the developer to create a DataProtectionStartup-derived type if all they wanted to configure was setting the application name.</span></span>

<span data-ttu-id="75f8d-124">Bu özel yapılandırma etkinleştirmek için Web.config dosyasına geri dönün ve Ara `<appSettings>` yapılandırma dosyasına eklenen paketini yükle öğesi.</span><span class="sxs-lookup"><span data-stu-id="75f8d-124">To enable this custom configuration, go back to Web.config and look for the `<appSettings>` element that the package install added to the config file.</span></span> <span data-ttu-id="75f8d-125">Aşağıdaki biçimlendirmede gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="75f8d-125">It will look like the following markup:</span></span>

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

<span data-ttu-id="75f8d-126">Boş değer, yeni oluşturduğunuz DataProtectionStartup türetilen tür bütünleştirilmiş kod tam adı ile doldurun.</span><span class="sxs-lookup"><span data-stu-id="75f8d-126">Fill in the blank value with the assembly-qualified name of the DataProtectionStartup-derived type you just created.</span></span> <span data-ttu-id="75f8d-127">Uygulamanın adını DataProtectionDemo ise, bu gibi görünür aşağıda.</span><span class="sxs-lookup"><span data-stu-id="75f8d-127">If the name of the application is DataProtectionDemo, this would look like the below.</span></span>

```xml
<add key="aspnet:dataProtectionStartupType"
     value="DataProtectionDemo.MyDataProtectionStartup, DataProtectionDemo" />
```

<span data-ttu-id="75f8d-128">Yeni yapılandırılmış veri koruma sisteminde artık uygulama kullanılmaya hazırdır.</span><span class="sxs-lookup"><span data-stu-id="75f8d-128">The newly-configured data protection system is now ready for use inside the application.</span></span>
