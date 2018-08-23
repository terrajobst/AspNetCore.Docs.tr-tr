---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: JSON ve XML serileştirme ASP.NET Web API'de | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 05/30/2012
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: 47967e6e1dd0e84b6059c07d7544c0e755fdf510
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754164"
---
<a name="json-and-xml-serialization-in-aspnet-web-api"></a><span data-ttu-id="4b455-102">JSON ve XML serileştirme ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="4b455-102">JSON and XML Serialization in ASP.NET Web API</span></span>
====================
<span data-ttu-id="4b455-103">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="4b455-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="4b455-104">Bu makalede, ASP.NET Web API'de JSON ve XML biçimlendiricileri açıklanır.</span><span class="sxs-lookup"><span data-stu-id="4b455-104">This article describes the JSON and XML formatters in ASP.NET Web API.</span></span>

<span data-ttu-id="4b455-105">ASP.NET Web API, bir *medya türü biçimlendiricisi* olabilir bir nesnedir:</span><span class="sxs-lookup"><span data-stu-id="4b455-105">In ASP.NET Web API, a *media-type formatter* is an object that can:</span></span>

- <span data-ttu-id="4b455-106">Bir HTTP nesnelerinden okuma CLR ileti gövdesi</span><span class="sxs-lookup"><span data-stu-id="4b455-106">Read CLR objects from an HTTP message body</span></span>
- <span data-ttu-id="4b455-107">Bir HTTP ileti gövdesine CLR nesnelerine yazma</span><span class="sxs-lookup"><span data-stu-id="4b455-107">Write CLR objects into an HTTP message body</span></span>

<span data-ttu-id="4b455-108">Web API'si, JSON ve XML için medya türü biçimlendiricileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="4b455-108">Web API provides media-type formatters for both JSON and XML.</span></span> <span data-ttu-id="4b455-109">Framework, varsayılan olarak bu biçimlendiricileri ardışık düzene ekler.</span><span class="sxs-lookup"><span data-stu-id="4b455-109">The framework inserts these formatters into the pipeline by default.</span></span> <span data-ttu-id="4b455-110">İstemciler HTTP isteği Accept üst bilgisi, JSON veya XML isteyebilir.</span><span class="sxs-lookup"><span data-stu-id="4b455-110">Clients can request either JSON or XML in the Accept header of the HTTP request.</span></span>

## <a name="contents"></a><span data-ttu-id="4b455-111">İçindekiler</span><span class="sxs-lookup"><span data-stu-id="4b455-111">Contents</span></span>

- [<span data-ttu-id="4b455-112">JSON medya türü biçimlendiricisi</span><span class="sxs-lookup"><span data-stu-id="4b455-112">JSON Media-Type Formatter</span></span>](#json_media_type_formatter)

    - [<span data-ttu-id="4b455-113">Salt okunur özellikler</span><span class="sxs-lookup"><span data-stu-id="4b455-113">Read-Only Properties</span></span>](#json_readonly)
    - [<span data-ttu-id="4b455-114">Tarihleri</span><span class="sxs-lookup"><span data-stu-id="4b455-114">Dates</span></span>](#json_dates)
    - [<span data-ttu-id="4b455-115">Girintileme</span><span class="sxs-lookup"><span data-stu-id="4b455-115">Indenting</span></span>](#json_indenting)
    - [<span data-ttu-id="4b455-116">Camel Casing</span><span class="sxs-lookup"><span data-stu-id="4b455-116">Camel Casing</span></span>](#json_camelcasing)
    - [<span data-ttu-id="4b455-117">Anonim ve zayıf yazılmış nesneler</span><span class="sxs-lookup"><span data-stu-id="4b455-117">Anonymous and Weakly-Typed Objects</span></span>](#json_anon)
- [<span data-ttu-id="4b455-118">XML medya türü biçimlendiricisi</span><span class="sxs-lookup"><span data-stu-id="4b455-118">XML Media-Type Formatter</span></span>](#xml_media_type_formatter)

    - [<span data-ttu-id="4b455-119">Salt okunur özellikler</span><span class="sxs-lookup"><span data-stu-id="4b455-119">Read-Only Properties</span></span>](#xml_readonly)
    - [<span data-ttu-id="4b455-120">Tarihleri</span><span class="sxs-lookup"><span data-stu-id="4b455-120">Dates</span></span>](#xml_dates)
    - [<span data-ttu-id="4b455-121">Girintileme</span><span class="sxs-lookup"><span data-stu-id="4b455-121">Indenting</span></span>](#xml_indenting)
    - [<span data-ttu-id="4b455-122">Ayar türü başına XML seri hale getiricileri genişletme</span><span class="sxs-lookup"><span data-stu-id="4b455-122">Setting Per-Type XML Serializers</span></span>](#xml_pertype)
- [<span data-ttu-id="4b455-123">JSON veya XML biçimlendiricisi kaldırılıyor</span><span class="sxs-lookup"><span data-stu-id="4b455-123">Removing the JSON or XML Formatter</span></span>](#removing_the_json_or_xml_formatter)
- [<span data-ttu-id="4b455-124">İşleme döngüsel nesne başvuruları</span><span class="sxs-lookup"><span data-stu-id="4b455-124">Handling Circular Object References</span></span>](#handling_circular_object_references)
- [<span data-ttu-id="4b455-125">Test nesne seri hale getirme</span><span class="sxs-lookup"><span data-stu-id="4b455-125">Testing Object Serialization</span></span>](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a><span data-ttu-id="4b455-126">JSON medya türü biçimlendiricisi</span><span class="sxs-lookup"><span data-stu-id="4b455-126">JSON Media-Type Formatter</span></span>

<span data-ttu-id="4b455-127">JSON biçimlendirme tarafından sağlanır **JsonMediaTypeFormatter** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="4b455-127">JSON formatting is provided by the **JsonMediaTypeFormatter** class.</span></span> <span data-ttu-id="4b455-128">Varsayılan olarak, **JsonMediaTypeFormatter** kullanan [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serileştirme gerçekleştirmek için kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="4b455-128">By default, **JsonMediaTypeFormatter** uses the [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) library to perform serialization.</span></span> <span data-ttu-id="4b455-129">Json.NET bir üçüncü taraf açık kaynak projesidir.</span><span class="sxs-lookup"><span data-stu-id="4b455-129">Json.NET is a third-party open source project.</span></span>

<span data-ttu-id="4b455-130">Tercih ederseniz, yapılandırabileceğiniz **JsonMediaTypeFormatter** kullanılacak sınıfı **DataContractJsonSerializer** Json.NET yerine.</span><span class="sxs-lookup"><span data-stu-id="4b455-130">If you prefer, you can configure the **JsonMediaTypeFormatter** class to use the **DataContractJsonSerializer** instead of Json.NET.</span></span> <span data-ttu-id="4b455-131">Bunu yapmak için ayarlanmış **UseDataContractJsonSerializer** özelliğini **true**:</span><span class="sxs-lookup"><span data-stu-id="4b455-131">To do so, set the **UseDataContractJsonSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a><span data-ttu-id="4b455-132">JSON Seri Hale Getirme</span><span class="sxs-lookup"><span data-stu-id="4b455-132">JSON Serialization</span></span>

<span data-ttu-id="4b455-133">Bu bölümde, JSON Biçimlendiricinin varsayılan kullanarak belirli bazı davranışları açıklanmaktadır [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serileştirici.</span><span class="sxs-lookup"><span data-stu-id="4b455-133">This section describes some specific behaviors of the JSON formatter, using the default [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializer.</span></span> <span data-ttu-id="4b455-134">Bu Json.NET kitaplığının kapsamlı belgeler olarak tasarlanmamıştır; Daha fazla bilgi için [Json.NET belgeleri](http://james.newtonking.com/projects/json/help/).</span><span class="sxs-lookup"><span data-stu-id="4b455-134">This is not meant to be comprehensive documentation of the Json.NET library; for more information, see the [Json.NET Documentation](http://james.newtonking.com/projects/json/help/).</span></span>

#### <a name="what-gets-serialized"></a><span data-ttu-id="4b455-135">Serileştirilmiş?</span><span class="sxs-lookup"><span data-stu-id="4b455-135">What Gets Serialized?</span></span>

<span data-ttu-id="4b455-136">Varsayılan olarak, tüm ortak özellikler ve alanları seri hale getirilmiş JSON biçiminde dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="4b455-136">By default, all public properties and fields are included in the serialized JSON.</span></span> <span data-ttu-id="4b455-137">Bir özellik veya alan atlamak için kendisiyle süslemek **JsonIgnore** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4b455-137">To omit a property or field, decorate it with the **JsonIgnore** attribute.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

<span data-ttu-id="4b455-138">İsterseniz bir &quot;katılımı&quot; yaklaşımı, sınıfıyla süslemek **DataContract** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4b455-138">If you prefer an &quot;opt-in&quot; approach, decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="4b455-139">Bu öznitelik varsa, sahip oldukları sürece üyeleri yoksayılır **DataMember**.</span><span class="sxs-lookup"><span data-stu-id="4b455-139">If this attribute is present, members are ignored unless they have the **DataMember**.</span></span> <span data-ttu-id="4b455-140">Ayrıca **DataMember** özel üyeler serileştirmek için.</span><span class="sxs-lookup"><span data-stu-id="4b455-140">You can also use **DataMember** to serialize private members.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="4b455-141">Salt okunur özellikler</span><span class="sxs-lookup"><span data-stu-id="4b455-141">Read-Only Properties</span></span>

<span data-ttu-id="4b455-142">Salt okunur özellikler varsayılan olarak serileştirilir.</span><span class="sxs-lookup"><span data-stu-id="4b455-142">Read-only properties are serialized by default.</span></span>

<a id="json_dates"></a>
### <a name="dates"></a><span data-ttu-id="4b455-143">Tarihleri</span><span class="sxs-lookup"><span data-stu-id="4b455-143">Dates</span></span>

<span data-ttu-id="4b455-144">Varsayılan olarak Json.NET tarihleri Yazar [ISO 8601](http://www.w3.org/TR/NOTE-datetime) biçimi.</span><span class="sxs-lookup"><span data-stu-id="4b455-144">By default, Json.NET writes dates in [ISO 8601](http://www.w3.org/TR/NOTE-datetime) format.</span></span> <span data-ttu-id="4b455-145">Tarihler UTC (Eşgüdümlü Evrensel Saat), "Z" soneki ile yazılır.</span><span class="sxs-lookup"><span data-stu-id="4b455-145">Dates in UTC (Coordinated Universal Time) are written with a "Z" suffix.</span></span> <span data-ttu-id="4b455-146">Bir saat dilimi farkı yerel saat tarihleri içerir.</span><span class="sxs-lookup"><span data-stu-id="4b455-146">Dates in local time include a time-zone offset.</span></span> <span data-ttu-id="4b455-147">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="4b455-147">For example:</span></span>

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

<span data-ttu-id="4b455-148">Varsayılan olarak, saat dilimini Json.NET korur.</span><span class="sxs-lookup"><span data-stu-id="4b455-148">By default, Json.NET preserves the time zone.</span></span> <span data-ttu-id="4b455-149">DateTimeZoneHandling özelliğini ayarlayarak bunu geçersiz kılabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="4b455-149">You can override this by setting the DateTimeZoneHandling property:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

<span data-ttu-id="4b455-150">Kullanmayı tercih ederseniz [Microsoft JSON tarih biçimi](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) ISO 8601 yerine ayarlamak **DateFormatHandling** serileştirici ayarlarını özelliği:</span><span class="sxs-lookup"><span data-stu-id="4b455-150">If you prefer to use [Microsoft JSON date format](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) instead of ISO 8601, set the **DateFormatHandling** property on the serializer settings:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="4b455-151">Girintileme</span><span class="sxs-lookup"><span data-stu-id="4b455-151">Indenting</span></span>

<span data-ttu-id="4b455-152">Girintili JSON yazmak için ayarlanmış **biçimlendirme** ayarını **Formatting.Intended**:</span><span class="sxs-lookup"><span data-stu-id="4b455-152">To write indented JSON, set the **Formatting** setting to **Formatting.Indented**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a><span data-ttu-id="4b455-153">Camel Casing</span><span class="sxs-lookup"><span data-stu-id="4b455-153">Camel Casing</span></span>

<span data-ttu-id="4b455-154">JSON özellik adları camel casing ile veri modelinizi değiştirmeden yazmak için ayarlanmış **CamelCasePropertyNamesContractResolver** seri hale getirici üzerinde:</span><span class="sxs-lookup"><span data-stu-id="4b455-154">To write JSON property names with camel casing, without changing your data model, set the **CamelCasePropertyNamesContractResolver** on the serializer:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a><span data-ttu-id="4b455-155">Anonim ve zayıf yazılmış nesneler</span><span class="sxs-lookup"><span data-stu-id="4b455-155">Anonymous and Weakly-Typed Objects</span></span>

<span data-ttu-id="4b455-156">Bir eylem yöntemi, anonim bir nesne döndürür ve JSON için seri hale getirme.</span><span class="sxs-lookup"><span data-stu-id="4b455-156">An action method can return an anonymous object and serialize it to JSON.</span></span> <span data-ttu-id="4b455-157">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="4b455-157">For example:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

<span data-ttu-id="4b455-158">Aşağıdaki JSON yanıt iletisi gövdesi içerir:</span><span class="sxs-lookup"><span data-stu-id="4b455-158">The response message body will contain the following JSON:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

<span data-ttu-id="4b455-159">JSON nesneleri istemcilerden gelen web API alır gevşek yapılandırılmış, için istek gövdesi seri durumdan çıkarabiliyorsa bir **Newtonsoft.Json.Linq.JObject** türü.</span><span class="sxs-lookup"><span data-stu-id="4b455-159">If your web API receives loosely structured JSON objects from clients, you can deserialize the request body to a **Newtonsoft.Json.Linq.JObject** type.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

<span data-ttu-id="4b455-160">Ancak, kesin türü belirtilmiş veri nesnelerini kullanmak daha iyi olur.</span><span class="sxs-lookup"><span data-stu-id="4b455-160">However, it is usually better to use strongly typed data objects.</span></span> <span data-ttu-id="4b455-161">Ardından verileri kendiniz ayrıştırmak gerekmez ve model doğrulama avantajlarından yararlanın.</span><span class="sxs-lookup"><span data-stu-id="4b455-161">Then you don't need to parse the data yourself, and you get the benefits of model validation.</span></span>

<span data-ttu-id="4b455-162">XML seri hale getirici anonim türler desteklemiyor veya **JObject** örnekleri.</span><span class="sxs-lookup"><span data-stu-id="4b455-162">The XML serializer does not support anonymous types or **JObject** instances.</span></span> <span data-ttu-id="4b455-163">JSON verilerinizi bu özellikleri kullanırsanız, XML biçimlendiricisi ardışık düzen tarafından bu makalenin sonraki bölümlerinde açıklandığı kaldırmalısınız.</span><span class="sxs-lookup"><span data-stu-id="4b455-163">If you use these features for your JSON data, you should remove the XML formatter from the pipeline, as described later in this article.</span></span>

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a><span data-ttu-id="4b455-164">XML medya türü biçimlendiricisi</span><span class="sxs-lookup"><span data-stu-id="4b455-164">XML Media-Type Formatter</span></span>

<span data-ttu-id="4b455-165">XML Biçimlendirme tarafından sağlanır **XmlMediaTypeFormatter** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="4b455-165">XML formatting is provided by the **XmlMediaTypeFormatter** class.</span></span> <span data-ttu-id="4b455-166">Varsayılan olarak, **XmlMediaTypeFormatter** kullanan **DataContractSerializer** serileştirme gerçekleştirmek için sınıf.</span><span class="sxs-lookup"><span data-stu-id="4b455-166">By default, **XmlMediaTypeFormatter** uses the **DataContractSerializer** class to perform serialization.</span></span>

<span data-ttu-id="4b455-167">Tercih ederseniz, yapılandırabileceğiniz **XmlMediaTypeFormatter** kullanılacak **XmlSerializer** yerine **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="4b455-167">If you prefer, you can configure the **XmlMediaTypeFormatter** to use the **XmlSerializer** instead of the **DataContractSerializer**.</span></span> <span data-ttu-id="4b455-168">Bunu yapmak için ayarlanmış **UseXmlSerializer** özelliğini **true**:</span><span class="sxs-lookup"><span data-stu-id="4b455-168">To do so, set the **UseXmlSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

<span data-ttu-id="4b455-169">**XmlSerializer** sınıf türleri daha dar bir kümesini destekler **DataContractSerializer**, ancak, elde edilen XML üzerinde daha fazla denetim sağlar.</span><span class="sxs-lookup"><span data-stu-id="4b455-169">The **XmlSerializer** class supports a narrower set of types than **DataContractSerializer**, but gives more control over the resulting XML.</span></span> <span data-ttu-id="4b455-170">Kullanmayı **XmlSerializer** varolan bir XML şemasına uyan gerekiyorsa.</span><span class="sxs-lookup"><span data-stu-id="4b455-170">Consider using **XmlSerializer** if you need to match an existing XML schema.</span></span>

### <a name="xml-serialization"></a><span data-ttu-id="4b455-171">XML seri hale getirme</span><span class="sxs-lookup"><span data-stu-id="4b455-171">XML Serialization</span></span>

<span data-ttu-id="4b455-172">Bu bölümde, XML biçimlendiricisi varsayılan kullanarak, belirli bazı davranışları açıklanmaktadır **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="4b455-172">This section describes some specific behaviors of the XML formatter, using the default **DataContractSerializer**.</span></span>

<span data-ttu-id="4b455-173">Varsayılan olarak, DataContractSerializer aşağıdaki gibi davranır:</span><span class="sxs-lookup"><span data-stu-id="4b455-173">By default, the DataContractSerializer behaves as follows:</span></span>

- <span data-ttu-id="4b455-174">Tüm ortak okuma/yazma özellikleri ve alanları serileştirilir.</span><span class="sxs-lookup"><span data-stu-id="4b455-174">All public read/write properties and fields are serialized.</span></span> <span data-ttu-id="4b455-175">Bir özellik veya alan atlamak için kendisiyle süslemek **IgnoreDataMember** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4b455-175">To omit a property or field, decorate it with the **IgnoreDataMember** attribute.</span></span>
- <span data-ttu-id="4b455-176">Özel ve korumalı üyeler seri duruma getirilmez.</span><span class="sxs-lookup"><span data-stu-id="4b455-176">Private and protected members are not serialized.</span></span>
- <span data-ttu-id="4b455-177">Salt okunur özelliklerini seri duruma getirilmez.</span><span class="sxs-lookup"><span data-stu-id="4b455-177">Read-only properties are not serialized.</span></span> <span data-ttu-id="4b455-178">(Ancak, bir salt okunur koleksiyon özelliği içeriğini serileştirilir.)</span><span class="sxs-lookup"><span data-stu-id="4b455-178">(However, the contents of a read-only collection property are serialized.)</span></span>
- <span data-ttu-id="4b455-179">Sınıf bildirimi içinde tam olarak göründükleri gibi sınıf ve üye adlarını XML'de yazılır.</span><span class="sxs-lookup"><span data-stu-id="4b455-179">Class and member names are written in the XML exactly as they appear in the class declaration.</span></span>
- <span data-ttu-id="4b455-180">Varsayılan XML ad alanı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4b455-180">A default XML namespace is used.</span></span>

<span data-ttu-id="4b455-181">Seri hale getirme hakkında daha fazla denetime ihtiyacınız varsa, sınıf ile donatmak **DataContract** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4b455-181">If you need more control over the serialization, you can decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="4b455-182">Bu öznitelik mevcut olduğunda, sınıf şu şekilde serileştirilmiş:</span><span class="sxs-lookup"><span data-stu-id="4b455-182">When this attribute is present, the class is serialized as follows:</span></span>

- <span data-ttu-id="4b455-183">&quot;Kabul et&quot; yaklaşım: özellikler ve alanları seri varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="4b455-183">&quot;Opt in&quot; approach: Properties and fields are not serialized by default.</span></span> <span data-ttu-id="4b455-184">Bir özellik veya alan seri hale getirmek için onunla süslemek **DataMember** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4b455-184">To serialize a property or field, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="4b455-185">Özel veya korumalı bir üye serileştirmek için onunla süslemek **DataMember** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4b455-185">To serialize a private or protected member, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="4b455-186">Salt okunur özelliklerini seri duruma getirilmez.</span><span class="sxs-lookup"><span data-stu-id="4b455-186">Read-only properties are not serialized.</span></span>
- <span data-ttu-id="4b455-187">Sınıf adı XML'de görüntülenme şeklini değiştirmek için Ayarla *adı* parametresinde **DataContract** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4b455-187">To change how the class name appears in the XML, set the *Name* parameter in the **DataContract** attribute.</span></span>
- <span data-ttu-id="4b455-188">Üye adı XML'de görüntülenme şeklini değiştirmek için Ayarla *adı* parametresinde **DataMember** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4b455-188">To change how a member name appears in the XML, set the *Name* parameter in the **DataMember** attribute.</span></span>
- <span data-ttu-id="4b455-189">XML ad alanı değiştirmek için Ayarla *Namespace* parametresinde **DataContract** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="4b455-189">To change the XML namespace, set the *Namespace* parameter in the **DataContract** class.</span></span>

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="4b455-190">Salt okunur özellikler</span><span class="sxs-lookup"><span data-stu-id="4b455-190">Read-Only Properties</span></span>

<span data-ttu-id="4b455-191">Salt okunur özelliklerini seri duruma getirilmez.</span><span class="sxs-lookup"><span data-stu-id="4b455-191">Read-only properties are not serialized.</span></span> <span data-ttu-id="4b455-192">Destek özel alanı salt okunur bir özellik varsa özel bir alanla işaretleyebilirsiniz **DataMember** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4b455-192">If a read-only property has a backing private field, you can mark the private field with the **DataMember** attribute.</span></span> <span data-ttu-id="4b455-193">Bu yaklaşım gerektirir **DataContract** sınıfı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4b455-193">This approach requires the **DataContract** attribute on the class.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a><span data-ttu-id="4b455-194">Tarihleri</span><span class="sxs-lookup"><span data-stu-id="4b455-194">Dates</span></span>

<span data-ttu-id="4b455-195">Tarihleri ISO 8601 biçiminde yazılır.</span><span class="sxs-lookup"><span data-stu-id="4b455-195">Dates are written in ISO 8601 format.</span></span> <span data-ttu-id="4b455-196">Örneğin, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span><span class="sxs-lookup"><span data-stu-id="4b455-196">For example, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span></span>

<a id="xml_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="4b455-197">Girintileme</span><span class="sxs-lookup"><span data-stu-id="4b455-197">Indenting</span></span>

<span data-ttu-id="4b455-198">Girintili XML yazmak için ayarlanmış **Girintile** özelliğini **true**:</span><span class="sxs-lookup"><span data-stu-id="4b455-198">To write indented XML, set the **Indent** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a><span data-ttu-id="4b455-199">Ayar türü başına XML seri hale getiricileri genişletme</span><span class="sxs-lookup"><span data-stu-id="4b455-199">Setting Per-Type XML Serializers</span></span>

<span data-ttu-id="4b455-200">Farklı CLR türleri için farklı XML seri hale getiricileri genişletme ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4b455-200">You can set different XML serializers for different CLR types.</span></span> <span data-ttu-id="4b455-201">Örneğin, gerektiren belirli bir veri nesnesi olabilir **XmlSerializer** geriye dönük uyumluluk için.</span><span class="sxs-lookup"><span data-stu-id="4b455-201">For example, you might have a particular data object that requires **XmlSerializer** for backward compatibility.</span></span> <span data-ttu-id="4b455-202">Kullanabileceğiniz **XmlSerializer** bu nesne ve kullanmaya devam **DataContractSerializer** diğer türleri için.</span><span class="sxs-lookup"><span data-stu-id="4b455-202">You can use **XmlSerializer** for this object and continue to use **DataContractSerializer** for other types.</span></span>

<span data-ttu-id="4b455-203">Belirli bir tür için bir XML serileştiricisi ayarlamak için çağrı **SetSerializer**.</span><span class="sxs-lookup"><span data-stu-id="4b455-203">To set an XML serializer for a particular type, call **SetSerializer**.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

<span data-ttu-id="4b455-204">Belirtebileceğiniz bir **XmlSerializer** veya türetilen herhangi bir nesne **XmlObjectSerializer**.</span><span class="sxs-lookup"><span data-stu-id="4b455-204">You can specify an **XmlSerializer** or any object that derives from **XmlObjectSerializer**.</span></span>

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a><span data-ttu-id="4b455-205">JSON veya XML biçimlendiricisi kaldırılıyor</span><span class="sxs-lookup"><span data-stu-id="4b455-205">Removing the JSON or XML Formatter</span></span>

<span data-ttu-id="4b455-206">Bunları kullanmak istemiyorsanız, biçimlendirici JSON veya XML biçimlendiricisi biçimlendiricileri, listeden kaldırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4b455-206">You can remove the JSON formatter or the XML formatter from the list of formatters, if you do not want to use them.</span></span> <span data-ttu-id="4b455-207">Bunu yapmak için temel neden şunlardır:</span><span class="sxs-lookup"><span data-stu-id="4b455-207">The main reasons to do this are:</span></span>

- <span data-ttu-id="4b455-208">Web sınırlamak için belirli bir ortama API yanıtları yazın.</span><span class="sxs-lookup"><span data-stu-id="4b455-208">To restrict your web API responses to a particular media type.</span></span> <span data-ttu-id="4b455-209">Örneğin, yalnızca JSON yanıtları desteği ve XML biçimlendiricisi kaldırmak karar verebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4b455-209">For example, you might decide to support only JSON responses, and remove the XML formatter.</span></span>
- <span data-ttu-id="4b455-210">Özel bir Biçimlendiricinin varsayılan biçimlendirici değiştirilecek.</span><span class="sxs-lookup"><span data-stu-id="4b455-210">To replace the default formatter with a custom formatter.</span></span> <span data-ttu-id="4b455-211">Örneğin, kendi özel uygulanışı JSON biçimlendirici ile JSON biçimlendirici yerini alabilir.</span><span class="sxs-lookup"><span data-stu-id="4b455-211">For example, you could replace the JSON formatter with your own custom implementation of a JSON formatter.</span></span>

<span data-ttu-id="4b455-212">Aşağıdaki kod, varsayılan biçimlendiricileri kaldırılacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="4b455-212">The following code shows how to remove the default formatters.</span></span> <span data-ttu-id="4b455-213">Bu çağrı, **uygulama\_Başlat** yöntemi, Global.asax dosyasında tanımlanmış.</span><span class="sxs-lookup"><span data-stu-id="4b455-213">Call this from your **Application\_Start** method, defined in Global.asax.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a><span data-ttu-id="4b455-214">İşleme döngüsel nesne başvuruları</span><span class="sxs-lookup"><span data-stu-id="4b455-214">Handling Circular Object References</span></span>

<span data-ttu-id="4b455-215">Varsayılan olarak, JSON ve XML biçimlendiricileri tüm nesneleri değerleri yazın.</span><span class="sxs-lookup"><span data-stu-id="4b455-215">By default, the JSON and XML formatters write all objects as values.</span></span> <span data-ttu-id="4b455-216">İki özellik de aynı nesneye başvuruyorsa ya da aynı nesne iki kez bir koleksiyonda görünürse, biçimlendirici nesne iki kez serileştirir.</span><span class="sxs-lookup"><span data-stu-id="4b455-216">If two properties refer to the same object, or if the same object appears twice in a collection, the formatter will serialize the object twice.</span></span> <span data-ttu-id="4b455-217">Nesne grafınızı döngüsünü içeriyorsa seri hale getirici bir özel durum oluşturmaz, bu grafikte bir döngü algıladığında, belirli bir sorunu olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="4b455-217">This is a particular problem if your object graph contains cycles, because the serializer will throw an exception when it detects a loop in the graph.</span></span>

<span data-ttu-id="4b455-218">Aşağıdaki nesne modelleri ve denetleyici göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="4b455-218">Consider the following object models and controller.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

<span data-ttu-id="4b455-219">Bu eylemi çağırma biçimlendirici için bir durum kodunu 500 (iç sunucu hatası) yanıtı istemciye izlendiği bir özel durum neden olur.</span><span class="sxs-lookup"><span data-stu-id="4b455-219">Invoking this action will cause the formatter to thrown an exception, which translates to a status code 500 (Internal Server Error) response to the client.</span></span>

<span data-ttu-id="4b455-220">Nesne başvuruları json'da korumak için aşağıdaki kodu ekleyin. **uygulama\_Başlat** Global.asax dosyasındaki yöntemi:</span><span class="sxs-lookup"><span data-stu-id="4b455-220">To preserve object references in JSON, add the following code to **Application\_Start** method in the Global.asax file:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

<span data-ttu-id="4b455-221">Denetleyici eylemini şimdi şuna benzer bir JSON döndürür:</span><span class="sxs-lookup"><span data-stu-id="4b455-221">Now the controller action will return JSON that looks like this:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

<span data-ttu-id="4b455-222">Seri hale getirici ekler bildirimi bir &quot;$id&quot; iki nesne için özellik.</span><span class="sxs-lookup"><span data-stu-id="4b455-222">Notice that the serializer adds an &quot;$id&quot; property to both objects.</span></span> <span data-ttu-id="4b455-223">Ayrıca, değeri bir nesne başvurusu ile değiştirir Employee.Department özelliği bir döngü oluşturur, bu nedenle, algıladığı: {&quot;$ref&quot;:&quot;1&quot;}.</span><span class="sxs-lookup"><span data-stu-id="4b455-223">Also, it detects that the Employee.Department property creates a loop, so it replaces the value with an object reference: {&quot;$ref&quot;:&quot;1&quot;}.</span></span>

> [!NOTE]
> <span data-ttu-id="4b455-224">Nesne başvuruları JSON'da standart değildir.</span><span class="sxs-lookup"><span data-stu-id="4b455-224">Object references are not standard in JSON.</span></span> <span data-ttu-id="4b455-225">Bu özelliği kullanmadan önce istemcilerinize sonuçları ayrıştırabiliyor olup olmayacağını göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="4b455-225">Before using this feature, consider whether your clients will be able to parse the results.</span></span> <span data-ttu-id="4b455-226">Graftan döngüleri kaldırmanız daha iyi olabilir.</span><span class="sxs-lookup"><span data-stu-id="4b455-226">It might be better simply to remove cycles from the graph.</span></span> <span data-ttu-id="4b455-227">Örneğin, çalışan bağlantıdan departmanı dön gerçekten bu örnekte gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="4b455-227">For example, the link from Employee back to Department is not really needed in this example.</span></span>


<span data-ttu-id="4b455-228">Nesne başvuruları XML'de korumak için iki seçeneğiniz vardır.</span><span class="sxs-lookup"><span data-stu-id="4b455-228">To preserve object references in XML, you have two options.</span></span> <span data-ttu-id="4b455-229">Daha basit bir seçenek eklemektir `[DataContract(IsReference=true)]` modeli sınıfınıza.</span><span class="sxs-lookup"><span data-stu-id="4b455-229">The simpler option is to add `[DataContract(IsReference=true)]` to your model class.</span></span> <span data-ttu-id="4b455-230">*IsReference* parametresi nesne başvuruları sağlar.</span><span class="sxs-lookup"><span data-stu-id="4b455-230">The *IsReference* parameter enables object references.</span></span> <span data-ttu-id="4b455-231">Unutmayın **DataContract** eklemeniz gerekecektir şekilde kabul etme, serileştirme yapar **DataMember** özelliklerine öznitelikleri:</span><span class="sxs-lookup"><span data-stu-id="4b455-231">Remember that **DataContract** makes serialization opt-in, so you will also need to add **DataMember** attributes to the properties:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

<span data-ttu-id="4b455-232">Biçimlendirici XML benzer üretecektir artık aşağıdaki:</span><span class="sxs-lookup"><span data-stu-id="4b455-232">Now the formatter will produce XML similar to following:</span></span>

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

<span data-ttu-id="4b455-233">Model sınıfınızın özniteliklerinde önlemek istiyorsanız, başka bir seçenek yoktur: yeni bir tür özel oluşturma **DataContractSerializer** ayarlayın ve örnek *preserveObjectReferences* için **true**  oluşturucuda.</span><span class="sxs-lookup"><span data-stu-id="4b455-233">If you want to avoid attributes on your model class, there is another option: Create a new type-specific **DataContractSerializer** instance and set *preserveObjectReferences* to **true** in the constructor.</span></span> <span data-ttu-id="4b455-234">Ardından bu örneği üzerinde XML medya türü biçimlendiricisi başına türü seri hale getirici ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="4b455-234">Then set this instance as a per-type serializer on the XML media-type formatter.</span></span> <span data-ttu-id="4b455-235">Aşağıdaki kod, bunun nasıl yapılacağını göster:</span><span class="sxs-lookup"><span data-stu-id="4b455-235">The following code show how to do this:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a><span data-ttu-id="4b455-236">Test nesne seri hale getirme</span><span class="sxs-lookup"><span data-stu-id="4b455-236">Testing Object Serialization</span></span>

<span data-ttu-id="4b455-237">Web API'nizi tasarlarken, veri nesnelerinizi nasıl serileştirilecek test etmek kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="4b455-237">As you design your web API, it is useful to test how your data objects will be serialized.</span></span> <span data-ttu-id="4b455-238">Denetleyici oluşturma veya bir denetleyici eylemi çağrılırken bunu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4b455-238">You can do this without creating a controller or invoking a controller action.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
