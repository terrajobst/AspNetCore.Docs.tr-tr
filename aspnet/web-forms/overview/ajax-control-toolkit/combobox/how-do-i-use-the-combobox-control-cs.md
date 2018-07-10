---
uid: web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-cs
title: ComboBox denetimi nasıl kullanabilirim? (C#) | Microsoft Docs
author: microsoft
description: ComboBox TextBox'ın esnekliği kullanıcıların seçebileceği seçeneklerin bir listesi ile birleştiren bir ASP.NET AJAX denetimidir.
ms.author: aspnetcontent
ms.date: 05/12/2009
ms.assetid: 0bbf4134-04df-4226-8930-d5bb99e27128
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 07da594647c9b43bd31644aa8c7fedb256a12916
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839287"
---
<a name="how-do-i-use-the-combobox-control-c"></a>ComboBox denetimi nasıl kullanabilirim? (C#)
====================
tarafından [Microsoft](https://github.com/microsoft)

> ComboBox TextBox'ın esnekliği kullanıcıların seçebileceği seçeneklerin bir listesi ile birleştiren bir ASP.NET AJAX denetimidir.


Bu öğreticide AJAX Denetim Araç Seti ComboBox denetimi açıklamak için hedefidir. ComboBox TextBox denetimi ile standart bir ASP.NET DropDownList denetimi arasındaki bir birlikte çalışır. Önceden var olan bir öğe listesinden seçin veya yeni bir öğe girin.

Bir ComboBox için otomatik tamamlama denetim genişletici benzer, ancak denetimleri farklı senaryolarda kullanılır. Otomatik Tamamlama genişletici eşleşen girişlerini almak için bir web hizmetini sorgular. ComboBox denetimi, buna karşılık, öğelerin kümesi ile başlatılır. Otomatik Tamamlama genişletici yapar kullanarak ComboBox denetimini kullanırken çok sayıda veri (bölümleri car milyonlarca) ile çalışırken algılama küçük bir veri kümesiyle çalışırken mantıklıdır (bölümleri car onlarca).

## <a name="selecting-from-a-static-list-of-items"></a>Statik bir öğe listesinden seçim yapma

S Başlat ComboBox denetimini kullanarak basit bir örnek sağlar. Açılan listede statik öğelerinin listesini görüntülemek istediğinizi düşünün. Ancak, liste eksiksiz değildir olasılığını açık kalma istersiniz. Listeye özel bir değer girmesini izin vermek istediğiniz.

Biz ll yeni bir ASP.NET Web Forms sayfası oluşturup sayfasında ComboBox denetimini kullanabilirsiniz. Yeni ASP.NET sayfası projenize ekleyin ve Tasarım görünümüne geçin.

ComboBox denetimi sayfasında kullanmak istiyorsanız sayfasına bir ScriptManager denetimi eklemeniz gerekir. ScriptManager denetimini AJAX uzantılar sekmesinde from beneath Tasarımcı yüzeyine sürükleyin. ScriptManager denetimini sayfanın en üstündeki eklemelisiniz; Açılış sunucu-tarafı hemen ekleyebilirsiniz &lt;form&gt; etiketi.

Ardından, ComboBox denetimi sayfaya sürükleyin. ComboBox denetimi, diğer AJAX Denetim Araç Seti denetimlerini ve denetim genişleticilerini (bkz. Şekil 1) ile araç kutusunda bulabilirsiniz.


[![Basit bir iş kartı oluşturma formu](how-do-i-use-the-combobox-control-cs/_static/image1.jpg)](how-do-i-use-the-combobox-control-cs/_static/image1.png)

**Şekil 01**: ComboBox denetimi araç kutusundan seçme ([tam boyutlu görüntüyü görmek için tıklatın](how-do-i-use-the-combobox-control-cs/_static/image2.png))


Biz ll ComboBox denetimi statik seçenek listesini görüntülemek için kullanın. Kullanıcı için kendi Gıda spiciness belirli bir düzeyde üç seçenek listesinden seçebilirsiniz: hafif, Orta ve sık erişimli (bkz: Şekil 2).


[![Statik bir öğe listesinden seçim yapma](how-do-i-use-the-combobox-control-cs/_static/image2.jpg)](how-do-i-use-the-combobox-control-cs/_static/image3.png)

**Şekil 02**: statik bir öğe listesinden seçerek ([tam boyutlu görüntüyü görmek için tıklatın](how-do-i-use-the-combobox-control-cs/_static/image4.png))


ComboBox denetimi için bu seçenekleri ekleyebilirsiniz iki yolu vardır. İlk olarak, fare Tasarım görünümü denetimin üzerine gelindiğinde düzenleme seçenekleri görev seçeneğini belirleyin ve öğesi Düzenleyicisi'ni açın (bkz: Şekil 3).


[![ComboBox öğeleri düzenleme](how-do-i-use-the-combobox-control-cs/_static/image3.jpg)](how-do-i-use-the-combobox-control-cs/_static/image5.png)

**Şekil 03**: düzenleme ComboBox öğeleri ([tam boyutlu görüntüyü görmek için tıklatın](how-do-i-use-the-combobox-control-cs/_static/image6.png))


İkinci seçenek, açılış ve kapanış aralığındaki öğelerin listesini eklemektir &lt;asp: ComboBox&gt; kaynak görünümünde etiketler. 1 listesi sayfasındaki öğelerin listesini olan güncelleştirilmiş ComboBox içerir.

**1 - Static.aspx listeleme**

[!code-aspx[Main](how-do-i-use-the-combobox-control-cs/samples/sample1.aspx)]

1'de listeleme sayfasını açtığınızda, ComboBox önceden mevcut seçeneklerden birini seçebilirsiniz. Diğer bir deyişle, ComboBox yalnızca bir DropDownList denetimi gibi çalışır.

Ancak, ayrıca var olan bir listede yer almayan yeni bir seçenek (örneğin, süper Spicy) girerek seçeneğiniz vardır. Bu nedenle, ComboBox de TextBox denetimi gibi çalışır.

Seçtiğiniz etiket denetiminde görünen form gönderdiğinizde olup önceden var olan seçtiğinizden bağımsız olarak öğesi veya özel bir öğeyi girin. Form btnSubmit ne zaman gönderdiğiniz\_işleyicisi yürütülür ve etiket güncelleştirir (bkz: Şekil 4).


[![Seçili öğeyi görüntüleme](how-do-i-use-the-combobox-control-cs/_static/image4.jpg)](how-do-i-use-the-combobox-control-cs/_static/image7.png)

**Şekil 04**: seçilen öğe görüntülemek ([tam boyutlu görüntüyü görmek için tıklatın](how-do-i-use-the-combobox-control-cs/_static/image8.png))


ComboBox bir form gönderildikten sonra seçili öğeyi almak için aynı DropDownList denetimi özellikleri destekler:

- SelectedItem.Text - seçilen öğenin metin özelliğini değerini görüntüler.
- SelectedItem.Value - seçilen öğenin değeri özelliğinin değeri görüntüler veya ComboBox yazdığınız metni görüntüler.
- SelectedValue - bu özellik varsayılan (ilk) seçili öğeye belirtmenize olanak tanıyan dışında SelectedItem.Value aynıdır.

Yazarsanız, ardından özel seçim ComboBox uygulamasına özel bir seçim SelectedItem.Text hem SelectedItem.Value özelliklerine atanır.

## <a name="selecting-the-list-of-items-from-the-database"></a>Öğelerin listesini veritabanından seçme

Bir veritabanından ComboBox görüntüler öğelerinin listesini alabilir. Örneğin, ComboBox SqlDataSource denetimi, ObjectDataSource Denetimi, bir LinqDataSource veya bir EntityDataSource bağlayabilirsiniz.

Bir ComboBox içinde filmler listesini görüntülemek istediğinizi düşünün. Film veritabanı tablosundan listenin film almak istediğiniz. Aşağıdaki adımları uygulayın:

1. Movies.aspx adlı bir sayfa oluşturun
2. Bir ScriptManager denetimi araç kutusunda sayfaya ScriptManager AJAX uzantılar sekmesinde altından sürükleyerek bu sayfaya ekleyin.
3. ComboBox denetimi ComboBox sayfaya sürükleyerek sayfasına ekleyin.
4. Tasarım görünümünde, farenizi ComboBox denetimin üzerine gelin ve seçin **veri kaynağı Seç** görev seçeneği (bkz: Şekil 5). Veri Kaynağı Yapılandırma Sihirbazı başlatılır.
5. İçinde **veri kaynağı seçin** adım, select &lt;yeni veri kaynağı&gt; seçeneği.
6. İçinde **bir veri kaynağı türü seçin** adım, veritabanını seçin.
7. İçinde **veri bağlantınızı seçin** adım, veritabanınızı (örneğin, MoviesDB.mdf) seçin.
8. İçinde **bağlantı dizesini uygulama yapılandırma dosyasına Kaydet** adım, bağlantı dizenizi Kaydet seçeneğini belirleyin.
9. İçinde **Select deyimi yapılandırma** adım, film veritabanı tablosunu seçin ve tüm sütunları seçin.
10. İçinde **Test sorgusu** adım, son düğmesini tıklatın.
11. Geri **veri kaynağı Seç** görüntülenecek alan için başlık sütunu ve kimlik sütunu için veri alanı (bkz. Şekil) adımını seçin.
12. Sihirbazı kapatmak için Tamam düğmesine tıklayın.


[![Veri kaynağı seçme](how-do-i-use-the-combobox-control-cs/_static/image5.jpg)](how-do-i-use-the-combobox-control-cs/_static/image9.png)

**Şekil 05**: veri kaynağı seçme ([tam boyutlu görüntüyü görmek için tıklatın](how-do-i-use-the-combobox-control-cs/_static/image10.png))


[![Metin ve değer veri alanlarını seçme](how-do-i-use-the-combobox-control-cs/_static/image6.jpg)](how-do-i-use-the-combobox-control-cs/_static/image11.png)

**Şekil 06**: metin ve değer veri alanlarını seçme ([tam boyutlu görüntüyü görmek için tıklatın](how-do-i-use-the-combobox-control-cs/_static/image12.png))


Yukarıdaki adımları tamamladıktan sonra filmler film veritabanı tablosundan temsil eden bir SqlDataSource denetimi ComboBox bağlıdır. Sayfa için kaynak listeleme (ben biraz biçimlendirme temizlendi) 2 şuna benzer.

**2 - Movies.aspx listeleme**

[!code-aspx[Main](how-do-i-use-the-combobox-control-cs/samples/sample2.aspx)]

ComboBox denetimi SqlDataSource denetimi işaret eden bir DataSourceID özelliği olduğuna dikkat edin. Veritabanından filmler listesi sayfasının bir tarayıcıda açtığınızda görüntülenir (bkz. Şekil 7). Ya da bir seçim listesinden bir filmi olabilir veya film ComboBox yazarak yeni bir film girin.


[![Filmler listesini görüntüleme](how-do-i-use-the-combobox-control-cs/_static/image7.jpg)](how-do-i-use-the-combobox-control-cs/_static/image13.png)

**Şekil 07**: filmler listesini görüntüleme ([tam boyutlu görüntüyü görmek için tıklatın](how-do-i-use-the-combobox-control-cs/_static/image14.png))


## <a name="setting-the-dropdownstyle"></a>DropDownStyle ayarlama

ComboBox DropDownStyle özelliği, ComboBox davranışını değiştirmek için kullanabilirsiniz. Bu özelliği var. kabul olası değerler:

- -Açılan oku ve tıkladığınızda bir açılır listede (varsayılan değer) ComboBox görüntüler özel bir değer girebilirsiniz.
- Basit - ComboBox otomatik olarak açılan listesini görüntüler ve özel bir değer girebilirsiniz.
- -DropDownList ComboBox yalnızca bir DropDownList denetimi gibi çalışır.

Öğelerin listesini görüntülendiğinde ve basit açılan arasında farklılık gösterir. ComboBox öğesine odak taşıdığınızda hemen basit söz konusu olduğunda, liste görüntülenir. Açılan söz konusu olduğunda, öğe listesini görmek için oka tıklamanız gerekir.

DropDownList değeri ComboBox denetimi, standart DropDownList denetimi gibi yalnızca bir çalışmasına neden olur. Ancak burada önemli bir fark yoktur. Denetimin önüne yerleştirilen herhangi bir denetime önünde görünür bir DropDownList denetimi sonsuz bir z-index ile'Internet Explorer'ın eski sürümlerini görüntüler. Bir HTML ComboBox işler çünkü &lt;div&gt; yerine bir HTML etiketi &lt;seçin&gt; etiketi ComboBox doğru uyar z-sıralamasıyla.

## <a name="setting-the-autocompletemode"></a>AutoCompleteMode özelliği ayarlama

ComboBox AutoCompleteMode özelliği özelliği, biri metin ComboBox yazdığında ne olacağını belirlemek için kullanın. Bu özellik, aşağıdaki olası değerleri kabul eder:

- Hiçbiri - (varsayılan değer) otomatik tamamlama herhangi bir davranış ComboBox sağlamaz.
- Öner - açılan kutusu listesi görüntülenir ve listede eşleşen öğe vurgular (bkz. Şekil 8).
- Append - açılan kutusu listesi görüntülemez ve (bkz. Şekil 9) yazdığınız üzerine listeden eşleşen öğe ekler.
- SuggestAppend - ComboBox hem görüntüler hem de (bkz. Şekil 10) yazdığınız üzerine listeden eşleşen öğe ekler.


[![ComboBox önerisinde](how-do-i-use-the-combobox-control-cs/_static/image8.jpg)](how-do-i-use-the-combobox-control-cs/_static/image15.png)

**Şekil 08**: ComboBox öneri yapar ([tam boyutlu görüntüyü görmek için tıklatın](how-do-i-use-the-combobox-control-cs/_static/image16.png))


[![ComboBox eşleşen metin ekler.](how-do-i-use-the-combobox-control-cs/_static/image9.jpg)](how-do-i-use-the-combobox-control-cs/_static/image17.png)

**Şekil 09**: ComboBox eşleşen metin ekler ([tam boyutlu görüntüyü görmek için tıklatın](how-do-i-use-the-combobox-control-cs/_static/image18.png))


[![ComboBox önerir ve ekler](how-do-i-use-the-combobox-control-cs/_static/image10.jpg)](how-do-i-use-the-combobox-control-cs/_static/image19.png)

**Şekil 10**: ComboBox önerir ve ekler ([tam boyutlu görüntüyü görmek için tıklatın](how-do-i-use-the-combobox-control-cs/_static/image20.png))


## <a name="summary"></a>Özet

Bu öğreticide, sabit bir öğe kümesini görüntülemek için ComboBox denetimi kullanmayı öğrendiniz. Biz, ComboBox denetimi hem de bir statik öğelerin kümesi ve bir veritabanı tablosuna bağlı. Son olarak, DropDownStyle ve AutoCompleteMode özelliği özelliklerini ayarlayarak ComboBox davranışını değiştirmek hakkında bilgi edindiniz.

> [!div class="step-by-step"]
> [Next](how-do-i-use-the-combobox-control-vb.md)
