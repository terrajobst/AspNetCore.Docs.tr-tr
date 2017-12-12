---
uid: web-forms/overview/getting-started/creating-a-basic-web-forms-page
title: "4.5 Web sayfası Visual Studio 2013'te Forms temel ASP.NET oluşturma | Microsoft Docs"
author: Erikre
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/03/2014
ms.topic: article
ms.assetid: a2f1c635-0817-4a9a-8c13-d5b5d29727c0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/creating-a-basic-web-forms-page
msc.type: authoredcontent
ms.openlocfilehash: 20e920ff63444c0d69cecb972619b07fe6d23097
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-basic-aspnet-45-web-forms-page-in-visual-studio-2013"></a>4.5 Web sayfası Visual Studio 2013'te Forms temel ASP.NET oluşturma
====================
Tarafından [Erik Reitan](https://github.com/Erikre)

Bu kılavuz, Web geliştirme ortamında bir giriş sağlar [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/en-us/downloads#vs) ve [Web için Visual Studio Express 2013 Microsoft](https://www.microsoft.com/visualstudio/11/en-us/downloads#express-web). Bu kılavuz, basit bir ASP.NET Web Forms sayfası oluşturma size rehberlik eder ve yeni sayfa oluşturma, denetimler ekleme ve kod yazma temel teknikleri göstermektedir.

Bu örneklerde gösterilen görevler aşağıdakileri içerir:

- Bir dosya sistemi Web Forms uygulaması projesi oluşturma.
- Visual Studio ile hakkında bilgi edinme.
- Bir ASP.NET sayfası oluşturuluyor.
- Denetimler ekleme.
- Olay işleyicileri ekleme.
- Çalıştıran ve bir sayfa Visual Studio'dan Test etme.

## <a name="prerequisites"></a>Önkoşullar


Bu kılavuzu tamamlamak için gerekir:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/en-us/downloads#vs) veya [Web için Microsoft Visual Studio Express 2013](https://www.microsoft.com/visualstudio/11/en-us/downloads#express-web). .NET Framework otomatik olarak yüklenir. 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 ve Web için Visual Studio Express 2013 Microsoft genellikle için Visual Studio Bu öğretici seri anılacaktır.  
    >   
    > Visual Studio kullanıyorsanız, bu kılavuzda, seçtiğiniz varsayar **Web geliştirme** ayarlar koleksiyonu, Visual Studio ilk başlattığınızda. Daha fazla bilgi için bkz: [nasıl yapılır: Web geliştirme ortamı ayarlarını seçin](https://msdn.microsoft.com/library/ff521558.aspx).


## <a name="creating-a-web-application-project-and-a-page"></a>Bir Web uygulaması projesi ve bir sayfa oluşturma

<a id="sectionToggle0"></a>

Kılavuzun bu bölümünde, bir Web uygulaması projesi oluşturun ve yeni bir sayfa ekleyin. Ayrıca, HTML metin ekleyebilir ve sayfayı tarayıcınızda çalıştırın.

### <a name="to-create-a-web-application-project"></a>Bir Web uygulaması projesi oluşturmak için

1. Microsoft Visual Studio'yu açın.
2. Üzerinde **dosya** menüsünde, select **yeni proje**.  
    ![Dosya menüsü](creating-a-basic-web-forms-page/_static/image1.png)

    **Yeni proje** iletişim kutusu görüntülenir.
3. Seçin **şablonları**  - &gt; **Visual C#**  - &gt; **Web** soldaki templates grubu.
4. Seçin **ASP.NET Web uygulaması** Orta sütunda şablonu.
5. Projenizin adı ***BasicWebApp*** tıklatıp **Tamam** düğmesi.   
![Yeni Proje iletişim kutusu](creating-a-basic-web-forms-page/_static/image2.png)
6. Ardından, **Web Forms** şablonu ve tıklatın **Tamam** projesi oluşturmak için düğmesi.  
![Yeni ASP.NET projesi iletişim kutusu](creating-a-basic-web-forms-page/_static/image3.png)  

    Visual Studio Web Forms şablona dayalı önceden oluşturulmuş işlevselliği içeren yeni bir proje oluşturur. Yalnızca size sağladığı bir *Home.aspx* sayfasında, bir *About.aspx* sayfasında, bir *Contact.aspx* sayfasında, ancak Ayrıca kullanıcıların kaydeder ve kaydeden üyelik işlevselliğini içerir kimlik bilgilerini böylece kullanıcılar Web sitenize oturum açabilirsiniz. Yeni bir sayfa oluşturulduğunda, varsayılan olarak Visual Studio sayfasında görüntüler. **kaynak** burada görebilirsiniz sayfanın HTML öğeleri görünüm. İçinde görür aşağıda gösterilmiştir **kaynak** adlı yeni bir Web sayfası oluşturduysanız görüntülemek *BasicWebApp.aspx*.  
    ![Kaynak Görünümü](creating-a-basic-web-forms-page/_static/image4.png)


### <a name="a-tour-of-the-visual-studio-web-development-environment"></a>Visual Studio Web geliştirme ortamı turu


Sayfa değiştirerek devam etmeden önce Visual Studio geliştirme ortamı tanımak kullanışlıdır. Aşağıdaki çizimde windows ve Visual Studio ve Web için Visual Studio Express kullanılabilen araçlar gösterir.

> [!NOTE] 
> 
> Bu diyagramda, varsayılan windows ve pencere konumlarını gösterilmektedir. **Görünüm** menüsünde ek windows görüntülemek ve bunları yeniden düzenleme ve windows tercihlerinize göre yeniden boyutlandır olanak tanır. Zaten penceresi düzenleme yapılan değişiklikler, gördüğünüz çizim eşleşmez.


 Visual Studio ortamı

![Visual Studio ortamı](creating-a-basic-web-forms-page/_static/image5.png)

### <a name="familiarize-yourself-with-the-web-designer"></a>Web Tasarımcısı ile edinin

Yukarıdaki çizimde inceleyin ve aşağıdaki listede, metni eşleşen en yaygın olarak açıklayan windows ve Araçlar kullanılır. (Tüm windows ve burada listelendiğini görmek araçları, yalnızca önceki çizimde işaretlenmiştir.)

- Araç çubukları. Metni, metin bulma ve benzeri biçimlendirme için komutları sağlar. Yalnızca çalıştığınız bazı araç çubukları kullanılabilir **tasarım** görünümü.
- **Çözüm Gezgini** penceresi. Dosya ve klasörleri Web uygulamanızda görüntüler.
- Belge penceresini açın. Sekmeli windows üzerinde çalıştığınız belgeleri görüntüler. Sekmeleri tıklatarak belgeler arasında geçiş yapabilirsiniz.
- **Özellikler** penceresi. Sayfa, HTML öğeleri, denetimleri ve diğer nesneleri ayarlarını değiştirmenizi sağlar.
- Sekmeleri görüntüleyin. Aynı belgenin farklı görünümler sunar. **Tasarım** görünümdür yakın WYSIWYG düzenleme yüzeyini. **Kaynak** görünümdür sayfasının HTML düzenleyicisi. **Bölünmüş** görünüm görüntüler her ikisi de **tasarım** Görünüm ve **kaynak** belge için Görünüm. İle çalışırsınız **tasarım** ve **kaynak** görünümler bu kılavuzda daha sonra. Web sayfalarını açmak tercih ederseniz **tasarım** görüntülemek, üzerinde **Araçları** menüsünde tıklatın **seçenekleri**seçin **HTML Tasarımcısı** düğümü ve değişiklik **Sayfaları içinde Başlat** seçeneği.
- **Araç kutusu**. Denetimleri ve sayfanıza sürükleyebilirsiniz HTML öğeleri sağlar. **Araç kutusu** öğeleri ortak işlevi tarafından gruplandırılır.
- S **erver Explorer**. Veritabanı bağlantıları görüntüler. Sunucu Gezgini görünür durumda değilse, Sunucu Gezgini için Görünüm menüsünde'ı tıklatın.


### <a name="creating-a-new-aspnet-web-forms-page"></a>Yeni bir ASP.NET Web Forms sayfası oluşturma


Kullanarak yeni bir Web Forms uygulaması oluşturduğunuzda **ASP.NET Web uygulaması** proje şablonu, Visual Studio ekler adlı bir ASP.NET sayfası (Web Forms sayfası) *Default.aspx*, birkaç diğer dosyaların yanı sıra ve klasörler. Kullanabileceğiniz *Default.aspx* sayfası, Web uygulamanız için giriş sayfası olarak. Ancak, bu kılavuzda oluşturacak ve yeni bir sayfa ile çalışır.

### <a name="to-add-a-page-to-the-web-application"></a>Sayfa Web uygulamasına eklemek için


1. Kapat *Default.aspx* sayfası. Bunu yapmak için dosya adını görüntüler sekmesine tıklayın ve sonra Kapat seçeneğini tıklatın.
2. İçinde **Çözüm Gezgini**, Web uygulamasının adını sağ tıklayın (uygulama adı bu öğreticideki **BasicWebSite**) ve ardından **Ekle**  - &gt; **Yeni öğe**.   
**Yeni Öğe Ekle** iletişim kutusu görüntülenir.
3. Seçin **Visual C#**  - &gt; **Web** soldaki templates grubu. Ardından, seçin **Web formu** ortasından listelemek ve adlandırın *FirstWebPage.aspx*.   
    ![Yeni Öğe Ekle iletişim kutusu](creating-a-basic-web-forms-page/_static/image6.png)
4. Tıklatın **Ekle** web sayfası projenize eklemek için.  
Visual Studio yeni sayfa oluşturur ve bunu açar.


### <a name="adding-html-to-the-page"></a>HTML sayfasına ekleme


Kılavuzun bu bölümünde sayfasına bazı statik metin ekleyeceksiniz.

### <a name="to-add-text-to-the-page"></a>Metin eklemek için


1. Belge penceresinin alt kısmındaki tıklatın **tasarım** geçmek için sekme **tasarım** görünümü.

    Tasarım görünümü geçerli sayfa WYSIWYG benzeri şekilde görüntüler. Bu noktada, sayfa dikdörtgen özetlenmektedir kesikli bir çizgi dışında boş olacak şekilde herhangi bir metin veya sayfadaki denetimleri yok. Bu dikdörtgen temsil eden bir **div** sayfadaki öğenin.
2. Kesikli çizgi tarafından özetlenen dikdörtgen içini tıklatın.
3. Tür **'na Hoş Geldiniz Visual Web Developer** ve basın **ENTER** iki kez.

    Aşağıdaki çizimde, yazdığınız metni gösterir **tasarım** görünümü.

    ![Hoş Geldiniz metin Tasarım görünümünde](creating-a-basic-web-forms-page/_static/image7.png "Tasarım görünümünde metin'na Hoş Geldiniz")
4. Geçiş **kaynak** görünümü.

    HTML dosyasındaki görebilirsiniz **kaynak** yazdığınız sırada oluşturduğunuz Görünüm **tasarım** görünümü.  
    ![Statik metin Web sayfası](creating-a-basic-web-forms-page/_static/image8.png)


### <a name="running-the-page"></a>Sayfa çalıştırma


Sayfaya denetimler ekleyerek devam etmeden önce ilk kez çalıştırabilirsiniz.

### <a name="to-run-the-page"></a>Sayfayı çalıştırmak için


1. İçinde **Çözüm Gezgini**, sağ *FirstWebPage.aspx* seçip **Başlangıç Sayfası Ayarla**.
2. Tuşuna **CTRL + F5** sayfayı çalıştırmak için.

    Tarayıcıda görüntülenen sayfadır. Oluşturduğunuz sayfa bir dosya adı uzantısına sahip olsa da *.aspx*, şu anda herhangi bir HTML sayfasında gibi çalışır.

    Bir sayfasını tarayıcıda görüntülemek için de sayfanın sağ tıklayarak **Çözüm Gezgini** seçip **tarayıcıda görüntüle**.
3. Web uygulaması durdurmak için tarayıcıyı kapatın.


## <a name="adding-and-programming-controls"></a>Ekleme ve denetimlerini programlama


<a id="sectionToggle1"></a>

Bu gibi durumlarda, sunucu denetimleri şimdi sayfasına ekleyeceksiniz. Düğmeleri, etiketler, metin kutuları ve diğer tanıdık denetimleri gibi sunucu denetimleri, Web formları sayfaları için tipik form işleme özellikleri sağlar. Ancak, istemci yerine sunucu üzerinde çalışan bir kod denetimleriyle programlama yapabilirsiniz.

Ekleyeceksiniz bir [düğmesini](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) denetimi, bir [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) denetimi ve bir [etiket](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) sayfasına denetlemek ve işlemek için kod yazma [tıklatın](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) olayı için [düğmesini](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) denetim.

### <a name="to-add-controls-to-the-page"></a>Denetimleri eklemek için


1. Tıklatın **tasarım** geçmek için sekme **tasarım** görünümü.
2. Ekleme noktasını sonunda put **Visual Web Developer'na Hoş** metin ve tuşuna **ENTER** bazı yer açmak için beş veya daha fazla kez **div** öğe kutusunun.
3. İçinde **araç**, genişletin **standart** önceden genişletilmemişse grup.  
Genişletmeniz gerekebilir Not **araç** penceresini görüntülemek için soldaki.
4. Sürükleme bir [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) sayfaya denetlemek ve ortasında bırakın **div** sahip öğe kutusunun **'na Hoş Geldiniz Visual Web Developer** ilk satırda.
5. Sürükleme bir [düğmesini](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) sayfaya denetlemek ve sağında bırakma [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) denetim.
6. Sürükleme bir [etiket](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) sayfaya denetlemek ve ayrı bir satırda aşağıdaki bırakma [düğmesini](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) denetim.
7. Yukarıdaki ekleme noktasını put [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) denetlemek ve ardından **adınızı girin:** .

    Bu statik HTML metin başlığıdır [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) denetim. Statik HTML ve sunucu denetimleri aynı sayfada karıştırabilirsiniz. Üç denetimleri nasıl görünür aşağıda gösterilmiştir **tasarım** görünümü.

    ![Tasarım görünümünde üç denetimleri](creating-a-basic-web-forms-page/_static/image9.png "Tasarım görünümünde üç denetimleri")


### <a name="setting-control-properties"></a>Denetim özelliklerini ayarlama


Visual Studio sayfadaki denetimlerin özelliklerini ayarlamak için çeşitli yollar sunar. Kılavuzun bu bölümünde, Özellikler hem de ayarlar **tasarım** Görünüm ve **kaynak** görünümü.

### <a name="to-set-control-properties"></a>Denetim özelliklerini ayarlamak için


1. İlk olarak, görüntüleme **özellikleri** gelen seçerek windows **Görünüm** menüsü -&gt; **diğer pencereler**  - &gt; **Properies penceresi**. Alternatif olarak seçebilirsiniz **F4** görüntülemek için **özellikleri** penceresi.
2. Seçin [düğmesini](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) denetim ve ardından **özellikleri** penceresindeki değerini ayarlayın **metin** için **görünen adı**. Girilen metin düğmesi Tasarımcısı'nda aşağıdaki çizimde gösterildiği gibi görünür.

    ![Düğme metni ayarlamak](creating-a-basic-web-forms-page/_static/image10.png "ayarlamak düğme metni")
3. Geçiş **kaynak** görünümü.

    **Kaynak** Visual Studio için sunucu denetimleri oluşturduğu öğeler de dahil olmak üzere sayfa için HTML görünümü görüntüler. Denetimleri bildirildiğinde HTML benzeri sözdizimi kullanarak etiket öneki kullanmasını dışında **asp:** ve öznitelik dahil **runat =&quot;server&quot;**.

    Denetim Özellikleri öznitelik olarak bildirilir. Ayarlandığında Örneğin, [metin](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.text.aspx) özelliği için [düğmesini](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) denetlemek, 1. adımda, gerçekte ayarı **metin** denetimin biçimlendirme özniteliği.

    > [!NOTE] 
    > 
    > Tüm denetimler içinde olduğunda bir **form** de özniteliğine sahip öğe **runat =&quot;server&quot;**. **Runat =&quot;server&quot;**  özniteliği ve **asp:** sayfa çalıştığında ASP.NET tarafından sunucuda işlenene denetimleri denetimi etiketleri işaretlemek için önek. Dışında kod  **&lt;runat form =&quot;server&quot; &gt;**  ve  **&lt;runat komut =&quot;server&quot; &gt;**  öğeleri yüzden ASP.NET kodu, açılış etiketinde bir öğesiyle iç olmalıdır tarayıcısına değişmeden gönderilir **runat =&quot;server&quot;**  özniteliği.
4. Sonra ek bir özellik için ekleyeceksiniz [etiket](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) denetim. Ekleme noktasını sonra doğrudan put **asp: Label** içinde  **&lt;asp: Label&gt;**  etiketi ve tuşuna basarak **boşluk**.

    Aşağı açılan liste için ayarlayabileceğiniz kullanılabilir özelliklerin listesi görüntüleyen görünür bir [etiket](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) denetim. Bu özellik denir **IntelliSense**, size yardımcı **kaynak** sayfasında sunucu denetimleri, HTML öğeleri ve diğer öğeleri sözdizimiyle görünümü. Aşağıdaki çizimde gösterildiği **IntelliSense** için aşağı açılan listesi [etiket](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) denetim.

    ![IntelliSense öznitelikleri](creating-a-basic-web-forms-page/_static/image11.png "IntelliSense öznitelikleri")
5. Seçin **ForeColor** ve ardından bir eşittir işareti yazın.

    IntelliSense renkleri listesini görüntüler.

    > [!NOTE] 
    > 
    > Görüntüleyebileceğiniz bir **IntelliSense** aşağı açılan liste basarak istediğiniz zaman **CTRL + J** kod görüntülerken.
6. İçin renk seçme  **[etiket](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)**  denetimin metin. Beyaz bir arka plan karşı okumak için koyu bir renk seçtiğinizden emin olun.

    **ForeColor** özniteliği, kapanış tırnak işareti de dahil olmak üzere, seçtiğiniz renkle tamamlandı.


### <a name="programming-the-button-control"></a>Düğme denetimi programlama


Bu kılavuz için kullanıcı metin kutusuna girer ve adını görüntüler adını okur kod yazacaksınız [etiket](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) denetim.

### <a name="add-a-default-button-event-handler"></a>Varsayılan düğmesi olay işleyicisi ekleme


1. Geçiş **tasarım** görünümü.
2. Çift [düğmesini](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) denetim.

    Varsayılan olarak, Visual Studio bir arka plan kod dosyasına geçer ve bir iskelet olay işleyicisi oluşturur [düğmesini](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) denetimin varsayılan olayı [tıklatın](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) olay. Arka plan kod dosyası, kullanıcı Arabirimi biçimlendirmeyi (HTML) (örneğin, C# ' ta) sunucu kodunuzdan ayırır.   
İmleç eklenen için bu olay işleyicisi için kod konumlandırıldı.

    > [!NOTE] 
    > 
    > Denetim içinde çift **tasarım** görünümdür olay işleyicileri oluşturabileceğiniz çeşitli yollardan biri.
3. İçinde **Button1\_tıklatın** olay işleyicisi, türü **Label1** ardından bir noktayla (**.**).

    Süre yazdığınızda **kimliği** etiketinin (**Label1**), Visual Studio için kullanılabilir üyeler listesini görüntüler [etiket](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) , aşağıda gösterildiği gibi denetleme Çizim. Üye yaygın olarak bir özellik, yöntemi veya olay.

    ![IntelliSense kod görünümünde](creating-a-basic-web-forms-page/_static/image12.png "kod görünümünde IntelliSense")
4. Son **tıklatın** aşağıdaki kod örneğinde gösterildiği şekilde okunacak şekilde düğmesi olay işleyicisi.

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample1.cs?highlight=3)]

    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample2.vb?highlight=2)]
5. Geçiş geri görüntülemeye **kaynak** sağ tıklanarak, HTML biçimlendirmesi görünümünü *FirstWebPage.aspx* içinde **Çözüm Gezgini** ve seçerek **görünümü Biçimlendirme**.
6. Kaydırma  **&lt;asp: düğme&gt;**  öğesi. Unutmayın  **&lt;asp: düğme&gt;**  öğesi öznitelik şimdi sahip **onclick =&quot;Button1\_tıklatın&quot;**.

    Bu öznitelik düğmenin bağlar [tıklatın](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) önceki adımda kodlanmış işleyici yöntemine olay.

    Olay işleyici yöntemleri herhangi bir ad olabilir; gördüğünüz adı, Visual Studio tarafından oluşturulan varsayılan addır. En önemli nokta için kullanılan adı olan **OnClick** öznitelik HTML kod arkasında tanımlanmış bir yöntem adı eşleşmelidir.


### <a name="running-the-page"></a>Sayfa çalıştırma


Sunucu denetimleri sayfasında artık test edebilirsiniz.

### <a name="to-run-the-page"></a>Sayfayı çalıştırmak için


1. Tuşuna **CTRL + F5** sayfasını tarayıcıda çalıştırmak için. Bir hata oluşursa, yukarıdaki adımları yeniden denetleyin.
2. Metin kutusuna bir ad girin ve tıklatın **görünen adı** düğmesi.

    Girdiğiniz ad [etiket](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) denetim. Düğmeye tıkladığınızda, sayfa Web sunucusuna gönderilen unutmayın. Ardından ASP.NET sayfası yeniden oluşturur, kodunuzun çalıştığı (Bu durumda, [düğmesini](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) denetimin [tıklatın](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) olay işleyicisi çalışır) ve ardından yeni sayfa tarayıcıya gönderir. Durum çubuğu tarayıcıda izlemek, sayfasının düğmesini her zaman Web sunucusuna gidiş dönüş yapıyor görebilirsiniz.
3. Sayfada sağ tıklayıp seçerek çalışırken sayfa kaynağını tarayıcıda görüntülemek **kaynağı görüntüle**.

    Sayfa kaynak kodu, hiçbir sunucu kodu olmadan HTML görürsünüz. Özellikle, görmezseniz  **&lt;asp:&gt;**  ile çalışmakta olan öğeleri **kaynak** görünümü. Sayfa çalıştığında, ASP.NET sunucu denetimleri işler ve denetimi temsil eden işlevleri gerçekleştirmek HTML öğeleri sayfaya işler. Örneğin,  **&lt;asp: düğme&gt;**  denetim HTML olarak işlenen  **&lt;giriş türü =&quot;gönderme&quot; &gt;**  öğesi.
4. Tarayıcıyı kapatın.


## <a name="working-with-additional-controls"></a>Ek denetimleri ile çalışma

<a id="sectionToggle2"></a>

Kılavuzun bu bölümünde, sizinle birlikte çalışır [Takvim](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) aynı anda ayda bir tarihlerini görüntüler denetim. [Takvim](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) denetimi düğmesi, metin kutusu ve etiket, çalışmaması ile daha karmaşık bir denetim ve sunucu denetimlerinin başka bazı özellikleri gösterilmektedir.

Bu bölümde, ekleyeceksiniz bir [System.Web.UI.WebControls.Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) sayfasına denetlemek ve biçimlendirin.

### <a name="to-add-a-calendar-control"></a>Takvim denetim eklemek için


1. Visual Studio'da geçmek **tasarım** görünümü.
2. Gelen **standart** bölümünü **araç**, sürükleyin bir [Takvim](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) sayfaya denetlemek ve aşağıdaki bırakma **div** öğesi, diğer denetimleri içerir.

    Takvim akıllı etiket panelinde görüntülenir. Bölmenin Seçili denetim için en yaygın görevleri gerçekleştirmenizi kolaylaştırır komutları görüntüler. Aşağıdaki çizimde gösterildiği [Takvim](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) denetim olarak işlenen **tasarım** görünümü.

    ![Takvim denetimi Tasarım görünümünde](creating-a-basic-web-forms-page/_static/image13.png "Takvim denetimi Tasarım görünümünde")
3. Akıllı etiket panelinde seçin **otomatik biçimlendirme**.

    **Otomatik biçim** iletişim kutusu görüntülenir, hangi takvim için bir biçimlendirme düzeni seçmenizi sağlar. Aşağıdaki çizimde gösterildiği **otomatik biçim** iletişim kutusu için [Takvim](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) denetim.

    ![Otomatik Biçim iletişim kutusu (Takvim denetimi)](creating-a-basic-web-forms-page/_static/image14.png "otomatik biçim iletişim kutusu (Takvim denetimi)")
4. Gelen **bir düzen seçin** listesinde, seçin **basit** ve ardından **Tamam**.
5. Geçiş **kaynak** görünümü.

    Gördüğünüz  **&lt;asp: Takvim&gt;**  öğesi. Bu öğe daha önce oluşturduğunuz basit denetimler için öğeleri daha çok uzun. Ayrıca, alt öğeleri gibi içerir  **&lt;WeekEndDayStyle&gt;**, çeşitli biçimlendirme ayarları temsil eder. Aşağıdaki çizimde gösterildiği [Takvim](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) denetim **kaynak** görünümü. (Gördüğünüz tam biçimlendirme **kaynak** görünüm farklı biraz gösterimden.)

    ![Takvim denetimi kaynak görünümünde](creating-a-basic-web-forms-page/_static/image15.png "Takvim denetimi kaynak görünümünde")


### <a name="programming-the-calendar-control"></a>Takvim denetimi programlama


Bu bölümde, programlayacaksınız [Takvim](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) şu anda seçili tarihini görüntülemek için denetim.

### <a name="to-program-the-calendar-control"></a>Program Takvim denetimi


1. İçinde **tasarım** görüntülemek için çift [Takvim](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) denetim.

    Yeni bir olay işleyicisi oluşturulur ve adlı arka plan kod dosyasına görüntülenen *FirstWebPage.aspx.cs*.
2. Son [SelectionChanged](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selectionchanged.aspx) aşağıdaki kod ile olay işleyicisi.


    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample3.cs?highlight=3)]


    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample4.vb?highlight=2)]

 Yukarıdaki kod etiket denetimi metin Takvim denetimi seçili tarihini ayarlar.


### <a name="running-the-page"></a>Sayfa çalıştırma


Takvim artık test edebilirsiniz.

### <a name="to-run-the-page"></a>Sayfayı çalıştırmak için


1. Tuşuna **CTRL + F5** sayfasını tarayıcıda çalıştırmak için.
2. Takvimdeki bir tarihi tıklayın.

    Tıklattığınız tarih görüntülenir [etiket](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) denetim.
3. Tarayıcıda, sayfa için kaynak kodunu görüntüleyin.

    Unutmayın [Takvim](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) denetim işlenen sayfası olarak bir **tablo**, her gün ile bir **td** öğesi.
4. Tarayıcıyı kapatın.


## <a name="next-steps"></a>Sonraki Adımlar


<a id="nextStepsToggle"></a>

Bu izlenecek Visual Studio sayfa Tasarımcısı'nın temel özellikleri gösterilen. Oluşturma ve Visual Studio Web Forms sayfası Düzenle anladığınıza göre diğer özellikleri incelemek isteyebilirsiniz. Örneğin, aşağıdakileri yapmak isteyebilirsiniz:

- ASP.NET Web Forms hakkında daha fazla adım adım öğretici serisi izleyerek bilgi [ASP.NET 4.5 Web formları ve Visual Studio 2013 ile çalışmaya başlama](getting-started-with-aspnet-45-web-forms/introduction-and-overview.md).
- Basamaklama stil sayfaları (CSS) hakkında daha fazla bilgi edinin. Ayrıntılar için bkz [çalışmaya CSS genel bakış](https://msdn.microsoft.com/library/bb398931.aspx).
