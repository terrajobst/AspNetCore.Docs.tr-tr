---
uid: web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-cs
title: ComboBox denetimi nasıl kullanabilirim? (C#) | Microsoft Docs
author: microsoft
description: ComboBox kullanıcıların seçim yapabileceğiniz seçeneklerin bir listesini bir metin kutusu esnekliğini birleştiren bir ASP.NET AJAX denetimdir.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 0bbf4134-04df-4226-8930-d5bb99e27128
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 20afd7334437a021f6f68216f84406eef5ea65c6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875233"
---
<a name="how-do-i-use-the-combobox-control-c"></a>ComboBox denetimi nasıl kullanabilirim? (C#)
====================
tarafından [Microsoft](https://github.com/microsoft)

> ComboBox kullanıcıların seçim yapabileceğiniz seçeneklerin bir listesini bir metin kutusu esnekliğini birleştiren bir ASP.NET AJAX denetimdir.


Bu öğretici AJAX Denetim Araç Seti ComboBox denetimi açıklamak için hedefidir. ComboBox bir standart ASP.NET DropDownList ve TextBox denetimi arasında birlikte çalışır. Önceden var olan bir öğe listesinden seçin veya yeni bir öğe girin.

ComboBox otomatik tamamlama denetim extender benzer, ancak denetim farklı senaryolarda kullanılır. Otomatik Tamamlama genişletici eşleşen girişlerini almak için bir web hizmetini sorgular. ComboBox denetimi buna karşılık, öğeleri kümesi ile başlatıldı. Otomatik Tamamlama genişletici yapar kullanarak çok sayıda veri (car bölümleri milyonlarca) ComboBox denetimi kullanırken çalışırken algılama küçük bir veri kümesi ile çalışırken mantıklı (car bölümleri onlarca).

## <a name="selecting-from-a-static-list-of-items"></a>Statik öğeler listeden seçme

ComboBox denetimi kullanarak basit bir örnek Başlat s olanak tanır. Statik öğelerin listesini aşağı açılan listesinde görüntülemek istediğinizi varsayalım. Ancak, liste eksiksiz değildir olasılığı açık ayrılmak istiyor. Listeye özel bir değer girmesini izin vermek istediğiniz.

Biz üm yeni bir ASP.NET Web Forms sayfası oluşturmak ve sayfanın ComboBox denetimi kullanın. Yeni ASP.NET sayfası projenize ekleyin ve Tasarım görünümüne geçin.

ComboBox denetimi sayfasında kullanmak istiyorsanız sayfaya bir ScriptManager denetimi eklemeniz gerekir. AJAX uzantılar sekmesi from beneath ScriptManager denetimi Tasarımcı yüzeyine sürükleyin. Sayfanın en üstünde ScriptManager denetimi eklemeniz gerekir; Açılış sunucu-tarafı hemen ekleyebilirsiniz &lt;form&gt; etiketi.

Ardından, ComboBox denetimi sayfaya sürükleyin. ComboBox denetimi araç diğer AJAX Denetim Araç Seti denetimler ve denetim Extender'larının (bkz. Şekil 1) bulabilirsiniz.


[![Bir iş kartı oluşturmak için basit form](how-do-i-use-the-combobox-control-cs/_static/image1.jpg)](how-do-i-use-the-combobox-control-cs/_static/image1.png)

**Şekil 01**: ComboBox Denetim Araç Kutusu'ndan seçme ([tam boyutlu görüntüyü görüntülemek için tıklatın](how-do-i-use-the-combobox-control-cs/_static/image2.png))


Biz üm ComboBox denetimi seçenekleri statik listesini görüntülemek için kullanın. Kullanıcı kendi yemek spiciness belirli bir düzeyde üç seçenek listesinden seçebilirsiniz: hafif, Orta ve etkin (bkz. Şekil 2).


[![Statik öğeler listeden seçme](how-do-i-use-the-combobox-control-cs/_static/image2.jpg)](how-do-i-use-the-combobox-control-cs/_static/image3.png)

**Şekil 02**: statik öğeler listeden seçerek ([tam boyutlu görüntüyü görüntülemek için tıklatın](how-do-i-use-the-combobox-control-cs/_static/image4.png))


Bu seçenekler ComboBox denetimine ekleyebilirsiniz iki yolu vardır. İlk olarak, farenizi denetimini Tasarım görünümünde gezinirken seçeneklerini Düzenle görev seçeneğini belirleyin ve öğesi Düzenleyicisi'ni açın (bkz: Şekil 3).


[![ComboBox öğelerini düzenleme](how-do-i-use-the-combobox-control-cs/_static/image3.jpg)](how-do-i-use-the-combobox-control-cs/_static/image5.png)

**Şekil 03**: ComboBox düzenleme öğeleri ([tam boyutlu görüntüyü görüntülemek için tıklatın](how-do-i-use-the-combobox-control-cs/_static/image6.png))


İkinci seçenek açma ve kapatma Between öğeleri listesi eklemektir &lt;asp: ComboBox&gt; kaynak görünümünde etiketler. Sayfa listeleme 1 öğe listesinin sahip güncelleştirilmiş ComboBox içerir.

**1 - Static.aspx listeleme**

[!code-aspx[Main](how-do-i-use-the-combobox-control-cs/samples/sample1.aspx)]

Listeleme 1'de sayfasını açtığınızda, ComboBox önceden mevcut seçeneklerden birini seçebilirsiniz. Diğer bir deyişle, ComboBox yalnızca bir DropDownList denetimi gibi çalışır.

Ancak, aynı zamanda mevcut listesinde olmayan yeni bir seçenek (örneğin, süper Spicy) girerek seçeneğiniz vardır. Bu nedenle, ComboBox TextBox denetimi gibi çalışır.

Etiket denetimi tercih ettiğiniz görünür formu gönderdiğinde olup, önceden var olan çekme bağımsız olarak öğesi veya özel bir öğesi girin. BtnSubmit form gönderme zaman\_işleyicisi yürütür ve etiket güncelleştirmeleri tıklatın (Şekil 4'e bakın).


[![Seçili öğeyi görüntüleme](how-do-i-use-the-combobox-control-cs/_static/image4.jpg)](how-do-i-use-the-combobox-control-cs/_static/image7.png)

**Şekil 04**: seçilen öğe görüntüleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](how-do-i-use-the-combobox-control-cs/_static/image8.png))


ComboBox form gönderildikten sonra seçilen öğeyi almak için aynı DropDownList denetimi özellikleri destekler:

- SelectedItem.Text - seçilen öğenin metni özelliğinin değeri görüntüler.
- SelectedItem.Value - seçilen öğenin değeri özelliğinin değeri görüntüler veya ComboBox yazılan metin görüntüler.
- SelectedValue - bu özellik, varsayılan (Başlangıç) seçili öğe belirtmenize olanak tanır ancak bu SelectedItem.Value aynıdır.

Yazarsanız, özel bir seçim ComboBox sonra özel seçeneği olarak hem SelectedItem.Text hem de SelectedItem.Value özelliklerine atanır.

## <a name="selecting-the-list-of-items-from-the-database"></a>Öğe listesinin veritabanından seçme

ComboBox görüntüler öğe listesinin bir veritabanından alabilir. Örneğin, bir SqlDataSource denetimi, bir ObjectDataSource Denetimi, bir LinqDataSource veya bir EntityDataSource ComboBox bağlayabilirsiniz.

ComboBox içinde filmler listesini görüntülemek istediğinizi varsayalım. Film veritabanı tablosundan filmler listesini almak istiyor. Aşağıdaki adımları uygulayın:

1. Movies.aspx adlı bir sayfa oluşturma
2. Bir ScriptManager denetimi, araç çubuğundaki sayfaya AJAX uzantılar sekmesi altında ScriptManager sürükleyerek sayfasına ekleyin.
3. ComboBox denetimi ComboBox sayfaya sürükleyerek sayfasına ekleyin.
4. Tasarım görünümünde, farenizi üzerinde ComboBox denetim getirin ve seçin **veri kaynağı Seç** görev seçeneği (bkz. Şekil 5). Veri Kaynağı Yapılandırma Sihirbazı başlatılır.
5. İçinde **veri kaynağı seçin** adım, select &lt;yeni veri kaynağı&gt; seçeneği.
6. İçinde **bir veri kaynağı türü seç** adım, veritabanını seçin.
7. İçinde **veri bağlantınızı** adım, veritabanınızı (örneğin, MoviesDB.mdf) seçin.
8. İçinde **bağlantı dizesini uygulama yapılandırma dosyasını Kaydet** adım, bağlantı dizenizi kaydetme seçeneğini seçin.
9. İçinde **Select deyimi yapılandırma** adım, filmler veritabanı tablosu seçin ve tüm sütunları seçin.
10. İçinde **Test sorgusu** adım, son düğmesini tıklatın.
11. Geri **veri kaynağı Seç** görüntülenecek alan için başlık sütunu ve kimlik sütunu için veri alan (bkz. Şekil) adımı seçin.
12. Sihirbazı kapatmak için Tamam düğmesini tıklatın.


[![Veri kaynağı seçme](how-do-i-use-the-combobox-control-cs/_static/image5.jpg)](how-do-i-use-the-combobox-control-cs/_static/image9.png)

**Şekil 05**: veri kaynağı seçme ([tam boyutlu görüntüyü görüntülemek için tıklatın](how-do-i-use-the-combobox-control-cs/_static/image10.png))


[![Veri metni ve değeri alanlarını seçme](how-do-i-use-the-combobox-control-cs/_static/image6.jpg)](how-do-i-use-the-combobox-control-cs/_static/image11.png)

**Şekil 06**: veri metin ve değer alanları seçme ([tam boyutlu görüntüyü görüntülemek için tıklatın](how-do-i-use-the-combobox-control-cs/_static/image12.png))


Yukarıdaki adımları tamamladıktan sonra filmler filmler veritabanı tablosundan temsil eden bir SqlDataSource denetimi ComboBox bağlıdır. Sayfa için bir kaynak (t biraz biçimlendirme temizlendi) 2 listeleme gibi görünüyor.

**Listing 2 - Movies.aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-cs/samples/sample2.aspx)]

ComboBox denetimi SqlDataSource denetim noktaları bir DataSourceID özelliği olduğuna dikkat edin. Bir tarayıcıda sayfasını açtığınızda, filmler veritabanından listesi görüntülenir (bkz. Şekil 7). Ya da bir çekme listesinden bir filmi olabilir veya film ComboBox yazarak yeni bir filmi girin.


[![Film listesini görüntüleme](how-do-i-use-the-combobox-control-cs/_static/image7.jpg)](how-do-i-use-the-combobox-control-cs/_static/image13.png)

**Şekil 07**: filmler listesini görüntüleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](how-do-i-use-the-combobox-control-cs/_static/image14.png))


## <a name="setting-the-dropdownstyle"></a>DropDownStyle ayarlama

ComboBox davranışını değiştirmek için ComboBox DropDownStyle özelliğini kullanabilirsiniz. Bu özellik var kabul olası değerler:

- Aşağı açılan - bir açılır liste oku ve tıkladığınızda (varsayılan değer) ComboBox görüntüler özel bir değer girebilirsiniz.
- Basit - ComboBox otomatik olarak açılır listesini görüntüler ve özel bir değer girebilirsiniz.
- DropDownList - ComboBox yalnızca bir DropDownList denetimi gibi çalışır.

Öğe listesinin görüntülendiğinde açılır arasında basit farklıdır. ComboBox odak taşıdığınızda hemen basit söz konusu olduğunda, liste görüntülenir. Açılan listesinde olması durumunda, öğelerin listesini görmek için oka tıklayın gerekir.

DropDownList değeri standart DropDownList denetimi gibi yalnızca bir iş için ComboBox denetimine neden olur. Ancak burada önemli bir fark yoktur. Denetimi, önündeki yerleştirilen herhangi bir denetim önünde görüntülenmesi için Internet Explorer'ın daha eski sürümleri sonsuz bir z-index DropDownList denetimiyle görüntüler. ComboBox bir HTML işleyen çünkü &lt;div&gt; yerine bir HTML etiketi &lt;seçin&gt; etiketi ComboBox doğru uyar z sıralamasını.

## <a name="setting-the-autocompletemode"></a>AutoCompleteMode özelliği ayarlama

ComboBox AutoCompleteMode özelliği özelliğini birisi metin ComboBox yazdığında ne olacağını belirlemek için kullanın. Bu özellik aşağıdaki olası değerlerini kabul eder:

- Hiçbiri - (varsayılan değer) ComboBox herhangi otomatik tamamlama davranışı sağlamaz.
- Önermek - ComboBox listesini görüntüler ve listedeki eşleşen öğe vurgular (bkz. Şekil 8).
- Append - ComboBox listesi görüntülemez ve (bkz. Şekil 9) yazdığınızı üzerine listeden eşleşen öğe ekler.
- SuggestAppend - ComboBox görüntüler hem eşleşen öğe (bkz. Şekil 10) yazdığınızı üzerine listeden ekler.


[![ComboBox öneride yapar](how-do-i-use-the-combobox-control-cs/_static/image8.jpg)](how-do-i-use-the-combobox-control-cs/_static/image15.png)

**Şekil 08**: ComboBox öneride yapar ([tam boyutlu görüntüyü görüntülemek için tıklatın](how-do-i-use-the-combobox-control-cs/_static/image16.png))


[![Eşleşen metin ComboBox ekler](how-do-i-use-the-combobox-control-cs/_static/image9.jpg)](how-do-i-use-the-combobox-control-cs/_static/image17.png)

**Şekil 09**: ComboBox eşleşen metin ekler ([tam boyutlu görüntüyü görüntülemek için tıklatın](how-do-i-use-the-combobox-control-cs/_static/image18.png))


[![ComboBox önerir ve ekler](how-do-i-use-the-combobox-control-cs/_static/image10.jpg)](how-do-i-use-the-combobox-control-cs/_static/image19.png)

**Şekil 10**: ComboBox önerir ve ekler ([tam boyutlu görüntüyü görüntülemek için tıklatın](how-do-i-use-the-combobox-control-cs/_static/image20.png))


## <a name="summary"></a>Özet

Bu öğreticide, ComboBox denetimi sabit bir öğe kümesini görüntülemek için nasıl kullanılacağı hakkında bilgi edindiniz. ComboBox denetimi hem öğelerinin ayarlamak statik ve bir veritabanı tablosu biz bağlı. Son olarak, DropDownStyle ve AutoCompleteMode özelliği özelliklerini ayarlayarak ComboBox davranışını değiştirmek nasıl öğrendiniz.

> [!div class="step-by-step"]
> [Next](how-do-i-use-the-combobox-control-vb.md)
