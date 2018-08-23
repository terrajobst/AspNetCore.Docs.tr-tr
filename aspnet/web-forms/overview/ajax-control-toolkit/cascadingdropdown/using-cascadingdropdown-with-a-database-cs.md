---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-cs
title: (C#) veritabanı ile CascadingDropDown kullanma | Microsoft Docs
author: wenz
description: Bir DropDownList yükleri değişiklikleri anoth değerleri ilişkili böylece AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 684f0c28-a490-4e5b-b5e5-5dfb77464b49
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-cs
msc.type: authoredcontent
ms.openlocfilehash: ed5057ee942ce57503b038cbd856fefaa3d287ce
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756274"
---
<a name="using-cascadingdropdown-with-a-database-c"></a>(C#) veritabanı ile CascadingDropDown kullanma
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1CS.pdf)

> Bir DropDownList yükleri değişiklikleri başka bir DropDownList değerleri ilişkili böylece AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir. Bunun çalışması sırada bir özel web hizmeti oluşturulması gerekir.


## <a name="overview"></a>Genel Bakış

Bir DropDownList yükleri değişiklikleri başka bir DropDownList değerleri ilişkili böylece AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir. (Örneği için BİZE durumları listesini bir liste sağlar ve sonraki listesi, bu durumda bulunan büyük şehirlerin ile doldurulur.) Bunun çalışması sırada bir özel web hizmeti oluşturulması gerekir.

## <a name="steps"></a>Adımlar

İlk olarak bir veri kaynağı gereklidir. Bu örnekte, AdventureWorks veritabanını ve Microsoft SQL Server 2005 Express Edition kullanır. Veritabanı (express sürüm dahil) bir Visual Studio yüklemesi isteğe bağlı bir parçasıdır ve ayrıca altında ayrı bir indirme olarak kullanılabilir [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). AdventureWorks veritabanı SQL Server 2005 örnekleri ve örnek veritabanları parçasıdır (adresinden [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = tr](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). En kolay yolu, veritabanını ayarlamak için Microsoft SQL Server Management Studio Express kullanmaktır ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = tr](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) ve ekleme `AdventureWorks.mdf` veritabanı dosyası.

Bu örnek için SQL Server 2005 Express Edition örneğini çağrıldığından emin olan varsayıyoruz `SQLEXPRESS` ve web sunucusu; ile aynı makinede bulunan varsayılan kurulumu da budur. Kurulumunuzu farklıysa, veritabanı için bağlantı bilgilerini uymak zorunda.

ASP.NET AJAX Denetim Araç Seti ve işlevlerini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir sayfada (ancak içinde &lt; `form` &gt; öğesi):

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample1.aspx)]

Sonraki adımda, iki DropDownList denetimi gereklidir. Bu örnekte, AdventureWorks satıcı ve ilgili kişi bilgileri kullanırız, bu nedenle bir liste kullanılabilir satıcılar, biri kullanılabilir kişi oluşturacağız:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample2.aspx)]

Ardından, iki CascadingDropDown Genişleticileri sayfaya eklenmesi gerekir. Bir ilk (Satıcılar) listesini doldurur ve diğeri ikinci (kişi) listesi girer. Aşağıdaki öznitelikler ayarlamanız gerekir:

- `ServicePath`: Liste girişlerini sunan bir web hizmeti URL'si
- `ServiceMethod`: Liste girişlerini sunma web metodu
- `TargetControlID`: Açılan listenin kimliği
- `Category`: Web yöntemi çağrıldığında gönderilen kategori bilgileri
- `PromptText`: Zaman uyumsuz olarak listesi verileri sunucudan yüklenirken görüntülenen metin
- `ParentControlID`: (isteğe bağlı) üst açılır listede, şu anki listenin Tetikleyicileri yüklenmesi

Kullanılan programlama diline bağlı olarak söz konusu web hizmeti adını değiştirir, ancak diğer öznitelik değerleri aynıdır. CascadingDropDown öğe için ilk açılan listede aşağıda verilmiştir:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample3.aspx)]

İkinci liste için Denetim genişleticilerini gerekir `ParentControlID` yükleniyor satıcıları listesi Tetikleyicileri girişi seçerek ilişkili kişiler listesi öğeleri, öznitelik.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample4.aspx)]

Asıl işi, ardından aşağıdaki ayarlamaları yapın web hizmetinde gerçekleştirilir. Unutmayın `[ScriptService]` özniteliği kullanılır, aksi takdirde, ASP.NET AJAX istemci tarafı komut dosyası kodunu web yöntemlere erişmek için JavaScript proxy'si oluşturulamıyor.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample5.aspx)]

CascadingDropDown tarafından çağrılan web yöntemler imzası aşağıdaki gibidir:

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample6.cs)]

Dönüş değeri bir dizi türünde olmalıdır, böylece `CascadingDropDownNameValue` Denetim Araç Seti tarafından tanımlanır. `GetVendors()` Yöntemi uygulamak oldukça kolay: kod AdventureWorks veritabanına bağlanır ve ilk 25 satıcıları sorgular. İlk parametre `CascadingDropDownNameValue` Oluşturucusu değerini başlıktır ikinci bir liste girdisi (öznitelik HTML'nin &lt; `option` &gt; öğesi). Kod aşağıdaki gibidir:

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample7.cs)]

İlişkili kişiler için satıcı alma (yöntem adı: `GetContactsForVendor()`) biraz zor olduğu. İlk olarak, ilk açılan listede seçili satıcı belirlenmesi gerekir. Denetim Araç Seti, görev için bir yardımcı yöntem tanımlar: `ParseKnownCategoryValuesString()` yöntemi döndürür bir `StringDictionary` açılan verilerle öğesi:

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample8.cs)]

Güvenlik nedeniyle, bu verileri önce doğrulanması gerekir. Bir satıcı girişi varsa bunu (çünkü `Category` özelliği ilk CascadingDropDown öğenin `"Vendor"`), seçili satıcı kimliği alınabilir:

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample9.cs)]

Yöntemin geri kalanını oldukça rahatça, ise. Satıcı Kimliği bir parametre olarak, tüm ilişkili kişiler için satıcıya alır bir SQL sorgusu için kullanılır. Bir kez daha, yöntem türünde bir dizi döndürür `CascadingDropDownNameValue`.

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample10.cs)]

ASP.NET sayfasını yüklemek ve kısa bir süre sonra satıcı listesini 25 girişleri ile doldurulur. Bir girişi seçin ve açılır listeden ikinci veri ile doldurulan nasıl dikkat edin.


[![İlk liste otomatik olarak doldurulur.](using-cascadingdropdown-with-a-database-cs/_static/image2.png)](using-cascadingdropdown-with-a-database-cs/_static/image1.png)

İlk liste otomatik olarak doldurulur ([tam boyutlu görüntüyü görmek için tıklatın](using-cascadingdropdown-with-a-database-cs/_static/image3.png))


[![İkinci liste ilk listede seçime göre doldurulur.](using-cascadingdropdown-with-a-database-cs/_static/image5.png)](using-cascadingdropdown-with-a-database-cs/_static/image4.png)

İkinci liste ilk listede seçime göre doldurulur ([tam boyutlu görüntüyü görmek için tıklatın](using-cascadingdropdown-with-a-database-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Önceki](filling-a-list-using-cascadingdropdown-cs.md)
> [İleri](presetting-list-entries-with-cascadingdropdown-cs.md)
