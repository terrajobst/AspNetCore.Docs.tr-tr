---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: Birim SignalR uygulamalarını test etme | Microsoft Docs
author: pfletcher
description: Bu makalede, SignalR 2.0 birim testi özelliklerinin nasıl kullanılacağını açıklar.
ms.author: aspnetcontent
ms.date: 06/10/2014
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: f46c6edd2002bcc6e62f4e4fe313c9e50e35aaff
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840920"
---
<a name="unit-testing-signalr-applications"></a>SignalR uygulamalarına birim testi
====================
tarafından [Patrick Fletcher](https://github.com/pfletcher)

> Bu makalede, SignalR 2 birim testi özelliklerini kullanmayı açıklar. 
> 
> ## <a name="software-versions-used-in-this-topic"></a>Bu konu başlığında kullanılan yazılım sürümleri
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR sürüm 2
>   
> 
> 
> ## <a name="questions-and-comments"></a>Sorularınız ve yorumlarınız
> 
> Lütfen bu öğreticide sevmediğinizi nasıl ve ne sayfanın alt kısmındaki açıklamalarda geliştirebileceğimiz hakkında geri bildirim bırakın. Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları gönderebilir [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).


<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a>Birim SignalR uygulamalarını test etme

SignalR uygulamanız için birim testleri oluşturmak için SignalR 2'de birim testi özellikleri kullanabilirsiniz. SignalR 2 içeren [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) test hub'ı yöntemlerinizi benzetimini yapmak için sahte bir nesne oluşturmak için kullanılan arabirim.

Bu bölümde, oluşturulan uygulama için birim testleri ekleyeceksiniz [Başlarken Öğreticisi](../getting-started/tutorial-getting-started-with-signalr.md) kullanarak [XUnit.net](https://github.com/xunit/xunit) ve [Moq](https://github.com/Moq/moq4).

XUnit.net test denetlemek için kullanılacak; Moq oluşturmak için kullanılacak bir [sahte](http://en.wikipedia.org/wiki/Mock_object) sınama nesnesi. Sahte işlem diğer çerçeveler, isterseniz kullanılabilir; [NSubstitute](http://nsubstitute.github.io/) de iyi bir seçimdir. Bu öğreticide, iki yolla sahte nesnenin oluşturulacağı gösterilmektedir: ilk olarak kullanarak bir `dynamic` (.NET Framework 4'te sunulmuştur) nesne ve ikinci bir arabirim kullanarak.

### <a name="contents"></a>İçindekiler

Bu öğreticide, aşağıdaki bölümleri içerir.

- [Birim testi ile dinamik](#dynamic)
- [Birim testi türüne göre](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a>Birim testi ile dinamik

Bu bölümde, oluşturulan uygulama için bir birim testi ekleyeceksiniz [Başlarken Öğreticisi](../getting-started/tutorial-getting-started-with-signalr.md) kullanarak dinamik bir nesne.

1. Yükleme [XUnit Çalıştırıcısı uzantısı](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) Visual Studio 2013 için.
2. Ya da tamamlamak [Başlarken Öğreticisi](../getting-started/tutorial-getting-started-with-signalr.md), veya tamamlanmış uygulamayı karşıdan [MSDN Kod Galerisi](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).
3. Başlarken uygulaması indirme sürümünü kullanıyorsanız, **Paket Yöneticisi Konsolu** tıklatıp **geri** SignalR paketini projeye eklemek için.

    ![Paketleri geri yükle](unit-testing-signalr-applications/_static/image1.png)
4. Birim testi için çözüme bir proje ekleyin. Çözümünüzde sağ **Çözüm Gezgini** seçip **Ekle**, **yeni proje...** . Altında **C#** düğümünü **Windows** düğümü. Seçin **sınıf kitaplığı**. Yeni proje adını **TestLibrary** tıklatıp **Tamam**.

    ![Test kitaplığı oluşturma](unit-testing-signalr-applications/_static/image2.png)
5. Bir başvuru test kitaplığı projesinde SignalRChat projeye ekleyin. Sağ **TestLibrary** seçin ve proje **Ekle**, **başvurusu...** . Seçin **projeleri** düğümünde **çözüm** düğüm ve onay **SignalRChat**. **Tamam**'ı tıklatın.

    ![Proje Başvurusu Ekle](unit-testing-signalr-applications/_static/image3.png)
6. SignalR ve Moq XUnit paketlere Ekle **TestLibrary** proje. İçinde **Paket Yöneticisi Konsolu**ayarlayın **varsayılan proje** açılan **TestLibrary**. Konsol penceresinde aşağıdaki komutları çalıştırın:

   - `Install-Package Microsoft.AspNet.SignalR`
   - `Install-Package Moq`
   - `Install-Package XUnit`

     ![Paketleri yükleme](unit-testing-signalr-applications/_static/image4.png)
7. Test dosyası oluşturun. Sağ **TestLibrary** projesine **Ekle...** , **Sınıfı**. Yeni bir sınıf adı **Tests.cs**.
8. Tests.cs içeriğini aşağıdaki kodla değiştirin.

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    Yukarıdaki kod test istemcisi kullanılarak oluşturulan `Mock` nesnesinden [Moq](https://github.com/Moq/moq4) tür kitaplığı [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (SignalR 2.1 içinde atama `dynamic` türü için parametre.) `IHubCallerConnectionContext` Arabirimi ile istemci üzerinde yöntemleri çağırmayı proxy nesnedir. `broadcastMessage` Çağıran, böylece işlev için sahte istemci sonra tanımlanmış `ChatHub` sınıfı. Sonra test motoru çağırır `Send` yöntemi `ChatHub` sırayla sahte çağrıları sınıfı `broadcastMessage` işlevi.
9. Basarak çözümü oluşturun **F6**.
10. Birim testini çalıştırın. Visual Studio'da **Test**, **Windows**, **Test Gezgini**. Test Gezgini penceresinde sağ **HubsAreMockableViaDynamic** seçip **seçili Testleri Çalıştır**.

    ![Test Gezgini](unit-testing-signalr-applications/_static/image5.png)
11. Test, Test Gezgini penceresinin alt bölmede denetleyerek geçtiğini doğrulayın. Pencere geçilen testleri gösterir.

    ![Test geçildi](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a>Birim testi türüne göre

Bu bölümde, oluşturulan uygulama için bir test ekleyeceksiniz [Başlarken Öğreticisi](../getting-started/tutorial-getting-started-with-signalr.md) test edilecek yöntemi içeren bir arabirim kullanarak.

1. Adım 1-7 tamamlamak [birim testi ile dinamik](#dynamic) yukarıdaki öğretici.
2. Tests.cs içeriğini aşağıdaki kodla değiştirin.

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    Yukarıdaki kod imzasını tanımlayan bir arabirimi oluşturulur `broadcastMessage` yöntemi için test motoru sahte bir istemci oluşturulur. Sahte bir istemci kullanarak sonra oluşturulan `Mock` nesnenin türü [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (SignalR 2.1 içinde atama `dynamic` tür parametresi için.) `IHubCallerConnectionContext` Arabirimi ile istemci üzerinde yöntemleri çağırmayı proxy nesnedir.

    Test sonra bir örneğini oluşturur `ChatHub`ve ardından sahte bir sürümüne oluşturan `broadcastMessage` yöntemi çağırarak sırayla çağrılır `Send` hub yöntemi.
3. Basarak çözümü oluşturun **F6**.
4. Birim testini çalıştırın. Visual Studio'da **Test**, **Windows**, **Test Gezgini**. Test Gezgini penceresinde sağ **HubsAreMockableViaDynamic** seçip **seçili Testleri Çalıştır**.

    ![Test Gezgini](unit-testing-signalr-applications/_static/image7.png)
5. Test, Test Gezgini penceresinin alt bölmede denetleyerek geçtiğini doğrulayın. Pencere geçilen testleri gösterir.

    ![Test geçildi](unit-testing-signalr-applications/_static/image8.png)
