---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-cs
title: "CascadingDropDown bir veritabanıyla (C#) kullanarak | Microsoft Docs"
author: wenz
description: "Böylece bir DropDownList yükleri değişiklikleri anoth değerleri ilişkili AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 684f0c28-a490-4e5b-b5e5-5dfb77464b49
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-cs
msc.type: authoredcontent
ms.openlocfilehash: 86d6b62259573433cff7054d50cc299da9e4f372
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="using-cascadingdropdown-with-a-database-c"></a>CascadingDropDown kullanarak bir veritabanıyla (C#)
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1CS.pdf)

> Böylece bir DropDownList yükleri değişiklikler başka bir DropDownList değerlerde ilişkili AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir. Bunun çalışması sırayla bir özel web hizmeti oluşturulması gerekir.


## <a name="overview"></a>Genel Bakış

Böylece bir DropDownList yükleri değişiklikler başka bir DropDownList değerlerde ilişkili AJAX Denetim Araç Seti CascadingDropDown denetiminde bir DropDownList denetimi genişletir. (Örneği için bir liste BİZE durumları bir listesini sağlar ve sonraki liste sonra bu durum önemli şehirlerde ile doldurulur.) Bunun çalışması sırayla bir özel web hizmeti oluşturulması gerekir.

## <a name="steps"></a>Adımlar

Öncelikle, bir veri kaynağı gereklidir. Bu örnekte, AdventureWorks veritabanını ve Microsoft SQL Server 2005 Express Edition kullanır. Veritabanı (express edition dahil) bir Visual Studio yükleme isteğe bağlı bir parçasıdır ve ayrıca altında ayrı bir yükleme olarak kullanılabilir [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). AdventureWorks veritabanını SQL Server 2005 örnekler ve örnek veritabanları parçasıdır (adresinden [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = tr](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). En kolay yolu veritabanını için Microsoft SQL Server Management Studio Express kullanmaktır ([https://www.microsoft.com/downloads/details.aspx? FamilyID c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796 =&amp;DisplayLang = tr](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) ve attach `AdventureWorks.mdf` veritabanı dosyası.

Bu örnek için SQL Server 2005 Express Edition'ın örneğini çağrıldığından emin olan varsayıyoruz `SQLEXPRESS` ve web sunucusu; ile aynı makinede bulunan varsayılan kurulum de budur. Kurulumunuzu farklıysa, veritabanı için bağlantı bilgilerini uymak zorunda.

ASP.NET AJAX ve Denetim Araç Seti işlevselliğini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir yere sayfada (ancak içinde &lt; `form` &gt; öğesi):

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample1.aspx)]

Sonraki adımda, iki DropDownList denetimi gereklidir. Bu örnekte, AdventureWorks satıcı ve ilgili kişi bilgileri kullanırız, böylece bir liste kullanılabilir satıcılar, diğeri kullanılabilir kişiler için oluşturuyoruz:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample2.aspx)]

Ardından, iki CascadingDropDown Extender'larının sayfaya eklenmesi gerekir. Bir ilk (Satıcılar) listesi doldurur ve diğeri ikinci (kişiler) listesi girer. Aşağıdaki öznitelikler ayarlamanız gerekir:

- `ServicePath`: Liste girişlerini sunan bir web hizmeti URL'si
- `ServiceMethod`: Liste girişlerini gönderiliyor web yöntemi
- `TargetControlID`: Aşağı açılan listesinin kimliği
- `Category`: Web yöntemi çağrıldığında gönderildi kategori bilgileri
- `PromptText`: Zaman uyumsuz olarak listesi verileri sunucudan yüklenirken görüntülenen metin
- `ParentControlID`: (isteğe bağlı) üst açılır listesinde bu Tetikleyicileri yükleme geçerli listesi

Kullanılan programlama dili bağlı olarak söz konusu web hizmeti adı değişir, ancak diğer tüm öznitelik değerleri aynıdır. İlk açılır listeden CascadingDropDown öğesi şöyledir:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample3.aspx)]

İkinci liste denetimi Extender'larının ayarlamanızı `ParentControlID` kişiler listesindeki öğeleri ilişkili bir giriş yüklenirken satıcılar listesi Tetikleyicileri seçerek böylece özniteliği.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample4.aspx)]

Asıl işi daha sonra şu şekilde ayarlanır web hizmetinde yapılır. Unutmayın `[ScriptService]` özniteliği kullanılır, ASP.NET AJAX istemci tarafı komut dosyası kodundan web yöntemleri erişmek için JavaScript proxy'si aksi oluşturulamıyor.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample5.aspx)]

İmza tarafından CascadingDropDown adlı web yöntemlerinin aşağıdaki gibidir:

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample6.cs)]

Dönüş değeri türünde bir dizi olmalıdır `CascadingDropDownNameValue` Denetim Araç Seti tarafından tanımlanır. `GetVendors()` Yöntemi uygulamak oldukça kolay: kod AdventureWorks veritabanına bağlanır ve ilk 25 satıcılar sorgular. İlk parametreyi `CascadingDropDownNameValue` Oluşturucusu listesi girişi, ikinci bir resim yazısını değeri olduğu (HTML'ın value özniteliği &lt; `option` &gt; öğesi). Kod aşağıdaki gibidir:

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample7.cs)]

Bir satıcı için ilişkili kişiler alma (yöntem adı: `GetContactsForVendor()`) biraz zor olduğu. Öncelikle, ilk açılır listeden seçilen satıcı belirlenmesi gerekir. Denetim Araç Seti, görev için bir yardımcı yöntem tanımlar: `ParseKnownCategoryValuesString()` yöntemi döndürür bir `StringDictionary` açılır verilerle öğe:

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample8.cs)]

Güvenlik nedeniyle, bu verileri ilk doğrulanması gerekir. Satıcı giriş varsa bunu (çünkü `Category` ilk CascadingDropDown öğesi özelliği ayarlanmış `"Vendor"`), seçili satıcı kimliği alınabilir:

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample9.cs)]

Yöntem kalan oldukça düz iletme, ise. Satıcı Kimliği, tüm ilişkili kişiler için satıcıya alır bir SQL sorgusu için parametre olarak kullanılır. Bir kez daha, türünde bir dizi yöntem `CascadingDropDownNameValue`.

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample10.cs)]

ASP.NET sayfası yük ve kısa bir süre sonra satıcı listesi 25 girişleri ile doldurulur. Bir giriş seçin ve ikinci açılır listeden veri ile nasıl doldurulur dikkat edin.


[![İlk liste otomatik olarak doldurulur.](using-cascadingdropdown-with-a-database-cs/_static/image2.png)](using-cascadingdropdown-with-a-database-cs/_static/image1.png)

İlk liste otomatik olarak doldurulur ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-cascadingdropdown-with-a-database-cs/_static/image3.png))


[![İkinci liste ilk listesinde yapılan seçime göre doldurulur.](using-cascadingdropdown-with-a-database-cs/_static/image5.png)](using-cascadingdropdown-with-a-database-cs/_static/image4.png)

İkinci liste ilk listesinde yapılan seçime göre doldurulur ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-cascadingdropdown-with-a-database-cs/_static/image6.png))

>[!div class="step-by-step"]
[Önceki](filling-a-list-using-cascadingdropdown-cs.md)
[sonraki](presetting-list-entries-with-cascadingdropdown-cs.md)
