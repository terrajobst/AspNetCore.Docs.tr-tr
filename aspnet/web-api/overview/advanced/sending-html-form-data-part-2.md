---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: "ASP.NET Web API'de HTML Form verileri gönderme: karşıya dosya yükleme ve çok parçalı MIME | Microsoft Docs"
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/21/2012
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: 875f9ac62901dfbafc8224af2982c1daf3afc9c5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757024"
---
<a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a>ASP.NET Web API'de HTML Form verileri gönderme: karşıya dosya yükleme ve çok parçalı MIME
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

## <a name="part-2-file-upload-and-multipart-mime"></a>2. Bölüm: Karşıya dosya yükleme ve çok parçalı MIME

Bu öğreticide, bir web API'sine dosyaları karşıya yükleme işlemi gösterilmektedir. Ayrıca, çok parçalı MIME verilerin nasıl işleneceğini açıklar.

> [!NOTE]
> [Tamamlanmış projeyi indirmek](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).


Bir dosyayı yüklemek için bir HTML formuna bir örneği aşağıda verilmiştir:

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

Bu form, metin girişi denetimi ve dosya giriş denetimi içerir. Bir formu bir dosya giriş denetimini içerdiğinde **Notenctype** özniteliği uymanız gereken &quot;multipart/form-data&quot;, belirten form çok parçalı MIME ileti gönderilir.

Çok parçalı MIME ileti biçimi bir örnek isteğiyle bakarak anlamak oldukça kolaydır:

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

Bu ileti ikiye bölünür *bölümleri*, her form denetimi için bir tane. Bölüm sınırları, kısa çizgi ile başlayan satırları tarafından belirtilir.

> [!NOTE]
> Bölüm sınırının rastgele bir bileşeni içerir (&quot;41184676334&quot;) sınır dize yanlışlıkla bir ileti bölümü içinde görünmüyor emin olmak için.


Bölüm içeriğine göre ve ardından bir veya daha fazla üst bilgileri, her ileti bölümü içerir.

- İçerik düzeni üstbilgisini denetimin adını içerir. Dosyalar için dosya adı da içerir.
- Content-Type üstbilgisi veri bölümünde açıklanır. Bu üst bilgisi çıkarılırsa, metin/düz varsayılandır.

Önceki örnekte, kullanıcı GrandCanyon.jpg, içerik türü görüntü/jpeg ile adlı bir dosya karşıya; ve metin girişi değerini &quot;Yaz tatil&quot;.

## <a name="file-upload"></a>Karşıya dosya yükleme

Artık dosyaları çok parçalı MIME iletiden okuyan bir Web API denetleyicisi göz atalım. Denetleyici dosyaları zaman uyumsuz olarak okur. Web API'si kullanarak zaman uyumsuz eylemleri destekleyen [görev-tabanlı programlama modeli](https://msdn.microsoft.com/library/dd460693.aspx). İlk olarak, kod aşağıdaki gibidir destekleyen .NET Framework 4.5 hedefliyorsanız **zaman uyumsuz** ve **await** anahtar sözcükleri.

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

Denetleyici eylemini herhangi bir parametre almaz dikkat edin. Medya türü biçimlendiricisi çağırmadan biz eylem içinde istek gövdesini işlemek olmasıdır.

**IsMultipartContent** yöntemi isteği çok parçalı MIME ileti içerip içermediğini denetler. Aksi durumda, denetleyici, HTTP durum kodu 415 (desteklenmeyen medya türü) döndürür.

**MultipartFormDataStreamProvider** sınıfı, karşıya yüklenen dosyalar için dosya akışları ayıran bir yardımcı nesnesi. MIME çok bölümlü iletinin okumak için çağrı **ReadAsMultipartAsync** yöntemi. Bu yöntem tüm ileti parçalarını ayıklar ve bunları tarafından sağlanan akışları Yazar **MultipartFormDataStreamProvider**.

Yöntemi tamamlandığında, dosyaları hakkında bilgi edinebilirsiniz **FileData** koleksiyonu özelliğinin, **MultipartFileData** nesneleri.

- **MultipartFileData.FileName** dosyanın kaydedildiği sunucusunda, yerel dosya adı.
- **MultipartFileData.Headers** bölüm başlığı içerir (*değil* istek üst bilgisi). Bu içeriğe erişmek için kullanabileceğiniz\_değerlendirme ve Content-Type üst bilgileri.

Adından da anlaşılacağı gibi **ReadAsMultipartAsync** zaman uyumsuz bir yöntemdir. Yöntemi tamamlandığında iş gerçekleştirmek için bir [devamlılık görevi](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) veya **await** anahtar sözcüğü (.NET 4.5).

Önceki kod .NET Framework 4.0 sürümünü şu şekildedir:

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a>Form denetimi verileri okuma

Daha önce gösterilen HTML formu, bir metin girişi denetimi vardı.

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

Denetim değer elde edebileceği **çıkışlardan form verisi** özelliği **MultipartFormDataStreamProvider**.

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

**Çıkışlardan form verisi** olduğu bir **NameValueCollection** , form denetimleri için ad/değer çiftleri içerir. Koleksiyonda yinelenen anahtarlar içerebilir. Bu formu göz önünde bulundurun:

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

İstek gövdesi şuna benzeyebilir:

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

Bu durumda, **çıkışlardan form verisi** aşağıdaki anahtar/değer çiftleri koleksiyonu içerebilir:

- Seyahat: gidiş dönüş
- Seçenekler: nonstop
- Seçenekler: tarih
- Lisans: penceresi
