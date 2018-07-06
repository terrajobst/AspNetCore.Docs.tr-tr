---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
title: Oluşturma özel AJAX Denetim Araç Seti denetim Genişleticisi (C#) | Microsoft Docs
author: microsoft
description: Özel Genişleticileri özelleştirmek ve yeni sınıflar oluşturmak zorunda kalmadan ASP.NET denetimleri yeteneklerini genişletmek etkinleştirin.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 96b56eca-a892-45a4-96b4-67e61178650a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: 0dd243ec1709324174ff48b52e7dfb5185d3aaa2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390959"
---
<a name="creating-a-custom-ajax-control-toolkit-control-extender-c"></a>Özel AJAX Denetim Araç Seti denetim Genişleticisi (C#) oluşturma
====================
tarafından [Microsoft](https://github.com/microsoft)

> Özel Genişleticileri özelleştirmek ve yeni sınıflar oluşturmak zorunda kalmadan ASP.NET denetimleri yeteneklerini genişletmek etkinleştirin.


Bu öğreticide, bir özel AJAX Denetim Araç Seti denetim Genişleticisi oluşturma konusunda bilgi edinin. Basit, ancak bir metin kutusuna bir metin yazın, etkin için devre dışı bir düğmenin durumu değişir kullanışlı, yeni genişletici oluştururuz. Bu öğreticide okuduktan sonra ASP.NET AJAX araç seti ile kendi denetim genişleticilerini genişletmek mümkün olacaktır.

Visual Studio veya Visual Web Developer kullanarak bir özel denetim genişleticilerini oluşturabilirsiniz (Visual Web Developer en son sürümüne sahip olduğunuzdan emin olun).

## <a name="overview-of-the-disabledbutton-extender"></a>DisabledButton genişletici genel bakış

Sunduğumuz yeni denetim genişletici DisabledButton genişletici adı verilir. Bu genişletici üç özellik vardır:

- TargetControlID - metin kutusu denetimini genişletir.
- TargetButtonIID - etkin veya devre dışı düğme.
- DisabledText - başlangıçta düğmede görüntülenen metni. Yazmaya başladığınızda, düğmenin düğme metin özelliğini değerini görüntüler.

Bir metin kutusu ve düğme denetimine DisabledButton genişletici bağlayın. Herhangi bir metin yazın önce düğmesi devre dışıdır ve metin kutusu ve düğme şöyle görünür:


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image1.png)

([Tam boyutlu görüntüyü görmek için tıklatın](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image3.png))


Yazılan metin başlattıktan sonra düğmesi etkinleştirilir ve metin kutusu ve düğme şöyle görünür:


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image4.png)

([Tam boyutlu görüntüyü görmek için tıklatın](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image6.png))


Bizim denetim Genişleticisi oluşturmak için size aşağıdaki üç dosyayı oluşturmanız gerekir:

- DisabledButtonExtender.cs - Bu dosya, Genişleticisi oluşturma, yönetme ve tasarım zamanında özelliklerini ayarlamanıza olanak sağlar sunucu tarafı denetim sınıfıdır. Ayrıca extender'ayarlanabilir özellikleri tanımlar. Bu özellikler aracılığıyla kod ve tasarım zamanında erişebilir ve DisableButtonBehavior.js dosyasında tanımlanan özellikler eşleşmesi.
- DisabledButtonBehavior.js--, Tüm istemci kod mantığınızı nereye ekleyeceksiniz bu dosyasıdır.
- Bu sınıf, DisabledButtonDesigner.cs - tasarım zamanı işlevselliği sağlar. Bu sınıf, Visual Studio/Visual Web Developer Tasarımcısı ile düzgün çalışması için Denetim genişletici istiyorsanız gerekir.

Bu nedenle bir denetim genişletici sunucu tarafı denetimlerdir, bir istemci-tarafı davranışı ve sunucu tarafı Tasarımcı sınıfını içerir. Aşağıdaki bölümlerde bu dosyaların üç oluşturmayı öğrenin.

## <a name="creating-the-custom-extender-website-and-project"></a>Özel genişletici Web sitesi ve proje oluşturma

İlk adım bir sınıf kitaplığı projesi ve Web sitesi Visual Studio/Visual Web Developer oluşturmaktır. Biz ll özel genişletici sınıf kitaplığı projesi oluşturun ve Web sitesi özel genişletici sınayın.

Web sitesiyle başlama s olanak tanır. Web sitesi oluşturmak için aşağıdaki adımları izleyin:

1. Menü seçeneğini **dosya, yeni Web sitesi**.
2. Seçin **ASP.NET Web sitesi** şablonu.
3. Yeni Web sitesi adı *websitesi1*.
4. Tıklayın **Tamam** düğmesi.

Ardından, biz denetim genişletici için kod içeren sınıf kitaplığı projesi oluşturmanız gerekir:

1. Menü seçeneğini **dosya, Ekle, yeni proje**.
2. Seçin **sınıf kitaplığı** şablonu.
3. Yeni sınıf kitaplığı adı ile **CustomExtenders**.
4. Tıklayın **Tamam** düğmesi.

Bu adımları tamamladıktan sonra Çözüm Gezgini penceresinde Şekil 1 gibi görünmelidir.


[![Web sitesi ve sınıf kitaplığı projesi ile çözüm](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image7.png)

**Şekil 01**: Web sitesi ve sınıf kitaplığı projesi olan çözüm ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image9.png))


Ardından, tüm gerekli derleme başvurularını sınıf kitaplığı projesine eklemeniz gerekir:

1. Menü seçeneği CustomExtenders projeye sağ tıklayıp **Başvuru Ekle**.
2. .NET sekmesini seçin.
3. Aşağıdaki derlemelere başvurular ekleyin:

    1. System.Web.dll
    2. System.Web.Extensions.dll
    3. System.Design.dll
    4. System.Web.Extensions.Design.dll
4. Gözat sekmesini seçin.
5. AjaxControlToolkit.dll derlemesine bir başvuru ekleyin. Başvuruluyor AJAX Denetim Araç Seti indirdiğiniz klasörde bulunur.

Bu adımları tamamladıktan sonra sınıf kitaplığı proje başvuruları klasörü Şekil 2'gibi görünmelidir.


[![Gerekli başvuruları olan başvuruları klasörü](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image10.png)

**Şekil 02**: gerekli başvuruları olan başvuruları klasörü ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image12.png))


## <a name="creating-the-custom-control-extender"></a>Özel denetim Genişleticisi oluşturma

Sınıf kitaplığımızı sahibiz, biz bizim genişletici denetimi oluşturmaya başlayabilirsiniz. Bir özel genişletici denetimi sınıfın (1 listeleme bakın) ile tam kemikleri Başlat s olanak tanır.

**1 - MyCustomExtender.cs listeleme**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample1.cs)]

Listeleme 1'deki denetim genişletici sınıfı hakkında dikkat edin birkaç şey vardır. İlk olarak, sınıfın temel ExtenderControlBase sınıfından devralan dikkat edin. Tüm AJAX Denetim Araç Seti genişletici denetimleri, bu temel sınıfından türetilir. Örneğin, temel sınıfın her denetim genişletici gerekli bir özelliktir TargetID özelliği içerir.

Ardından, sınıfın istemci komut dosyası için ilgili aşağıdaki iki öznitelik eklediğine dikkat edin:

- Bir derlemede gömülü olan bir kaynak olarak eklenmek üzere bir dosya WebResource - neden olur.
- ClientScriptResource - bir derlemeden alınacak bir betik kaynak neden olur.

WebResource özniteliği özel genişletici derlendiğinde MyControlBehavior.js JavaScript dosyasını derlemesine gömmek için kullanılır. ClientScriptResource öznitelik, bir web sayfasında özel genişletici kullanıldığında derlemeden MyControlBehavior.js betiği almak için kullanılır.


Sırayla çalışması Web kaynağı ve ClientScriptResource öznitelikler için JavaScript dosyası katıştırılmış bir kaynağı derlemeniz gerekir. Çözüm Gezgini penceresinde dosyayı seçin, özellik sayfasını açın ve değer atamak *gömülü kaynak* için **derleme eylemi** özelliği.


Denetim genişletici TargetControlType özniteliği eklediğine dikkat edin. Bu öznitelik tarafından denetim genişletici genişletilmiş denetim türünü belirtmek için kullanılır. Listeleme 1 olması durumunda, Denetim genişletici TextBox genişletmek için kullanılır.

Son olarak, özel genişletici MyProperty adlı bir özellik eklediğine dikkat edin. Özellik ExtenderControlProperty özniteliği ile işaretlenir. GetPropertyValue() ve SetPropertyValue() yöntemleri, özellik değeri, istemci tarafı davranışı için sunucu taraflı denetim genişletici geçirmek için kullanılır.

Devam edip bizim DisabledButton genişletici kodunu uygulamak s olanak tanır. Bu genişletici için kod listeleme 2'de bulunabilir.

**2 - DisabledButtonExtender.cs listeleme**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample2.cs)]

Listeleme 2 DisabledButton genişletici TargetButtonID ve DisabledText adlı iki özelliğe sahiptir. TargetButtonID özelliğine uygulanan IDReferenceProperty düğme denetiminin kimliği dışında her şey bu özelliğine atama engeller.

WebResource ve ClientScriptResource öznitelikleri ile bu genişletici DisabledButtonBehavior.js adlı bir dosyada bulunan bir istemci-tarafı davranışı ilişkilendirin. Bu JavaScript dosyası sonraki bölümde ele alır.

## <a name="creating-the-custom-extender-behavior"></a>Özel genişletici davranışı oluşturma

Bir denetim genişletici istemci-tarafı bileşeninin bir davranış olarak adlandırılır. Devre dışı bırakıp düğmeyi etkinleştirerek fiili mantığı DisabledButton davranışı yer alır. JavaScript kod davranışı için listeleme 3'te eklenmiştir.

**3 - DisabledButton.js listeleme**

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample3.js)]

Listeleme 3 JavaScript dosyasında DisabledButtonBehavior adlı bir istemci-tarafı sınıf içerir. Sunucu tarafı ikizi gibi bu sınıf TargetButtonID adlı iki özellik içerir ve kullanarak erişebileceğiniz DisabledText alma\_TargetButtonID/set\_TargetButtonID ve\_DisabledText/set\_ DisabledText.

Initialize() yöntemi olay işleyici keyup davranışı için hedef öğe ile ilişkilendirir. Her zaman bir harf bu davranışı ile ilişkili metin kutusuna yazdığınız işleyici keyup yürütür. İşleyici keyup etkinleştirir veya davranışla ilişkili metin herhangi bir metin içerip içermediğini bağlı olarak düğmesini devre dışı bırakır.

JavaScript dosyası katıştırılmış bir kaynağı olarak listeleme 3'te derlemek unutmayın. Çözüm Gezgini penceresinde dosyayı seçin, özellik sayfasını açın ve değer atamak *gömülü kaynak* için **derleme eylemi** özelliği (bkz: Şekil 3). Bu seçenek, hem Visual Studio ve Visual Web Developer kullanılabilir.


[![Bir JavaScript dosyası katıştırılmış bir kaynağı ekleme](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image13.png)

**Şekil 03**: katıştırılmış bir kaynağı bir JavaScript dosyası ekleme ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image15.png))


## <a name="creating-the-custom-extender-designer"></a>Özel genişletici Tasarımcısı oluşturma

Bizim genişletici tamamlanması oluşturmak için gereken son bir sınıf yoktur. Biz listeleme 4'te Tasarımcı sınıfını oluşturmanız gerekir. Bu sınıf, Visual Studio/Visual Web Developer Tasarımcısı ile doğru şekilde davranmaz genişletici yapmak için gereklidir.

**4 - DisabledButtonDesigner.cs listeleme**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample4.cs)]

Listeleme 4 tasarımcıda Tasarımcısı özniteliğine sahip DisabledButton genişletici ile ilişkilendirin. Bu DisabledButtonExtender Sınıf Tasarımcısı özniteliği uygulamak yapmanız gerekir:

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample5.cs)]

## <a name="using-the-custom-extender"></a>Özel genişletici kullanma

Biz DisabledButton denetim Genişleticisi oluşturma tamamladınız, ASP.NET sitemizin kullanılacak zaman var. İlk olarak biz özel genişletici araç kutusuna eklemeniz gerekir. Aşağıdaki adımları uygulayın:

1. Çözüm Gezgini penceresinde sayfanın çift tıklayarak bir ASP.NET sayfasını açın.
2. Araç kutusunu sağ tıklatın ve menü seçeneğini **öğelerini Seç**.
3. Araç kutusu öğelerini Seç iletişim kutusunda CustomExtenders.dll derlemeye göz atın.
4. Tıklayın **Tamam** iletişim kutusunu kapatmak için düğme.

Bu adımları tamamladıktan sonra DisabledButton denetim genişletici araç kutusunda görünmesi gerekir (bkz: Şekil 4).


[![Araç kutusunda DisabledButton](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image16.png)

**Şekil 04**: araç kutusunda DisabledButton ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image18.png))


Ardından, size yeni bir ASP.NET sayfası oluşturmanız gerekir. Aşağıdaki adımları uygulayın:

1. ShowDisabledButton.aspx adlı yeni bir ASP.NET sayfası oluşturun.
2. Bir ScriptManager sayfaya sürükleyin.
3. TextBox denetimi sayfaya sürükleyin.
4. Bir düğme denetimi sayfaya sürükleyin.
5. Özellikler penceresinde düğmesi ID özelliği değere değiştirin <em>btnSave</em> ve metin özelliğini değerini *Kaydet\**.
  

Bir standart ASP.NET metin kutusu ve düğme denetimi ile bir sayfa oluşturduk.

Ardından, biz TextBox denetimi DisabledButton genişletici ile genişletin gerekir:

1. Seçin **ekleme genişletici** görev seçeneği genişletici Sihirbazı iletişim kutusu açmak için (bkz: Şekil 5). İletişim kutusu özel bizim DisabledButton genişletici içerdiğine dikkat edin.
2. DisabledButton genişletici seçip tıklayın **Tamam** düğmesi.


[![Genişletici Sihirbazı iletişim kutusu](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image19.png)

**Şekil 05**: genişletici Sihirbazı iletişim kutusu ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image21.png))


Son olarak, biz DisabledButton genişletici özelliklerini ayarlayabilirsiniz. TextBox denetimi özelliklerini değiştirerek DisabledButton genişletici özelliklerini değiştirebilirsiniz:

1. Tasarımcıda seçin.
2. Özellikler penceresinde Genişleticileri düğümünü genişletin (bkz. Şekil 6).
3. Değer atamak *Kaydet* DisabledText özelliği ve değerini *btnSave* TargetButtonID özelliğine.


[![Genişletici özelliklerini ayarlama](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image22.png)

**Şekil 06**: genişletici özellikleri ayarlama ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image24.png))


Sayfa çalıştırdığınızda (F5 tuşlarına basarak) düğme denetimini başlangıçta devre dışı bırakıldı. Aşağıdaki metin kutusuna metin girerek başlamadan hemen sonra (bkz. Şekil 7) denetim düğmesi etkin.


[![Uygulamada DisabledButton genişletici](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image25.png)

**Şekil 07**: DisabledButton extender uygulamada ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image27.png))


## <a name="summary"></a>Özet

AJAX Denetim Araç Seti ile özel genişletici denetimleri nasıl genişletebileceğiniz açıklamak için bu öğreticinin amacı oluştu. Bu öğreticide, bir basit DisabledButton denetim genişletici oluşturduk. DisabledButtonExtender sınıfı DisabledButtonBehavior JavaScript davranışları ve DisabledButtonDesigner sınıfı oluşturarak bu genişletici uyguladık. Bir özel denetim genişletici oluşturduğunuzda bir dizi benzer adımları izleyin.

> [!div class="step-by-step"]
> [Önceki](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
> [İleri](get-started-with-the-ajax-control-toolkit-vb.md)
