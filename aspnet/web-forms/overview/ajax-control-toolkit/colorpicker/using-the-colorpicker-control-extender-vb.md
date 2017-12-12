---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
title: ColorPicker denetim Extender (VB) ile | Microsoft Docs
author: microsoft
description: "ColorPicker kullanıcı Arabirimi ile bir açılan denetiminde istemci-tarafı renk çekme işlevselliği sağlayan ASP.NET AJAX genişletici ' dir. Tüm ASP.NET eklenebilecek..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 577ae07b-a872-4818-a804-bca489b40ad0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: 7453845909b2c0bd8d6b476b19d0fbc5050f7460
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="using-the-colorpicker-control-extender-vb"></a>ColorPicker denetim genişletici (VB) kullanma
====================
tarafından [Microsoft](https://github.com/microsoft)

> ColorPicker kullanıcı Arabirimi ile bir açılan denetiminde istemci-tarafı renk çekme işlevselliği sağlayan ASP.NET AJAX genişletici ' dir. Herhangi bir ASP.NET TextBox denetimi eklenebilir. .


Bu öğreticinin amacı AJAX Denetim Araç Seti ColorPicker denetim genişletici nasıl kullanabileceğinizi açıklar belirlemektir. ColorPicker denetim genişletici bir renk seçmenizi sağlayan bir açılır iletişim kutusu görüntüler. Bir renk seçmek bir kullanıcı için bir sezgisel bir kullanıcı arabirimi sağlamak istediğinizde ColorPicker yararlıdır.

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a>TextBox denetimi ColorPicker denetim genişletici ile genişletme

Örneğin, özelleştirilmiş kartvizitler oluşturmak ziyaretçileri sağlayan bir Web sitesi oluşturmak istediğinizi varsayalım. Ziyaretçi için bir iş kart metni girin ve rengi seçin. ASP.NET sayfası listeleme 1 txtCardText ve txtCardColor adlı iki TextBox denetimleri içerir. Formu gönderdiğinde, seçili değerler görüntülenir (bkz: Şekil 1).


[![Bir iş kartı oluşturmak için basit form](using-the-colorpicker-control-extender-vb/_static/image1.jpg)](using-the-colorpicker-control-extender-vb/_static/image1.png)

**Şekil 01**: bir iş kartı oluşturmak için basit form ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-the-colorpicker-control-extender-vb/_static/image2.png))


**1 - CreateCard.aspx listeleme**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample1.aspx)]

Formun listeleme 1 çalışır, ancak bir iyi kullanıcı deneyimi sağlamaz. Bir renk metin kutusuna yazmak kullanıcı vardır. Özel renk - kullanıcı istiyorsa, örneğin, yalnızca pea yeşil - sonra kullanıcı sağ gölgesini HTML renk kodu yardıma olmadan tahmin gerekir.

ColorPicker denetim genişletici daha iyi bir kullanıcı deneyimi oluşturmak için kullanabilirsiniz. TextBox denetimi odağı taşıdığınızda ColorPicker bir renk iletişim kutusu görüntüler (bkz: Şekil 2).


[![ColorPicker denetim genişletici](using-the-colorpicker-control-extender-vb/_static/image2.jpg)](using-the-colorpicker-control-extender-vb/_static/image3.png)

**Şekil 02**: ColorPicker denetim genişletici ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-the-colorpicker-control-extender-vb/_static/image4.png))


ColorPicker denetim genişletici listeleme 1 formunda ile kullanmak için iki adımları tamamlamanız gerekir:

1. Sayfaya bir ScriptManager denetimi ekleyin
2. ColorPicker denetim genişletici sayfasına ekleme

ColorPicker kullanabilmeniz için önce bir ScriptManager sayfanıza eklemeniz gerekir. ScriptManager eklemek için sağ açılış sunucu tarafı uygundur &lt;form&gt; etiketi. (ScriptManager AJAX uzantılar sekmesi altında bulunur) araç sayfaya ScriptManager sürükleyin. Alternatif olarak, aşağıdaki etiketi açma sunucu tarafı form etiketi altında kaynak görünüme yazabilirsiniz:

&lt;ASP: ScriptManager kimliği = "ScriptManager1" runat = "Server" /&gt;

Tasarım görünümünde ColorPicker denetim genişletici eklemek için en kolay yoludur. Farenizi TextBox txtCardColor gelin, bir akıllı görev seçeneği etkinleştirir görünür genişletici eklemenizi (bkz: Şekil 3). Bu seçeneği seçerseniz, (bkz: Şekil 4) genişletici Sihirbazı görünür.


[![Genişletici ekleme](using-the-colorpicker-control-extender-vb/_static/image3.jpg)](using-the-colorpicker-control-extender-vb/_static/image5.png)

**Şekil 03**: genişletici ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-the-colorpicker-control-extender-vb/_static/image6.png))


[![Genişletici Sihirbazı'nı kullanarak bir denetim extender seçme](using-the-colorpicker-control-extender-vb/_static/image4.jpg)](using-the-colorpicker-control-extender-vb/_static/image7.png)

**Şekil 04**: denetim genişletici genişletici Sihirbazı'nı seçerek ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-the-colorpicker-control-extender-vb/_static/image8.png))


ColorPicker genişletici ile TextBox txtCardColor genişletmek için ColorPicker genişletici seçebilirsiniz. İletişim kutusunu kapatmak için Tamam'ı tıklatın.

Bu değişiklikleri yaptıktan sonra sayfa kaynağı listeleme 2 gibi görünüyor.

**2 - CreateCard.aspx (ile ColorPicker) listeleme**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample2.aspx)]

Sayfa artık doğrudan txtCardColor TextBox denetimi görünen ColorPickerExtender denetim içerdiğine dikkat edin. Böylece bir Renk Seçici iletişim kutusu görüntüler ColorPickerExtender denetim txtCardColor denetim genişletir.

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a>Renk Seçici iletişim düğme kullanarak

ColorPicker genişletici aşağıdaki özellikleri destekler:

- PopupButtonId - görünmesi Renk Seçici iletişim neden sayfasındaki bir düğmeyi Kimliği'ni tıklatın.
- PopupPosition - hedef denetimin Renk Seçici iletişim göre konumu. Olası değerler şunlardır: mutlak, merkezi, sol alt, BottomRight, sol üst, sağ üst, sağ ve sol (sol alt varsayılandır).
- SampleControlId - seçilen rengin görüntüleyen bir denetim kimliği'ni tıklatın.
- SelectedColor - ColorPicker tarafından seçilen ilk renk.

Renk Seçici iletişim nasıl görüntülenir ve seçilen rengin nasıl görüntüleneceğini özelleştirmek için bu özellikleri kullanabilirsiniz. Sayfanın listeleme 3'te birkaç bu özelliklerin nasıl kullanabileceğiniz gösterilmektedir.

**3 - CreateCardButton.aspx listeleme**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample3.aspx)]

Renk Seç sayfasında listeleme 3'te içerir (bkz. Şekil 5) düğmesine tıklayın. Bu düğmeye tıkladığınızda, TextBox Renk Seçici iletişim kutusu görüntülenir. İletişim kutusundan bir renk seçerseniz seçili renk etiket denetimi lblSample arka plan rengi olarak görünür.

ColorPicker PopupButtonID özelliği, Renk Seç düğmesini ColorPicker extender ile ilişkilendirmek için kullanılır. PopupButtonID özelliği için bir değer sağladığında, hedef denetimi odağa sahip olduğunda Renk Seçici iletişim kutusu artık görünür. İletişim kutusunu görüntülemek için düğmesini gerekir.

SampleControlID özelliği ColorPicker seçili renkle görüntüleyen bir denetim ilişkilendirmek için kullanılır. ColorPicker bu denetim arka plan rengini şu anda seçili renk değiştirir.


[![Düğme için Renk Seçici iletişim kutusunu görüntüleme](using-the-colorpicker-control-extender-vb/_static/image5.jpg)](using-the-colorpicker-control-extender-vb/_static/image9.png)

**Şekil 05**: düğme için Renk Seçici iletişim kutusunu görüntüleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-the-colorpicker-control-extender-vb/_static/image10.png))


## <a name="summary"></a>Özet

Bu öğreticide, ColorPicker denetim genişletici açılan Renk Seçici iletişim kutusunu görüntülemek için nasıl kullanılacağı hakkında bilgi edindiniz. İlk olarak, odak TextBox denetimine taşındığında nasıl iletişim kutusunu görüntüleyebilir incelendi. Ardından, Renk Seçici iletişim düğmesine tıklandığında görüntüleyen bir düğmeye oluşturma hakkında bilgi edindiniz.

>[!div class="step-by-step"]
[Önceki](using-the-colorpicker-control-extender-cs.md)
