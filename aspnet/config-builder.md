---
uid: config-builder
title: ASP.NET için yapılandırma oluşturucular
author: rick-anderson
description: Dış kaynaklardan web.config değerleri dışındaki kaynaklardan yapılandırma verilerini almayı öğrenin.
ms.author: riande
ms.date: 10/29/2018
ms.technology: aspnet
msc.type: content
ms.openlocfilehash: d5a3916c3df9778d14be80342bafbc3456a69a03
ms.sourcegitcommit: f2d14a7518d6ee51aca9333818ac1276e7b5ecef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50134541"
---
# <a name="configuration-builders-for-aspnet"></a><span data-ttu-id="3bea9-103">ASP.NET için yapılandırma oluşturucular</span><span class="sxs-lookup"><span data-stu-id="3bea9-103">Configuration builders for ASP.NET</span></span>

<span data-ttu-id="3bea9-104">Tarafından [Stephen Molloy](https://github.com/StephenMolloy) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3bea9-104">By [Stephen Molloy](https://github.com/StephenMolloy) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3bea9-105">Yapılandırma Oluşturucular, dış kaynaklardan yapılandırma değerlerini almak ASP.NET uygulamaları için modern ve Çevik bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="3bea9-105">Configuration builders provide a modern and agile mechanism for ASP.NET apps to get configuration values from external sources.</span></span>

<span data-ttu-id="3bea9-106">Yapılandırma oluşturucular:</span><span class="sxs-lookup"><span data-stu-id="3bea9-106">Configuration builders:</span></span>

* <span data-ttu-id="3bea9-107">.NET Framework 4.7.1 kullanılabilir ve sonrasında bulunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="3bea9-107">Are available in .NET Framework 4.7.1 and later.</span></span>
* <span data-ttu-id="3bea9-108">Yapılandırma değerleri okumak için esnek bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="3bea9-108">Provide a flexible mechanism for reading configuration values.</span></span>
* <span data-ttu-id="3bea9-109">Bir kapsayıcıya Taşı ve bulut odaklı bir ortam gibi bazı uygulamalarının ihtiyaçlarını temel adres.</span><span class="sxs-lookup"><span data-stu-id="3bea9-109">Address some of the basic needs of apps as they move into a container and cloud focused environment.</span></span>
* <span data-ttu-id="3bea9-110">Yapılandırma verileri (örneğin, Azure Key Vault ve ortam değişkenleri) önceden kullanılamayan kaynaklardan çizerek geliştirmek için .NET yapılandırma sistemini kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3bea9-110">Can be used to improve protection of configuration data by drawing from sources previously unavailable (for example, Azure Key Vault and environment variables) in the .NET configuration system.</span></span>

## <a name="keyvalue-configuration-builders"></a><span data-ttu-id="3bea9-111">Anahtar/değer yapılandırma oluşturucular</span><span class="sxs-lookup"><span data-stu-id="3bea9-111">Key/value configuration builders</span></span>

<span data-ttu-id="3bea9-112">Yapılandırma oluşturucular tarafından işlenebilen yaygın bir senaryo, bir anahtar/değer desenler izleyen yapılandırma bölümlerinin temel anahtar/değer değiştirme mekanizma sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="3bea9-112">A common scenario that can be handled by configuration builders is to provide a basic key/value replacement mechanism for configuration sections that follow a key/value pattern.</span></span> <span data-ttu-id="3bea9-113">.NET Framework kavramı ConfigurationBuilders, belirli yapılandırma bölümlerini veya düzenleri için sınırlı değildir.</span><span class="sxs-lookup"><span data-stu-id="3bea9-113">The .NET Framework concept of ConfigurationBuilders is not limited to specific configuration sections or patterns.</span></span> <span data-ttu-id="3bea9-114">Ancak, birçok yapılandırma oluşturucular içinde `Microsoft.Configuration.ConfigurationBuilders` ([github](https://github.com/aspnet/MicrosoftConfigurationBuilders)), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders) anahtar/değer desen içindeki iş.</span><span class="sxs-lookup"><span data-stu-id="3bea9-114">However, many of the configuration builders in `Microsoft.Configuration.ConfigurationBuilders` ([github](https://github.com/aspnet/MicrosoftConfigurationBuilders)), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders) work within the key/value pattern.</span></span>

## <a name="keyvalue-configuration-builders-settings"></a><span data-ttu-id="3bea9-115">Anahtar/değer yapılandırma oluşturucular ayarları</span><span class="sxs-lookup"><span data-stu-id="3bea9-115">Key/value configuration builders settings</span></span>

<span data-ttu-id="3bea9-116">Tüm anahtar/değer yapılandırma oluşturucular aşağıdaki ayarları uygulamak `Microsoft.Configuration.ConfigurationBuilders`.</span><span class="sxs-lookup"><span data-stu-id="3bea9-116">The following settings apply to all key/value configuration builders in `Microsoft.Configuration.ConfigurationBuilders`.</span></span>

### <a name="mode"></a><span data-ttu-id="3bea9-117">Mod</span><span class="sxs-lookup"><span data-stu-id="3bea9-117">Mode</span></span>

<span data-ttu-id="3bea9-118">Yapılandırma oluşturucular bir dış bilgi kaynağı olan anahtar/değer yapılandırma sistemi seçilen anahtar/değer öğeleri doldurmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="3bea9-118">The configuration builders use an external source of key/value information to populate selected key/value elements of the configuration system.</span></span> <span data-ttu-id="3bea9-119">Özellikle, `<appSettings/>` ve `<connectionStrings/>` bölümleri yapılandırma oluşturucular özel olarak değerlendirilmesi alırsınız.</span><span class="sxs-lookup"><span data-stu-id="3bea9-119">Specifically, the `<appSettings/>` and `<connectionStrings/>` sections receive special treatment from the configuration builders.</span></span> <span data-ttu-id="3bea9-120">Oluşturucular, üç modlarında çalışır:</span><span class="sxs-lookup"><span data-stu-id="3bea9-120">The builders work in three modes:</span></span>

* <span data-ttu-id="3bea9-121">`Strict` -Varsayılan modu.</span><span class="sxs-lookup"><span data-stu-id="3bea9-121">`Strict` - The default mode.</span></span> <span data-ttu-id="3bea9-122">Bu modda, yapılandırma Oluşturucu yalnızca iyi bilinen bir anahtar/değer merkezli yapılandırma bölümleri üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="3bea9-122">In this mode, the configuration builder only operates on well-known key/value-centric configuration sections.</span></span> <span data-ttu-id="3bea9-123">`Strict` Her anahtar bölümü modunu numaralandırır.</span><span class="sxs-lookup"><span data-stu-id="3bea9-123">`Strict` mode enumerates each key in the section.</span></span> <span data-ttu-id="3bea9-124">Dış kaynak olarak eşleşen bir anahtar bulunursa:</span><span class="sxs-lookup"><span data-stu-id="3bea9-124">If a matching key is found in the external source:</span></span>

   * <span data-ttu-id="3bea9-125">Yapılandırma oluşturucular elde edilen yapılandırma bölümündeki değeri bir dış kaynaktan değeriyle değiştirin.</span><span class="sxs-lookup"><span data-stu-id="3bea9-125">The configuration builders replace the value in the resulting configuration section with the value from the external source.</span></span>
* <span data-ttu-id="3bea9-126">`Greedy` -Bu modda yakından ilgilidir `Strict` modu.</span><span class="sxs-lookup"><span data-stu-id="3bea9-126">`Greedy` - This mode is closely related to `Strict` mode.</span></span> <span data-ttu-id="3bea9-127">Orijinal yapılandırmada bulunan anahtarlarına sınırlı kalmak yerine:</span><span class="sxs-lookup"><span data-stu-id="3bea9-127">Rather than being limited to keys that already exist in the original configuration:</span></span>

  * <span data-ttu-id="3bea9-128">Yapılandırma oluşturucular bir dış kaynaktan tüm anahtar/değer çiftleri elde edilen yapılandırma bölümüne ekler.</span><span class="sxs-lookup"><span data-stu-id="3bea9-128">The configuration builders adds all key/value pairs from the external source into the resulting configuration section.</span></span>

* <span data-ttu-id="3bea9-129">`Expand` -Ham XML yapılandırma bölümü nesnesine ayrıştırılmadan önce çalışır.</span><span class="sxs-lookup"><span data-stu-id="3bea9-129">`Expand` - Operates on the raw XML before it's parsed into a configuration section object.</span></span> <span data-ttu-id="3bea9-130">Bu, bir dizedeki belirteçlerin bir genişletme olarak düşünülebilir.</span><span class="sxs-lookup"><span data-stu-id="3bea9-130">It can be thought of as an expansion of tokens in a string.</span></span> <span data-ttu-id="3bea9-131">Ham XML dizesi desenle eşleşen herhangi bir bölümünü `${token}` belirteci genişletme bir adaydır.</span><span class="sxs-lookup"><span data-stu-id="3bea9-131">Any part of the raw XML string that matches the pattern `${token}` is a candidate for token expansion.</span></span> <span data-ttu-id="3bea9-132">Karşılık gelen değer, dış kaynak bulunamadı, belirteç değiştirilmez.</span><span class="sxs-lookup"><span data-stu-id="3bea9-132">If no corresponding value is found in the external source, then the token is not changed.</span></span> <span data-ttu-id="3bea9-133">Oluşturucular bu modda, bunlarla sınırlı değildir `<appSettings/>` ve `<connectionStrings/>` bölümler.</span><span class="sxs-lookup"><span data-stu-id="3bea9-133">Builders in this mode are not limited to the `<appSettings/>` and `<connectionStrings/>` sections.</span></span>

<span data-ttu-id="3bea9-134">Aşağıdaki biçimlendirmeden *web.config* sağlayan [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) içinde `Strict` modu:</span><span class="sxs-lookup"><span data-stu-id="3bea9-134">The following markup from *web.config* enables the [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) in `Strict` mode:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebDefault.config?name=snippet)]

<span data-ttu-id="3bea9-135">Aşağıdaki kod okuma `<appSettings/>` ve `<connectionStrings/>` önceki gösterilen *web.config* dosyası:</span><span class="sxs-lookup"><span data-stu-id="3bea9-135">The following code reads the `<appSettings/>` and `<connectionStrings/>` shown in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/About.aspx.cs)]

<span data-ttu-id="3bea9-136">Yukarıdaki kod özellik değerlerini ayarlar:</span><span class="sxs-lookup"><span data-stu-id="3bea9-136">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="3bea9-137">Değerler *web.config* dosyası anahtarları ortam değişkenleri ayarlanmamış.</span><span class="sxs-lookup"><span data-stu-id="3bea9-137">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="3bea9-138">Ortam değerlerini değişken, eğer ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="3bea9-138">The values of the environment variable, if set.</span></span>

<span data-ttu-id="3bea9-139">Örneğin, `ServiceID` içerir:</span><span class="sxs-lookup"><span data-stu-id="3bea9-139">For example, `ServiceID` will contain:</span></span>

* <span data-ttu-id="3bea9-140">"ServiceID value" web.config öğesinden, ortam değişkenini `ServiceID` ayarlı değil.</span><span class="sxs-lookup"><span data-stu-id="3bea9-140">"ServiceID value from web.config", if the environment variable `ServiceID` is not set.</span></span>
* <span data-ttu-id="3bea9-141">Değerini `ServiceID` ortam değişkeni, eğer ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="3bea9-141">The value of the `ServiceID` environment variable, if set.</span></span>

<span data-ttu-id="3bea9-142">Aşağıdaki görüntüde `<appSettings/>` önceki anahtarları/değerleri *web.config* ortam Düzenleyicisi'nde dosya kümesi:</span><span class="sxs-lookup"><span data-stu-id="3bea9-142">The following image shows the `<appSettings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![ortam Düzenleyicisi](config-builder/static/env.png)

<span data-ttu-id="3bea9-144">Not: Çıkın ve ortam değişkenleri içindeki değişiklikleri görmek için Visual Studio'yu yeniden başlatmanız gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="3bea9-144">Note: You might need to exit and restart Visual Studio to see changes in environment variables.</span></span>

### <a name="prefix-handling"></a><span data-ttu-id="3bea9-145">Ön işleme</span><span class="sxs-lookup"><span data-stu-id="3bea9-145">Prefix handling</span></span>

<span data-ttu-id="3bea9-146">Anahtar ön ayarı anahtarlarının çünkü basitleştirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="3bea9-146">Key prefixes can simplify setting keys because:</span></span>

* <span data-ttu-id="3bea9-147">.NET Framework yapılandırma karmaşık ve iç içe geçmiş ' dir.</span><span class="sxs-lookup"><span data-stu-id="3bea9-147">The .NET Framework configuration is complex and nested.</span></span>
* <span data-ttu-id="3bea9-148">Dış anahtar/değer kaynakları, temel ve düz yapısı tarafından yaygın olarak.</span><span class="sxs-lookup"><span data-stu-id="3bea9-148">External key/value sources are commonly basic and flat by nature.</span></span> <span data-ttu-id="3bea9-149">Örneğin, ortam değişkenleri içe değil.</span><span class="sxs-lookup"><span data-stu-id="3bea9-149">For example, environment variables are not nested.</span></span>

<span data-ttu-id="3bea9-150">Hem de ekleme için aşağıdaki yaklaşımlardan birini kullanın `<appSettings/>` ve `<connectionStrings/>` içine ortam değişkenlerini aracılığıyla yapılandırma:</span><span class="sxs-lookup"><span data-stu-id="3bea9-150">Use any of the following approaches to inject both `<appSettings/>` and `<connectionStrings/>` into the configuration via environment variables:</span></span>

* <span data-ttu-id="3bea9-151">İle `EnvironmentConfigBuilder` varsayılan `Strict` modu ve yapılandırma dosyasında uygun anahtar adları.</span><span class="sxs-lookup"><span data-stu-id="3bea9-151">With the `EnvironmentConfigBuilder` in the default `Strict` mode and the appropriate key names in the configuration file.</span></span> <span data-ttu-id="3bea9-152">Önceki kod ve biçimlendirme, bu yaklaşım alır.</span><span class="sxs-lookup"><span data-stu-id="3bea9-152">The preceding code and markup takes this approach.</span></span> <span data-ttu-id="3bea9-153">Yapabilecekleriniz bu yaklaşımı kullanarak **değil** anahtarları hem de aynı şekilde adlandırılmış sahiptir `<appSettings/>` ve `<connectionStrings/>`.</span><span class="sxs-lookup"><span data-stu-id="3bea9-153">Using this approach you can **not** have identically named keys in both `<appSettings/>` and `<connectionStrings/>`.</span></span>
* <span data-ttu-id="3bea9-154">İki kullanın `EnvironmentConfigBuilder`s'te `Greedy` farklı öneklerle modu ve `stripPrefix`.</span><span class="sxs-lookup"><span data-stu-id="3bea9-154">Use two `EnvironmentConfigBuilder`s in `Greedy` mode with distinct prefixes and `stripPrefix`.</span></span> <span data-ttu-id="3bea9-155">Bu yaklaşımda, uygulamanın okuyabileceği `<appSettings/>` ve `<connectionStrings/>` yapılandırma dosyasını güncelleştirmek gerek olmadan.</span><span class="sxs-lookup"><span data-stu-id="3bea9-155">With this approach, the app can read `<appSettings/>` and `<connectionStrings/>` without needing to update the configuration file.</span></span> <span data-ttu-id="3bea9-156">Sonraki bölümde [stripPrefix](#stripprefix), bunun nasıl yapılacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="3bea9-156">The next section,  [stripPrefix](#stripprefix),  shows how to do this.</span></span>
* <span data-ttu-id="3bea9-157">İki kullanın `EnvironmentConfigBuilder`s'te `Greedy` farklı öneklerle modu.</span><span class="sxs-lookup"><span data-stu-id="3bea9-157">Use two `EnvironmentConfigBuilder`s in `Greedy` mode with distinct prefixes.</span></span> <span data-ttu-id="3bea9-158">Bu yaklaşımla ön eke göre anahtar adları farklı olmalıdır gibi yinelenen anahtar adlara sahip olamaz.</span><span class="sxs-lookup"><span data-stu-id="3bea9-158">With this approach you can't have duplicate key names as key names must differ by prefix.</span></span>  <span data-ttu-id="3bea9-159">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="3bea9-159">For example:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefix.config?name=snippet&highlight=11-99)]

<span data-ttu-id="3bea9-160">Önceki biçimlendirme, aynı düz anahtar/değer kaynak yapılandırması için iki farklı bölümlerde doldurmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3bea9-160">With the preceding markup, the same flat key/value source can be used to populate configuration for two different sections.</span></span>

<span data-ttu-id="3bea9-161">Aşağıdaki görüntüde `<appSettings/>` ve `<connectionStrings/>` önceki anahtarları/değerleri *web.config* ortam Düzenleyicisi'nde dosya kümesi:</span><span class="sxs-lookup"><span data-stu-id="3bea9-161">The following image shows the `<appSettings/>` and `<connectionStrings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![ortam Düzenleyicisi](config-builder/static/prefix.png)

<span data-ttu-id="3bea9-163">Aşağıdaki kod okuma `<appSettings/>` ve `<connectionStrings/>` anahtarları/değerleri önceki yer alan *web.config* dosyası:</span><span class="sxs-lookup"><span data-stu-id="3bea9-163">The following code reads the `<appSettings/>` and `<connectionStrings/>` keys/values contained in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/Contact.aspx.cs?name=snippet)]

<span data-ttu-id="3bea9-164">Yukarıdaki kod özellik değerlerini ayarlar:</span><span class="sxs-lookup"><span data-stu-id="3bea9-164">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="3bea9-165">Değerler *web.config* dosyası anahtarları ortam değişkenleri ayarlanmamış.</span><span class="sxs-lookup"><span data-stu-id="3bea9-165">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="3bea9-166">Ortam değerlerini değişken, eğer ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="3bea9-166">The values of the environment variable, if set.</span></span>

<span data-ttu-id="3bea9-167">Örneğin, önceki'yi kullanarak *web.config* dosya, anahtarları/değerleri önceki ortam Düzenleyicisi resmi ve önceki kod, aşağıdaki değerleri ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="3bea9-167">For example, using the previous *web.config* file, the keys/values in the previous  environment editor image, and the previous code, the following values are set:</span></span>

|  <span data-ttu-id="3bea9-168">Anahtar</span><span class="sxs-lookup"><span data-stu-id="3bea9-168">Key</span></span>              | <span data-ttu-id="3bea9-169">Değer</span><span class="sxs-lookup"><span data-stu-id="3bea9-169">Value</span></span> |
| ----------------- | ------------ |
|     <span data-ttu-id="3bea9-170">AppSetting_ServiceID</span><span class="sxs-lookup"><span data-stu-id="3bea9-170">AppSetting_ServiceID</span></span>           | <span data-ttu-id="3bea9-171">Env değişkenlerinden AppSetting_ServiceID</span><span class="sxs-lookup"><span data-stu-id="3bea9-171">AppSetting_ServiceID from env variables</span></span>|
|    <span data-ttu-id="3bea9-172">AppSetting_default</span><span class="sxs-lookup"><span data-stu-id="3bea9-172">AppSetting_default</span></span>            | <span data-ttu-id="3bea9-173">Env AppSetting_default değeri</span><span class="sxs-lookup"><span data-stu-id="3bea9-173">AppSetting_default value from env</span></span> |
|       <span data-ttu-id="3bea9-174">ConnStr_default</span><span class="sxs-lookup"><span data-stu-id="3bea9-174">ConnStr_default</span></span>         | <span data-ttu-id="3bea9-175">Env gelen ConnStr_default val</span><span class="sxs-lookup"><span data-stu-id="3bea9-175">ConnStr_default val from env</span></span>|

### <a name="stripprefix"></a><span data-ttu-id="3bea9-176">stripPrefix</span><span class="sxs-lookup"><span data-stu-id="3bea9-176">stripPrefix</span></span>

<span data-ttu-id="3bea9-177">`stripPrefix`: boolean, varsayılan olarak `false`.</span><span class="sxs-lookup"><span data-stu-id="3bea9-177">`stripPrefix`: boolean, defaults to `false`.</span></span> 

<span data-ttu-id="3bea9-178">Önceki XML işaretlemesini bağlantı dizeleri uygulama ayarlarından ayırır, ancak tüm anahtarları gerektirir *web.config* dosyasını belirtilen önek kullanın.</span><span class="sxs-lookup"><span data-stu-id="3bea9-178">The preceding XML markup separates app settings from connection strings but requires all the keys in the *web.config* file to use the specified prefix.</span></span> <span data-ttu-id="3bea9-179">Örneğin, ön eki `AppSetting` eklenmelidir `ServiceID` anahtarı ("AppSetting_ServiceID").</span><span class="sxs-lookup"><span data-stu-id="3bea9-179">For example, the prefix `AppSetting` must be added to the `ServiceID` key ("AppSetting_ServiceID").</span></span> <span data-ttu-id="3bea9-180">İle `stripPrefix`, ön eki kullanılmaz *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="3bea9-180">With `stripPrefix`, the prefix is not used in the *web.config* file.</span></span> <span data-ttu-id="3bea9-181">Önek yapılandırma Oluşturucu kaynağı (örneğin, ortamdaki.) gereklidir Çoğu geliştirici kullanacağı tahmin `stripPrefix`.</span><span class="sxs-lookup"><span data-stu-id="3bea9-181">The prefix is required in the configuration builder source (for example, in the environment.) We anticipate most developers will use `stripPrefix`.</span></span>

<span data-ttu-id="3bea9-182">Uygulamalar genellikle önek Şerit.</span><span class="sxs-lookup"><span data-stu-id="3bea9-182">Applications typically strip off the prefix.</span></span> <span data-ttu-id="3bea9-183">Aşağıdaki *web.config* önek kaldırır:</span><span class="sxs-lookup"><span data-stu-id="3bea9-183">The following *web.config* strips the prefix:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefixStrip.config?name=snippet&highlight=14,19)]

<span data-ttu-id="3bea9-184">Önceki içinde *web.config* dosyası `default` anahtardır hem de `<appSettings/>` ve `<connectionStrings/>`.</span><span class="sxs-lookup"><span data-stu-id="3bea9-184">In the preceding *web.config* file, the `default` key is in both the `<appSettings/>` and `<connectionStrings/>`.</span></span>

<span data-ttu-id="3bea9-185">Aşağıdaki görüntüde `<appSettings/>` ve `<connectionStrings/>` önceki anahtarları/değerleri *web.config* ortam Düzenleyicisi'nde dosya kümesi:</span><span class="sxs-lookup"><span data-stu-id="3bea9-185">The following image shows the `<appSettings/>` and `<connectionStrings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![ortam Düzenleyicisi](config-builder/static/prefix.png)

<span data-ttu-id="3bea9-187">Aşağıdaki kod okuma `<appSettings/>` ve `<connectionStrings/>` anahtarları/değerleri önceki yer alan *web.config* dosyası:</span><span class="sxs-lookup"><span data-stu-id="3bea9-187">The following code reads the `<appSettings/>` and `<connectionStrings/>` keys/values contained in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/About2.aspx.cs?name=snippet)]

<span data-ttu-id="3bea9-188">Yukarıdaki kod özellik değerlerini ayarlar:</span><span class="sxs-lookup"><span data-stu-id="3bea9-188">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="3bea9-189">Değerler *web.config* dosyası anahtarları ortam değişkenleri ayarlanmamış.</span><span class="sxs-lookup"><span data-stu-id="3bea9-189">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="3bea9-190">Ortam değerlerini değişken, eğer ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="3bea9-190">The values of the environment variable, if set.</span></span>

<span data-ttu-id="3bea9-191">Örneğin, önceki'yi kullanarak *web.config* dosya, anahtarları/değerleri önceki ortam Düzenleyicisi resmi ve önceki kod, aşağıdaki değerleri ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="3bea9-191">For example, using the previous *web.config* file, the keys/values in the previous  environment editor image, and the previous code, the following values are set:</span></span>

|  <span data-ttu-id="3bea9-192">Anahtar</span><span class="sxs-lookup"><span data-stu-id="3bea9-192">Key</span></span>              | <span data-ttu-id="3bea9-193">Değer</span><span class="sxs-lookup"><span data-stu-id="3bea9-193">Value</span></span> |
| ----------------- | ------------ |
|     <span data-ttu-id="3bea9-194">ServiceId</span><span class="sxs-lookup"><span data-stu-id="3bea9-194">ServiceID</span></span>           | <span data-ttu-id="3bea9-195">Env değişkenlerinden AppSetting_ServiceID</span><span class="sxs-lookup"><span data-stu-id="3bea9-195">AppSetting_ServiceID from env variables</span></span>|
|    <span data-ttu-id="3bea9-196">default</span><span class="sxs-lookup"><span data-stu-id="3bea9-196">default</span></span>            | <span data-ttu-id="3bea9-197">Env AppSetting_default değeri</span><span class="sxs-lookup"><span data-stu-id="3bea9-197">AppSetting_default value from env</span></span> |
|    <span data-ttu-id="3bea9-198">default</span><span class="sxs-lookup"><span data-stu-id="3bea9-198">default</span></span>         | <span data-ttu-id="3bea9-199">Env gelen ConnStr_default val</span><span class="sxs-lookup"><span data-stu-id="3bea9-199">ConnStr_default val from env</span></span>|

### <a name="tokenpattern"></a><span data-ttu-id="3bea9-200">tokenPattern</span><span class="sxs-lookup"><span data-stu-id="3bea9-200">tokenPattern</span></span>

<span data-ttu-id="3bea9-201">`tokenPattern`: Dize, varsayılan olarak `@"\$\{(\w+)\}"`</span><span class="sxs-lookup"><span data-stu-id="3bea9-201">`tokenPattern`: String, defaults to `@"\$\{(\w+)\}"`</span></span>

<span data-ttu-id="3bea9-202">`Expand` Oluşturucular davranışını nasıl görüneceğini belirteçleri için ham XML arar `${token}`.</span><span class="sxs-lookup"><span data-stu-id="3bea9-202">The `Expand` behavior of the builders searches the raw XML for tokens that look like `${token}`.</span></span> <span data-ttu-id="3bea9-203">Arama varsayılan normal ifade ile yapılır `@"\$\{(\w+)\}"`.</span><span class="sxs-lookup"><span data-stu-id="3bea9-203">Searching is done with the default regular expression `@"\$\{(\w+)\}"`.</span></span> <span data-ttu-id="3bea9-204">Eşleşen karakter kümesini `\w` XML ve birçok yapılandırma kaynaklarını izin verilenden daha katı.</span><span class="sxs-lookup"><span data-stu-id="3bea9-204">The set of characters that matches `\w` is more strict than XML and many configuration sources allow.</span></span> <span data-ttu-id="3bea9-205">Kullanım `tokenPattern` ne zaman daha fazla karakter daha `@"\$\{(\w+)\}"` belirteç adı gereklidir.</span><span class="sxs-lookup"><span data-stu-id="3bea9-205">Use `tokenPattern` when more characters than `@"\$\{(\w+)\}"` are required in the token name.</span></span>

<span data-ttu-id="3bea9-206">`tokenPattern`: Dize:</span><span class="sxs-lookup"><span data-stu-id="3bea9-206">`tokenPattern`: String:</span></span>

* <span data-ttu-id="3bea9-207">Geliştiricilerin belirteci eşleştirmek için kullanılan normal ifade değiştirme olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="3bea9-207">Allows developers to change the regex that is used for token matching.</span></span>
* <span data-ttu-id="3bea9-208">İyi biçimlendirilmiş tehlikeli olmayan bir normal ifade olduğundan emin olmak için doğrulama gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="3bea9-208">No validation is done to make sure it is a well-formed, non-dangerous regex.</span></span>
* <span data-ttu-id="3bea9-209">Bir yakalama grubu içermelidir.</span><span class="sxs-lookup"><span data-stu-id="3bea9-209">It must contain a capture group.</span></span> <span data-ttu-id="3bea9-210">Tüm belirteç tüm normal ifade ile eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="3bea9-210">The entire regex must match the entire token.</span></span> <span data-ttu-id="3bea9-211">İlk yakalama yapılandırma kaynağında aramak için belirteç adı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3bea9-211">The first capture must be the token name to look up in the configuration source.</span></span>

## <a name="configuration-builders-in-microsoftconfigurationconfigurationbuilders"></a><span data-ttu-id="3bea9-212">Yapılandırma oluşturucular Microsoft.Configuration.ConfigurationBuilders içinde</span><span class="sxs-lookup"><span data-stu-id="3bea9-212">Configuration builders in Microsoft.Configuration.ConfigurationBuilders</span></span>

### <a name="environmentconfigbuilder"></a><span data-ttu-id="3bea9-213">EnvironmentConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="3bea9-213">EnvironmentConfigBuilder</span></span>

```xml
<add name="Environment"
    [mode|prefix|stripPrefix|tokenPattern] 
    type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Environment" />
```

<span data-ttu-id="3bea9-214">[EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):</span><span class="sxs-lookup"><span data-stu-id="3bea9-214">The [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):</span></span>

* <span data-ttu-id="3bea9-215">Yapılandırma oluşturucular çalıştırmaktır.</span><span class="sxs-lookup"><span data-stu-id="3bea9-215">Is the simplest of the configuration builders.</span></span>
* <span data-ttu-id="3bea9-216">Değerleri ortamından okur.</span><span class="sxs-lookup"><span data-stu-id="3bea9-216">Reads values from the environment.</span></span>
* <span data-ttu-id="3bea9-217">Herhangi bir ek yapılandırma seçeneği yok.</span><span class="sxs-lookup"><span data-stu-id="3bea9-217">Does not have any additional configuration options.</span></span>
* <span data-ttu-id="3bea9-218">`name` Rastgele öznitelik değeri.</span><span class="sxs-lookup"><span data-stu-id="3bea9-218">The `name` attribute value is arbitrary.</span></span>

<span data-ttu-id="3bea9-219">**Not:** bir Windows kapsayıcı ortamında, çalışma zamanında ayarlama değişkenleri yalnızca EntryPoint işlem ortamına.</span><span class="sxs-lookup"><span data-stu-id="3bea9-219">**Note:** In a Windows container environment, variables set at run time are only injected into the EntryPoint process environment.</span></span> <span data-ttu-id="3bea9-220">Uygulamaları, hizmet olarak çalıştırmak veya bir giriş noktası olmayan işlem değil çekme bu değişkenleri, aksi takdirde kapsayıcıdaki bir mekanizma aracılığıyla eklenmiş sürece.</span><span class="sxs-lookup"><span data-stu-id="3bea9-220">Apps that run as a service or a non-EntryPoint process do not pick up these variables unless they are otherwise injected through a mechanism in the container.</span></span> <span data-ttu-id="3bea9-221">İçin [IIS](https://github.com/Microsoft/iis-docker/pull/41)/[ASP.NET](https://github.com/Microsoft/aspnet-docker)-kapsayıcılara, geçerli sürümü [ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41) bu işleme *DefaultAppPool* yalnızca.</span><span class="sxs-lookup"><span data-stu-id="3bea9-221">For [IIS](https://github.com/Microsoft/iis-docker/pull/41)/[ASP.NET](https://github.com/Microsoft/aspnet-docker)-based containers, the current version of [ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41) handles this in the *DefaultAppPool* only.</span></span> <span data-ttu-id="3bea9-222">Diğer Windows tabanlı kapsayıcı çeşitleri, giriş noktası olmayan işlemler için kendi ekleme mekanizması geliştirmeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="3bea9-222">Other Windows-based container variants may need to develop their own injection mechanism for non-EntryPoint processes.</span></span>

### <a name="usersecretsconfigbuilder"></a><span data-ttu-id="3bea9-223">UserSecretsConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="3bea9-223">UserSecretsConfigBuilder</span></span>

> [!WARNING]
> <span data-ttu-id="3bea9-224">Hiçbir zaman parolaları, hassas bağlantı dizelerini ve diğer hassas verileri kaynak kodundaki.</span><span class="sxs-lookup"><span data-stu-id="3bea9-224">Never store passwords, sensitive connection strings, or other sensitive data in source code.</span></span> <span data-ttu-id="3bea9-225">Üretim gizli dizileri kullanılmamalıdır geliştirme veya test için.</span><span class="sxs-lookup"><span data-stu-id="3bea9-225">Production secrets should not be used for development or test.</span></span>

```xml
<add name="UserSecrets"
    [mode|prefix|stripPrefix|tokenPattern]
    (userSecretsId="{secret string, typically a GUID}" | userSecretsFile="~\secrets.file")
    [optional="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.UserSecrets" />
```

<span data-ttu-id="3bea9-226">Bu yapılandırma oluşturucusu benzer bir özellik sağlar [ASP.NET Core gizli dizi Yöneticisi](/aspnet/core/security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="3bea9-226">This configuration builder provides a feature similar to [ASP.NET Core Secret Manager](/aspnet/core/security/app-secrets).</span></span>

<span data-ttu-id="3bea9-227">[UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) .NET Framework projelerinde kullanılabilir, ancak bir gizli dizi dosyası belirtilmelidir.</span><span class="sxs-lookup"><span data-stu-id="3bea9-227">The [UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) can be used in .NET Framework projects, but a secrets file must be specified.</span></span> <span data-ttu-id="3bea9-228">Alternatif olarak, tanımlayabileceğiniz `UserSecretsId` projesi özelliğinde dosya ve doğru konumda okuma için ham gizli dizi dosyası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="3bea9-228">Alternatively, you can define the `UserSecretsId` property in the project file and create the raw secrets file in the correct location for reading.</span></span> <span data-ttu-id="3bea9-229">Dış bağımlılıkları projenize dışında tutmak için gizli olarak biçimlendirilmiş bir XML dosyasıdır.</span><span class="sxs-lookup"><span data-stu-id="3bea9-229">To keep external dependencies out of your project, the secret file is XML formatted.</span></span> <span data-ttu-id="3bea9-230">Bir uygulama ayrıntısı olan XML biçimlendirmesi ve biçimi üzerinde kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="3bea9-230">The XML formatting is an implementation detail, and the format should not be relied upon.</span></span> <span data-ttu-id="3bea9-231">Paylaşmanız gerekiyorsa bir *secrets.json* dosya ile .NET Core projelerinde, kullanmayı [SimpleJsonConfigBuilder](#simplejsonconfig).</span><span class="sxs-lookup"><span data-stu-id="3bea9-231">If you need to share a *secrets.json* file with .NET Core projects, consider using the [SimpleJsonConfigBuilder](#simplejsonconfig).</span></span> <span data-ttu-id="3bea9-232">`SimpleJsonConfigBuilder` Biçim .NET Core, bir uygulama ayrıntısı değişebilir düşünülmelidir.</span><span class="sxs-lookup"><span data-stu-id="3bea9-232">The `SimpleJsonConfigBuilder` format for .NET Core should also be considered an implementation detail subject to change.</span></span>

<span data-ttu-id="3bea9-233">Yapılandırma öznitelikleri için `UserSecretsConfigBuilder`:</span><span class="sxs-lookup"><span data-stu-id="3bea9-233">Configuration attributes for `UserSecretsConfigBuilder`:</span></span>

* <span data-ttu-id="3bea9-234">`userSecretsId` -Bu, bir XML gizli dizi dosyası tanımlamak için tercih edilen yöntemdir.</span><span class="sxs-lookup"><span data-stu-id="3bea9-234">`userSecretsId` - This is the preferred method for identifying an XML secrets file.</span></span> <span data-ttu-id="3bea9-235">Kullanan .NET Core için benzer çalışır bir `UserSecretsId` proje özelliği bu tanımlayıcıyı depolamak için.</span><span class="sxs-lookup"><span data-stu-id="3bea9-235">It works similar to .NET Core, which uses a `UserSecretsId` project property to store this identifier.</span></span> <span data-ttu-id="3bea9-236">Dize benzersiz olmalıdır, bu bir GUID olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="3bea9-236">The string must be unique, it doesn't need to be a GUID.</span></span> <span data-ttu-id="3bea9-237">Bu öznitelik ile `UserSecretsConfigBuilder` iyi bilinen bir yerel konumda arayın (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) bu tanımlayıcısına ait olan bir gizli dizi dosyası için.</span><span class="sxs-lookup"><span data-stu-id="3bea9-237">With this attribute, the `UserSecretsConfigBuilder` look in a well-known local location (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) for a secrets file belonging to this identifier.</span></span>
* <span data-ttu-id="3bea9-238">`userSecretsFile` Gizli dizileri içeren dosyayı belirtmek isteğe bağlı öznitelik.</span><span class="sxs-lookup"><span data-stu-id="3bea9-238">`userSecretsFile` - An optional attribute specifying the file containing the secrets.</span></span> <span data-ttu-id="3bea9-239">`~` Karakter başına uygulama kökü başvuru için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3bea9-239">The `~` character can be used at the start to reference the application root.</span></span> <span data-ttu-id="3bea9-240">Bu öznitelik veya `userSecretsId` özniteliği gereklidir.</span><span class="sxs-lookup"><span data-stu-id="3bea9-240">Either this attribute or the `userSecretsId` attribute is required.</span></span> <span data-ttu-id="3bea9-241">Her ikisi de belirtilirse, `userSecretsFile` önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="3bea9-241">If both are specified, `userSecretsFile` takes precedence.</span></span>
* <span data-ttu-id="3bea9-242">`optional`: boolean, varsayılan değer `true` -gizli dizi dosyası bulunamazsa özel durum engeller.</span><span class="sxs-lookup"><span data-stu-id="3bea9-242">`optional`: boolean, default value `true` - Prevents an exception if the secrets file cannot be found.</span></span> 
* <span data-ttu-id="3bea9-243">`name` Rastgele öznitelik değeri.</span><span class="sxs-lookup"><span data-stu-id="3bea9-243">The `name` attribute value is arbitrary.</span></span>

<span data-ttu-id="3bea9-244">Gizli dizi dosyası aşağıdaki biçime sahiptir:</span><span class="sxs-lookup"><span data-stu-id="3bea9-244">The secrets file has the following format:</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<root>
  <secrets ver="1.0">
    <secret name="secret key name" value="secret value" />
  </secrets>
</root>
```

### <a name="azurekeyvaultconfigbuilder"></a><span data-ttu-id="3bea9-245">AzureKeyVaultConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="3bea9-245">AzureKeyVaultConfigBuilder</span></span>

```xml
<add name="AzureKeyVault"
    [mode|prefix|stripPrefix|tokenPattern]
    (vaultName="MyVaultName" |
     uri="https:/MyVaultName.vault.azure.net")
    [connectionString="connection string"]
    [version="secrets version"]
    [preloadSecretNames="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Azure" />
```

<span data-ttu-id="3bea9-246">[AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) depolanan değerlerini okur [Azure anahtar kasası](/azure/key-vault/key-vault-whatis).</span><span class="sxs-lookup"><span data-stu-id="3bea9-246">The [AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) reads values stored in the [Azure Key Vault](/azure/key-vault/key-vault-whatis).</span></span>

<span data-ttu-id="3bea9-247">`vaultName` olduğundan (kasa adı) ya da kasaya bir URI gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="3bea9-247">`vaultName` is required (either the name of the vault or a URI to the vault).</span></span> <span data-ttu-id="3bea9-248">Diğer öznitelikleri bağlanmak için hangi kasası hakkında daha fazla denetime izin ver, ancak uygulama ile birlikte çalışan bir ortamda çalışır durumda değilse, yalnızca gerekli olan `Microsoft.Azure.Services.AppAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="3bea9-248">The other attributes allow control about which vault to connect to, but are only necessary if the application is not running in an environment that works with `Microsoft.Azure.Services.AppAuthentication`.</span></span> <span data-ttu-id="3bea9-249">Azure Hizmetleri kimlik doğrulaması kitaplığı otomatik olarak bağlantı bilgilerini Yürütme Ortamı'ndan mümkünse almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3bea9-249">The Azure Services Authentication library is used to automatically pick up connection information from the execution environment if possible.</span></span> <span data-ttu-id="3bea9-250">Otomatik olarak geçersiz bir bağlantı dizesi sağlayarak bağlantı bilgilerini almak.</span><span class="sxs-lookup"><span data-stu-id="3bea9-250">You can override automatically pick up of connection information by providing a connection string.</span></span>

* <span data-ttu-id="3bea9-251">`vaultName` -Gerekli if `uri` içinde sağlanmadı.</span><span class="sxs-lookup"><span data-stu-id="3bea9-251">`vaultName` - Required if `uri` in not provided.</span></span> <span data-ttu-id="3bea9-252">Anahtar/değer çiftleri okunacağı Azure aboneliğinizdeki kasasının adını belirtir.</span><span class="sxs-lookup"><span data-stu-id="3bea9-252">Specifies the name of the vault in your Azure subscription from which to read key/value pairs.</span></span>
* <span data-ttu-id="3bea9-253">`connectionString` -Bir bağlantı dizesi tarafından kullanılabilmesine [AzureServiceTokenProvider](https://docs.microsoft.com/en-us/azure/key-vault/service-to-service-authentication#connection-string-support)</span><span class="sxs-lookup"><span data-stu-id="3bea9-253">`connectionString` - A connection string usable by [AzureServiceTokenProvider](https://docs.microsoft.com/en-us/azure/key-vault/service-to-service-authentication#connection-string-support)</span></span>
* <span data-ttu-id="3bea9-254">`uri` -Diğer Key Vault sağlayıcılarıyla belirtilen bağlandığı `uri` değeri.</span><span class="sxs-lookup"><span data-stu-id="3bea9-254">`uri` - Connects to other Key Vault providers with the specified `uri` value.</span></span> <span data-ttu-id="3bea9-255">Belirtilmezse, Azure (`vaultName`) kasası sağlayıcısıdır.</span><span class="sxs-lookup"><span data-stu-id="3bea9-255">If not specified, Azure (`vaultName`) is the vault provider.</span></span>
* <span data-ttu-id="3bea9-256">`version` -Azure Key Vault gizli dizileri için bir sürüm oluşturma özelliği sağlar.</span><span class="sxs-lookup"><span data-stu-id="3bea9-256">`version` - Azure Key Vault provides a versioning feature for secrets.</span></span> <span data-ttu-id="3bea9-257">Varsa `version` belirtilirse, oluşturucu yalnızca bu sürümüyle eşleşen gizli anahtarları alır.</span><span class="sxs-lookup"><span data-stu-id="3bea9-257">If `version` is specified, the builder only retrieves secrets matching this version.</span></span>
* <span data-ttu-id="3bea9-258">`preloadSecretNames` -Varsayılan olarak, bu oluşturucu querys **tüm** başlatıldığında adları anahtar kasasında anahtar.</span><span class="sxs-lookup"><span data-stu-id="3bea9-258">`preloadSecretNames` - By default, this builder querys **all** key names in the key vault when it is initialized.</span></span> <span data-ttu-id="3bea9-259">Tüm anahtar değerlerin okuma önlemek için bu öznitelik ayarlanan `false`.</span><span class="sxs-lookup"><span data-stu-id="3bea9-259">To prevent reading all key values, set this attribute to `false`.</span></span> <span data-ttu-id="3bea9-260">Bu ayar `false` gizli bir anda okur.</span><span class="sxs-lookup"><span data-stu-id="3bea9-260">Setting this to `false` reads secrets one at a time.</span></span> <span data-ttu-id="3bea9-261">Bir kerede yararlı kasa "Get" erişim sağlar, ancak değil "List" erişim için gizli dizileri okunuyor.</span><span class="sxs-lookup"><span data-stu-id="3bea9-261">Reading secrets one at a time can useful if the vault allows "Get" access but not "List" access.</span></span> <span data-ttu-id="3bea9-262">**Not:** kullanırken `Greedy` modu `preloadSecretNames` olmalıdır `true` (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="3bea9-262">**Note:** When using `Greedy` mode, `preloadSecretNames` must be `true` (the default.)</span></span>

### <a name="keyperfileconfigbuilder"></a><span data-ttu-id="3bea9-263">KeyPerFileConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="3bea9-263">KeyPerFileConfigBuilder</span></span>

```xml
<add name="KeyPerFile"
    [mode|prefix|stripPrefix|tokenPattern]
    (directoryPath="PathToSourceDirectory")
    [ignorePrefix="ignore."]
    [keyDelimiter=":"]
    [optional="false"]
    type="Microsoft.Configuration.ConfigurationBuilders.KeyPerFileConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.KeyPerFile" />
```

<span data-ttu-id="3bea9-264">[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) değerler kaynağı olarak bir dizin dosyalarını kullanan bir temel yapılandırma oluşturucusu.</span><span class="sxs-lookup"><span data-stu-id="3bea9-264">[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) is a basic configuration builder that uses a directory's files as a source of values.</span></span> <span data-ttu-id="3bea9-265">Bir dosyanın adını anahtarı ve içeriği değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="3bea9-265">A file's name is the key, and the contents are the value.</span></span> <span data-ttu-id="3bea9-266">Bu yapılandırma Oluşturucu düzenlenmiş kapsayıcı ortamında çalışırken yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="3bea9-266">This configuration builder can be useful when running in an orchestrated container environment.</span></span> <span data-ttu-id="3bea9-267">Docker Swarm ve Kubernetes gibi sistemleri `secrets` kendi düzenlenmiş windows kapsayıcılarına bu dosya başına anahtar şekilde.</span><span class="sxs-lookup"><span data-stu-id="3bea9-267">Systems like Docker Swarm and Kubernetes provide `secrets` to their orchestrated windows containers in this key-per-file manner.</span></span>

<span data-ttu-id="3bea9-268">Öznitelik ayrıntıları:</span><span class="sxs-lookup"><span data-stu-id="3bea9-268">Attribute details:</span></span>

* <span data-ttu-id="3bea9-269">`directoryPath` -Gerekli.</span><span class="sxs-lookup"><span data-stu-id="3bea9-269">`directoryPath` - Required.</span></span> <span data-ttu-id="3bea9-270">Değerleri için aranacak bir yolunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="3bea9-270">Specifies a path to look in for values.</span></span> <span data-ttu-id="3bea9-271">Gizli dizileri depolanır Windows için docker *C:\ProgramData\Docker\secrets* varsayılan dizin.</span><span class="sxs-lookup"><span data-stu-id="3bea9-271">Docker for Windows secrets are stored in the *C:\ProgramData\Docker\secrets* directory by default.</span></span>
* <span data-ttu-id="3bea9-272">`ignorePrefix` -Bu ön ekine sahip dosyalar hariç tutulur.</span><span class="sxs-lookup"><span data-stu-id="3bea9-272">`ignorePrefix` - Files that start with this prefix are excluded.</span></span> <span data-ttu-id="3bea9-273">"Yoksay." varsayılan kullanır.</span><span class="sxs-lookup"><span data-stu-id="3bea9-273">Defaults to "ignore.".</span></span>
* <span data-ttu-id="3bea9-274">`keyDelimiter` -Varsayılan değer `null`.</span><span class="sxs-lookup"><span data-stu-id="3bea9-274">`keyDelimiter` - Default value is `null`.</span></span> <span data-ttu-id="3bea9-275">Belirtilmişse yapılandırma Oluşturucu anahtar adları bu sınırlayıcı ile oluşturma dizinin birden çok düzeyi erişir.</span><span class="sxs-lookup"><span data-stu-id="3bea9-275">If specified, the configuration builder traverses multiple levels of the directory, building up key names with this delimiter.</span></span> <span data-ttu-id="3bea9-276">Bu değer ise `null`, yapılandırma Oluşturucu yalnızca en üst düzeyinde dizinin arar.</span><span class="sxs-lookup"><span data-stu-id="3bea9-276">If this value is `null`, the configuration builder only looks at the top level of the directory.</span></span>
* <span data-ttu-id="3bea9-277">`optional` -Varsayılan değer `false`.</span><span class="sxs-lookup"><span data-stu-id="3bea9-277">`optional` -  Default value is `false`.</span></span> <span data-ttu-id="3bea9-278">Kaynak dizin yoksa yapılandırma Oluşturucu hatalara neden belirtir.</span><span class="sxs-lookup"><span data-stu-id="3bea9-278">Specifies whether the configuration builder should cause errors if the source directory doesn't exist.</span></span>

### <a name="simplejsonconfigbuilder"></a><span data-ttu-id="3bea9-279">SimpleJsonConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="3bea9-279">SimpleJsonConfigBuilder</span></span>

> [!WARNING]
> <span data-ttu-id="3bea9-280">Hiçbir zaman parolaları, hassas bağlantı dizelerini ve diğer hassas verileri kaynak kodundaki.</span><span class="sxs-lookup"><span data-stu-id="3bea9-280">Never store passwords, sensitive connection strings, or other sensitive data in source code.</span></span> <span data-ttu-id="3bea9-281">Üretim gizli dizileri kullanılmamalıdır geliştirme veya test için.</span><span class="sxs-lookup"><span data-stu-id="3bea9-281">Production secrets should not be used for development or test.</span></span>

```xml
<add name="SimpleJson"
    [mode|prefix|stripPrefix|tokenPattern]
    jsonFile="~\config.json"
    [optional="true"]
    [jsonMode="(Flat|Sectional)"]
    type="Microsoft.Configuration.ConfigurationBuilders.SimpleJsonConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Json" />
```

<span data-ttu-id="3bea9-282">.NET core projeleri, sık yapılandırmanız için JSON dosyalarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="3bea9-282">.NET Core projects frequently use JSON files for configuration.</span></span> <span data-ttu-id="3bea9-283">[SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) Oluşturucu .NET Framework kullanılacak .NET Core JSON dosyaları sağlar.</span><span class="sxs-lookup"><span data-stu-id="3bea9-283">The [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) builder allows .NET Core JSON files to be used in the .NET Framework.</span></span> <span data-ttu-id="3bea9-284">Bu yapılandırma oluşturucusu olan belirli bir anahtar/değer .NET Framework yapılandırma alanlarının bir düz anahtar/değer kaynağından temel bir eşleme sağlar.</span><span class="sxs-lookup"><span data-stu-id="3bea9-284">This configuration builder is provides a basic mapping from a flat key/value source into specific key/value areas of .NET Framework configuration.</span></span> <span data-ttu-id="3bea9-285">Bu yapılandırma oluşturucusu mu **değil** hiyerarşik yapılandırmalarını sağlayın.</span><span class="sxs-lookup"><span data-stu-id="3bea9-285">This configuration builder does **not** provide for hierarchical configurations.</span></span> <span data-ttu-id="3bea9-286">JSON yedekleme dosyası, karmaşık bir hiyerarşik nesnesi değil bir sözlüğe benzerdir.</span><span class="sxs-lookup"><span data-stu-id="3bea9-286">The JSON backing file is similar to a dictionary, not a complex hierarchical object.</span></span> <span data-ttu-id="3bea9-287">Çok düzeyli, hiyerarşik bir dosya kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3bea9-287">A multi-level hierarchical file can be used.</span></span> <span data-ttu-id="3bea9-288">Bu sağlayıcı `flatten`s her kullanarak düzeyinde özellik adı ekleyerek derinliği `:` sınırlayıcı olarak.</span><span class="sxs-lookup"><span data-stu-id="3bea9-288">This provider `flatten`s the depth by appending the property name at each level using `:` as a delimiter.</span></span>

<span data-ttu-id="3bea9-289">Öznitelik ayrıntıları:</span><span class="sxs-lookup"><span data-stu-id="3bea9-289">Attribute details:</span></span>

* <span data-ttu-id="3bea9-290">`jsonFile` -Gerekli.</span><span class="sxs-lookup"><span data-stu-id="3bea9-290">`jsonFile` - Required.</span></span> <span data-ttu-id="3bea9-291">Okunacak JSON dosyasını belirtir.</span><span class="sxs-lookup"><span data-stu-id="3bea9-291">Specifies the JSON file to read from.</span></span> <span data-ttu-id="3bea9-292">`~` Karakter başında uygulama kök başvurmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3bea9-292">The `~` character can be used at the start to reference the app root.</span></span>
* <span data-ttu-id="3bea9-293">`optional` -Boolean, varsayılan değer: `true`.</span><span class="sxs-lookup"><span data-stu-id="3bea9-293">`optional` - Boolean,  default value is `true`.</span></span> <span data-ttu-id="3bea9-294">Engelleyen bir JSON dosyası bulunamazsa özel durumları atma.</span><span class="sxs-lookup"><span data-stu-id="3bea9-294">Prevents throwing exceptions if the JSON file cannot be found.</span></span>
* <span data-ttu-id="3bea9-295">`jsonMode` - `[Flat|Sectional]`.</span><span class="sxs-lookup"><span data-stu-id="3bea9-295">`jsonMode` - `[Flat|Sectional]`.</span></span> <span data-ttu-id="3bea9-296">`Flat` varsayılandır.</span><span class="sxs-lookup"><span data-stu-id="3bea9-296">`Flat` is the default.</span></span> <span data-ttu-id="3bea9-297">Zaman `jsonMode` olduğu `Flat`, JSON dosyası bir düz tek anahtar/değer kaynağıdır.</span><span class="sxs-lookup"><span data-stu-id="3bea9-297">When `jsonMode` is `Flat`, the JSON file is a single flat key/value source.</span></span> <span data-ttu-id="3bea9-298">`EnvironmentConfigBuilder` Ve `AzureKeyVaultConfigBuilder` düz tek anahtar/değer kaynaklardır.</span><span class="sxs-lookup"><span data-stu-id="3bea9-298">The `EnvironmentConfigBuilder` and `AzureKeyVaultConfigBuilder` are also single flat key/value sources.</span></span> <span data-ttu-id="3bea9-299">Zaman `SimpleJsonConfigBuilder` yapılandırılan `Sectional` modu:</span><span class="sxs-lookup"><span data-stu-id="3bea9-299">When the `SimpleJsonConfigBuilder` is configured in `Sectional` mode:</span></span>

  * <span data-ttu-id="3bea9-300">JSON dosyası, yalnızca en üst düzeyde birden fazla sözlüklerine kavramsal olarak ayrılmıştır.</span><span class="sxs-lookup"><span data-stu-id="3bea9-300">The JSON file is conceptually divided just at the top level into multiple dictionaries.</span></span>
  * <span data-ttu-id="3bea9-301">Her sözlük yalnızca kendilerine iliştirilmiş en üst düzey özellik adıyla eşleşen bir yapılandırma bölümü uygulanır.</span><span class="sxs-lookup"><span data-stu-id="3bea9-301">Each of the dictionaries is only applied to the configuration section that matches the top-level property name attached to them.</span></span> <span data-ttu-id="3bea9-302">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="3bea9-302">For example:</span></span>

```json
    {
        "appSettings" : {
            "setting1" : "value1",
            "setting2" : "value2",
            "complex" : {
                "setting1" : "complex:value1",
                "setting2" : "complex:value2",
            }
        }
    }
```

## <a name="implementing-a-custom-keyvalue-configuration-builder"></a><span data-ttu-id="3bea9-303">Bir özel anahtar/değer yapılandırma Oluşturucu uygulama</span><span class="sxs-lookup"><span data-stu-id="3bea9-303">Implementing a custom key/value configuration builder</span></span>

<span data-ttu-id="3bea9-304">Yapılandırma oluşturucular gereksinimlerinizi karşılamıyorsa, özel bir tane yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3bea9-304">If the configuration builders don't meet your needs, you can write a custom one.</span></span> <span data-ttu-id="3bea9-305">`KeyValueConfigBuilder` Temel sınıf değiştirme modları ve çoğu ön eki sorunları işler.</span><span class="sxs-lookup"><span data-stu-id="3bea9-305">The `KeyValueConfigBuilder` base class handles substitution modes and most prefix concerns.</span></span> <span data-ttu-id="3bea9-306">Yalnızca uygulama projesinde gerekir:</span><span class="sxs-lookup"><span data-stu-id="3bea9-306">An implementing project need only:</span></span>

* <span data-ttu-id="3bea9-307">Temel sınıfından devralır ve anahtar/değer çiftleri ile temel bir kaynağı uygulama `GetValue` ve `GetAllValues`:</span><span class="sxs-lookup"><span data-stu-id="3bea9-307">Inherit from the base class and implement a basic source of key/value pairs through the `GetValue` and `GetAllValues`:</span></span>
* <span data-ttu-id="3bea9-308">Ekleme [Microsoft.Configuration.ConfigurationBuilders.Base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) projeye.</span><span class="sxs-lookup"><span data-stu-id="3bea9-308">Add the [Microsoft.Configuration.ConfigurationBuilders.Base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) to the project.</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/MyCustomConfigBuilder.cs)]

<span data-ttu-id="3bea9-309">`KeyValueConfigBuilder` Taban sınıfı sağlar iş ve tutarlı bir davranış çok anahtar/değer arasında yapılandırma oluşturucular.</span><span class="sxs-lookup"><span data-stu-id="3bea9-309">The `KeyValueConfigBuilder` base class provides much of the work and consistent behavior across key/value configuration builders.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3bea9-310">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="3bea9-310">Additional resources</span></span>

* [<span data-ttu-id="3bea9-311">Yapılandırma oluşturucular GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="3bea9-311">Configuration Builders GitHub repository</span></span>](https://github.com/aspnet/MicrosoftConfigurationBuilders)
* [<span data-ttu-id="3bea9-312">.NET kullanarak Azure Key Vault hizmetten hizmete kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="3bea9-312">Service-to-service authentication to Azure Key Vault using .NET</span></span>](/azure/key-vault/service-to-service-authentication#connection-string-support)
