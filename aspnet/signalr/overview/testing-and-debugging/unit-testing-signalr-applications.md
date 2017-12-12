---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: "SignalR uygulamalarında birim testi | Microsoft Docs"
author: pfletcher
description: "Bu makalede, SignalR 2.0 birim testi özelliklerini kullanmayı açıklar."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: e55efd644dd4b6fb57061ffb89a5c041136c7b5e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="unit-testing-signalr-applications"></a>Birim testi SignalR uygulamalarını
====================
tarafından [CAN Fletcher'dan](https://github.com/pfletcher)

> Bu makalede, SignalR 2 birim testi özelliklerini kullanmayı açıklar. 
> 
> ## <a name="software-versions-used-in-this-topic"></a>Bu konuda kullanılan yazılım sürümleri
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
> Lütfen Bu öğretici beğendiğinizi nasıl ve ne biz sayfanın sonundaki açıklamalarında artabileceğini görüşlerinizi. Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları nakledebilirsiniz [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).


<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a>SignalR uygulamalarında birim testi

SignalR uygulamanız için birim testleri oluşturmak için birim testi özellikleri SignalR 2'de kullanabilirsiniz. SignalR 2 içeren [IHubCallerConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) test hub'ı yöntemlerinizi benzetimini yapmak için sahte bir nesneyi oluşturmak için kullanılan arabirim.

Bu bölümde, oluşturduğunuz uygulama için birim testleri ekleyeceksiniz [Başlarken Öğreticisi](../getting-started/tutorial-getting-started-with-signalr.md) kullanarak [XUnit.net](https://github.com/xunit/xunit) ve [Moq](https://github.com/Moq/moq4).

XUnit.net test denetlemek için kullanılacak; Moq oluşturmak için kullanılacak bir [mock](http://en.wikipedia.org/wiki/Mock_object) test etmek için nesne. Diğer mocking çerçeveleri isterseniz kullanılabilir; [NSubstitute](http://nsubstitute.github.io/) de iyi bir seçimdir. Bu öğretici iki yolla sahte nesne ayarlanacağı gösterilmiştir: ilk olarak kullanarak bir `dynamic` (.NET Framework 4'te tanıtılan) nesne ve ikincisi, bir arabirim kullanarak.

### <a name="contents"></a>İçindekiler

Bu öğretici aşağıdaki bölümleri içerir.

- [Birim testi ile dinamik](#dynamic)
- [Birim türü tarafından testi](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a>Birim testi ile dinamik

Bu bölümde, oluşturduğunuz uygulama için bir birim testi ekleyeceksiniz [Başlarken Öğreticisi](../getting-started/tutorial-getting-started-with-signalr.md) bir dinamik Nesne kullanma.

1. Yükleme [XUnit Çalıştırıcısı uzantısı](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) Visual Studio 2013 için.
2. Ya da tamamlamak [Başlarken Öğreticisi](../getting-started/tutorial-getting-started-with-signalr.md), veya tamamlanmış uygulamadan indirme [MSDN kod Galerisi'nden](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).
3. Başlarken uygulaması indirme sürümünü kullanıyorsanız, açık **Paket Yöneticisi Konsolu** tıklatıp **geri** SignalR paketini projeye eklemek için.

    ![Paket geri yükleme](unit-testing-signalr-applications/_static/image1.png)
4. Birim testi için çözümü bir proje ekleyin. Çözümünüze sağ **Çözüm Gezgini** seçip **Ekle**, **yeni proje...** . Altında **C#** düğümü, select **Windows** düğümü. Seçin **sınıf kitaplığı**. Yeni proje adı **TestLibrary** tıklatıp **Tamam**.

    ![Test kitaplığı oluşturma](unit-testing-signalr-applications/_static/image2.png)
5. Bir başvuru test kitaplığı projesinde SignalRChat projeye ekleyin. Sağ **TestLibrary** proje ve seçin **Ekle**, **başvuru...** . Seçin **projeleri** düğümü altında **çözüm** düğümü ve onay **SignalRChat**. **Tamam**'ı tıklatın.

    ![Proje Başvurusu Ekle](unit-testing-signalr-applications/_static/image3.png)
6. SignalR, Moq ve XUnit paketlere Ekle **TestLibrary** projesi. İçinde **Paket Yöneticisi Konsolu**ayarlayın **varsayılan proje** açılır **TestLibrary**. Konsol penceresinde aşağıdaki komutları çalıştırın:

    - `Install-Package Microsoft.AspNet.SignalR`
    - `Install-Package Moq`
    - `Install-Package XUnit`

    ![Paket yüklemek için](unit-testing-signalr-applications/_static/image4.png)
7. Test dosyası oluşturun. Sağ **TestLibrary** proje ve tıklatın **Ekle...** , **Sınıfı**. Yeni sınıf **Tests.cs**.
8. Tests.cs içeriğini aşağıdaki kodla değiştirin.

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    Yukarıdaki kod test istemcisi kullanılarak oluşturulan `Mock` nesnesinin [Moq](https://github.com/Moq/moq4) tür kitaplığı [IHubCallerConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (SignalR 2.1 atamak `dynamic` türü için parametre.) `IHubCallerConnectionContext` İstemcide yöntemleri ile başlattığınıza proxy nesnesi bir arabirimdir. `broadcastMessage` Çağıran böylece işlevi sahte istemcisi ardından tanımlanan `ChatHub` sınıfı. Test altyapısı sonra çağırır `Send` yöntemi `ChatHub` sırayla mocked çağırır sınıfı `broadcastMessage` işlevi.
9. Tuşlarına basarak çözümü oluşturun **F6**.
10. Birim testi çalıştırma. Visual Studio'da seçin **Test**, **Windows**, **Test Gezgini**. Test Gezgini penceresinde sağ **HubsAreMockableViaDynamic** seçip **seçili Testleri Çalıştır**.

    ![Test Gezgini](unit-testing-signalr-applications/_static/image5.png)
11. Test Gezgini penceresinin alt bölmesinde denetleyerek geçilen testi doğrulayın. Pencere testini geçtiğini gösterir.

    ![Sınama başarılı](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a>Birim türü tarafından testi

Bu bölümde, oluşturduğunuz uygulama için bir sınama ekleyeceksiniz [Başlarken Öğreticisi](../getting-started/tutorial-getting-started-with-signalr.md) sınanacak yöntemi içeren bir arabirim kullanarak.

1. Adım 1-7'de tamamlamak [birim testi ile dinamik](#dynamic) yukarıdaki öğretici.
2. Tests.cs içeriğini aşağıdaki kodla değiştirin.

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    Yukarıdaki kod imzası tanımlayan bir arabirim oluşturulur `broadcastMessage` yöntemi için test altyapısı sahte istemci oluşturacaktır. Sahte bir istemci, ardından kullanılarak oluşturulur `Mock` türünde nesne [IHubCallerConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (SignalR 2.1 atamak `dynamic` tür parametresi için.) `IHubCallerConnectionContext` İstemcide yöntemleri ile başlattığınıza proxy nesnesi bir arabirimdir.

    Test sonra bir örneğini oluşturur `ChatHub`ve sahte bir sürümüne oluşturur `broadcastMessage` çağırarak sırayla çağrılır yöntemi `Send` hub'ında yöntemi.
3. Tuşlarına basarak çözümü oluşturun **F6**.
4. Birim testi çalıştırma. Visual Studio'da seçin **Test**, **Windows**, **Test Gezgini**. Test Gezgini penceresinde sağ **HubsAreMockableViaDynamic** seçip **seçili Testleri Çalıştır**.

    ![Test Gezgini](unit-testing-signalr-applications/_static/image7.png)
5. Test Gezgini penceresinin alt bölmesinde denetleyerek geçilen testi doğrulayın. Pencere testini geçtiğini gösterir.

    ![Sınama başarılı](unit-testing-signalr-applications/_static/image8.png)
