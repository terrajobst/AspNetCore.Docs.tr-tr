---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
title: ColorPicker denetim genişletici (VB) kullanarak | Microsoft Docs
author: microsoft
description: ColorPicker kullanıcı Arabirimi ile bir açılan denetimden içinde istemci tarafı renk çekme işlevselliği sağlayan bir ASP.NET AJAX genişletici ' dir. Tüm ASP.NET eklenebilecek...
ms.author: aspnetcontent
ms.date: 05/12/2009
ms.assetid: 577ae07b-a872-4818-a804-bca489b40ad0
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: e7375dcfc354e931f30d2250081f424bd2149953
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828137"
---
<a name="using-the-colorpicker-control-extender-vb"></a>ColorPicker denetim genişletici (VB) kullanarak
====================
tarafından [Microsoft](https://github.com/microsoft)

> ColorPicker kullanıcı Arabirimi ile bir açılan denetimden içinde istemci tarafı renk çekme işlevselliği sağlayan bir ASP.NET AJAX genişletici ' dir. Herhangi bir ASP.NET TextBox denetimi eklenebilir. .


Bu öğreticinin amacı, AJAX Denetim Araç Seti ColorPicker denetim genişletici nasıl kullanabileceğinizi açıklar sağlamaktır. ColorPicker denetim genişletici rengi seçmenize olanak sağlayan bir açılır iletişim kutusu görüntüler. ColorPicker bir renk seçmek bir kullanıcı için bir sezgisel kullanıcı arabirimiyle sağlamak istediğinizde yararlıdır.

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a>ColorPicker denetim genişletici ile bir metin kutusu denetimini genişletme

Örneğin, özelleştirilmiş iş kartları oluşturmak ziyaretçilerinin sağlayan bir Web sitesi oluşturmak istediğinizi düşünün. Ziyaretçi için bir iş kartı metin girebilir ve renk seçin. ASP.NET sayfası listeleme 1 txtCardText ve txtCardColor adlı iki TextBox denetimleri içerir. Formu gönderdiğinde, seçili değerler görüntülenir (bkz. Şekil 1).


[![Basit bir iş kartı oluşturma formu](using-the-colorpicker-control-extender-vb/_static/image1.jpg)](using-the-colorpicker-control-extender-vb/_static/image1.png)

**Şekil 01**: bir iş kartı oluşturmak için basit bir form ([tam boyutlu görüntüyü görmek için tıklatın](using-the-colorpicker-control-extender-vb/_static/image2.png))


**1 - CreateCard.aspx listeleme**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample1.aspx)]

1 listeleme çalışır, ancak biçiminde harika kullanıcı deneyimini sağlamaz. Kullanıcı bir renk TextBox'a türüne sahiptir. Örneğin, kullanıcı özel bir renk - isterse yoğun saatlerin yeşil - ardından kullanıcı yalnızca doğru gölge HTML rengi kodunu yardıma gerek kalmadan şekil gerekir.

ColorPicker denetim genişletici daha iyi bir kullanıcı deneyimi oluşturmak için kullanabilirsiniz. TextBox denetimi için odak taşıdığınızda ColorPicker bir renk iletişim kutusu görüntüler (bkz: Şekil 2).


[![ColorPicker denetim genişletici](using-the-colorpicker-control-extender-vb/_static/image2.jpg)](using-the-colorpicker-control-extender-vb/_static/image3.png)

**Şekil 02**: ColorPicker denetim genişletici ([tam boyutlu görüntüyü görmek için tıklatın](using-the-colorpicker-control-extender-vb/_static/image4.png))


ColorPicker denetim genişletici listeleme 1 biçiminde kullanmak için iki adımı tamamlamamız gerekir:

1. Bir ScriptManager denetimi sayfasına ekleme
2. ColorPicker denetim genişletici sayfasına ekleme

ColorPicker kullanabilmeniz için önce bir ScriptManager sayfanıza eklemeniz gerekir. ScriptManager eklemek iyi bir sağ açılış sunucu tarafı yerdir &lt;form&gt; etiketi. ScriptManager, sayfaya (ScriptManager AJAX uzantılar sekmesi altında bulunur) araç kutusundan sürükleyebilirsiniz. Alternatif olarak, aşağıdaki etiketi altında sunucu tarafı formu etiketiyle kaynak görünüme yazabilirsiniz:

&lt;ASP: ScriptManager kimliği = "ScriptManager1" runat = "server" /&gt;

Tasarım görünümünde ColorPicker denetim genişletici sayfasına eklemek için en kolay yoludur. Farenizi metin txtCardColor gelin, akıllı görev seçeneği sağlar görünür bir genişletici eklemeyi (bkz: Şekil 3). Bu seçeneği seçerseniz, genişletici Sihirbazı'nı (bkz: Şekil 4) görünür.


[![Bir Genişletici ekleme](using-the-colorpicker-control-extender-vb/_static/image3.jpg)](using-the-colorpicker-control-extender-vb/_static/image5.png)

**Şekil 03**: Bir Genişletici ekleme ([tam boyutlu görüntüyü görmek için tıklatın](using-the-colorpicker-control-extender-vb/_static/image6.png))


[![Genişletici Sihirbazı ile bir denetim genişletici seçme](using-the-colorpicker-control-extender-vb/_static/image4.jpg)](using-the-colorpicker-control-extender-vb/_static/image7.png)

**Şekil 04**: bir denetim genişletici genişletici Sihirbazı'nı seçerek ([tam boyutlu görüntüyü görmek için tıklatın](using-the-colorpicker-control-extender-vb/_static/image8.png))


' % S'txtCardColor TextBox ColorPicker genişletici ile genişletmek için ColorPicker genişletici seçebilirsiniz. İletişim kutusunu kapatmak için Tamam'a tıklayın.

Bu değişiklikleri yaptıktan sonra sayfa için kaynak listeleme 2 gibi görünüyor.

**2 - CreateCard.aspx (ColorPicker ile) listeleme**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample2.aspx)]

Sayfa artık doğrudan txtCardColor TextBox denetiminde görünen ColorPickerExtender denetim içerdiğine dikkat edin. Böylece bir Renk Seçici iletişim kutusu görüntüler ColorPickerExtender denetimi txtCardColor denetim genişletir.

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a>Renk Seçici iletişim kutusunu başlatmak için bir düğme kullanma

ColorPicker genişletici aşağıdaki özellikleri destekler:

- PopupButtonId - kimliği bir düğmeye sayfasında görüntülenecek Renk Seçici iletişim kutusunu neden olur.
- PopupPosition - Renk Seçici iletişim kutusu, hedef denetime göre konumu. Olası değerler şunlardır: mutlak, merkezi, sol alt, BottomRight, sol üst, sağ üst, sağ ve sol (sol alt varsayılandır).
- SampleControlId - seçilen rengin görüntüleyen bir denetimi Kimliği'ni tıklatın.
- SelectedColor - ColorPicker tarafından seçilen ilk renk.

Bu özellikler, bir Renk Seçici iletişim kutusunu nasıl görüntüleneceğini ve seçilen rengin nasıl görüntüleneceğini özelleştirmek için kullanabilirsiniz. Sayfa listesi 3'te birkaç bu özelliklerin nasıl kullanabileceğinizi gösterir.

**3 - CreateCardButton.aspx listeleme**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample3.aspx)]

Renk Seç sayfasında listeleme 3 içerir (bkz: Şekil 5) düğmesi. Bu düğmeye tıkladığınızda, metin kutusunun Renk Seçici iletişim kutusu görünür. İletişim kutusundan bir renk seçin, seçilen rengin lblSample etiket denetiminin arka plan rengi görünür.

ColorPicker PopupButtonID özelliği, çekme rengi düğmesi ColorPicker genişletici ile ilişkilendirmek için kullanılır. PopupButtonID özelliği için bir değer sağladığında, hedef denetim odağa sahip olduğunda Renk Seçici iletişim bundan böyle görünür. İletişim kutusunu görüntülemek için düğmesine tıklamanız gerekir.

SampleControlID özellik ColorPicker seçilen rengi görüntüleyen bir denetimi ilişkilendirmek için kullanılır. ColorPicker bu denetimin arka plan rengini şu anda seçilen renge değişir.


[![Renk Seçici iletişim kutusunu bir düğme ile görüntüleme](using-the-colorpicker-control-extender-vb/_static/image5.jpg)](using-the-colorpicker-control-extender-vb/_static/image9.png)

**Şekil 05**: Renk Seçici iletişim kutusunu bir düğme ile görüntüleme ([tam boyutlu görüntüyü görmek için tıklatın](using-the-colorpicker-control-extender-vb/_static/image10.png))


## <a name="summary"></a>Özet

Bu öğreticide, bir açılan Renk Seçici iletişim kutusunu görüntülemek için ColorPicker denetim genişletici kullanmayı öğrendiniz. İlk olarak, odağı bir TextBox denetimine taşındığında nasıl iletişim görüntüleyebilirsiniz incelenir. Ardından, düğmeye tıklandığında, Renk Seçici iletişim kutusunu görüntüleyen bir düğmenin nasıl oluşturulacağını öğrendiniz.

> [!div class="step-by-step"]
> [Önceki](using-the-colorpicker-control-extender-cs.md)
