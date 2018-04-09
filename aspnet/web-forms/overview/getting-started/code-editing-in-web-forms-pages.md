---
uid: web-forms/overview/getting-started/code-editing-in-web-forms-pages
title: ASP.NET Web Forms Visual Studio 2013'te kod düzenleme | Microsoft Docs
author: Erikre
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/03/2014
ms.topic: article
ms.assetid: 5344b74e-b888-479a-92bc-601a33bd61a2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/code-editing-in-web-forms-pages
msc.type: authoredcontent
ms.openlocfilehash: 79b10df04432490d6338dadb8f7ddd36192beb3e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="code-editing-aspnet-web-forms-in-visual-studio-2013"></a>Visual Studio 2013'te kod düzenleme ASP.NET Web formları
====================
Tarafından [Erik Reitan](https://github.com/Erikre)

Birçok ASP.NET Web formu sayfalarında, Visual Basic, C# veya başka bir dil kodu yazın. Visual Studio'da Kod Düzenleyicisi hataları önlemenize yardımcı olurken kod hızlı yazmanıza yardımcı olabilir. Ayrıca, düzenleyici yapmanız gereken çalışma miktarını azaltmak için yeniden kullanılabilir kod oluşturma yol sağlar.

Bu kılavuzda Visual Studio kod düzenleyicisinin çeşitli özellikleri gösterir.

Bu gözden geçirme sırasında öğreneceksiniz nasıl yapılır:

- Satır içi kodlama hataları düzeltin.
- Yeniden düzenlemeniz ve kod yeniden adlandırın.
- Değişkenleri ve nesneleri yeniden adlandırın.
- Kod parçacıkları ekleyin.

## <a name="prerequisites"></a>Önkoşullar


Bu kılavuzu tamamlamak için gerekir:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) veya [Web için Microsoft Visual Studio Express 2013](https://www.microsoft.com/visualstudio/11/downloads#express-web). .NET Framework otomatik olarak yüklenir. 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 ve Web için Visual Studio Express 2013 Microsoft genellikle için Visual Studio Bu öğretici seri anılacaktır.  
    >   
    > Visual Studio kullanıyorsanız, bu kılavuzda, seçtiğiniz varsayar **Web geliştirme** ayarlar koleksiyonu, Visual Studio ilk başlattığınızda. Daha fazla bilgi için bkz: [nasıl yapılır: Web geliştirme ortamı ayarlarını seçin](https://msdn.microsoft.com/library/ff521558.aspx).

  Visual Studio ve ASP.NET bir giriş için bkz: [Visual Studio 2013'te temel ASP.NET 4.5 Web Forms sayfası oluşturma](creating-a-basic-web-forms-page.md).   
 

## <a name="creating-a-web-application-project-and-a-page"></a>Bir Web uygulaması projesi ve bir sayfa oluşturma

<a id="sectionToggle0"></a>

Kılavuzun bu bölümünde, bir Web uygulaması projesi oluşturun ve yeni bir sayfa ekleyin.

### <a name="to-create-a-web-application-project"></a>Bir Web uygulaması projesi oluşturmak için

1. Microsoft Visual Studio'yu açın.
2. Üzerinde **dosya** menüsünde, select **yeni proje**.  
    ![Dosya menüsü](code-editing-in-web-forms-pages/_static/image1.png)

    **Yeni proje** iletişim kutusu görüntülenir.
3. Seçin **şablonları**  - &gt; **Visual C#**  - &gt; **Web** soldaki templates grubu.
4. Seçin **ASP.NET Web uygulaması** Orta sütunda şablonu.
5. Projenizin adı ***BasicWebApp*** tıklatıp **Tamam** düğmesi.   
![Yeni Proje iletişim kutusu](code-editing-in-web-forms-pages/_static/image2.png)
6. Ardından, **Web Forms** şablonu ve tıklatın **Tamam** projesi oluşturmak için düğmesi.  
![Yeni ASP.NET projesi iletişim kutusu](code-editing-in-web-forms-pages/_static/image3.png)  

    Visual Studio Web Forms şablona dayalı önceden oluşturulmuş işlevselliği içeren yeni bir proje oluşturur.


## <a name="creating-a-new-aspnet-web-forms-page"></a>Yeni bir ASP.NET Web Forms sayfası oluşturma


Kullanarak yeni bir Web Forms uygulaması oluşturduğunuzda **ASP.NET Web uygulaması** proje şablonu, Visual Studio ekler adlı bir ASP.NET sayfası (Web Forms sayfası) *Default.aspx*, birkaç diğer dosyaların yanı sıra ve klasörler. Kullanabileceğiniz *Default.aspx* sayfası, Web uygulamanız için giriş sayfası olarak. Ancak, bu kılavuzda oluşturacak ve yeni bir sayfa ile çalışır.

### <a name="to-add-a-page-to-the-web-application"></a>Sayfa Web uygulamasına eklemek için


1. İçinde **Çözüm Gezgini**, Web uygulamasının adını sağ tıklayın (uygulama adı bu öğreticideki **BasicWebSite**) ve ardından **Ekle**  - &gt; **Yeni öğe**.   
**Yeni Öğe Ekle** iletişim kutusu görüntülenir.
2. Seçin **Visual C#**  - &gt; **Web** soldaki templates grubu. Ardından, seçin **Web formu** ortasından listelemek ve adlandırın *FirstWebPage.aspx*.   
    ![Yeni Öğe Ekle iletişim kutusu](code-editing-in-web-forms-pages/_static/image4.png)
3. Tıklatın **Ekle** Web Forms sayfası projenize eklemek için.  
 Visual Studio yeni sayfa oluşturur ve bunu açar.
4. Ardından, bu yeni sayfa varsayılan başlangıç sayfası olarak ayarlayın. İçinde **Çözüm Gezgini**, adlı yeni sayfa sağ *FirstWebPage.aspx* seçip **başlangıç sayfası olarak ayarla**. Bizim ilerleme test etmek için bu uygulamayı bir sonraki çalıştırmanızda bu yeni sayfa tarayıcıdaki otomatik olarak görürsünüz.


## <a name="correcting-inline-coding-errors"></a>Satır içi kodlama hataları düzeltme


Visual Studio Kod düzenleyicisinde, kod yazmak ve bir hata yaptıysanız, Kod düzenleyicisinde hatayı düzeltmek için yardımcı hatalarını önlemek için yardımcı olur. Kılavuzun bu bölümünde, bir satırı Düzenleyicisi'nde hata düzeltme özellikleri gösteren kod yazacaksınız.

### <a name="to-correct-simple-coding-errors-in-visual-studio"></a>Basit kodlama hataları düzeltmek için Visual Studio'da


1. İçinde **tasarım** görüntülemek için bir işleyici oluşturmak için boş sayfa çift **yük** sayfası için olay.   
   Bazı kodlar yazmak için yalnızca bir yer olarak olay işleyicisi kullanıyorsunuz.
2. İşleyicisinin içinden bir hata ve tuşuna içeren aşağıdaki satırdan **ENTER**:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample1.cs)]

   Bastığınızda **ENTER**, Kod düzenleyicisinde yeşil ve kırmızı alt çizgileri yerleştirir (genellikle çağrısı &quot;dalgalı&quot; satırları) sorunları olan kodu alanlarını altında. Yeşil alt çizgi, bir uyarı gösterir. Kırmızı alt çizgi düzeltmeniz gerekir bir hata gösterir. 

    Fare işaretçisini tutun `myStr` hakkında uyarı bildiren bir araç ipucu görmek için. Ayrıca, fare işaretçisini hata iletisini görmek için kırmızı altı çizili basılı tutun.

    Aşağıdaki resimde, alt çizgileri koduyla gösterir.

    ![Hoş Geldiniz metin Tasarım görünümünde](code-editing-in-web-forms-pages/_static/image5.png "Tasarım görünümünde metin'na Hoş Geldiniz")  
   Noktalı virgül ekleyerek hatanın düzeltilmesi gerektiğini `;` satırın sonuna. Uyarı yalnızca size, kullanmadığınız bildirir `myStr` henüz değişkeni.  

    > [!NOTE] 
    > 
    > Visual Studio'da ayarlar seçerek biçimlendirme geçerli kodunuzu görüntülemek **Araçları**  - &gt; **seçenekleri**  - &gt; **yazı tipleri ve Renkleri**.


## <a name="refactoring-and-renaming"></a>Yeniden düzenleme ve yeniden adlandırma

Yeniden düzenleme anlamak ve işlevselliğini korurken korumak için daha kolay hale getirmek için kodunuzu yeniden yapılandırma içeren bir yazılım Metodoloji olur. Basit bir örnek kod bir veritabanından veri almak için bir olay işleyicisi yazma olabilir. Sayfanız geliştirdikçe birkaç farklı işleyicilerini veri erişim için gereken bulur. Bu nedenle, bir veri erişim yöntemi sayfasında oluşturarak ve yöntemine yönelik çağrılar işleyicileri ekleme sayfanın kodu yeniden.

Kod Düzenleyicisi'ni yeniden düzenleme çeşitli görevleri gerçekleştirmenize yardımcı olacak araçlar içerir. Bu kılavuzda, iki düzenleme teknikleri ile çalışır: değişkenleri yeniden adlandırma ve yöntemleri ayıklanıyor. Yeniden düzenleme diğer seçenekler alanları Kapsüllenen, yerel değişkenleri yöntemi parametrelerine yükseltme ve yöntem parametreleri yönetme içerir. Yeniden düzenleme bu seçeneklerin kullanılabilirliği kodu konumda bağlıdır.

### <a name="refactoring-code"></a>Kodu yeniden düzenleme

Bir ortak yeniden düzenleme (extract) oluşturmak için bir senaryodur içinde bir yöntem gibi başka bir üye kodu yönteminden. Bu, özgün üye boyutunu azaltır ve ayıklanan kodu yeniden kullanılabilir hale getirir.

Kılavuzun bu bölümünde, bazı basit kod yazın ve ardından bir yöntem ondan ayıklayın. C# programlama dili kullanan bir sayfa oluşturacak şekilde yeniden düzenleme C# ' ta desteklenir.

### <a name="to-extract-a-method-in-a-c-page"></a>Bir C# sayfasında bir yöntem ayıklamak için

1. Geçiş **tasarım** görünümü.
2. İçinde **araç**, gelen **standart** sekmesinde, sürükleyin bir [düğmesini](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) sayfaya denetim.
3. Çift tıklatın **düğmesini** denetim için bir işleyici oluşturmak için kendi [tıklatın](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) olay ve ardından aşağıdaki vurgulanmış kodu ekleyin:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample2.cs?highlight=3-16)]

   Kod oluşturur bir **ArrayList** nesnesi değerlerle yüklemek için bir döngü kullanır ve içeriğini görüntülemek için başka bir döngü kullanır **ArrayList** nesnesi.
4. Tuşuna **CTRL + F5** sayfayı çalıştırın ve ardından **düğmesini** aşağıdaki çıkış gördüğünüzden emin olmak için:   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample3.html)]
5. Kod Düzenleyicisi'ne dönmek ve olay işleyicisini aşağıdaki satırları seçin.   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample4.html)]
6. Seçime sağ tıklayın, **yeniden düzenlemeniz**ve ardından **ayıklama yöntemi**. 

    **Ayıklama yöntemi** iletişim kutusu görüntülenir.
7. İçinde **yeni yöntem adı** kutusuna **DisplayArray**ve ardından **Tamam**. 

    Kod Düzenleyicisi adlı yeni bir yöntem oluşturur `DisplayArray`ve yeni yöntemine bir çağrı yerleştirir **tıklatın** döngü bulunduğu başlangıçta işleyici.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample5.cs?highlight=12)]
8. Tuşuna **CTRL + F5** sayfa yeniden çalıştırmanız ve tıklatın **düğmesini**.

    Önce yaptığınız gibi sayfa aynı çalışır. `DisplayArray` Yöntemi çağrısı yerden şimdi olabilir sayfa sınıfındaki.

## <a name="renaming-variables"></a>Değişkenleri yeniden adlandırma

Değişkenleri yanı sıra ile nesneleri çalışırken zaten kodunuzda başvurulan sonra bunları yeniden adlandırmak isteyebilirsiniz. Ancak, değişkenler ve nesneleri yeniden adlandırma başvurulardan birini yeniden adlandırma kaçırılması durumunda ayırmak kodu neden olabilir. Bu nedenle, yeniden adlandırma gerçekleştirmek için yeniden düzenleme kullanabilirsiniz.

### <a name="to-use-refactoring-to-rename-a-variable"></a>Bir değişken yeniden adlandırmak için yeniden düzenleme kullanmak için


1. İçinde **tıklatın** olay işleyicisi, aşağıdaki satırı bulun:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample6.cs)]
2. Değişken adını sağ `alist`, seçin **yeniden düzenlemeniz**ve ardından **yeniden adlandırma**.

    **Yeniden adlandırma** iletişim kutusu görüntülenir.
3. İçinde **yeni bir ad** kutusuna **ArrayList1** ve emin olun **başvuru değişikliklerini Önizleme** onay kutusunun seçili. Sonra **Tamam**'a tıklayın.

    **Değişiklikleri Önizle** iletişim kutusu belirir ve adlandırdığınız değişkeni tüm başvuruları içeren bir ağaç görüntüler.
4. Tıklatın **Uygula** kapatmak için **Değişiklikleri Önizle** iletişim kutusu.

    Özellikle, seçtiğiniz örneğine başvuru değişkenleri yeniden adlandırılmıştır. Ancak, unutmayın, değişkeni `alist` aşağıdaki satırda yeniden adlandırılmaz.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample7.cs)]

    Değişkeni `alist` değişkeni aynı değeri göstermiyor çünkü bu satırda yeniden adlandırılmış değil `alist` adlandırdığınız. Değişkeni `alist` içinde `DisplayArray` bu yöntem için yerel bir değişken bildirimidir. Bu değişkenleri yeniden adlandırmak için yeniden düzenleme kullanarak Bul ve Değiştir eylemi Düzenleyicisi'nde yalnızca gerçekleştirmeden daha farklı olduğunu gösterir; ile çalışma değişkeni semantiği bilgisini yeniden adlandırır değişkenlerle yeniden düzenleme.


## <a name="inserting-snippets"></a>Kod parçacıkları ekleme

Web Forms geliştiriciler sık gerçekleştirmeniz gereken birçok kodlama görevleri olduğundan, Kod Düzenleyicisi parçacıkları ya da önceden kod bloklarını kitaplığını sağlar. Bu parçacıkları sayfanıza ekleyebilirsiniz.

Visual Studio'da kullandığınız her bir dilin kod parçacıkları Ekle şekilde küçük farklar vardır. Kod parçacıkları ekleme hakkında daha fazla bilgi için bkz: [Visual Basic IntelliSense kodu parçacıkları](https://msdn.microsoft.com/library/18yz4be4.aspx). Kod parçacıkları Visual C# dilinde ekleme hakkında daha fazla bilgi için bkz: [Visual C# kod parçacıkları](https://msdn.microsoft.com/library/z41h7fat.aspx).

## <a name="next-steps"></a>Sonraki Adımlar

Bu izlenecek kodunuzdaki hataları düzeltme, kodu yeniden düzenleme, değişkenleri yeniden adlandırma ve kod parçacıkları kodunuza ekleme için Visual Studio 2010 kod düzenleyicisini, temel özellikleri gösterilen. Ek özellikler Düzenleyicisi'nde uygulama geliştirme hızlı ve kolay hale getirebilirsiniz. Örneğin, aşağıdakileri yapabilirsiniz:

- IntelliSense seçeneklerini değiştirerek, kod parçacıkları yönetme ve çevrimiçi kod parçacıkları için arama gibi IntelliSense özellikleri hakkında daha fazla bilgi. Daha fazla bilgi için bkz: [kullanarak IntelliSense](https://msdn.microsoft.com/library/hcw1s69b.aspx).
- Kendi kod parçacıkları oluşturmayı öğrenin. Daha fazla bilgi için bkz: [oluşturma ve kullanma IntelliSense kod parçacıkları](https://msdn.microsoft.com/library/ms165392.aspx)
- IntelliSense kod parçacıkları parçacıkları özelleştirme ve sorun giderme gibi Visual Basic'e özel özellikleri hakkında daha fazla bilgi edinin. Daha fazla bilgi için bkz: [Visual Basic IntelliSense kodu parçacıkları](https://msdn.microsoft.com/library/18yz4be4.aspx)
- C# hakkında daha fazla bilgi-IntelliSense, belirli özelliklerini yeniden düzenleme ve kod parçacıkları gibi. Daha fazla bilgi için bkz: [Visual C# IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx).
