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
ms.openlocfilehash: 331d0e520a1fd8ec84aecd09a9c9e6d286c5893b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a>ASP.NET Web API HTML Form verileri gönderme: dosya karşıya yükleme ve çok parçalı MIME
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

## <a name="part-2-file-upload-and-multipart-mime"></a>2. Kısım: Karşıya dosya yükleme ve çok parçalı MIME

Bu öğretici bir web API dosyaları karşıya nasıl yükleneceğini gösterir. Ayrıca, çok parçalı MIME verileri işlemek nasıl açıklanır.

> [!NOTE]
> [Tamamlanan projenizi indirin](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).


Bir dosyayı karşıya yükleme için bir HTML formuna bir örneği burada verilmiştir:

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

Bu form, metin girişi denetiminin ve dosya giriş denetimi içerir. Bir form dosya giriş denetiminin içerdiğinde **enctype** özniteliği her zaman olmalıdır &quot;multipart/form-data&quot;, belirten formun çok parçalı MIME ileti olarak gönderilir.

Çok parçalı MIME ileti biçimi bir örnek isteğiyle bakarak anlamak oldukça kolaydır:

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

Bu ileti ikiye ayrılmıştır *bölümleri*, her form denetimi için bir tane. Bölümü sınırları, kısa çizgi ile başlayan satırlar tarafından belirtilir.

> [!NOTE]
> Rastgele bir bileşen bölümü sınır içerir (&quot;41184676334&quot;) sınır dizesi içinde ileti bölümü yanlışlıkla görünmez emin olmak için.


Her ileti parçası bölümü içeriğine göre izlenen bir veya daha fazla üstbilgileri içerir.

- İçerik düzeni üstbilgisini denetiminin adını içerir. Dosyalar için dosya adı da içerir.
- Content-Type üstbilgisi bölümündeki verileri açıklar. Bu üst atlanırsa, varsayılan metin/düz ' dir.

Önceki örnekte, kullanıcı GrandCanyon.jpg, içerik türü görüntü/jpeg ile adlı bir dosyayı karşıya; ve metin giriş değeri &quot;Yaz tatil&quot;.

## <a name="file-upload"></a>Karşıya dosya yükleme

Artık çok parçalı MIME iletiden dosyaları okuyan Web API denetleyicisi bakalım. Denetleyici dosyaları zaman uyumsuz olarak okur. Web API kullanarak zaman uyumsuz eylemleri destekleyen [görev tabanlı programlama modeli](https://msdn.microsoft.com/library/dd460693.aspx). İlk olarak, işte kodu destekleyen .NET Framework 4.5 hedefliyorsanız **zaman uyumsuz** ve **await** anahtar sözcükler.

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

Denetleyici eylemini herhangi bir parametre almaz dikkat edin. Medya türü biçimlendiricisi çağırmadan biz eylem içinde istek gövdesini işlemek olmasıdır.

**IsMultipartContent** yöntemi istek çok parçalı MIME ileti içerip içermediğini denetler. Aksi durumda, denetleyici HTTP durum kodu 415 (desteklenmeyen medya türü) döndürür.

**MultipartFormDataStreamProvider** sınıftır karşıya yüklenen dosyalar için dosya akışları ayıran bir yardımcı nesnesi. Çok parçalı MIME ileti okumak için çağırın **ReadAsMultipartAsync** yöntemi. Bu yöntem tüm ileti parçalarının ayıklar ve bunları tarafından sağlanan akışlar Yazar **MultipartFormDataStreamProvider**.

Yöntem tamamlandığında dosyaları hakkında bilgi edinebilirsiniz **FileData** bir koleksiyon özelliği, **MultipartFileData** nesneleri.

- **MultipartFileData.FileName** dosyasının kaydedildiği sunucusunda, yerel dosya adıdır.
- **MultipartFileData.Headers** bölümü üstbilgisi içeriyor (*değil* istek üstbilgisi). Bu içeriğe erişmek için kullanabileceğiniz\_değerlendirme ve Content-Type üst bilgileri.

Adı da anlaşılacağı gibi **ReadAsMultipartAsync** zaman uyumsuz bir yöntemdir. Yöntemi tamamlandıktan sonra çalışmayı gerçekleştirmek için kullanın bir [devamlılık görevi](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) veya **await** anahtar sözcüğü (.NET 4.5).

Önceki kod .NET Framework 4.0 sürümünü şöyledir:

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a>Okuma formu denetim verileri

Daha önce gösterilen HTML formu metin girişi denetiminin vardı.

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

Denetimden değerini alabilir **FormData** özelliği **MultipartFormDataStreamProvider**.

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

**FormData** olan bir **NameValueCollection** form denetimlerini için ad/değer çiftleri içerir. Koleksiyon yinelenen anahtarlar içerebilir. Bu form göz önünde bulundurun:

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

İstek gövdesini şuna benzeyebilir:

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

Bu durumda, **FormData** aşağıdaki anahtar/değer çiftleri koleksiyonu içerebilir:

- Seyahat: gidiş
- Seçenekler: nonstop
- Seçenekler: tarihleri
- Bilgisayar başına: penceresi
