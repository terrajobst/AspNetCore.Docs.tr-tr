---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
title: "Özel bir AJAX oluşturma Denetim Araç Seti denetim genişletici (VB) | Microsoft Docs"
author: microsoft
description: "Özel Extender'larının özelleştirebilir ve ASP.NET denetimleri yeni sınıflar oluşturmak zorunda kalmadan yeteneklerini olanak sağlar."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 18b29834-c991-4e0c-b533-44d358fbfc9c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: 3e8fceb3c7570aa1bf085c8e1037736254e74ef9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-custom-ajax-control-toolkit-control-extender-vb"></a>Özel AJAX Denetim Araç Seti denetim genişletici (VB) oluşturma
====================
tarafından [Microsoft](https://github.com/microsoft)

> Özel Extender'larının özelleştirebilir ve ASP.NET denetimleri yeni sınıflar oluşturmak zorunda kalmadan yeteneklerini olanak sağlar.


Bu öğreticide, bir özel AJAX Denetim Araç Seti denetim genişletici oluşturmayı öğrenin. Metni metin kutusuna yazdığınızda etkin için devre dışı bir düğme durumu değişiklikleri yararlı, yeni genişletici ancak basit bir oluşturuyoruz. Bu öğretici okuduktan sonra ASP.NET AJAX araç seti ile kendi denetim Extender'larının süresini uzatabilir olacaktır.

Visual Studio veya Visual Web Developer kullanarak özel denetim Extender'larının oluşturma (Visual Web Developer en son sürümüne sahip olduğunuzdan emin olun).

## <a name="overview-of-the-disabledbutton-extender"></a>DisabledButton genişletici genel bakış

Bizim yeni denetim genişletici DisabledButton genişletici olarak adlandırılır. Bu genişletici üç özelliği vardır:

- TargetControlID - denetimi metin kutusu.
- TargetButtonIID - etkin veya devre dışı düğmesi.
- DisabledText - düğmesini başlangıçta görüntülenen metin. Yazmaya başladığınızda, düğmenin düğme metni özelliğinin değeri görüntüler.

Bir metin kutusu ve düğme denetimine DisabledButton genişletici bağlayın. Herhangi bir metin yazın önce düğmesi devre dışı bırakılır ve metin kutusu ve düğmesi şöyle:


[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image1.png)

([Tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image3.png))


Metin yazmaya başlayın sonra düğmesi etkinleştirilir ve metin kutusu ve düğmesi şöyle:


[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image4.png)

([Tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image6.png))


Bizim denetim genişletici oluşturmak için aşağıdaki üç dosyaları oluşturmak ihtiyacımız var:

- DisabledButtonExtender.vb - bu dosyanın extender oluşturma yönetmek ve tasarım zamanında özelliklerini ayarlamanıza olanak sağlar sunucu tarafı denetimi sınıftır. Ayrıca, extender'ayarlanabilir özellikleri tanımlar. Bu özellikler kodu aracılığıyla ve tasarım zamanında erişilebilir ve DisableButtonBehavior.js dosyasında tanımlanan özellikler eşleşmesi.
- DisabledButtonBehavior.js--, Tüm istemci komut dosyası mantığınızı burada ekleyeceksiniz bu dosyasıdır.
- Bu sınıf DisabledButtonDesigner.vb - tasarım zamanı işlevselliği sağlar. Visual Studio/Visual Web Developer Tasarımcısı ile düzgün çalışması için Denetim genişletici istiyorsanız bu sınıf gerekir.

Bu nedenle denetim genişletici bir sunucu tarafı denetimi, bir istemci-tarafı davranışı ve sunucu tarafı Tasarımcı sınıfını oluşur. Aşağıdaki bölümlerde bu dosyaları üç oluşturmayı öğrenin.

## <a name="creating-the-custom-extender-website-and-project"></a>Proje ve özel genişletici Web sitesi oluşturma

İlk adım bir sınıf kitaplığı proje ve Web sitesi Visual Studio/Visual Web Developer oluşturmaktır. Biz üm özel genişletici sınıf kitaplığı projesi oluşturun ve Web sitesi özel genişletici test.

Web sitesiyle başlama s olanak tanır. Web sitesi oluşturmak için aşağıdaki adımları izleyin:

1. Menü seçeneğini **dosya, yeni Web sitesi**.
2. Seçin **ASP.NET Web sitesi** şablonu.
3. Yeni Web sitesi adı *websitesi1*.
4. Tıklatın **Tamam** düğmesi.

Ardından, Denetim genişletici kodunu içeren sınıf kitaplığı projesi oluşturmak ihtiyacımız var:

1. Menü seçeneğini **dosya, Ekle, yeni proje**.
2. Seçin **sınıf kitaplığı** şablonu.
3. Yeni sınıf kitaplığı adıyla **CustomExtenders**.
4. Tıklatın **Tamam** düğmesi.

Bu adımları tamamladıktan sonra Çözüm Gezgini penceresi Şekil 1 gibi görünmelidir.


[![Web sitesi ve sınıf kitaplığı proje Çözümle](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image7.png)

**Şekil 01**: Web sitesi ve sınıf kitaplığı proje çözümüyle ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image9.png))


Ardından, tüm gerekli derleme başvurularını sınıf kitaplığı projesine eklemeniz gerekir:

1. CustomExtenders projeye sağ tıklayın ve menü seçeneğini **Başvuru Ekle**.
2. .NET sekmesini seçin.
3. Aşağıdaki derlemelere başvurular ekleyin:

    1. System.Web.dll
    2. System.Web.Extensions.dll
    3. System.Design.dll
    4. System.Web.Extensions.Design.dll
4. Gözat sekmesini seçin.
5. AjaxControlToolkit.dll derlemesine başvuru ekleyin. Bu derleme AJAX Denetim Araç Seti indirdiğiniz klasörde bulunur.

Tüm sağ başvuruları projenize sağ tıklatıp Özellikler'i seçerek ve başvurular sekmesini tıklatarak eklediğiniz emin olun (bkz: Şekil 2).


[![Başvurular klasörünü gerekli başvuruları](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image10.png)

**Şekil 02**: gerekli başvurularıyla başvurular klasörünü ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image12.png))


## <a name="creating-the-custom-control-extender"></a>Özel denetim Genişletici Oluşturma

Bizim sınıf kitaplığı sahibiz, biz bizim genişletici denetim oluşturmaya başlayabilirsiniz. Bir özel genişletici denetim sınıfın (1 listeleme bakın) ile bare kemikler Başlat s olanak tanır.

**1 - MyCustomExtender.vb listeleme**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample1.vb)]

Listeleme 1'deki denetim genişletici sınıfı hakkında fark birkaç şey vardır. İlk olarak, taban ExtenderControlBase sınıftan sınıfı dikkat edin. Tüm AJAX Denetim Araç Seti genişletici denetimleri bu temel sınıfından türetilir. Örneğin, temel sınıfın her denetim genişletici gerekli bir özelliktir TargetID özelliğini içerir.

Ardından, sınıf istemci komut dosyası için ilgili aşağıdaki iki öznitelikleri içerdiğine dikkat edin:

- Web kaynağı - derlemedeki katıştırılmış bir kaynağı olarak eklenmek üzere bir dosya neden olur.
- ClientScriptResource - bir derlemeye ait alınması bir komut dosyası kaynağı neden olur.

Web kaynağı öznitelik özel genişletici derlendiğinde MyControlBehavior.js JavaScript dosyası derlemeye eklemek için kullanılır. ClientScriptResource öznitelik, bir web sayfasında özel genişletici kullanıldığında MyControlBehavior.js komut dosyası derlemeden almak için kullanılır.


Çalışmak Web kaynağı ve ClientScriptResource öznitelikleri sırayla katıştırılmış bir kaynağı JavaScript dosyası derleme gerekir. Çözüm Gezgini penceresinde dosyayı seçin, özellik sayfasını açın ve değer atamak *katıştırılmış kaynak* için **yapı eylemi** özelliği.


Denetim genişletici TargetControlType özniteliği içerdiğine dikkat edin. Bu öznitelik tarafından denetim genişletici genişletilmiş denetim türünü belirtmek için kullanılır. Listeleme 1 söz konusu olduğunda, Denetim genişletici TextBox genişletmek için kullanılır.

Son olarak, özel genişletici MyProperty adlı bir özellik içerdiğine dikkat edin. Özellik ExtenderControlProperty özniteliği ile işaretlenir. GetPropertyValue() ve SetPropertyValue() yöntemleri, istemci-tarafı davranışı sunucu taraflı denetim genişletici özellik değeri geçirmek için kullanılır.

Bir tane bizim DisabledButton genişletici kodunu uygulamak s olanak tanır. Bu genişletici kodunu listeleme 2'de bulunabilir.

**2 - DisabledButtonExtender.vb listeleme**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample2.vb)]

2. listeleme DisabledButton genişletici TargetButtonID ve DisabledText adlı iki özelliklere sahiptir. TargetButtonID özelliğine uygulanan IDReferenceProperty düğme denetiminin Kimliğini dışında her şey bu özelliğe atanmasını engeller.

Web kaynağı ve ClientScriptResource öznitelikleri bu extender ile DisabledButtonBehavior.js adlı bir dosyada bulunan bir istemci-tarafı davranışı ilişkilendirin. Bu JavaScript dosyası sonraki bölümde aşağıdakiler ele.

## <a name="creating-the-custom-extender-behavior"></a>Özel genişletici davranışı oluşturma

Bir denetim genişletici istemci-tarafı bileşeninin bir davranış adı verilir. Devre dışı bırakma ve etkinleştirme düğmesi fiili mantığı DisabledButton davranışı yer alır. JavaScript kodu davranışı için listeleme 3'te dahil edilir.

**3 - DisabledButton.js listeleme**

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample3.js)]

JavaScript dosyası listeleme 3'te DisabledButtonBehavior adlı bir istemci-tarafı sınıf içerir. Bu sınıfı, sunucu tarafı twin gibi TargetButtonID adlı iki özellik içerir ve kullanarak erişebileceğiniz DisabledText almak\_TargetButtonID/set\_TargetButtonID ve\_DisabledText/set\_ DisabledText.

Önce Initialize() yöntemini keyup olay işleyicisi davranışı için hedef öğe ile ilişkilendirir. Bu davranışı ile ilişkili metin bir harf yazdığınız her zaman keyup işleyici yürütür. Keyup işleyici etkinleştirir veya davranışı ile ilişkili metin kutusu herhangi bir metin içerip içermediğini bağlı olarak düğmesini devre dışı bırakır.

Katıştırılmış bir kaynağı olarak listeleniyor 3'te JavaScript dosyası derleme unutmayın. Çözüm Gezgini penceresinde dosyayı seçin, özellik sayfasını açın ve değer atamak *katıştırılmış kaynak* için **yapı eylemi** özelliği (bkz: Şekil 3). Bu seçenek, Visual Studio ve Visual Web Developer içinde kullanılabilir.


[![Katıştırılmış bir kaynağı bir JavaScript dosyası ekleme](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image13.png)

**Şekil 03**: katıştırılmış bir kaynağı bir JavaScript dosyası ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image15.png))


## <a name="creating-the-custom-extender-designer"></a>Özel genişletici Tasarımcısı oluşturma

Bizim genişletici tamamlamak için oluşturmanız gereken bir son sınıfı yok. Tasarımcı sınıfını listeleme 4'te oluşturmak gerekir. Bu sınıf, Visual Studio/Visual Web Developer tasarımcı ile birlikte düzgün çalışmasını genişletici yapmak için gereklidir.

**4 - DisabledButtonDesigner.vb listeleme**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample4.vb)]

Listeleme 4 tasarımcıda Tasarımcısı özniteliğiyle DisabledButton genişletici ile ilişkilendirin. Bu gibi DisabledButtonExtender sınıfına Tasarımcısı özniteliği uygulamanız gerekir:

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample5.vb)]

## <a name="using-the-custom-extender"></a>Özel genişletici kullanma

Biz DisabledButton denetim genişletici oluşturmayı tamamladıktan, bizim ASP.NET Web sitesi kullanmak için zaman yapılır. İlk olarak, biz araç kutusuna özel genişletici eklemeniz gerekir. Aşağıdaki adımları uygulayın:

1. Bir ASP.NET sayfasının Solution Explorer penceresi sayfasında çift tıklatarak açın.
2. Araç kutusunu sağ tıklatın ve menü seçeneğini **öğeleri Seç**.
3. Araç kutusu öğelerini Seç iletişim kutusunda, CustomExtenders.dll derlemeye göz atın.
4. Tıklatın **Tamam** düğmesi iletişim kutusunu kapatın.

Bu adımları tamamladıktan sonra DisabledButton denetim genişletici araç kutusunda görüntülenmesi gerekir (bkz: Şekil 4).


[![Araç çubuğundaki DisabledButton](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image16.png)

**Şekil 04**: araç çubuğundaki DisabledButton ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image18.png))


Ardından, size yeni bir ASP.NET sayfası oluşturmanız gerekir. Aşağıdaki adımları uygulayın:

1. ShowDisabledButton.aspx adlı yeni bir ASP.NET sayfası oluşturun.
2. Bir ScriptManager sayfaya sürükleyin.
3. TextBox denetimi sayfaya sürükleyin.
4. Düğme denetimi sayfaya sürükleyin.
5. Özellikler penceresinde düğmesi ID özelliği değere değiştirin *btnSave* ve değeri metin özelliğini *kaydetmek\**.
  

Standart bir düğmeyi ve ASP.NET TextBox denetimi ile bir sayfa oluşturduk.

Ardından, TextBox denetimi DisabledButton genişletici ile genişletmek ihtiyacımız var:

1. Seçin **eklemek genişletici** görev genişletici Sihirbazı iletişim kutusunu açmak için seçeneği (bkz. Şekil 5). İletişim kutusu özel bizim DisabledButton genişletici içerdiğine dikkat edin.
2. DisabledButton genişletici tıklatıp **Tamam** düğmesi.


[![Genişletici Sihirbazı iletişim kutusu](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image19.png)

**Şekil 05**: genişletici sihirbaz iletişim ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image21.png))


Son olarak, biz DisabledButton genişletici özellikleri ayarlayabilirsiniz. TextBox denetimi özelliklerini değiştirerek DisabledButton genişletici özelliklerini değiştirebilirsiniz:

1. TextBox Tasarımcısı'nda seçin.
2. Özellikleri penceresinde Extender'larının düğümünü genişletin (bkz. Şekil 6).
3. Değer atamak *kaydetmek* DisabledText özelliği ve değerini *btnSave* TargetButtonID özelliğine.


[![Genişletici özelliklerini ayarlama](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image22.png)

**Şekil 06**: genişletici özelliklerini ayarlama ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image24.png))


Sayfa (F5 tuşuna tarafından) çalıştırdığınızda, düğme denetim başlangıçta devre dışı bırakıldı. Metin kutusuna bir metin girerek başlar başlamaz denetimi düğmesi (bkz. Şekil 7) etkin.


[![Eylem DisabledButton genişletici](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image25.png)

**Şekil 07**: DisabledButton genişletici eylem ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image27.png))


## <a name="summary"></a>Özet

AJAX Denetim Araç Seti özel genişletici denetimleri ile nasıl genişletebileceğini açıklamak için bu öğreticinin amacı oluştu. Bu öğreticide, bir basit DisabledButton denetim genişletici oluşturduk. Biz bu genişletici DisabledButtonExtender sınıfı, bir DisabledButtonBehavior JavaScript davranışını ve DisabledButtonDesigner sınıfı oluşturarak uygulanmadı. Her bir özel denetim extender oluşturduğunuzda benzer birtakım adımları izleyin.

>[!div class="step-by-step"]
[Önceki](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)
