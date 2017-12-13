---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: "ASP.NET Web API HTML Form verileri gönderme: dosya karşıya yükleme ve çok parçalı MIME | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/21/2012
ms.topic: article
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: 3df59aab2a0c43f4a4f5c59530b0655f68d95cc7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a><span data-ttu-id="b7d64-102">ASP.NET Web API HTML Form verileri gönderme: dosya karşıya yükleme ve çok parçalı MIME</span><span class="sxs-lookup"><span data-stu-id="b7d64-102">Sending HTML Form Data in ASP.NET Web API: File Upload and Multipart MIME</span></span>
====================
<span data-ttu-id="b7d64-103">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b7d64-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-2-file-upload-and-multipart-mime"></a><span data-ttu-id="b7d64-104">2. Kısım: Karşıya dosya yükleme ve çok parçalı MIME</span><span class="sxs-lookup"><span data-stu-id="b7d64-104">Part 2: File Upload and Multipart MIME</span></span>

<span data-ttu-id="b7d64-105">Bu öğretici bir web API dosyaları karşıya nasıl yükleneceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="b7d64-105">This tutorial shows how to upload files to a web API.</span></span> <span data-ttu-id="b7d64-106">Ayrıca, çok parçalı MIME verileri işlemek nasıl açıklanır.</span><span class="sxs-lookup"><span data-stu-id="b7d64-106">It also describes how to process multipart MIME data.</span></span>

> [!NOTE]
> <span data-ttu-id="b7d64-107">[Tamamlanan projenizi indirin](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span><span class="sxs-lookup"><span data-stu-id="b7d64-107">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span></span>


<span data-ttu-id="b7d64-108">Bir dosyayı karşıya yükleme için bir HTML formuna bir örneği burada verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="b7d64-108">Here is an example of an HTML form for uploading a file:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

<span data-ttu-id="b7d64-109">Bu form, metin girişi denetiminin ve dosya giriş denetimi içerir.</span><span class="sxs-lookup"><span data-stu-id="b7d64-109">This form contains a text input control and a file input control.</span></span> <span data-ttu-id="b7d64-110">Bir form dosya giriş denetiminin içerdiğinde **enctype** özniteliği her zaman olmalıdır &quot;multipart/form-data&quot;, belirten formun çok parçalı MIME ileti olarak gönderilir.</span><span class="sxs-lookup"><span data-stu-id="b7d64-110">When a form contains a file input control, the **enctype** attribute should always be &quot;multipart/form-data&quot;, which specifies that the form will be sent as a multipart MIME message.</span></span>

<span data-ttu-id="b7d64-111">Çok parçalı MIME ileti biçimi bir örnek isteğiyle bakarak anlamak oldukça kolaydır:</span><span class="sxs-lookup"><span data-stu-id="b7d64-111">The format of a multipart MIME message is easiest to understand by looking at an example request:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

<span data-ttu-id="b7d64-112">Bu ileti ikiye ayrılmıştır *bölümleri*, her form denetimi için bir tane.</span><span class="sxs-lookup"><span data-stu-id="b7d64-112">This message is divided into two *parts*, one for each form control.</span></span> <span data-ttu-id="b7d64-113">Bölümü sınırları, kısa çizgi ile başlayan satırlar tarafından belirtilir.</span><span class="sxs-lookup"><span data-stu-id="b7d64-113">Part boundaries are indicated by the lines that start with dashes.</span></span>

> [!NOTE]
> <span data-ttu-id="b7d64-114">Rastgele bir bileşen bölümü sınır içerir (&quot;41184676334&quot;) sınır dizesi içinde ileti bölümü yanlışlıkla görünmez emin olmak için.</span><span class="sxs-lookup"><span data-stu-id="b7d64-114">The part boundary includes a random component (&quot;41184676334&quot;) to ensure that the boundary string does not accidentally appear inside a message part.</span></span>


<span data-ttu-id="b7d64-115">Her ileti parçası bölümü içeriğine göre izlenen bir veya daha fazla üstbilgileri içerir.</span><span class="sxs-lookup"><span data-stu-id="b7d64-115">Each message part contains one or more headers, followed by the part contents.</span></span>

- <span data-ttu-id="b7d64-116">İçerik düzeni üstbilgisini denetiminin adını içerir.</span><span class="sxs-lookup"><span data-stu-id="b7d64-116">The Content-Disposition header includes the name of the control.</span></span> <span data-ttu-id="b7d64-117">Dosyalar için dosya adı da içerir.</span><span class="sxs-lookup"><span data-stu-id="b7d64-117">For files, it also contains the file name.</span></span>
- <span data-ttu-id="b7d64-118">Content-Type üstbilgisi bölümündeki verileri açıklar.</span><span class="sxs-lookup"><span data-stu-id="b7d64-118">The Content-Type header describes the data in the part.</span></span> <span data-ttu-id="b7d64-119">Bu üst atlanırsa, varsayılan metin/düz ' dir.</span><span class="sxs-lookup"><span data-stu-id="b7d64-119">If this header is omitted, the default is text/plain.</span></span>

<span data-ttu-id="b7d64-120">Önceki örnekte, kullanıcı GrandCanyon.jpg, içerik türü görüntü/jpeg ile adlı bir dosyayı karşıya; ve metin giriş değeri &quot;Yaz tatil&quot;.</span><span class="sxs-lookup"><span data-stu-id="b7d64-120">In the previous example, the user uploaded a file named GrandCanyon.jpg, with content type image/jpeg; and the value of the text input was &quot;Summer Vacation&quot;.</span></span>

## <a name="file-upload"></a><span data-ttu-id="b7d64-121">Karşıya dosya yükleme</span><span class="sxs-lookup"><span data-stu-id="b7d64-121">File Upload</span></span>

<span data-ttu-id="b7d64-122">Artık çok parçalı MIME iletiden dosyaları okuyan Web API denetleyicisi bakalım.</span><span class="sxs-lookup"><span data-stu-id="b7d64-122">Now let's look at a Web API controller that reads files from a multipart MIME message.</span></span> <span data-ttu-id="b7d64-123">Denetleyici dosyaları zaman uyumsuz olarak okur.</span><span class="sxs-lookup"><span data-stu-id="b7d64-123">The controller will read the files asynchronously.</span></span> <span data-ttu-id="b7d64-124">Web API kullanarak zaman uyumsuz eylemleri destekleyen [görev tabanlı programlama modeli](https://msdn.microsoft.com/library/dd460693.aspx).</span><span class="sxs-lookup"><span data-stu-id="b7d64-124">Web API supports asynchronous actions using the [task-based programming model](https://msdn.microsoft.com/library/dd460693.aspx).</span></span> <span data-ttu-id="b7d64-125">İlk olarak, işte kodu destekleyen .NET Framework 4.5 hedefliyorsanız **zaman uyumsuz** ve **await** anahtar sözcükler.</span><span class="sxs-lookup"><span data-stu-id="b7d64-125">First, here is the code if you are targeting .NET Framework 4.5, which supports the **async** and **await** keywords.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

<span data-ttu-id="b7d64-126">Denetleyici eylemini herhangi bir parametre almaz dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="b7d64-126">Notice that the controller action does not take any parameters.</span></span> <span data-ttu-id="b7d64-127">Medya türü biçimlendiricisi çağırmadan biz eylem içinde istek gövdesini işlemek olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="b7d64-127">That's because we process the request body inside the action, without invoking a media-type formatter.</span></span>

<span data-ttu-id="b7d64-128">**IsMultipartContent** yöntemi istek çok parçalı MIME ileti içerip içermediğini denetler.</span><span class="sxs-lookup"><span data-stu-id="b7d64-128">The **IsMultipartContent** method checks whether the request contains a multipart MIME message.</span></span> <span data-ttu-id="b7d64-129">Aksi durumda, denetleyici HTTP durum kodu 415 (desteklenmeyen medya türü) döndürür.</span><span class="sxs-lookup"><span data-stu-id="b7d64-129">If not, the controller returns HTTP status code 415 (Unsupported Media Type).</span></span>

<span data-ttu-id="b7d64-130">**MultipartFormDataStreamProvider** sınıftır karşıya yüklenen dosyalar için dosya akışları ayıran bir yardımcı nesnesi.</span><span class="sxs-lookup"><span data-stu-id="b7d64-130">The **MultipartFormDataStreamProvider** class is a helper object that allocates file streams for uploaded files.</span></span> <span data-ttu-id="b7d64-131">Çok parçalı MIME ileti okumak için çağırın **ReadAsMultipartAsync** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b7d64-131">To read the multipart MIME message, call the **ReadAsMultipartAsync** method.</span></span> <span data-ttu-id="b7d64-132">Bu yöntem tüm ileti parçalarının ayıklar ve bunları tarafından sağlanan akışlar Yazar **MultipartFormDataStreamProvider**.</span><span class="sxs-lookup"><span data-stu-id="b7d64-132">This method extracts all of the message parts and writes them into the streams provided by the **MultipartFormDataStreamProvider**.</span></span>

<span data-ttu-id="b7d64-133">Yöntem tamamlandığında dosyaları hakkında bilgi edinebilirsiniz **FileData** bir koleksiyon özelliği, **MultipartFileData** nesneleri.</span><span class="sxs-lookup"><span data-stu-id="b7d64-133">When the method completes, you can get information about the files from the **FileData** property, which is a collection of **MultipartFileData** objects.</span></span>

- <span data-ttu-id="b7d64-134">**MultipartFileData.FileName** dosyasının kaydedildiği sunucusunda, yerel dosya adıdır.</span><span class="sxs-lookup"><span data-stu-id="b7d64-134">**MultipartFileData.FileName** is the local file name on the server, where the file was saved.</span></span>
- <span data-ttu-id="b7d64-135">**MultipartFileData.Headers** bölümü üstbilgisi içeriyor (*değil* istek üstbilgisi).</span><span class="sxs-lookup"><span data-stu-id="b7d64-135">**MultipartFileData.Headers** contains the part header (*not* the request header).</span></span> <span data-ttu-id="b7d64-136">Bu içeriğe erişmek için kullanabileceğiniz\_değerlendirme ve Content-Type üst bilgileri.</span><span class="sxs-lookup"><span data-stu-id="b7d64-136">You can use this to access the Content\_Disposition and Content-Type headers.</span></span>

<span data-ttu-id="b7d64-137">Adı da anlaşılacağı gibi **ReadAsMultipartAsync** zaman uyumsuz bir yöntemdir.</span><span class="sxs-lookup"><span data-stu-id="b7d64-137">As the name suggests, **ReadAsMultipartAsync** is an asynchronous method.</span></span> <span data-ttu-id="b7d64-138">Yöntemi tamamlandıktan sonra çalışmayı gerçekleştirmek için kullanın bir [devamlılık görevi](https://msdn.microsoft.com/en-us/library/ee372288.aspx) (.NET 4.0) veya **await** anahtar sözcüğü (.NET 4.5).</span><span class="sxs-lookup"><span data-stu-id="b7d64-138">To perform work after the method completes, use a [continuation task](https://msdn.microsoft.com/en-us/library/ee372288.aspx) (.NET 4.0) or the **await** keyword (.NET 4.5).</span></span>

<span data-ttu-id="b7d64-139">Önceki kod .NET Framework 4.0 sürümünü şöyledir:</span><span class="sxs-lookup"><span data-stu-id="b7d64-139">Here is the .NET Framework 4.0 version of the previous code:</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a><span data-ttu-id="b7d64-140">Okuma formu denetim verileri</span><span class="sxs-lookup"><span data-stu-id="b7d64-140">Reading Form Control Data</span></span>

<span data-ttu-id="b7d64-141">Daha önce gösterilen HTML formu metin girişi denetiminin vardı.</span><span class="sxs-lookup"><span data-stu-id="b7d64-141">The HTML form that I showed earlier had a text input control.</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

<span data-ttu-id="b7d64-142">Denetimden değerini alabilir **FormData** özelliği **MultipartFormDataStreamProvider**.</span><span class="sxs-lookup"><span data-stu-id="b7d64-142">You can get the value of the control from the **FormData** property of the **MultipartFormDataStreamProvider**.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

<span data-ttu-id="b7d64-143">**FormData** olan bir **NameValueCollection** form denetimlerini için ad/değer çiftleri içerir.</span><span class="sxs-lookup"><span data-stu-id="b7d64-143">**FormData** is a **NameValueCollection** that contains name/value pairs for the form controls.</span></span> <span data-ttu-id="b7d64-144">Koleksiyon yinelenen anahtarlar içerebilir.</span><span class="sxs-lookup"><span data-stu-id="b7d64-144">The collection can contain duplicate keys.</span></span> <span data-ttu-id="b7d64-145">Bu form göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="b7d64-145">Consider this form:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

<span data-ttu-id="b7d64-146">İstek gövdesini şuna benzeyebilir:</span><span class="sxs-lookup"><span data-stu-id="b7d64-146">The request body might look like this:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

<span data-ttu-id="b7d64-147">Bu durumda, **FormData** aşağıdaki anahtar/değer çiftleri koleksiyonu içerebilir:</span><span class="sxs-lookup"><span data-stu-id="b7d64-147">In that case, the **FormData** collection would contain the following key/value pairs:</span></span>

- <span data-ttu-id="b7d64-148">Seyahat: gidiş</span><span class="sxs-lookup"><span data-stu-id="b7d64-148">trip: round-trip</span></span>
- <span data-ttu-id="b7d64-149">Seçenekler: nonstop</span><span class="sxs-lookup"><span data-stu-id="b7d64-149">options: nonstop</span></span>
- <span data-ttu-id="b7d64-150">Seçenekler: tarihleri</span><span class="sxs-lookup"><span data-stu-id="b7d64-150">options: dates</span></span>
- <span data-ttu-id="b7d64-151">Bilgisayar başına: penceresi</span><span class="sxs-lookup"><span data-stu-id="b7d64-151">seat: window</span></span>
