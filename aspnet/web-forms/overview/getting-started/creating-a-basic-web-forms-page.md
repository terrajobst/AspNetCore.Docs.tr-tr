---
uid: web-forms/overview/getting-started/creating-a-basic-web-forms-page
title: Bir temel ASP.NET 4.5 Web Forms sayfası oluşturmak için Visual Studio 2013 kullanarak
author: Erikre
description: ''
ms.author: riande
ms.date: 03/03/2014
ms.assetid: a2f1c635-0817-4a9a-8c13-d5b5d29727c0
msc.legacyurl: /web-forms/overview/getting-started/creating-a-basic-web-forms-page
msc.type: authoredcontent
ms.openlocfilehash: eb1a4632caf00097012bd1757da44016a076630f
ms.sourcegitcommit: f43f430a166a7ec137fcad12ded0372747227498
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49391290"
---
# <a name="using-visual-studio-2013-to-create-a-basic-aspnet-45-web-forms-page"></a>Bir temel ASP.NET 4.5 Web Forms sayfası oluşturmak için Visual Studio 2013 kullanarak

=== tarafından [Erik Reitan](https://github.com/Erikre)

[!INCLUDE[](~/includes/rp.md)]

Bu izlenecek yol, Web geliştirme ortamında bir giriş sağlar [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) ve [Web için Visual Studio Express 2013 Microsoft](https://www.microsoft.com/visualstudio/11/downloads#express-web). Bu izlenecek yol, basit bir ASP.NET Web Forms sayfası oluşturma işleminde size yol gösterir ve yeni sayfa oluşturma, denetim ekleme ve kod yazmaya temel teknikleri gösterir.

Bu kılavuzda gösterilen görevler aşağıdakileri içerir:

- Bir dosya sistemi Web Forms uygulaması projesi oluşturun.
- Visual Studio ile hakkında bilgi edinme.
- Bir ASP.NET sayfasına oluşturuluyor.
- Denetimler ekleme.
- Olay işleyicileri ekleme.
- Çalıştıran ve Visual Studio'dan bir sayfasını test etme.

## <a name="prerequisites"></a>Önkoşullar

Bu izlenecek yolu tamamlamak için şunlar gerekir:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) veya [Web için Microsoft Visual Studio Express 2013](https://www.microsoft.com/visualstudio/11/downloads#express-web). .NET Framework otomatik olarak yüklenir. 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 ve Web için Visual Studio Express 2013 Microsoft genellikle için Visual Studio Bu öğretici serisinin denir.  
    >   
    > Visual Studio kullanıyorsanız, bu kılavuzda, seçtiğiniz varsayılır **Web geliştirme** ayarlar koleksiyonu, Visual Studio'yu ilk başlattığınızda. Daha fazla bilgi için [nasıl yapılır: Web geliştirme ortamı ayarlarını seçin](https://msdn.microsoft.com/library/ff521558.aspx).


## <a name="creating-a-web-application-project-and-a-page"></a>Bir Web uygulaması projesi ve bir sayfa oluşturma

<a id="sectionToggle0"></a>

Kılavuzun bu bölümünde, bir Web uygulaması projesi oluşturmak ve yeni bir sayfa ekleyin. Ayrıca, HTML metni ekleyin ve sayfayı tarayıcınızda çalıştırın.

### <a name="to-create-a-web-application-project"></a>Bir Web uygulaması projesi oluşturmak için

1. Microsoft Visual Studio'yu açın.
2. Üzerinde **dosya** menüsünde **yeni proje**.  
    ![Dosya menüsü](creating-a-basic-web-forms-page/_static/image1.png)

    **Yeni proje** iletişim kutusu görüntülenir.
3. Seçin **şablonları**  - &gt; **Visual C#**  - &gt; **Web** soldaki şablonları grubu.
4. Seçin **ASP.NET Web uygulaması** Orta sütunda şablonu.
5. Projenizi adlandırın ***BasicWebApp*** tıklatıp **Tamam** düğmesi.   
![Yeni Proje iletişim kutusu](creating-a-basic-web-forms-page/_static/image2.png)
6. Ardından, **Web Forms** şablonu ve tıklatın **Tamam** projeyi oluşturmak için.  
![Yeni ASP.NET projesi iletişim kutusu](creating-a-basic-web-forms-page/_static/image3.png)  

    Visual Studio Web Forms şablonu temel alan önceden oluşturulmuş işlevler içeren yeni bir proje oluşturur. Yalnızca size sağladığı bir *Home.aspx* sayfasında, bir *About.aspx* sayfasında, bir *Contact.aspx* sayfasında, ancak aynı zamanda kullanıcıların kaydeder ve kaydeden üyelik işlevselliğini içerir kimlik bilgilerini ve böylece kullanıcılar Web sitenize oturum açabilirsiniz. Yeni sayfa oluşturulduğunda, varsayılan olarak, Visual Studio sayfasında görüntüler **kaynak** sayfanın HTML öğelerini görebileceğiniz görünümü. Aşağıdaki gösterimde içinde görür **kaynak** adlı yeni bir Web sayfası oluşturduysanız görüntülemek *BasicWebApp.aspx*.  
    ![Kaynak Görünümü](creating-a-basic-web-forms-page/_static/image4.png)


### <a name="a-tour-of-the-visual-studio-web-development-environment"></a>Visual Studio Web geliştirme ortamını turu


Sayfa değiştirerek devam etmeden önce Visual Studio geliştirme ortamıyla tanımak kullanışlıdır. Aşağıda windows ve Visual Studio ve Web için Visual Studio Express araçları gösterilmektedir.

> [!NOTE] 
> 
> Bu diyagram, varsayılan windows ve pencere konumlarını gösterir. **Görünümü** menüsünde ek windows görüntüleme ve bunları yeniden düzenleme ve kendi tercihlerinize göre windows yeniden boyutlandırma olanak tanır. Değişiklikler için pencere düzenini yapılmadıysa, gördüğünüz şekilde eşleşmez.


 Visual Studio ortamı

![Visual Studio ortamı](creating-a-basic-web-forms-page/_static/image5.png)

### <a name="familiarize-yourself-with-the-web-designer"></a>Web Tasarımcısı ile kendinizi alıştırın

Yukarıdaki resimde inceleyin ve aşağıdaki listede, metni eşleşen pencere ve Araçlar kullanılan en yaygın olarak açıklar. (Tüm windows ve burada listelenen gördüğünüz araçları, yalnızca önceki resimde işaretlenmiştir.)

- Araç çubukları. Metin, metin bulma ve benzeri biçimlendirme için komutları sağlar. Bazı araç çubukları, yalnızca içinde çalışmakta olduğunuz seçtiğinizde kullanılabilir **tasarım** görünümü.
- **Çözüm Gezgini** penceresi. Dosya ve klasörleri Web uygulamanızda görüntüler.
- Belge penceresi. Sekmeli windows üzerinde çalıştığınız belge görüntüler. Sekmeleri tıklayarak belgeler arasında geçiş yapabilirsiniz.
- **Özellikleri** penceresi. Sayfa, HTML öğeleri, denetimleri ve diğer nesneler için ayarları değiştirmenize izin verir.
- Sekmeleri görüntüleyin. Aynı belgede farklı görünümleri ile var. **Tasarım** yakın WYSIWYG düzenleme surface görünümüdür. **Kaynak** görünüm sayfası için HTML düzenleyici olur. **Bölünmüş** görünüm görüntüler hem de **tasarım** görünümü ve **kaynak** belge görünümü. Aşağıdakilerle çalışacaksınız **tasarım** ve **kaynak** daha sonra bu kılavuzdaki görünümlerin. Web sayfaları'nda açmak isterseniz **tasarım** görünümünü **Araçları** menüsünde tıklayın **seçenekleri**seçin **HTML Tasarımcısı** düğüm ve değiştirme **Başlangıç sayfaları içinde** seçeneği.
- **Araç kutusu**. Denetimleri ve sayfanıza sürükleyebilirsiniz HTML öğeleri sağlar. **Araç kutusu** öğeler, ortak işlevi tarafından gruplanır.
- S **unucu Gezgini**. Veritabanı bağlantıları görüntüler. Sunucu Gezgini görünür değilse, Sunucu Gezgini Görünüm menüsünde'a tıklayın.


### <a name="creating-a-new-aspnet-web-forms-page"></a>Yeni bir ASP.NET Web Forms sayfası oluşturma


Yeni Web Forms kullanarak bir uygulama oluşturduğunuzda **ASP.NET Web uygulaması** proje şablonu, Visual Studio ekler adlı bir ASP.NET sayfasına (Web Forms sayfası) *Default.aspx*, yanı sıra birkaç diğer dosya ve klasörler. Kullanabileceğiniz *Default.aspx* sayfasında Web uygulamanız için giriş sayfası olarak. Ancak, bu kılavuz için oluşturur ve yeni bir sayfa ile çalışır.

### <a name="to-add-a-page-to-the-web-application"></a>Web uygulaması için bir sayfa eklemek için


1. Kapat *Default.aspx* sayfası. Bunu yapmak için dosya adını görüntüler sekmesine tıklayın ve sonra Kapat seçeneğine tıklayın.
2. İçinde **Çözüm Gezgini**, Web uygulamasının adını sağ tıklayın (Bu öğreticide uygulama adı **BasicWebSite**) ve ardından **Ekle**  - &gt; **Yeni öğe**.   
**Yeni Öğe Ekle** iletişim kutusu görüntülenir.
3. Seçin **Visual C#**  - &gt; **Web** soldaki şablonları grubu. Ardından, **Web formu** ortasından listesinde ve adlandırın *FirstWebPage.aspx*.   
    ![Yeni Öğe Ekle iletişim kutusu](creating-a-basic-web-forms-page/_static/image6.png)
4. Tıklayın **Ekle** projenize web sayfasına eklenecek.  
Visual Studio, yeni bir sayfa oluşturur ve açar.


### <a name="adding-html-to-the-page"></a>HTML sayfasına ekleme


Kılavuzun bu bölümünde, statik metin sayfasına ekler.

### <a name="to-add-text-to-the-page"></a>Metin eklemek için


1. Belge penceresinin en altında tıklayın **tasarım** geçmek için sekmesinde **tasarım** görünümü.

    Tasarım görünümü sayfayı WYSIWYG benzeri şekilde görüntüler. Bu noktada, sayfa dikdörtgen özetleyen bir kesikli çizgiye dışında boş olacak şekilde herhangi bir metin veya sayfadaki denetimleri yoktur. Bu dikdörtgen temsil eden bir **div** sayfadaki öğeyi.
2. Kesikli çizgiye tarafından özetlenen dikdörtgenin içindeki tıklayın.
3. Tür **Hoş Geldiniz Visual Web Developer** basın **ENTER** iki kez.

    Aşağıdaki çizimde, yazdığınız metni **tasarım** görünümü.

    ![Hoş Geldiniz metni Tasarım görünümünde](creating-a-basic-web-forms-page/_static/image7.png "metin Tasarım görünümünde Hoş Geldiniz")
4. Geçiş **kaynak** görünümü.

    HTML dosyasındaki gördüğünüz **kaynak** yazdığınız sırada oluşturduğunuz görünümü **tasarım** görünümü.  
    ![Statik metin ile Web sayfası](creating-a-basic-web-forms-page/_static/image8.png)


### <a name="running-the-page"></a>Sayfa çalıştırma


Sayfa denetimleri ekleyerek devam etmeden önce ilk kez çalıştırabilirsiniz.

### <a name="to-run-the-page"></a>Sayfayı çalıştırmak için


1. İçinde **Çözüm Gezgini**, sağ *FirstWebPage.aspx* seçip **Başlangıç Sayfası Ayarla**.
2. Tuşuna **CTRL + F5** sayfayı çalıştırmak için.

    Sayfanın tarayıcıda görüntülenir. Sayfanın oluşturduğunuz bir dosya adı uzantısına sahip olsa da *.aspx*, şu anda herhangi bir HTML sayfası gibi çalışır.

    Bir sayfa tarayıcıda görüntülenecek ayrıca sayfasında sağ tıklatabilirsiniz **Çözüm Gezgini** seçip **tarayıcıda görüntüle**.
3. Web uygulamasını durdurmak için tarayıcıyı kapatın.


## <a name="adding-and-programming-controls"></a>Ekleme ve denetimlerini programlama


<a id="sectionToggle1"></a>

Artık, sunucu denetimleri sayfasına ekleyeceksiniz. Düğmeler, etiketler, metin kutuları ve tanıdık diğer denetimler gibi sunucu denetimleri, Web formları sayfaları için tipik form işleme özellikleri sunar. Ancak, istemci yerine sunucu üzerinde çalışan kod denetimleriyle programlayabilirsiniz.

Ekleyeceksiniz bir [düğmesi](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) denetimi, bir [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) denetimi ve bir [etiket](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) sayfasına denetlemek ve işlemek için kod yazma [tıklayın](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) için olay [düğmesi](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) denetimi.

### <a name="to-add-controls-to-the-page"></a>Çalışma sayfasına denetimler ekleme


1. Tıklayın **tasarım** geçmek için sekmesinde **tasarım** görünümü.
2. Ekleme noktasını sonunda put **Hoş Geldiniz Visual Web Developer** metin ve ENTER tuşuna **ENTER** içinde biraz yer açmamız için beş veya daha fazla kez **div** öğe kutusunun.
3. İçinde **araç kutusu**, genişletme **standart** önceden genişletilmemişse grup.  
Genişletmeniz gerekebilir Not **araç kutusu** penceresini görüntülemek için soldaki.
4. Sürükleme bir [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) sayfaya denetlemek ve ortasında bırakın **div** olan öğenin kutu **Hoş Geldiniz Visual Web Developer** ilk satırında.
5. Sürükleme bir [düğmesi](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) sayfaya denetlemek ve sağındaki açılan [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) denetimi.
6. Sürükleme bir [etiket](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) sayfaya denetlemek ve ayrı bir satıra aşağıdaki bırakma [düğmesi](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) denetimi.
7. Yukarıdaki noktasını koymak [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) denetlemek ve ardından yazın **adınızı girin:** .

    Bu statik bir HTML metin başlığıdır [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) denetimi. Statik HTML ve aynı sayfadaki sunucu denetimleri karıştırabilirsiniz. Aşağıdaki gösterimde üç denetim nasıl görünür **tasarım** görünümü.

    ![Üç denetimleri Tasarım görünümünde](creating-a-basic-web-forms-page/_static/image9.png "Tasarım görünümünde üç denetimleri")


### <a name="setting-control-properties"></a>Denetim özelliklerini ayarlama


Visual Studio sayfadaki denetimleri özelliklerini ayarlamak için çeşitli yollar sunar. Kılavuzun bu bölümünde, Özellikler hem de ayarlayacak **tasarım** görünümü ve **kaynak** görünümü.

### <a name="to-set-control-properties"></a>Denetim özelliklerini ayarlamak için


1. İlk olarak, görüntüleme **özellikleri** öğesinden seçilerek windows **görünümü** menüsü -&gt; **diğer Windows**  - &gt; **Parametrenize penceresi**. Alternatif olarak seçebilirsiniz **F4** görüntülenecek **özellikleri** penceresi.
2. Seçin [düğmesi](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) denetimi ve ardından **özellikleri** penceresinde değerini ayarlayın **metin** için **görünen ad**. Girdiğiniz metin Tasarımcısı'nda düğme aşağıdaki çizimde gösterildiği gibi görünür.

    ![Düğme metnini ayarlayın](creating-a-basic-web-forms-page/_static/image10.png "Ayarla düğmesi metni")
3. Geçiş **kaynak** görünümü.

    **Kaynak** görünüm, HTML sunucu denetimleri için Visual Studio oluşturduğu öğeleri dahil olmak üzere sayfa için görüntüler. Denetimleri bildirilir HTML benzeri sözdizimi kullanarak etiket öneki kullanmalarıdır **asp:** ve öznitelik **runat =&quot;sunucu&quot;**.

    Denetim özelliklerini öznitelik olarak bildirilir. Ayarlandığında, [metin](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.text.aspx) özelliği [düğmesi](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) denetim, 1. adımda ayarı gerçekten **metin** denetimin biçimlendirme özniteliği.

    > [!NOTE] 
    > 
    > İçindeki tüm denetimleri olan bir **form** özniteliği olan öğe **runat =&quot;sunucu&quot;**. **Runat =&quot;sunucu&quot;**  özniteliği ve **asp:** sayfa çalıştığında, sunucu üzerinde ASP.NET tarafından işlenen şekilde denetim etiketlerini denetimleri işaretlemek için önek. Dışında kod **&lt;form runat =&quot;sunucu&quot; &gt;** ve **&lt;runat = betiği =&quot;sunucu&quot; &gt;** öğeleri ASP.NET kodu, açılış etiketinde bir öğenin içinde olmalıdır neden olan tarayıcıya değişmeden gönderilir **runat =&quot;sunucu&quot;**  özniteliği.
4. Sonra ek bir özellik ekleyeceksiniz [etiket](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) denetimi. Ekleme noktasından sonra doğrudan put **asp: Etiket** içinde **&lt;asp: Etiket&gt;** etiketleyin ve tuşuna **boşluk**.

    Belirleyebileceğiniz ayarlar için kullanılabilen özelliklerinin listesini görüntüleyen açılır listede görünür bir [etiket](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) denetimi. Bu özellik, denir **IntelliSense**, size yardımcı olur **kaynak** sayfasında sunucu denetimi, HTML öğeleri ve diğer öğeleri söz dizimi ile görüntüle. Aşağıdaki çizimde gösterildiği **IntelliSense** için açılır listede [etiket](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) denetimi.

    ![IntelliSense öznitelikleri](creating-a-basic-web-forms-page/_static/image11.png "IntelliSense öznitelikleri")
5. Seçin **ForeColor** ve ardından bir eşittir işareti yazın.

    IntelliSense renkleri listesini görüntüler.

    > [!NOTE] 
    > 
    > Görüntüleyebileceğiniz bir **IntelliSense** açılır listede tuşuna basarak istediğiniz zaman **CTRL + J** kod görüntülerken.
6. İçin bir renk seçin **[etiket](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)** denetiminin metin. Beyaz arka plan karşı okumak için koyu bir renk seçtiğinizden emin olun.

    **ForeColor** özniteliği, kapanış tırnak işareti de dahil olmak üzere, seçtiğiniz rengi ile tamamlandı.


### <a name="programming-the-button-control"></a>Düğme denetimini programlama


Bu kılavuz için kullanıcının metin kutusu içine girer ve sonra adını görüntüler adı okuyan kod yazacaksınız [etiket](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) denetimi.

### <a name="add-a-default-button-event-handler"></a>Varsayılan düğme olayı işleyicisi ekleyin


1. Geçiş **tasarım** görünümü.
2. Çift [düğmesi](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) denetimi.

    Varsayılan olarak, Visual Studio bir arka plan kod dosyasına geçer ve için iskelet olay işleyicisi oluşturur [düğmesi](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) denetimin varsayılan olayı [tıklayın](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) olay. Arka plan kod dosyası, kullanıcı Arabiriminde biçimlendirmeyi (HTML) (C# gibi), sunucu kodundan ayırır.   
   İmleci eklenen için bu olay işleyicisi için kod konumlandırıldı.

    > [!NOTE] 
    > 
    > Bir denetime çift tıklama **tasarım** görünümü, olay işleyicileri oluşturabileceğiniz çeşitli yollar yalnızca biridir.
3. İçinde **Button1\_tıklayın** olay işleyicisi, türü **Label1** bir nokta (**.**).

    Süresi yazdığınızda **kimliği** etiketin (**Label1**), Visual Studio için kullanılabilir üyeler bir listesini görüntüler [etiket](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) aşağıda gösterildiği gibi denetim Şekil. Üye yaygın bir özellik, yöntemi veya olay.

    ![IntelliSense kod görünümünde](creating-a-basic-web-forms-page/_static/image12.png "IntelliSense içinde kod görünümü")
4. Son **tıklayın** aşağıdaki kod örneğinde gösterildiği şekilde okunacak şekilde düğmesi için olay işleyicisi.

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample1.cs?highlight=3)]

    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample2.vb?highlight=2)]
5. Geçiş görüntülemeye geri **kaynak** sağ tıklayarak, HTML biçimlendirmesi görünümünü *FirstWebPage.aspx* içinde **Çözüm Gezgini** seçerek **görüntüle Biçimlendirme**.
6. Kaydırma **&lt;asp: Button&gt;** öğesi. Unutmayın **&lt;asp: Button&gt;** artık öğesinin öznitelik **onclick =&quot;Button1\_tıklayın&quot;**.

    Bu öznitelik düğmenin bağlar [tıklayın](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) olay işleyicisi yöntemine önceki adımda kodlanmış.

    Olay işleyicisi yöntemleri, herhangi bir ad olabilir; gördüğünüz adı, Visual Studio tarafından oluşturulan varsayılan addır. Önemli adı için kullanılan noktasıdır **OnClick** HTML öznitelik arka plan kod içinde tanımlanan bir metot adı eşleşmelidir.


### <a name="running-the-page"></a>Sayfa çalıştırma


Sayfadaki sunucu denetimleri artık test edebilirsiniz.

### <a name="to-run-the-page"></a>Sayfayı çalıştırmak için


1. Tuşuna **CTRL + F5** sayfanın tarayıcıda çalıştırılacak. Bir hata oluşursa, yukarıdaki adımları yeniden denetleyin.
2. Metin kutusuna bir ad girin ve tıklayın **görünen ad** düğmesi.

    Girdiğiniz ad, görüntülenen [etiket](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) denetimi. Düğmeye tıkladığınızda, sayfa Web sunucusuna gönderilen unutmayın. Ardından ASP.NET sayfası yeniden oluşturur, kodunuz çalışır (Bu durumda, [düğmesi](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) denetimin [tıklayın](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) olay işleyicisi çalışır) ve ardından yeni bir sayfa tarayıcıya gönderir. Durum çubuğu tarayıcıda izleyin, sayfanın düğmesine her zaman Web sunucuya gidiş dönüş yapıyor görebilirsiniz.
3. Tarayıcıda, sayfada sağ tıklatıp seçerek çalıştırmakta sayfa kaynağını görüntüleyerek **kaynağı görüntüle**.

    Sayfa kaynak kodunda herhangi bir sunucu kodunu HTML bakın. Özellikle, görmez **&lt;asp:&gt;** ile çalışmakta olduğunuz öğeleri **kaynak** görünümü. Sayfa çalıştığında, ASP.NET sunucu denetimleri işler ve denetimi temsil eden işlevleri gerçekleştiren HTML öğelerini sayfasına işler. Örneğin, **&lt;asp: Button&gt;** denetimi, HTML olarak işlenir **&lt;giriş türü =&quot;gönderme&quot; &gt;** öğe.
4. Tarayıcıyı kapatın.


## <a name="working-with-additional-controls"></a>Ek denetimler ile çalışma

<a id="sectionToggle2"></a>

Kılavuzun bu bölümünde, sizinle birlikte çalışır [Takvim](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) denetimi, aynı anda ayda bir tarih görüntüler. [Takvim](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) denetimi, çalışmaması ile düğme, metin kutusu ve etiket değerinden daha karmaşık bir denetimdir ve başka bazı özellikleri, sunucu denetimleri gösterir.

Bu bölümde, ekleyeceksiniz bir [System.Web.UI.WebControls.Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) sayfasına denetlemek ve biçimlendirin.

### <a name="to-add-a-calendar-control"></a>Bir Takvim denetimi eklemek için


1. Visual Studio'da geçiş **tasarım** görünümü.
2. Gelen **standart** bölümünü **araç kutusu**, sürükleyin bir [Takvim](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) sayfaya denetlemek ve aşağıdaki açılan **div** öğesi, başka denetimler de içerir.

    Takvimin akıllı etiket paneli görüntülenir. Panelinde Seçili denetim için en yaygın görevleri kolaylaştıran komutları görüntüler. Aşağıdaki çizimde gösterildiği [Takvim](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) olarak işlenen denetim **tasarım** görünümü.

    ![Takvim denetimi Tasarım görünümünde](creating-a-basic-web-forms-page/_static/image13.png "Takvim denetimi Tasarım görünümünde")
3. Akıllı etiket panelinde seçin **otomatik biçimlendirme**.

    **Otomatik biçimlendirme** iletişim kutusu görüntülenir, takvim için bir biçimlendirme düzeni seçmenize olanak sağlar. Aşağıdaki çizimde gösterildiği **otomatik biçimlendirme** iletişim kutusu için [Takvim](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) denetimi.

    ![Otomatik Biçim iletişim kutusu (Takvim denetimi)](creating-a-basic-web-forms-page/_static/image14.png "otomatik biçimlendirme iletişim kutusu (Takvim denetimi)")
4. Gelen **bir düzen seçin** listesinde, seçin **basit** ve ardından **Tamam**.
5. Geçiş **kaynak** görünümü.

    Gördüğünüz **&lt;asp: Takvim&gt;** öğesi. Bu öğe daha önce oluşturduğunuz basit denetimler için öğeleri daha uzun. Ayrıca, alt gibi içerir  **&lt;WeekEndDayStyle&gt;**, çeşitli biçimlendirme ayarlarını temsil eder. Aşağıdaki çizimde gösterildiği [Takvim](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) denetim **kaynak** görünümü. (Gördüğünüz tam biçimlendirmeyi **kaynak** görünümü farklı gösterimden.)

    ![Takvim denetimi kaynak görünümünde](creating-a-basic-web-forms-page/_static/image15.png "Takvim denetimi kaynak görünümünde")


### <a name="programming-the-calendar-control"></a>Takvim denetimlerini programlama


Bu bölümde, programlayacaksınız [Takvim](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) denetimi şu anda seçili tarihini görüntülemek için.

### <a name="to-program-the-calendar-control"></a>Programa Takvim denetimi


1. İçinde **tasarım** görüntülemek için çift [Takvim](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) denetimi.

    Yeni bir olay işleyici oluşturulur ve adlandırılmış arka plan kod dosyasında görüntülenen *FirstWebPage.aspx.cs*.
2. Son [SelectionChanged](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selectionchanged.aspx) aşağıdaki kod ile olay işleyicisi.

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample3.cs?highlight=3)]


    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample4.vb?highlight=2)]

    Yukarıdaki kod, etiket denetiminin metin Takvim denetiminin seçili tarihe ayarlar.


### <a name="running-the-page"></a>Sayfa çalıştırma


Artık, Takvim test edebilirsiniz.

### <a name="to-run-the-page"></a>Sayfayı çalıştırmak için


1. Tuşuna **CTRL + F5** sayfanın tarayıcıda çalıştırılacak.
2. Takvimdeki bir tarihi tıklayın.

    Tıkladığınız tarih görüntülenen [etiket](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) denetimi.
3. Tarayıcıda sayfası için kaynak kodunu görüntüleyin.

    Unutmayın [Takvim](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) denetim işlenen sayfasına bir **tablo**, her gün ile bir **td** öğesi.
4. Tarayıcıyı kapatın.


## <a name="next-steps"></a>Sonraki Adımlar


<a id="nextStepsToggle"></a>

Bu izlenecek yol, Visual Studio sayfa tasarımcıyı temel özelliklerinde gösterilen. Visual Studio'da Web Forms sayfası oluşturup yapacağınızı anladığınıza göre diğer özellikleri incelemek isteyebilirsiniz. Örneğin, aşağıdakileri yapmak isteyebilirsiniz:

- ASP.NET Web Forms hakkında daha fazla adım adım öğretici serisinin izleyerek bilgi [ASP.NET 4.5 Web Forms ve Visual Studio 2013 ile çalışmaya başlama](getting-started-with-aspnet-45-web-forms/introduction-and-overview.md).
- Basamaklama stil sayfaları (CSS) hakkında daha fazla bilgi edinin. Ayrıntılar için bkz [CSS özeti ile birlikte çalışma](https://msdn.microsoft.com/library/bb398931.aspx).
