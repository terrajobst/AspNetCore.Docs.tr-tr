---
title: ASP.NET Çekirdek yapılandırmasını geçir
author: ardalis
description: Bir ASP.NET MVC projesinde ASP.NET Core MVC projesinde geçir öğrenin.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/configuration
ms.openlocfilehash: 5bb89401ac54b54810fe5724b293ae8ed7e5afef
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="migrate-configuration-to-aspnet-core"></a><span data-ttu-id="840e9-103">ASP.NET Çekirdek yapılandırmasını geçir</span><span class="sxs-lookup"><span data-stu-id="840e9-103">Migrate configuration to ASP.NET Core</span></span>

<span data-ttu-id="840e9-104">Tarafından [Steve Smith](https://ardalis.com/) ve [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="840e9-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="840e9-105">Önceki makalede, biz başlangıcından [ASP.NET Core MVC için ASP.NET MVC projesinde geçirmek](mvc.md).</span><span class="sxs-lookup"><span data-stu-id="840e9-105">In the previous article, we began [migrate an ASP.NET MVC project to ASP.NET Core MVC](mvc.md).</span></span> <span data-ttu-id="840e9-106">Bu makalede, yapılandırmasını geçir.</span><span class="sxs-lookup"><span data-stu-id="840e9-106">In this article, we migrate configuration.</span></span>

<span data-ttu-id="840e9-107">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="840e9-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="setup-configuration"></a><span data-ttu-id="840e9-108">Kurulum yapılandırma</span><span class="sxs-lookup"><span data-stu-id="840e9-108">Setup Configuration</span></span>

<span data-ttu-id="840e9-109">ASP.NET Core artık kullanan *Global.asax* ve *web.config* ASP.NET önceki sürümlerinde kullanılan dosyaları.</span><span class="sxs-lookup"><span data-stu-id="840e9-109">ASP.NET Core no longer uses the *Global.asax* and *web.config* files that previous versions of ASP.NET utilized.</span></span> <span data-ttu-id="840e9-110">Uygulama başlangıç mantığı yerleştirilen ASP.NET önceki sürümlerde bir `Application_StartUp` yöntemi içinde *Global.asax*.</span><span class="sxs-lookup"><span data-stu-id="840e9-110">In earlier versions of ASP.NET, application startup logic was placed in an `Application_StartUp` method within *Global.asax*.</span></span> <span data-ttu-id="840e9-111">Daha sonra ASP.NET MVC, bir *haline* dosya; proje kök dizininde dahil ve uygulama başlatıldığında çağrıldı.</span><span class="sxs-lookup"><span data-stu-id="840e9-111">Later, in ASP.NET MVC, a *Startup.cs* file was included in the root of the project; and, it was called when the application started.</span></span> <span data-ttu-id="840e9-112">ASP.NET Core geliştirmiştir bu yaklaşım tamamen tüm başlangıç mantığı koyarak *haline* dosya.</span><span class="sxs-lookup"><span data-stu-id="840e9-112">ASP.NET Core has adopted this approach completely by placing all startup logic in the *Startup.cs* file.</span></span>

<span data-ttu-id="840e9-113">*Web.config* dosya ASP.NET Core de değiştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="840e9-113">The *web.config* file has also been replaced in ASP.NET Core.</span></span> <span data-ttu-id="840e9-114">Yapılandırma kendisini artık yapılandırılabilir, açıklanan uygulaması başlangıç yordamını bir parçası olarak *haline*.</span><span class="sxs-lookup"><span data-stu-id="840e9-114">Configuration itself can now be configured, as part of the application startup procedure described in *Startup.cs*.</span></span> <span data-ttu-id="840e9-115">Yapılandırma hala XML dosyalarını kullanan, ancak genellikle ASP.NET Core projeleri yapılandırma değerlerini bir JSON biçimli dosyasında gibi yerleştirir *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="840e9-115">Configuration can still utilize XML files, but typically ASP.NET Core projects will place configuration values in a JSON-formatted file, such as *appsettings.json*.</span></span> <span data-ttu-id="840e9-116">ASP.NET Core'nın yapılandırma sistemi sağlayabilir ortam değişkenlerini de kolayca erişebileceği bir [daha güvenli ve sağlam konumu](xref:security/app-secrets) ortama özgü değerleri.</span><span class="sxs-lookup"><span data-stu-id="840e9-116">ASP.NET Core's configuration system can also easily access environment variables, which can provide a [more secure and robust location](xref:security/app-secrets) for environment-specific values.</span></span> <span data-ttu-id="840e9-117">Bu, özellikle bağlantı dizeleri ve kaynak denetimine iade döndürmemelidir API anahtarları gibi gizli anahtarları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="840e9-117">This is especially true for secrets like connection strings and API keys that shouldn't be checked into source control.</span></span> <span data-ttu-id="840e9-118">Bkz: [yapılandırma](xref:fundamentals/configuration/index) ASP.NET çekirdek yapılandırması hakkında daha fazla bilgi edinmek için.</span><span class="sxs-lookup"><span data-stu-id="840e9-118">See [Configuration](xref:fundamentals/configuration/index) to learn more about configuration in ASP.NET Core.</span></span>

<span data-ttu-id="840e9-119">Bu makalede, biz kısmen geçirilen ASP.NET Core projeden ile başlıyorsanız [önceki makaleyi](mvc.md).</span><span class="sxs-lookup"><span data-stu-id="840e9-119">For this article, we are starting with the partially-migrated ASP.NET Core project from [the previous article](mvc.md).</span></span> <span data-ttu-id="840e9-120">Yapılandırma kurulumu, aşağıdaki oluşturucusu ve özelliğini eklemek için *haline* proje kök dizininde bulunan dosyası:</span><span class="sxs-lookup"><span data-stu-id="840e9-120">To setup configuration, add the following constructor and property to the *Startup.cs* file located in the root of the project:</span></span>

[!code-csharp[](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-21)]

<span data-ttu-id="840e9-121">Unutmayın, bu noktada, *haline* dosyasını olmaz derleyin, biz aşağıdaki eklemek gerek duyduğunuz `using` deyimi:</span><span class="sxs-lookup"><span data-stu-id="840e9-121">Note that at this point, the *Startup.cs* file won't compile, as we still need to add the following `using` statement:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="840e9-122">Ekleme bir *appsettings.json* uygun öğeyi şablonunu kullanarak projenin kök dosyaya:</span><span class="sxs-lookup"><span data-stu-id="840e9-122">Add an *appsettings.json* file to the root of the project using the appropriate item template:</span></span>

![AppSettings JSON ekleme](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a><span data-ttu-id="840e9-124">Web.config yapılandırma ayarlarını geçirme</span><span class="sxs-lookup"><span data-stu-id="840e9-124">Migrate Configuration Settings from web.config</span></span>

<span data-ttu-id="840e9-125">ASP.NET MVC Projemizin gerekli veritabanı bağlantı dizesine dahil *web.config*, `<connectionStrings>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="840e9-125">Our ASP.NET MVC project included the required database connection string in *web.config*, in the `<connectionStrings>` element.</span></span> <span data-ttu-id="840e9-126">ASP.NET Core Projemizin Biz bu bilgileri depolamak için kalacaklarını *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="840e9-126">In our ASP.NET Core project, we are going to store this information in the *appsettings.json* file.</span></span> <span data-ttu-id="840e9-127">Açık *appsettings.json*, aşağıdakileri içeren, not edin:</span><span class="sxs-lookup"><span data-stu-id="840e9-127">Open *appsettings.json*, and note that it already includes the following:</span></span>

[!code-json[](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]


<span data-ttu-id="840e9-128">Vurgulanan satırda, yukarıda gösterilen veritabanından adını değiştirmek **_CHANGE_ME** veritabanınızın adını.</span><span class="sxs-lookup"><span data-stu-id="840e9-128">In the highlighted line depicted above, change the name of the database from **_CHANGE_ME** to the name of your database.</span></span>

## <a name="summary"></a><span data-ttu-id="840e9-129">Özet</span><span class="sxs-lookup"><span data-stu-id="840e9-129">Summary</span></span>

<span data-ttu-id="840e9-130">ASP.NET Core tüm başlangıç mantığı uygulama için bir tek, bağımlılıklar ve gerekli hizmetleri tanımlanan yapılandırılmış ve dosyasına yerleştirir.</span><span class="sxs-lookup"><span data-stu-id="840e9-130">ASP.NET Core places all startup logic for the application in a single file, in which the necessary services and dependencies can be defined and configured.</span></span> <span data-ttu-id="840e9-131">Yerini *web.config* dosya bir esnek yapılandırma özelliğiyle dosya biçimlerinde, ortam değişkenleri yanı sıra, JSON gibi çeşitli yararlanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="840e9-131">It replaces the *web.config* file with a flexible configuration feature that can leverage a variety of file formats, such as JSON, as well as environment variables.</span></span>
