---
uid: web-forms/overview/getting-started/code-editing-in-web-forms-pages
title: Visual Studio 2013'te ASP.NET Web Forms kodunu düzenleme | Microsoft Docs
author: Erikre
description: ''
ms.author: aspnetcontent
ms.date: 03/03/2014
ms.assetid: 5344b74e-b888-479a-92bc-601a33bd61a2
msc.legacyurl: /web-forms/overview/getting-started/code-editing-in-web-forms-pages
msc.type: authoredcontent
ms.openlocfilehash: c8f0184ffcec0a988dc8bc814e16936c173659c7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815979"
---
<a name="code-editing-aspnet-web-forms-in-visual-studio-2013"></a>Visual Studio 2013'te kod düzenleme ASP.NET Web formları
====================
tarafından [Erik Reitan](https://github.com/Erikre)

Çok sayıda ASP.NET Web formu sayfalarında, Visual Basic, C# veya başka bir dilde kod yazın. Visual Studio Kod düzenleyicisinde hataları önlemeye yardımcı olurken hızla kod yazmanıza yardımcı olabilir. Ayrıca, düzenleyici gerçekleştirmeniz gereken iş miktarını azaltmaya yardımcı olmak için yeniden kullanılabilir kod oluşturma yol sağlar.

Bu izlenecek yol, Visual Studio Kod Düzenleyicisi'nin çeşitli özelliklerini gösterir.

Bu kılavuz boyunca, öğreneceksiniz nasıl yapılır:

- Satır içi kodlama hataları düzeltin.
- Yeniden düzenleme ve kod yeniden adlandırın.
- Değişkenleri ve nesneleri yeniden adlandırın.
- Kod parçacıkları ekleyin.

## <a name="prerequisites"></a>Önkoşullar


Bu izlenecek yolu tamamlamak için şunlar gerekir:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) veya [Web için Microsoft Visual Studio Express 2013](https://www.microsoft.com/visualstudio/11/downloads#express-web). .NET Framework otomatik olarak yüklenir. 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 ve Web için Visual Studio Express 2013 Microsoft genellikle için Visual Studio Bu öğretici serisinin denir.  
    >   
    > Visual Studio kullanıyorsanız, bu kılavuzda, seçtiğiniz varsayılır **Web geliştirme** ayarlar koleksiyonu, Visual Studio'yu ilk başlattığınızda. Daha fazla bilgi için [nasıl yapılır: Web geliştirme ortamı ayarlarını seçin](https://msdn.microsoft.com/library/ff521558.aspx).

  Visual Studio ve ASP.NET giriş için bkz [Visual Studio 2013'te temel ASP.NET 4.5 Web Forms sayfası oluşturma](creating-a-basic-web-forms-page.md).   
 

## <a name="creating-a-web-application-project-and-a-page"></a>Bir Web uygulaması projesi ve bir sayfa oluşturma

<a id="sectionToggle0"></a>

Kılavuzun bu bölümünde, bir Web uygulaması projesi oluşturmak ve yeni bir sayfa ekleyin.

### <a name="to-create-a-web-application-project"></a>Bir Web uygulaması projesi oluşturmak için

1. Microsoft Visual Studio'yu açın.
2. Üzerinde **dosya** menüsünde **yeni proje**.  
    ![Dosya menüsü](code-editing-in-web-forms-pages/_static/image1.png)

    **Yeni proje** iletişim kutusu görüntülenir.
3. Seçin **şablonları**  - &gt; **Visual C#**  - &gt; **Web** soldaki şablonları grubu.
4. Seçin **ASP.NET Web uygulaması** Orta sütunda şablonu.
5. Projenizi adlandırın ***BasicWebApp*** tıklatıp **Tamam** düğmesi.   
![Yeni Proje iletişim kutusu](code-editing-in-web-forms-pages/_static/image2.png)
6. Ardından, **Web Forms** şablonu ve tıklatın **Tamam** projeyi oluşturmak için.  
![Yeni ASP.NET projesi iletişim kutusu](code-editing-in-web-forms-pages/_static/image3.png)  

    Visual Studio Web Forms şablonu temel alan önceden oluşturulmuş işlevler içeren yeni bir proje oluşturur.


## <a name="creating-a-new-aspnet-web-forms-page"></a>Yeni bir ASP.NET Web Forms sayfası oluşturma


Yeni Web Forms kullanarak bir uygulama oluşturduğunuzda **ASP.NET Web uygulaması** proje şablonu, Visual Studio ekler adlı bir ASP.NET sayfasına (Web Forms sayfası) *Default.aspx*, yanı sıra birkaç diğer dosya ve klasörler. Kullanabileceğiniz *Default.aspx* sayfasında Web uygulamanız için giriş sayfası olarak. Ancak, bu kılavuz için oluşturur ve yeni bir sayfa ile çalışır.

### <a name="to-add-a-page-to-the-web-application"></a>Web uygulaması için bir sayfa eklemek için


1. İçinde **Çözüm Gezgini**, Web uygulamasının adını sağ tıklayın (Bu öğreticide uygulama adı **BasicWebSite**) ve ardından **Ekle**  - &gt; **Yeni öğe**.   
**Yeni Öğe Ekle** iletişim kutusu görüntülenir.
2. Seçin **Visual C#**  - &gt; **Web** soldaki şablonları grubu. Ardından, **Web formu** ortasından listesinde ve adlandırın *FirstWebPage.aspx*.   
    ![Yeni Öğe Ekle iletişim kutusu](code-editing-in-web-forms-pages/_static/image4.png)
3. Tıklayın **Ekle** Web Forms sayfası projenize eklenecek.  
 Visual Studio, yeni bir sayfa oluşturur ve açar.
4. Ardından, bu yeni sayfa varsayılan başlangıç sayfası olarak ayarlayın. İçinde **Çözüm Gezgini**, adlı yeni bir sayfa sağ *FirstWebPage.aspx* seçip **başlangıç sayfası olarak ayarla**. İlerlememizin, test etmek için bu uygulamayı bir sonraki çalıştırmanızda otomatik olarak bu tarayıcıdaki yeni bir sayfa görürsünüz.


## <a name="correcting-inline-coding-errors"></a>Satır içi kodlama hataları düzeltme


Visual Studio Kod düzenleyicisinde kod yazma ve bir hata yaptıysanız, Kod Düzenleyicisi hatayı düzeltmek için yardımcı olan hataları önlemek için yardımcı olur. Kılavuzun bu bölümünde bir Düzenleyicisi'nde hata düzeltme özellikleri gösteren bir kod satırı yazar.

### <a name="to-correct-simple-coding-errors-in-visual-studio"></a>Visual Studio'da basit kodlama hatalarını düzeltmek için


1. İçinde **tasarım** görünümünde, boş bir sayfa için bir işleyici oluşturmak için çift tıklatın **yük** sayfa için olay.   
   Olay işleyicisi yalnızca bir yer olarak bazı kodlar yazma için kullanırsınız.
2. İşleyici içinde bir hata ve ENTER tuşuna içeren aşağıdaki satırdan **ENTER**:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample1.cs)]

   Bastığınızda **ENTER**, Kod Düzenleyicisi, yeşil ve kırmızı alt çizgiler yerleştirir (sık çağrı &quot;dalgalı&quot; satırları) sorunları kod alanlarının altında. Yeşil bir çizgi bir uyarı gösterir. Kırmızı alt çizgi, düzeltmeniz gerekir bir hata olduğunu gösterir. 

    Fare işaretçisini tutun `myStr` hakkında uyarı bildiren bir araç ipucu görmek için. Ayrıca, fare işaretçisini hata iletisini görmek için kırmızı alt çizgi üzerinde tutun.

    Aşağıdaki görüntüde, alt çizgi ile kod gösterilmektedir.

    ![Hoş Geldiniz metni Tasarım görünümünde](code-editing-in-web-forms-pages/_static/image5.png "metin Tasarım görünümünde Hoş Geldiniz")  
   Noktalı virgül ekleyerek hatanın düzeltilmesi gereken `;` satırın sonuna. Uyarı yeterlidir, kullanmadığınız alacağınızı `myStr` henüz değişkeni.  

    > [!NOTE] 
    > 
    > Visual Studio'da ayarlar seçerek biçimlendirme, geçerli kod görüntüleme **Araçları**  - &gt; **seçenekleri**  - &gt; **yazı tipleri ve Renkleri**.


## <a name="refactoring-and-renaming"></a>Yeniden düzenleme ve yeniden adlandırma

Yeniden düzenleme, kodunuzu anlamak ve işlevselliğini korurken korumak için daha kolay hale getirmek için yeniden yapılandırma içeren bir yazılım Metodoloji olur. Basit bir örnek veritabanından veri almak için bir olay işleyicisine kod yazma olabilir. Sayfanız geliştirirken, birkaç farklı işleyicilerindeki verilere erişmek gereken keşfedin. Bu nedenle, bir veri erişim yöntemi sayfasında oluşturarak ve yöntemine yönelik çağrılar işleyicileri ekleme sayfanın kodun yeniden.

Kod Düzenleyicisi, yeniden düzenleme çeşitli görevleri gerçekleştirmenize yardımcı olacak araçlar içerir. Bu kılavuzda, yeniden düzenleme iki teknikleri ile çalışır: değişkenleri yeniden adlandırma ve yöntemleri ayıklanıyor. Diğer yeniden düzenleme seçeneklerini alanları Kapsüllenen, yöntem parametreleri yerel değişkenlere yükseltme ve yöntem parametreleri yönetme içerir. Bu yeniden düzenleme seçeneklerini kullanılabilirliğini kodun konumuna bağlıdır.

### <a name="refactoring-code"></a>Kodu yeniden düzenleme

Genel bir yeniden düzenleme senaryoyu (Ayıkla) oluşturmaktır bir yöntem içinde bir yöntemi gibi başka bir üye kodu. Bu ilk üyenin boyutunu azaltır ve ayıklanan kod yeniden kullanılabilir hale getirir.

Kılavuzun bu bölümünde bazı basit kod yazın ve ardından bir yöntem bundan ayıklayın. C# programlama dili kullanan bir sayfa oluşturacak şekilde yeniden düzenleme C# ' ta desteklenir.

### <a name="to-extract-a-method-in-a-c-page"></a>Bir C# sayfasında bir yöntem ayıklamak için

1. Geçiş **tasarım** görünümü.
2. İçinde **araç kutusu**, gelen **standart** sekmesinde, sürükleyin bir [düğmesi](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) sayfaya denetim.
3. Çift **düğmesi** denetim işleyicisi oluşturmak için kendi [tıklayın](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) olay ve ardından aşağıdaki vurgulanmış kodu ekleyin:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample2.cs?highlight=3-16)]

   Kod oluşturur bir **ArrayList** nesnesi değerlerle yüklemek için bir döngü kullanır ve ardından içeriğini görüntülemek için başka bir döngü kullanır **ArrayList** nesne.
4. Tuşuna **CTRL + F5** sayfayı çalıştırın ve ardından **düğmesi** aşağıdaki çıktıyı gördüğünüzü emin olmak için:   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample3.html)]
5. Kod Düzenleyicisi'ne dönmek ve olay işleyicisi aşağıdaki satırları seçin.   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample4.html)]
6. Seçime sağ tıklayın, **yeniden düzenleme**ve ardından **yöntemi ayıklama**. 

    **Yöntemi ayıklama** iletişim kutusu görüntülenir.
7. İçinde **yeni yöntem adı** kutusuna **DisplayArray**ve ardından **Tamam**. 

    Kod Düzenleyicisi adlı yeni bir yöntem oluşturur `DisplayArray`ve yeni yönteme bir çağrı koyar **tıklayın** döngü nerede oluştuğunu başlangıçta işleyici.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample5.cs?highlight=12)]
8. Tuşuna **CTRL + F5** yeniden çalıştırırsanız ve **düğmesi**.

    Önce yaptığınız gibi sayfa aynı şekilde çalışır. `DisplayArray` Yöntemi çağrısı nerede olursanız olun artık olabilir sayfa sınıfında.

## <a name="renaming-variables"></a>Değişkenleri yeniden adlandırma

Değişkenleri, yanı sıra nesneleri çalışırken, zaten kodunuzda başvurulan sonra bunları yeniden adlandırmak isteyebilirsiniz. Ancak, değişkenler ve nesneler yeniden adlandırma başvurulardan birini yeniden adlandırma kaçırmanıza kesmek için kodu neden olabilir. Bu nedenle, yeniden adlandırma gerçekleştirmek için yeniden düzenleme kullanabilirsiniz.

### <a name="to-use-refactoring-to-rename-a-variable"></a>Bir değişkeni yeniden adlandırmak için yeniden düzenleme kullanmak için


1. İçinde **tıklayın** olay işleyicisi, aşağıdaki satırı bulun:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample6.cs)]
2. Değişken adına sağ tıklayın `alist`, seçin **yeniden düzenleme**ve ardından **Yeniden Adlandır**.

    **Yeniden Adlandır** iletişim kutusu görüntülenir.
3. İçinde **yeni adı** kutusuna **ArrayList1** emin **başvuru değişikliklerini önizle** onay kutusunun seçili. Sonra **Tamam**'a tıklayın.

    **Değişiklikleri Önizle** iletişim kutusu görünür ve tüm başvuruları yeniden adlandırdığınız değişkeni içeren bir ağaç görüntüler.
4. Tıklayın **Uygula** kapatmak için **Değişiklikleri Önizle** iletişim kutusu.

    Özellikle, seçtiğiniz örneğine başvurma değişkenleri yeniden adlandırılır. Ancak, unutmayın, değişken `alist` aşağıdaki satırda yeniden adlandırılmaz.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample7.cs)]

    Değişken `alist` değişkeni aynı değeri temsil etmediğinden bu satırda yeniden adlandırılmış değil `alist` adlandırdığınız. Değişken `alist` içinde `DisplayArray` bu yöntem için bir yerel değişken bildirimidir. Bu değişkenler yeniden adlandırmak için yeniden düzenleme kullanarak yalnızca düzenleyicide Bul ve Değiştir eylemini gerçekleştirmekten daha farklı olduğunu gösterir; yeniden düzenleme ile çalışma değişkeni semantiği bilgisi değişkenlerle yeniden adlandırır.


## <a name="inserting-snippets"></a>Kod parçacıkları ekleme

Web Forms geliştiriciler sık yapmanız gereken birçok kodlama görevleri olduğundan, Kod Düzenleyicisi kod parçacıkları veya önceden kod blokları bir kitaplık sağlar. Bu kod parçacıklarının sayfanıza ekleyebilirsiniz.

Visual Studio'da kullandığınız her bir dilin kod parçacıkları ekleme küçük farklılıklar vardır. Kod parçacıkları ekleme hakkında daha fazla bilgi için bkz: [Visual Basic IntelliSense kodu parçacıkları](https://msdn.microsoft.com/library/18yz4be4.aspx). Visual C# içinde kod parçacıkları ekleme hakkında daha fazla bilgi için bkz: [Visual C# kod parçacıkları](https://msdn.microsoft.com/library/z41h7fat.aspx).

## <a name="next-steps"></a>Sonraki Adımlar

Bu izlenecek kodunuzdaki hataları düzeltme, kodu yeniden düzenleme, yeniden adlandırma değişkenleri ve kodunuza kod parçacıkları ekleme için Visual Studio 2010 Kod Düzenleyicisi'ni temel özelliklerinde gösterilen. Ek özellik Düzenleyicisi'nde uygulama geliştirme hızlı ve kolay hale getirebilirsiniz. Örneğin, aşağıdakileri yapabilirsiniz:

- IntelliSense kod parçacıkları için çevrimiçi arama IntelliSense seçeneklerini değiştirme ve kod parçacıkları yönetme gibi özellikleri hakkında daha fazla bilgi edinin. Daha fazla bilgi için [IntelliSense kullanarak](https://msdn.microsoft.com/library/hcw1s69b.aspx).
- Kendi kod parçacıklarınızı oluşturmayı öğrenin. Daha fazla bilgi için [oluşturma ve IntelliSense kod parçacıkları kullanma](https://msdn.microsoft.com/library/ms165392.aspx)
- Visual Basic'e özel IntelliSense kod parçacıkları, kod parçacıkları özelleştirme ve sorun giderme gibi özellikleri hakkında daha fazla bilgi edinin. Daha fazla bilgi için [Visual Basic IntelliSense kod parçacıkları](https://msdn.microsoft.com/library/18yz4be4.aspx)
- C# hakkında daha fazla bilgi-IntelliSense, yeniden düzenleme ve kod parçacıkları gibi belirli özellikleri. Daha fazla bilgi için [Visual C# IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx).
