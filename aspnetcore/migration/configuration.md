---
title: ASP.NET Core yapılandırma geçirme
author: ardalis
description: Yapılandırma, bir ASP.NET Core MVC projesini ASP.NET MVC projesinde geçirmeyi öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: migration/configuration
ms.openlocfilehash: 5a1c4d0cbbdf74a00073c654e78a05f44948caae
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50205918"
---
# <a name="migrate-configuration-to-aspnet-core"></a><span data-ttu-id="44207-103">ASP.NET Core yapılandırma geçirme</span><span class="sxs-lookup"><span data-stu-id="44207-103">Migrate configuration to ASP.NET Core</span></span>

<span data-ttu-id="44207-104">Tarafından [Steve Smith](https://ardalis.com/) ve [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="44207-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="44207-105">Önceki makalede biz başlangıcından [bir ASP.NET MVC projesi için ASP.NET Core MVC geçirme](xref:migration/mvc).</span><span class="sxs-lookup"><span data-stu-id="44207-105">In the previous article, we began [migrate an ASP.NET MVC project to ASP.NET Core MVC](xref:migration/mvc).</span></span> <span data-ttu-id="44207-106">Bu makalede, biz yapılandırma geçirin.</span><span class="sxs-lookup"><span data-stu-id="44207-106">In this article, we migrate configuration.</span></span>

<span data-ttu-id="44207-107">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="44207-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="setup-configuration"></a><span data-ttu-id="44207-108">Kurulum yapılandırması</span><span class="sxs-lookup"><span data-stu-id="44207-108">Setup configuration</span></span>

<span data-ttu-id="44207-109">ASP.NET Core bundan böyle *Global.asax* ve *web.config* ASP.NET'in önceki sürümlerinde kullanılan dosya.</span><span class="sxs-lookup"><span data-stu-id="44207-109">ASP.NET Core no longer uses the *Global.asax* and *web.config* files that previous versions of ASP.NET utilized.</span></span> <span data-ttu-id="44207-110">ASP.NET önceki sürümlerinde, uygulama başlangıç mantığı yerleştirildiği bir `Application_StartUp` yöntemi içinde *Global.asax*.</span><span class="sxs-lookup"><span data-stu-id="44207-110">In earlier versions of ASP.NET, application startup logic was placed in an `Application_StartUp` method within *Global.asax*.</span></span> <span data-ttu-id="44207-111">Daha sonra ASP.NET MVC, bir *Startup.cs* dosyası; proje kök dizininde dahil ve uygulama başlatıldığında çağrılır.</span><span class="sxs-lookup"><span data-stu-id="44207-111">Later, in ASP.NET MVC, a *Startup.cs* file was included in the root of the project; and, it was called when the application started.</span></span> <span data-ttu-id="44207-112">ASP.NET Core geliştirmiştir bu yaklaşım tamamen tüm başlangıç mantığı yerleştirerek *Startup.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="44207-112">ASP.NET Core has adopted this approach completely by placing all startup logic in the *Startup.cs* file.</span></span>

<span data-ttu-id="44207-113">*Web.config* dosya içinde ASP.NET Core de değiştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="44207-113">The *web.config* file has also been replaced in ASP.NET Core.</span></span> <span data-ttu-id="44207-114">Kendi yapılandırma artık yapılandırılabilir, açıklanan uygulaması başlangıç yordamını bir parçası olarak *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="44207-114">Configuration itself can now be configured, as part of the application startup procedure described in *Startup.cs*.</span></span> <span data-ttu-id="44207-115">Yapılandırma hala XML dosyalarını kullanmasına, ancak genellikle ASP.NET Core projeleri yapılandırma değerlerini JSON biçimli bir dosya içinde gibi yerleştireceğiniz *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="44207-115">Configuration can still utilize XML files, but typically ASP.NET Core projects will place configuration values in a JSON-formatted file, such as *appsettings.json*.</span></span> <span data-ttu-id="44207-116">ASP.NET Core'nın yapılandırma sistemi sağlayabilen ortam değişkenlerini de bir kolayca erişebileceği bir [daha güvenli ve sağlam konumu](xref:security/app-secrets) ortama özgü değerler için.</span><span class="sxs-lookup"><span data-stu-id="44207-116">ASP.NET Core's configuration system can also easily access environment variables, which can provide a [more secure and robust location](xref:security/app-secrets) for environment-specific values.</span></span> <span data-ttu-id="44207-117">Bu, özellikle bağlantı dizelerini ve kaynak denetimine iade olmamalıdır API anahtarları gibi gizli dizileri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="44207-117">This is especially true for secrets like connection strings and API keys that shouldn't be checked into source control.</span></span> <span data-ttu-id="44207-118">Bkz: [yapılandırma](xref:fundamentals/configuration/index) ASP.NET core'da yapılandırma hakkında daha fazla bilgi edinmek için.</span><span class="sxs-lookup"><span data-stu-id="44207-118">See [Configuration](xref:fundamentals/configuration/index) to learn more about configuration in ASP.NET Core.</span></span>

<span data-ttu-id="44207-119">Bu makalede kısmen geçirilen ASP.NET Core projesinden ile başlıyoruz [önceki makalede](xref:migration/mvc).</span><span class="sxs-lookup"><span data-stu-id="44207-119">For this article, we are starting with the partially migrated ASP.NET Core project from [the previous article](xref:migration/mvc).</span></span> <span data-ttu-id="44207-120">Yapılandırma kurulumu, aşağıdaki oluşturucusu ve özelliğini eklemek için *Startup.cs* proje kök dizininde bulunan dosya:</span><span class="sxs-lookup"><span data-stu-id="44207-120">To setup configuration, add the following constructor and property to the *Startup.cs* file located in the root of the project:</span></span>

[!code-csharp[](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-16)]

<span data-ttu-id="44207-121">Unutmayın, bu noktada, *Startup.cs* dosyasını olmaz derlemek, biz yine de aşağıdakileri eklemeniz gerektiği `using` deyimi:</span><span class="sxs-lookup"><span data-stu-id="44207-121">Note that at this point, the *Startup.cs* file won't compile, as we still need to add the following `using` statement:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="44207-122">Ekleme bir *appsettings.json* uygun öğe şablonu kullanarak proje kök dosya:</span><span class="sxs-lookup"><span data-stu-id="44207-122">Add an *appsettings.json* file to the root of the project using the appropriate item template:</span></span>

![AppSettings JSON ekleyin](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a><span data-ttu-id="44207-124">Web.config dosyasından yapılandırma ayarlarını geçirme</span><span class="sxs-lookup"><span data-stu-id="44207-124">Migrate configuration settings from web.config</span></span>

<span data-ttu-id="44207-125">ASP.NET MVC Projemizin gerekli veritabanı bağlantı dizesine dahil *web.config*, `<connectionStrings>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="44207-125">Our ASP.NET MVC project included the required database connection string in *web.config*, in the `<connectionStrings>` element.</span></span> <span data-ttu-id="44207-126">ASP.NET Core Projemizin bu bilgileri depolamak için kullanacağız *appsettings.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="44207-126">In our ASP.NET Core project, we are going to store this information in the *appsettings.json* file.</span></span> <span data-ttu-id="44207-127">Açık *appsettings.json*, aşağıdakileri içerir, not edin:</span><span class="sxs-lookup"><span data-stu-id="44207-127">Open *appsettings.json*, and note that it already includes the following:</span></span>

[!code-json[](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]

<span data-ttu-id="44207-128">Yukarıda gösterilen vurgulanan satırında, veritabanı adını değiştirin **_CHANGE_ME** veritabanınızın adı.</span><span class="sxs-lookup"><span data-stu-id="44207-128">In the highlighted line depicted above, change the name of the database from **_CHANGE_ME** to the name of your database.</span></span>

## <a name="summary"></a><span data-ttu-id="44207-129">Özet</span><span class="sxs-lookup"><span data-stu-id="44207-129">Summary</span></span>

<span data-ttu-id="44207-130">ASP.NET Core tüm başlangıç mantıksal uygulama için bir tek, gerekli hizmetlerini ve bağımlılıklarını tanımlanan yapılandırılmış ve dosyasına yerleştirir.</span><span class="sxs-lookup"><span data-stu-id="44207-130">ASP.NET Core places all startup logic for the application in a single file, in which the necessary services and dependencies can be defined and configured.</span></span> <span data-ttu-id="44207-131">Değiştirir *web.config* dosyası ortam değişkenlerini yanı sıra JSON gibi dosya biçimlerinde çeşitli yararlanabilen esnek yapılandırma özelliği.</span><span class="sxs-lookup"><span data-stu-id="44207-131">It replaces the *web.config* file with a flexible configuration feature that can leverage a variety of file formats, such as JSON, as well as environment variables.</span></span>
