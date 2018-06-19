---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: ASP.NET Web API ile OData v4 türleri Aç | Microsoft Docs
author: microsoft
description: OData v4 içinde bir açık tür tanımında bildirilen herhangi bir özellik yanı sıra Dinamik Özellikler içeren bir stuctured türü türüdür. Aç...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2014
ms.topic: article
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: fe67b9a11a82b55d5f3e0e5f1b0cee10a58833d2
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2018
ms.locfileid: "29153278"
---
<a name="open-types-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="754ff-104">ASP.NET Web API ile OData v4 türleri Aç</span><span class="sxs-lookup"><span data-stu-id="754ff-104">Open Types in OData v4 with ASP.NET Web API</span></span>
====================
<span data-ttu-id="754ff-105">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="754ff-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="754ff-106">OData v4 içinde bir *açmak türü* tür tanımında bildirilen herhangi bir özellik yanı sıra Dinamik Özellikler içeren bir stuctured türüdür.</span><span class="sxs-lookup"><span data-stu-id="754ff-106">In OData v4, an *open type* is a stuctured type that contains dynamic properties, in addition to any properties that are declared in the type definition.</span></span> <span data-ttu-id="754ff-107">Açık türleri, veri modelleri için esneklik eklemenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="754ff-107">Open types let you add flexibility to your data models.</span></span> <span data-ttu-id="754ff-108">Bu öğretici ASP.NET Web API OData açık türleri kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="754ff-108">This tutorial shows how to use open types in ASP.NET Web API OData.</span></span>
> 
> <span data-ttu-id="754ff-109">Bu öğreticide, ASP.NET Web API'de OData uç noktası oluşturmak nasıl zaten bildiğinizi varsayar.</span><span class="sxs-lookup"><span data-stu-id="754ff-109">This tutorial assumes that you already know how to create an OData endpoint in ASP.NET Web API.</span></span> <span data-ttu-id="754ff-110">Aksi takdirde, okuyarak başlamanız [bir OData v4 uç noktası oluşturma](create-an-odata-v4-endpoint.md) ilk.</span><span class="sxs-lookup"><span data-stu-id="754ff-110">If not, start by reading [Create an OData v4 Endpoint](create-an-odata-v4-endpoint.md) first.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="754ff-111">Öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="754ff-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="754ff-112">Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="754ff-112">Web API OData 5.3</span></span>
> - <span data-ttu-id="754ff-113">OData v4</span><span class="sxs-lookup"><span data-stu-id="754ff-113">OData v4</span></span>


<span data-ttu-id="754ff-114">İlk olarak, bazı OData terminolojisi:</span><span class="sxs-lookup"><span data-stu-id="754ff-114">First, some OData terminology:</span></span>

- <span data-ttu-id="754ff-115">Varlık türü: anahtar ile yapılandırılmış bir tür.</span><span class="sxs-lookup"><span data-stu-id="754ff-115">Entity type: A structured type with a key.</span></span>
- <span data-ttu-id="754ff-116">Karmaşık tür: bir anahtar olmadan yapılandırılmış bir tür.</span><span class="sxs-lookup"><span data-stu-id="754ff-116">Complex type: A structured type without a key.</span></span>
- <span data-ttu-id="754ff-117">Açık tür: dinamik özelliklere sahip bir tür.</span><span class="sxs-lookup"><span data-stu-id="754ff-117">Open type: A type with dynamic properties.</span></span> <span data-ttu-id="754ff-118">Varlık türleri ve karmaşık türler açık olabilir.</span><span class="sxs-lookup"><span data-stu-id="754ff-118">Both entity types and complex types can be open.</span></span>

<span data-ttu-id="754ff-119">Dinamik özelliğinin değeri bir ilkel tür, karmaşık tür veya numaralandırma türü olabilir; veya bu türlerinden herhangi birini koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="754ff-119">The value of a dynamic property can be a primitive type, complex type, or enumeration type; or a collection of any of those types.</span></span> <span data-ttu-id="754ff-120">Açık türleri hakkında daha fazla bilgi için bkz: [OData v4 belirtimi](http://www.odata.org/documentation/odata-version-4-0/).</span><span class="sxs-lookup"><span data-stu-id="754ff-120">For more information about open types, see the [OData v4 specification](http://www.odata.org/documentation/odata-version-4-0/).</span></span>

## <a name="install-the-web-odata-libraries"></a><span data-ttu-id="754ff-121">Web OData kitaplıkları yükleme</span><span class="sxs-lookup"><span data-stu-id="754ff-121">Install the Web OData Libraries</span></span>

<span data-ttu-id="754ff-122">NuGet paketi en son Web API OData kitaplıkları Yükleme Yöneticisi'ni kullanın.</span><span class="sxs-lookup"><span data-stu-id="754ff-122">Use NuGet Package Manager to install the latest Web API OData libraries.</span></span> <span data-ttu-id="754ff-123">Paket Yöneticisi konsolu penceresinden:</span><span class="sxs-lookup"><span data-stu-id="754ff-123">From the Package Manager Console window:</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a><span data-ttu-id="754ff-124">CLR türlerini tanımlayın</span><span class="sxs-lookup"><span data-stu-id="754ff-124">Define the CLR Types</span></span>

<span data-ttu-id="754ff-125">EDM modelleri CLR türleri olarak tanımlayarak başlatın.</span><span class="sxs-lookup"><span data-stu-id="754ff-125">Start by defining the EDM models as CLR types.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="754ff-126">Varlık veri modeli (EDM) oluşturulduğunda</span><span class="sxs-lookup"><span data-stu-id="754ff-126">When the Entity Data Model (EDM) is created,</span></span>

- <span data-ttu-id="754ff-127">`Category`bir numaralandırma türü değil.</span><span class="sxs-lookup"><span data-stu-id="754ff-127">`Category` is an enumeration type.</span></span>
- <span data-ttu-id="754ff-128">`Address`karmaşık bir tür değil.</span><span class="sxs-lookup"><span data-stu-id="754ff-128">`Address` is a complex type.</span></span> <span data-ttu-id="754ff-129">(Dolayısıyla bir varlık türü, bir anahtar yok.)</span><span class="sxs-lookup"><span data-stu-id="754ff-129">(It does not have a key, so it is not an entity type.)</span></span>
- <span data-ttu-id="754ff-130">`Customer`bir varlık türüdür.</span><span class="sxs-lookup"><span data-stu-id="754ff-130">`Customer` is an entity type.</span></span> <span data-ttu-id="754ff-131">(Bir anahtara sahip.)</span><span class="sxs-lookup"><span data-stu-id="754ff-131">(It has a key.)</span></span>
- <span data-ttu-id="754ff-132">`Press`açık bir karmaşık türü değil.</span><span class="sxs-lookup"><span data-stu-id="754ff-132">`Press` is an open complex type.</span></span>
- <span data-ttu-id="754ff-133">`Book`bir açık varlık türüdür.</span><span class="sxs-lookup"><span data-stu-id="754ff-133">`Book` is an open entity type.</span></span>

<span data-ttu-id="754ff-134">Açık bir tür oluşturmak için CLR türü türünde bir özellik olması gerekir `IDictionary<string, object>`, dinamik özellikleri içerir.</span><span class="sxs-lookup"><span data-stu-id="754ff-134">To create an open type, the CLR type must have a property of type `IDictionary<string, object>`, which holds the dynamic properties.</span></span>

## <a name="build-the-edm-model"></a><span data-ttu-id="754ff-135">EDM modeli oluşturma</span><span class="sxs-lookup"><span data-stu-id="754ff-135">Build the EDM Model</span></span>

<span data-ttu-id="754ff-136">Kullanırsanız **ODataConventionModelBuilder** EDM oluşturmak için `Press` ve `Book` olmadığına göre açık türü olarak otomatik olarak eklenen bir `IDictionary<string, object>` özelliği.</span><span class="sxs-lookup"><span data-stu-id="754ff-136">If you use **ODataConventionModelBuilder** to create the EDM, `Press` and `Book` are automatically added as open types, based on the presence of a `IDictionary<string, object>` property.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="754ff-137">Ayrıca EDM açıkça kullanarak oluşturabilirsiniz **ODataModelBuilder**.</span><span class="sxs-lookup"><span data-stu-id="754ff-137">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a><span data-ttu-id="754ff-138">Bir OData denetleyicisi ekleme</span><span class="sxs-lookup"><span data-stu-id="754ff-138">Add an OData Controller</span></span>

<span data-ttu-id="754ff-139">Ardından, bir OData denetleyicisi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="754ff-139">Next, add an OData controller.</span></span> <span data-ttu-id="754ff-140">Bu öğretici için yalnızca destekler GET ve POST istekleri ve bir bellek içi listesi varlıkları depolamak için kullandığını basitleştirilmiş bir denetleyici kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="754ff-140">For this tutorial, we'll use a simplified controller that just supports GET and POST requests, and uses an in-memory list to store entities.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

<span data-ttu-id="754ff-141">Dikkat ilk `Book` örneğinin dinamik özellik vardır.</span><span class="sxs-lookup"><span data-stu-id="754ff-141">Notice that the first `Book` instance has no dynamic properties.</span></span> <span data-ttu-id="754ff-142">İkinci `Book` örneği aşağıdaki dinamik özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="754ff-142">The second `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="754ff-143">"Yayımlanan": basit türü</span><span class="sxs-lookup"><span data-stu-id="754ff-143">"Published": Primitive type</span></span>
- <span data-ttu-id="754ff-144">"Yazar": ilkel türler koleksiyonu</span><span class="sxs-lookup"><span data-stu-id="754ff-144">"Authors": Collection of primitive types</span></span>
- <span data-ttu-id="754ff-145">"OtherCategories": numaralandırma türleri koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="754ff-145">"OtherCategories": Collection of enumeration types.</span></span>

<span data-ttu-id="754ff-146">Ayrıca, `Press` özelliği, `Book` örneği aşağıdaki dinamik özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="754ff-146">Also, the `Press` property of that `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="754ff-147">"Blog": basit türü</span><span class="sxs-lookup"><span data-stu-id="754ff-147">"Blog": Primitive type</span></span>
- <span data-ttu-id="754ff-148">"Adres": karmaşık türü</span><span class="sxs-lookup"><span data-stu-id="754ff-148">"Address": Complex type</span></span>

## <a name="query-the-metadata"></a><span data-ttu-id="754ff-149">Sorgu meta veriler</span><span class="sxs-lookup"><span data-stu-id="754ff-149">Query the Metadata</span></span>

<span data-ttu-id="754ff-150">OData meta veri belgesi almak için bir GET isteği Gönder `~/$metadata`.</span><span class="sxs-lookup"><span data-stu-id="754ff-150">To get the OData metadata document, send a GET request to `~/$metadata`.</span></span> <span data-ttu-id="754ff-151">Yanıt gövdesi şuna benzer görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="754ff-151">The response body should look similar to this:</span></span>

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

<span data-ttu-id="754ff-152">Meta veri belgeden görebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="754ff-152">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="754ff-153">İçin `Book` ve `Press` türleri, değeri `OpenType` özniteliği doğrudur.</span><span class="sxs-lookup"><span data-stu-id="754ff-153">For the `Book` and `Press` types, the value of the `OpenType` attribute is true.</span></span> <span data-ttu-id="754ff-154">`Customer` Ve `Address` türleri, bu öznitelik zorunda kalmaz.</span><span class="sxs-lookup"><span data-stu-id="754ff-154">The `Customer` and `Address` types don't have this attribute.</span></span>
- <span data-ttu-id="754ff-155">`Book` Varlık türüne sahip üç bildirilmiş özellikleri: ISBN, başlık ve tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="754ff-155">The `Book` entity type has three declared properties: ISBN, Title, and Press.</span></span> <span data-ttu-id="754ff-156">OData meta veri içermemesi `Book.Properties` CLR sınıfından özelliği.</span><span class="sxs-lookup"><span data-stu-id="754ff-156">The OData metadata does not include the `Book.Properties` property from the CLR class.</span></span>
- <span data-ttu-id="754ff-157">Benzer şekilde, `Press` karmaşık türü var. yalnızca iki bildirilmiş özellikleri: adı ve kategori.</span><span class="sxs-lookup"><span data-stu-id="754ff-157">Similarly, the `Press` complex type has only two declared properties: Name and Category.</span></span> <span data-ttu-id="754ff-158">Meta veri içermemesi `Press.DynamicProperties` CLR sınıfından özelliği.</span><span class="sxs-lookup"><span data-stu-id="754ff-158">The metadata does not include the `Press.DynamicProperties` property from the CLR class.</span></span>

## <a name="query-an-entity"></a><span data-ttu-id="754ff-159">Sorgu bir varlık</span><span class="sxs-lookup"><span data-stu-id="754ff-159">Query an Entity</span></span>

<span data-ttu-id="754ff-160">ISBN defteriyle "978-0-7356-7942-9 için" eşit almak için bir GET isteği Gönder `~/Books('978-0-7356-7942-9')`.</span><span class="sxs-lookup"><span data-stu-id="754ff-160">To get the book with ISBN equal to "978-0-7356-7942-9", send a GET request to `~/Books('978-0-7356-7942-9')`.</span></span> <span data-ttu-id="754ff-161">Yanıt gövdesi aşağıdakine benzer görünmelidir.</span><span class="sxs-lookup"><span data-stu-id="754ff-161">The response body should look similar to the following.</span></span> <span data-ttu-id="754ff-162">(Daha okunabilir yapmak için girintili.)</span><span class="sxs-lookup"><span data-stu-id="754ff-162">(Indented to make it more readable.)</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

<span data-ttu-id="754ff-163">Dinamik özellikler dahil satır içi bildirilen özelliklere sahip olduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="754ff-163">Notice that the dynamic properties are included inline with the declared properties.</span></span>

## <a name="post-an-entity"></a><span data-ttu-id="754ff-164">Bir varlık sonrası</span><span class="sxs-lookup"><span data-stu-id="754ff-164">POST an Entity</span></span>

<span data-ttu-id="754ff-165">Kitap varlık eklemek için bir POST isteği Gönder `~/Books`.</span><span class="sxs-lookup"><span data-stu-id="754ff-165">To add a Book entity, send a POST request to `~/Books`.</span></span> <span data-ttu-id="754ff-166">İstemci istek yükünde dinamik özellikleri ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="754ff-166">The client can set dynamic properties in the request payload.</span></span>

<span data-ttu-id="754ff-167">Burada, örnek istek verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="754ff-167">Here is an example request.</span></span> <span data-ttu-id="754ff-168">"Price" ve "Yayımlanan" özellikleri unutmayın.</span><span class="sxs-lookup"><span data-stu-id="754ff-168">Note the "Price" and "Published" properties.</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

<span data-ttu-id="754ff-169">Denetleyici yönteminde kesme noktası ayarlarsanız, Web API için bu özellikleri eklenen görebilirsiniz `Properties` sözlük.</span><span class="sxs-lookup"><span data-stu-id="754ff-169">If you set a breakpoint in the controller method, you can see that Web API added these properties to the `Properties` dictionary.</span></span>

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a><span data-ttu-id="754ff-170">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="754ff-170">Additional Resources</span></span>

[<span data-ttu-id="754ff-171">OData açık türü örneği</span><span class="sxs-lookup"><span data-stu-id="754ff-171">OData Open Type Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
