---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: "JSON ve XML serileştirme ASP.NET Web API'de | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2012
ms.topic: article
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: b1fcaf70cc38d73da0a454764520197b97f34b26
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="json-and-xml-serialization-in-aspnet-web-api"></a><span data-ttu-id="e6f4b-102">JSON ve XML serileştirme ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="e6f4b-102">JSON and XML Serialization in ASP.NET Web API</span></span>
====================
<span data-ttu-id="e6f4b-103">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e6f4b-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="e6f4b-104">Bu makalede, ASP.NET Web API içinde JSON ve XML biçimlendiricileri açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-104">This article describes the JSON and XML formatters in ASP.NET Web API.</span></span>

<span data-ttu-id="e6f4b-105">ASP.NET Web API içinde bir *medya türü biçimlendiricisi* olabilir bir nesnedir:</span><span class="sxs-lookup"><span data-stu-id="e6f4b-105">In ASP.NET Web API, a *media-type formatter* is an object that can:</span></span>

- <span data-ttu-id="e6f4b-106">Gövde okuma CLR nesneleri bir HTTP iletisi</span><span class="sxs-lookup"><span data-stu-id="e6f4b-106">Read CLR objects from an HTTP message body</span></span>
- <span data-ttu-id="e6f4b-107">Bir HTTP ileti gövdesini yazma CLR nesneleri</span><span class="sxs-lookup"><span data-stu-id="e6f4b-107">Write CLR objects into an HTTP message body</span></span>

<span data-ttu-id="e6f4b-108">Web API, JSON ve XML için medya türü biçimlendiricilerini sağlar.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-108">Web API provides media-type formatters for both JSON and XML.</span></span> <span data-ttu-id="e6f4b-109">Framework bu biçimlendiricileri varsayılan olarak ardışık düzenine ekler.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-109">The framework inserts these formatters into the pipeline by default.</span></span> <span data-ttu-id="e6f4b-110">İstemciler HTTP isteği kabul et üstbilgisinde JSON veya XML isteyebilir.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-110">Clients can request either JSON or XML in the Accept header of the HTTP request.</span></span>

## <a name="contents"></a><span data-ttu-id="e6f4b-111">İçindekiler</span><span class="sxs-lookup"><span data-stu-id="e6f4b-111">Contents</span></span>

- [<span data-ttu-id="e6f4b-112">JSON medya türü biçimlendiricisi</span><span class="sxs-lookup"><span data-stu-id="e6f4b-112">JSON Media-Type Formatter</span></span>](#json_media_type_formatter)

    - [<span data-ttu-id="e6f4b-113">Salt okunur özellikler</span><span class="sxs-lookup"><span data-stu-id="e6f4b-113">Read-Only Properties</span></span>](#json_readonly)
    - [<span data-ttu-id="e6f4b-114">Tarihleri</span><span class="sxs-lookup"><span data-stu-id="e6f4b-114">Dates</span></span>](#json_dates)
    - [<span data-ttu-id="e6f4b-115">Girintileme</span><span class="sxs-lookup"><span data-stu-id="e6f4b-115">Indenting</span></span>](#json_indenting)
    - [<span data-ttu-id="e6f4b-116">Ortası büyük/küçük harf</span><span class="sxs-lookup"><span data-stu-id="e6f4b-116">Camel Casing</span></span>](#json_camelcasing)
    - [<span data-ttu-id="e6f4b-117">Anonim ve zayıf yazılmış nesneleri</span><span class="sxs-lookup"><span data-stu-id="e6f4b-117">Anonymous and Weakly-Typed Objects</span></span>](#json_anon)
- [<span data-ttu-id="e6f4b-118">XML medya türü biçimlendiricisi</span><span class="sxs-lookup"><span data-stu-id="e6f4b-118">XML Media-Type Formatter</span></span>](#xml_media_type_formatter)

    - [<span data-ttu-id="e6f4b-119">Salt okunur özellikler</span><span class="sxs-lookup"><span data-stu-id="e6f4b-119">Read-Only Properties</span></span>](#xml_readonly)
    - [<span data-ttu-id="e6f4b-120">Tarihleri</span><span class="sxs-lookup"><span data-stu-id="e6f4b-120">Dates</span></span>](#xml_dates)
    - [<span data-ttu-id="e6f4b-121">Girintileme</span><span class="sxs-lookup"><span data-stu-id="e6f4b-121">Indenting</span></span>](#xml_indenting)
    - [<span data-ttu-id="e6f4b-122">Tür başına XML serileştiricileri ayarlama</span><span class="sxs-lookup"><span data-stu-id="e6f4b-122">Setting Per-Type XML Serializers</span></span>](#xml_pertype)
- [<span data-ttu-id="e6f4b-123">JSON veya XML biçimlendiricisi kaldırma</span><span class="sxs-lookup"><span data-stu-id="e6f4b-123">Removing the JSON or XML Formatter</span></span>](#removing_the_json_or_xml_formatter)
- [<span data-ttu-id="e6f4b-124">İşleme döngüsel nesne başvuruları</span><span class="sxs-lookup"><span data-stu-id="e6f4b-124">Handling Circular Object References</span></span>](#handling_circular_object_references)
- [<span data-ttu-id="e6f4b-125">Sınama nesnesi seri hale getirme</span><span class="sxs-lookup"><span data-stu-id="e6f4b-125">Testing Object Serialization</span></span>](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a><span data-ttu-id="e6f4b-126">JSON medya türü biçimlendiricisi</span><span class="sxs-lookup"><span data-stu-id="e6f4b-126">JSON Media-Type Formatter</span></span>

<span data-ttu-id="e6f4b-127">JSON biçimlendirme tarafından sağlanır **JsonMediaTypeFormatter** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-127">JSON formatting is provided by the **JsonMediaTypeFormatter** class.</span></span> <span data-ttu-id="e6f4b-128">Varsayılan olarak, **JsonMediaTypeFormatter** kullanan [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serileştirme gerçekleştirmek için kitaplık.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-128">By default, **JsonMediaTypeFormatter** uses the [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) library to perform serialization.</span></span> <span data-ttu-id="e6f4b-129">Json.NET üçüncü taraf açık kaynaklı bir projedir.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-129">Json.NET is a third-party open source project.</span></span>

<span data-ttu-id="e6f4b-130">İsterseniz, yapılandırabileceğiniz **JsonMediaTypeFormatter** kullanmak için sınıf **DataContractJsonSerializer** Json.NET yerine.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-130">If you prefer, you can configure the **JsonMediaTypeFormatter** class to use the **DataContractJsonSerializer** instead of Json.NET.</span></span> <span data-ttu-id="e6f4b-131">Bunu yapmak için ayarlanmış **UseDataContractJsonSerializer** özelliğine **true**:</span><span class="sxs-lookup"><span data-stu-id="e6f4b-131">To do so, set the **UseDataContractJsonSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a><span data-ttu-id="e6f4b-132">JSON Seri Hale Getirme</span><span class="sxs-lookup"><span data-stu-id="e6f4b-132">JSON Serialization</span></span>

<span data-ttu-id="e6f4b-133">Bu bölümde varsayılan kullanılarak JSON biçimlendirici belirli bazı davranışlarını anlatır [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) seri hale getirici.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-133">This section describes some specific behaviors of the JSON formatter, using the default [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializer.</span></span> <span data-ttu-id="e6f4b-134">Json.NET kitaplığının kapsamlı belgeler olması düşünülmemiştir; Daha fazla bilgi için bkz: [Json.NET belgelerine](http://james.newtonking.com/projects/json/help/).</span><span class="sxs-lookup"><span data-stu-id="e6f4b-134">This is not meant to be comprehensive documentation of the Json.NET library; for more information, see the [Json.NET Documentation](http://james.newtonking.com/projects/json/help/).</span></span>

#### <a name="what-gets-serialized"></a><span data-ttu-id="e6f4b-135">Serileştirilmiş?</span><span class="sxs-lookup"><span data-stu-id="e6f4b-135">What Gets Serialized?</span></span>

<span data-ttu-id="e6f4b-136">Varsayılan olarak, tüm genel özellikleri ve alanları serileştirilmiş JSON'de dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-136">By default, all public properties and fields are included in the serialized JSON.</span></span> <span data-ttu-id="e6f4b-137">Bir özellik veya alan atlamak için kendisiyle tasarlamanız **JsonIgnore** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-137">To omit a property or field, decorate it with the **JsonIgnore** attribute.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

<span data-ttu-id="e6f4b-138">İsterseniz bir &quot;katılımı&quot; yaklaşımını, sınıfıyla tasarlamanız **DataContract** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-138">If you prefer an &quot;opt-in&quot; approach, decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="e6f4b-139">Bu öznitelik varsa, sahip oldukları sürece üyeleri yoksayılır **DataMember**.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-139">If this attribute is present, members are ignored unless they have the **DataMember**.</span></span> <span data-ttu-id="e6f4b-140">Aynı zamanda **DataMember** özel üyeler seri hale getirilemedi.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-140">You can also use **DataMember** to serialize private members.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="e6f4b-141">Salt okunur özellikler</span><span class="sxs-lookup"><span data-stu-id="e6f4b-141">Read-Only Properties</span></span>

<span data-ttu-id="e6f4b-142">Salt okunur özellikler varsayılan olarak serileştirilir.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-142">Read-only properties are serialized by default.</span></span>

<a id="json_dates"></a>
### <a name="dates"></a><span data-ttu-id="e6f4b-143">tarihleri</span><span class="sxs-lookup"><span data-stu-id="e6f4b-143">Dates</span></span>

<span data-ttu-id="e6f4b-144">Varsayılan olarak, Json.NET tarihleri Yazar [ISO 8601](http://www.w3.org/TR/NOTE-datetime) biçimi.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-144">By default, Json.NET writes dates in [ISO 8601](http://www.w3.org/TR/NOTE-datetime) format.</span></span> <span data-ttu-id="e6f4b-145">Tarihler, UTC (Eşgüdümlü Evrensel Saat) "Z" soneki ile yazılır.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-145">Dates in UTC (Coordinated Universal Time) are written with a "Z" suffix.</span></span> <span data-ttu-id="e6f4b-146">Yerel saat tarihleri bir saat dilimi farkı içerir.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-146">Dates in local time include a time-zone offset.</span></span> <span data-ttu-id="e6f4b-147">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="e6f4b-147">For example:</span></span>

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

<span data-ttu-id="e6f4b-148">Varsayılan olarak, saat dilimi Json.NET korur.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-148">By default, Json.NET preserves the time zone.</span></span> <span data-ttu-id="e6f4b-149">DateTimeZoneHandling özelliğini ayarlayarak bunu geçersiz kılabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="e6f4b-149">You can override this by setting the DateTimeZoneHandling property:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

<span data-ttu-id="e6f4b-150">Kullanmayı tercih ederseniz, [Microsoft JSON tarih biçimi](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) ISO 8601 yerine ayarlamak **DateFormatHandling** özelliği seri hale getirici ayarları:</span><span class="sxs-lookup"><span data-stu-id="e6f4b-150">If you prefer to use [Microsoft JSON date format](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) instead of ISO 8601, set the **DateFormatHandling** property on the serializer settings:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="e6f4b-151">Girintileme</span><span class="sxs-lookup"><span data-stu-id="e6f4b-151">Indenting</span></span>

<span data-ttu-id="e6f4b-152">Girintili JSON yazmak için ayarlanmış **biçimlendirme** ayarını **Formatting.Intended**:</span><span class="sxs-lookup"><span data-stu-id="e6f4b-152">To write indented JSON, set the **Formatting** setting to **Formatting.Indented**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a><span data-ttu-id="e6f4b-153">Ortası büyük/küçük harf</span><span class="sxs-lookup"><span data-stu-id="e6f4b-153">Camel Casing</span></span>

<span data-ttu-id="e6f4b-154">JSON özellik adlarını ortası büyük/küçük harf, veri modelinizi değiştirmeden yazmak için ayarlanmış **CamelCasePropertyNamesContractResolver** seri hale getirici üzerinde:</span><span class="sxs-lookup"><span data-stu-id="e6f4b-154">To write JSON property names with camel casing, without changing your data model, set the **CamelCasePropertyNamesContractResolver** on the serializer:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a><span data-ttu-id="e6f4b-155">Anonim ve zayıf yazılmış nesneleri</span><span class="sxs-lookup"><span data-stu-id="e6f4b-155">Anonymous and Weakly-Typed Objects</span></span>

<span data-ttu-id="e6f4b-156">Bir eylem yöntemi, bir anonim nesnenin dönün ve JSON için seri hale.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-156">An action method can return an anonymous object and serialize it to JSON.</span></span> <span data-ttu-id="e6f4b-157">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="e6f4b-157">For example:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

<span data-ttu-id="e6f4b-158">Yanıt ileti gövdesi aşağıdaki JSON içerir:</span><span class="sxs-lookup"><span data-stu-id="e6f4b-158">The response message body will contain the following JSON:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

<span data-ttu-id="e6f4b-159">JSON nesnelerinin istemcilerinden API alır geniş web yapılandırılmış, istek gövdesi seri durumdan çıkarabiliyorsa bir **Newtonsoft.Json.Linq.JObject** türü.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-159">If your web API receives loosely structured JSON objects from clients, you can deserialize the request body to a **Newtonsoft.Json.Linq.JObject** type.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

<span data-ttu-id="e6f4b-160">Ancak, kesin türü belirtilmiş veri nesneleri kullanmak daha iyi olur.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-160">However, it is usually better to use strongly typed data objects.</span></span> <span data-ttu-id="e6f4b-161">Ardından verileri kendiniz ayrıştırma gerekmez ve model doğrulama avantajları elde edin.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-161">Then you don't need to parse the data yourself, and you get the benefits of model validation.</span></span>

<span data-ttu-id="e6f4b-162">XML serileştiriciyi anonim türler desteklemiyor veya **JObject** örnekleri.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-162">The XML serializer does not support anonymous types or **JObject** instances.</span></span> <span data-ttu-id="e6f4b-163">Bu özellikleri JSON verileriniz için kullanırsanız, XML biçimlendiricisi ardışık düzen tarafından bu makalenin sonraki bölümlerinde açıklandığı gibi kaldırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-163">If you use these features for your JSON data, you should remove the XML formatter from the pipeline, as described later in this article.</span></span>

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a><span data-ttu-id="e6f4b-164">XML medya türü biçimlendiricisi</span><span class="sxs-lookup"><span data-stu-id="e6f4b-164">XML Media-Type Formatter</span></span>

<span data-ttu-id="e6f4b-165">XML Biçimlendirme tarafından sağlanır **XmlMediaTypeFormatter** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-165">XML formatting is provided by the **XmlMediaTypeFormatter** class.</span></span> <span data-ttu-id="e6f4b-166">Varsayılan olarak, **XmlMediaTypeFormatter** kullanan **DataContractSerializer** serileştirme gerçekleştirmek için sınıf.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-166">By default, **XmlMediaTypeFormatter** uses the **DataContractSerializer** class to perform serialization.</span></span>

<span data-ttu-id="e6f4b-167">İsterseniz, yapılandırabileceğiniz **XmlMediaTypeFormatter** kullanmak için **XmlSerializer** yerine **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-167">If you prefer, you can configure the **XmlMediaTypeFormatter** to use the **XmlSerializer** instead of the **DataContractSerializer**.</span></span> <span data-ttu-id="e6f4b-168">Bunu yapmak için ayarlanmış **UseXmlSerializer** özelliğine **true**:</span><span class="sxs-lookup"><span data-stu-id="e6f4b-168">To do so, set the **UseXmlSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

<span data-ttu-id="e6f4b-169">**XmlSerializer** sınıfı türlerinden daha dar bir kümesini destekler **DataContractSerializer**, ancak, sonuçta elde edilen XML üzerinde daha fazla denetim sağlar.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-169">The **XmlSerializer** class supports a narrower set of types than **DataContractSerializer**, but gives more control over the resulting XML.</span></span> <span data-ttu-id="e6f4b-170">Kullanmayı **XmlSerializer** var olan bir XML şeması ile eşleşmesi gerekiyorsa.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-170">Consider using **XmlSerializer** if you need to match an existing XML schema.</span></span>

### <a name="xml-serialization"></a><span data-ttu-id="e6f4b-171">XML seri hale getirme</span><span class="sxs-lookup"><span data-stu-id="e6f4b-171">XML Serialization</span></span>

<span data-ttu-id="e6f4b-172">Bu bölümde XML biçimlendiricisi varsayılan kullanılarak, belirli bazı davranışlarını anlatır **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-172">This section describes some specific behaviors of the XML formatter, using the default **DataContractSerializer**.</span></span>

<span data-ttu-id="e6f4b-173">Varsayılan olarak, DataContractSerializer aşağıdaki gibi davranır:</span><span class="sxs-lookup"><span data-stu-id="e6f4b-173">By default, the DataContractSerializer behaves as follows:</span></span>

- <span data-ttu-id="e6f4b-174">Tüm ortak okuma/yazma özellikleri ve alanları serileştirilir.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-174">All public read/write properties and fields are serialized.</span></span> <span data-ttu-id="e6f4b-175">Bir özellik veya alan atlamak için kendisiyle tasarlamanız **IgnoreDataMember** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-175">To omit a property or field, decorate it with the **IgnoreDataMember** attribute.</span></span>
- <span data-ttu-id="e6f4b-176">Özel ve korumalı üyeleri seri değildir.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-176">Private and protected members are not serialized.</span></span>
- <span data-ttu-id="e6f4b-177">Salt okunur özellikler seri değildir.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-177">Read-only properties are not serialized.</span></span> <span data-ttu-id="e6f4b-178">(Ancak, bir salt okunur koleksiyonun özelliği içeriğini serileştirilir.)</span><span class="sxs-lookup"><span data-stu-id="e6f4b-178">(However, the contents of a read-only collection property are serialized.)</span></span>
- <span data-ttu-id="e6f4b-179">Sınıf bildiriminde tam olarak göründükleri gibi sınıfı ve üye adları XML'de yazılır.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-179">Class and member names are written in the XML exactly as they appear in the class declaration.</span></span>
- <span data-ttu-id="e6f4b-180">Varsayılan bir XML ad alanı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-180">A default XML namespace is used.</span></span>

<span data-ttu-id="e6f4b-181">Seri hale getirme hakkında daha fazla denetime ihtiyacınız varsa, sınıfıyla tasarlamanız **DataContract** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-181">If you need more control over the serialization, you can decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="e6f4b-182">Bu öznitelik mevcut olduğunda sınıfı şu şekilde seri değildir:</span><span class="sxs-lookup"><span data-stu-id="e6f4b-182">When this attribute is present, the class is serialized as follows:</span></span>

- <span data-ttu-id="e6f4b-183">&quot;Kabul&quot; yaklaşım: özellikleri ve alanları değil serileştirilir varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-183">&quot;Opt in&quot; approach: Properties and fields are not serialized by default.</span></span> <span data-ttu-id="e6f4b-184">Bir özellik veya alan serileştirmek için ile işaretleme **DataMember** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-184">To serialize a property or field, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="e6f4b-185">Özel veya korumalı bir üye serileştirmek için ile işaretleme **DataMember** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-185">To serialize a private or protected member, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="e6f4b-186">Salt okunur özellikler seri değildir.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-186">Read-only properties are not serialized.</span></span>
- <span data-ttu-id="e6f4b-187">Sınıf adı XML'de görünme biçimini değiştirmek için ayarlayın *adı* parametresinde **DataContract** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-187">To change how the class name appears in the XML, set the *Name* parameter in the **DataContract** attribute.</span></span>
- <span data-ttu-id="e6f4b-188">Üye adı XML'de görünme biçimini değiştirmek için ayarlayın *adı* parametresinde **DataMember** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-188">To change how a member name appears in the XML, set the *Name* parameter in the **DataMember** attribute.</span></span>
- <span data-ttu-id="e6f4b-189">XML ad alanı değiştirmek için ayarlayın *Namespace* parametresinde **DataContract** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-189">To change the XML namespace, set the *Namespace* parameter in the **DataContract** class.</span></span>

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="e6f4b-190">Salt okunur özellikler</span><span class="sxs-lookup"><span data-stu-id="e6f4b-190">Read-Only Properties</span></span>

<span data-ttu-id="e6f4b-191">Salt okunur özellikler seri değildir.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-191">Read-only properties are not serialized.</span></span> <span data-ttu-id="e6f4b-192">Bir yedekleme özel alan salt okunur bir özellik varsa, özel alanıyla işaretleyebilirsiniz **DataMember** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-192">If a read-only property has a backing private field, you can mark the private field with the **DataMember** attribute.</span></span> <span data-ttu-id="e6f4b-193">Bu yaklaşım gerektirir **DataContract** Sınıf özniteliği.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-193">This approach requires the **DataContract** attribute on the class.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a><span data-ttu-id="e6f4b-194">tarihleri</span><span class="sxs-lookup"><span data-stu-id="e6f4b-194">Dates</span></span>

<span data-ttu-id="e6f4b-195">Tarihleri ISO 8601 biçiminde yazılır.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-195">Dates are written in ISO 8601 format.</span></span> <span data-ttu-id="e6f4b-196">Örneğin, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-196">For example, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span></span>

<a id="xml_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="e6f4b-197">Girintileme</span><span class="sxs-lookup"><span data-stu-id="e6f4b-197">Indenting</span></span>

<span data-ttu-id="e6f4b-198">Girintili XML yazmak için ayarlanmış **girinti** özelliğine **true**:</span><span class="sxs-lookup"><span data-stu-id="e6f4b-198">To write indented XML, set the **Indent** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a><span data-ttu-id="e6f4b-199">Tür başına XML serileştiricileri ayarlama</span><span class="sxs-lookup"><span data-stu-id="e6f4b-199">Setting Per-Type XML Serializers</span></span>

<span data-ttu-id="e6f4b-200">Farklı CLR türleri için farklı XML serileştiricileri ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-200">You can set different XML serializers for different CLR types.</span></span> <span data-ttu-id="e6f4b-201">Örneğin, gerektiren belirli veri nesnesi olabilir **XmlSerializer** geriye dönük uyumluluk için.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-201">For example, you might have a particular data object that requires **XmlSerializer** for backward compatibility.</span></span> <span data-ttu-id="e6f4b-202">Kullanabileceğiniz **XmlSerializer** bu nesne ve kullanmaya devam **DataContractSerializer** diğer türleri.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-202">You can use **XmlSerializer** for this object and continue to use **DataContractSerializer** for other types.</span></span>

<span data-ttu-id="e6f4b-203">Belirli bir tür için bir XML serileştiricisi ayarlamak için arama **SetSerializer**.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-203">To set an XML serializer for a particular type, call **SetSerializer**.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

<span data-ttu-id="e6f4b-204">Belirleyebileceğiniz bir **XmlSerializer** veya türetilen herhangi bir nesne **XmlObjectSerializer**.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-204">You can specify an **XmlSerializer** or any object that derives from **XmlObjectSerializer**.</span></span>

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a><span data-ttu-id="e6f4b-205">JSON veya XML biçimlendiricisi kaldırma</span><span class="sxs-lookup"><span data-stu-id="e6f4b-205">Removing the JSON or XML Formatter</span></span>

<span data-ttu-id="e6f4b-206">Bunları kullanmak istemiyorsanız, JSON biçimlendirici veya XML biçimlendiricisi biçimlendiricileri, listeden kaldırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-206">You can remove the JSON formatter or the XML formatter from the list of formatters, if you do not want to use them.</span></span> <span data-ttu-id="e6f4b-207">Bunu yapmak için nedenler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="e6f4b-207">The main reasons to do this are:</span></span>

- <span data-ttu-id="e6f4b-208">Web kısıtlamak için belirli bir ortam API yanıtlarını yazın.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-208">To restrict your web API responses to a particular media type.</span></span> <span data-ttu-id="e6f4b-209">Örneğin, yalnızca JSON yanıtları desteklemek ve XML biçimlendiricisi kaldırmak karar verebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-209">For example, you might decide to support only JSON responses, and remove the XML formatter.</span></span>
- <span data-ttu-id="e6f4b-210">Varsayılan biçimlendirici ile özel bir biçimlendirici değiştirmek için.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-210">To replace the default formatter with a custom formatter.</span></span> <span data-ttu-id="e6f4b-211">Örneğin, kendi özel JSON biçimlendirici uygulamasıyla JSON biçimlendirici değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-211">For example, you could replace the JSON formatter with your own custom implementation of a JSON formatter.</span></span>

<span data-ttu-id="e6f4b-212">Aşağıdaki kod, varsayılan biçimlendiricileri kaldırmak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-212">The following code shows how to remove the default formatters.</span></span> <span data-ttu-id="e6f4b-213">Bu çağrı, **uygulama\_Başlat** yöntemi, Global.asax dosyasında tanımlanmış.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-213">Call this from your **Application\_Start** method, defined in Global.asax.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a><span data-ttu-id="e6f4b-214">İşleme döngüsel nesne başvuruları</span><span class="sxs-lookup"><span data-stu-id="e6f4b-214">Handling Circular Object References</span></span>

<span data-ttu-id="e6f4b-215">Varsayılan olarak, JSON ve XML biçimlendiricileri değerleri olarak tüm nesneler yazma.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-215">By default, the JSON and XML formatters write all objects as values.</span></span> <span data-ttu-id="e6f4b-216">İki özellik de aynı nesneye başvuruda bulunuyorsa veya aynı nesne iki kez koleksiyonunda görüntülenirse, biçimlendirici nesne iki kez serileştirir.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-216">If two properties refer to the same object, or if the same object appears twice in a collection, the formatter will serialize the object twice.</span></span> <span data-ttu-id="e6f4b-217">Nesne grafiği döngüsünü içeriyorsa grafiğinde bir döngü algıladığında seri hale getirici bir özel durum atar belirli bir sorunu olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-217">This is a particular problem if your object graph contains cycles, because the serializer will throw an exception when it detects a loop in the graph.</span></span>

<span data-ttu-id="e6f4b-218">Aşağıdaki nesne modelleri ve denetleyici göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-218">Consider the following object models and controller.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

<span data-ttu-id="e6f4b-219">Bu eylemi çağırma biçimlendirici için bir durum kodu 500 (Dahili Sunucu hatası) yanıt istemciye çeviren bir özel durum neden olur.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-219">Invoking this action will cause the formatter to thrown an exception, which translates to a status code 500 (Internal Server Error) response to the client.</span></span>

<span data-ttu-id="e6f4b-220">JSON içinde nesne başvuruları korumak için aşağıdaki kodu ekleyin **uygulama\_Başlat** yöntemi Global.asax dosyasında:</span><span class="sxs-lookup"><span data-stu-id="e6f4b-220">To preserve object references in JSON, add the following code to **Application\_Start** method in the Global.asax file:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

<span data-ttu-id="e6f4b-221">Denetleyici eylemini şöyle JSON şimdi döndürür:</span><span class="sxs-lookup"><span data-stu-id="e6f4b-221">Now the controller action will return JSON that looks like this:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

<span data-ttu-id="e6f4b-222">Seri hale getirici ekler bildirimi bir &quot;$id&quot; hem nesnelerini özelliğine.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-222">Notice that the serializer adds an &quot;$id&quot; property to both objects.</span></span> <span data-ttu-id="e6f4b-223">Ayrıca, değeri bir nesne başvurusu yerini şekilde Employee.Department özelliği'nın bir döngü oluşturuyor algılar: {&quot;$ref&quot;:&quot;1&quot;}.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-223">Also, it detects that the Employee.Department property creates a loop, so it replaces the value with an object reference: {&quot;$ref&quot;:&quot;1&quot;}.</span></span>

> [!NOTE]
> <span data-ttu-id="e6f4b-224">Nesne başvuruları JSON'de standart değildir.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-224">Object references are not standard in JSON.</span></span> <span data-ttu-id="e6f4b-225">Bu özelliği kullanmadan önce istemcilerinizin sonuçları ayrıştırabiliyor olup olmayacağını göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-225">Before using this feature, consider whether your clients will be able to parse the results.</span></span> <span data-ttu-id="e6f4b-226">Yalnızca döngüleri Grafikten kaldırmak daha iyi olabilir.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-226">It might be better simply to remove cycles from the graph.</span></span> <span data-ttu-id="e6f4b-227">Örneğin, çalışan bağlantıdan departmanı dön gerçekten bu örnekte, gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-227">For example, the link from Employee back to Department is not really needed in this example.</span></span>


<span data-ttu-id="e6f4b-228">Nesne başvuruları XML korumak için iki seçeneğiniz vardır.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-228">To preserve object references in XML, you have two options.</span></span> <span data-ttu-id="e6f4b-229">Eklemek için daha basit seçenektir `[DataContract(IsReference=true)]` modeli sınıfınıza.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-229">The simpler option is to add `[DataContract(IsReference=true)]` to your model class.</span></span> <span data-ttu-id="e6f4b-230">*IsReference* parametresi nesne başvuruları sağlar.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-230">The *IsReference* parameter enables object references.</span></span> <span data-ttu-id="e6f4b-231">Unutmayın **DataContract** eklemeniz gerekir böylece katılımı, serileştirme yapar **DataMember** öznitelikleri özellikler:</span><span class="sxs-lookup"><span data-stu-id="e6f4b-231">Remember that **DataContract** makes serialization opt-in, so you will also need to add **DataMember** attributes to the properties:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

<span data-ttu-id="e6f4b-232">Biçimlendirici XML benzer üretecektir artık aşağıdaki:</span><span class="sxs-lookup"><span data-stu-id="e6f4b-232">Now the formatter will produce XML similar to following:</span></span>

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

<span data-ttu-id="e6f4b-233">Model sınıfı özniteliklerinde önlemek istiyorsanız, başka bir seçenek yoktur: yeni bir tür özel Oluştur **DataContractSerializer** örneği ve ayarlama *preserveObjectReferences* için **true**  oluşturucuda.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-233">If you want to avoid attributes on your model class, there is another option: Create a new type-specific **DataContractSerializer** instance and set *preserveObjectReferences* to **true** in the constructor.</span></span> <span data-ttu-id="e6f4b-234">Ardından bu örnek XML medya türü biçimlendirici başına türü seri hale getirici ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-234">Then set this instance as a per-type serializer on the XML media-type formatter.</span></span> <span data-ttu-id="e6f4b-235">Aşağıdaki kod bunun nasıl yapılacağı gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="e6f4b-235">The following code show how to do this:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a><span data-ttu-id="e6f4b-236">Sınama nesnesi seri hale getirme</span><span class="sxs-lookup"><span data-stu-id="e6f4b-236">Testing Object Serialization</span></span>

<span data-ttu-id="e6f4b-237">Web API tasarlarken, veri nesnelerinizi serileştirilmiş nasıl test etmek kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-237">As you design your web API, it is useful to test how your data objects will be serialized.</span></span> <span data-ttu-id="e6f4b-238">Bir denetleyici oluşturma veya bir denetleyici eylemi çağırma bunu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e6f4b-238">You can do this without creating a controller or invoking a controller action.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
