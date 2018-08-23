---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
title: Web Farm Framework ile bir sunucu grubu oluşturma | Microsoft Docs
author: jrjlee
description: Bu konuda, Web grubu çerçevesi (WFF) 2.0 oluşturmak ve bir web sunucu grubundan sunucuları koleksiyonu yapılandırmak için kullanmayı açıklar.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 656dd06d-806c-467c-863d-9fc45e5ba3ab
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
msc.type: authoredcontent
ms.openlocfilehash: b650a05a22f18ffdcc114a9a64054dd0a34bc041
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754937"
---
<a name="creating-a-server-farm-with-the-web-farm-framework"></a>Web Farm Framework ile bir sunucu grubu oluşturma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF'yi indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konuda, Web grubu çerçevesi (WFF) 2.0 oluşturmak ve bir web sunucu grubundan sunucuları koleksiyonu yapılandırmak için kullanmayı açıklar.


WFF, web platformu ürünlerini ve bileşenleri, web uygulamaları, Web siteleri ve yapılandırma ayarlarının birden çok yük dengeli web sunucusu arasında eşitleme sağlar. Birden fazla web sunucusu, hazırlama ve üretim ortamları gibi gereken senaryolarda bu, dağıtım ve yapılandırma işlemini büyük ölçüde kolaylaştırabilir. Bir web uygulaması tek bir sunucuya dağıtabilirsiniz&#x2014; *birincil sunucu*&#x2014;ve WFF otomatik olarak, web uygulaması sunucularındaki tüm diğer web sunucu grubundaki çoğaltın.

## <a name="understanding-the-web-farm-framework"></a>Web Farm Framework anlama

WFF 2.0 sağlanması, yönetilmesi ve bir web sunucu grubu için içerik dağıtmak için kullanabilirsiniz. WFF dağıtım üç ana sunucu rolünden oluşur:

- *Denetleyici sunucusu*. Bu sunucuyu oluşturmak ve WFF sunucu grupları yapılandırmak için kullanın. Web platformu bileşenlerini, yapılandırma ayarları ve uygulamaları sunucu grubunda web sunucuları arasında eşitleme controller sunucusunu yönetir. 2.0 WFF denetleyicisi sunucusuna yükleyin ve denetleyici sunucunun sırayla bir sunucu grubundaki sunucuların her birinde WFF Aracısı'nı yükler. Denetleyici sunucusu kavramsal olarak herhangi bir WFF sunucu grubuna ait değil ve bir tek denetleyici sunucusunun birden çok sunucu grupları yönetebilirsiniz. Bu senaryoda, sunucu grubu hazırlama ve üretim sunucu grubu oluşturmak ve yönetmek için tek bir WFF denetleyicisi sunucu kullanın.
- *Birincil sunucu*. Her WFF sunucu grubu, tek bir birincil sunucu içerir. Web platformu bileşenlerini yükleyin veya birincil sunucu uygulamalarını dağıtma, yaptığınız değişiklikler diğer sunuculara sunucu grubundaki WFF eşitler.
- *İkincil sunucu*. Her WFF sunucu grubu bir veya daha fazla ikincil sunucularını içerir. Sunucu grubu içindeki her ikincil sunucuya birincil sunucuya yaptığınız tüm değişiklikler çoğaltılır.

Bu, bu sunucu rolleri için Fabrikam, Inc. hazırlama ve üretim ortamlarına nasıl ilişkili olduğunu gösterir:

![](creating-a-server-farm-with-the-web-farm-framework/_static/image1.png)

Bu senaryoda, hazırlık ortamı ve üretim ortamında her ikisi de WFF sunucu grupları yapılandırılır. Tek bir WFF denetleyicisi sunucu her iki grupları yönetir. Her bir sunucu grubu içinde tüm değişiklikleri birincil sunucuya her ikincil sunucuya çoğaltılır.

Hazırlama ve üretim ortamınızı yapılandırmak başlamadan önce kendiniz WFF 2.0 ile ilgili önemli kavramlar hakkında bilgi için bu makaleleri okumanızı öneririz:

- [IIS 7 için Web Farm Framework 2.0 genel bakış](https://go.microsoft.com/?linkid=9805126)
- [IIS 7 için Web grubu Framework 2.0 ile bir sunucu grubu ayarlama](https://go.microsoft.com/?linkid=9805127)
- [Sistem ve IIS 7 için Web Farm Framework 2.0 Platform gereksinimleri](https://go.microsoft.com/?linkid=9805128)

## <a name="task-overview"></a>Görev genel bakış

Bu konudaki yönergeler ve görevleri tamamlamak için en az üç sunucu gerekir&#x2014;bir WFF denetleyicisi, sunucu grubu için bir birincil web sunucusu ve bir veya daha fazla ikincil web sunucuları sunucu grubu. Daha fazla ikincil sunucu, herhangi bir zamanda WFF sunucu grubuna ekleyebilirsiniz. Oluşturma ve yapılandırma WFF sunucu grubu için şunları yapmanız gerekir, hazırlama veya üretim ortamı için bir yüksek düzeyde:

- Internet Information Services (IIS) 7.5 ve WFF 2.0 yükleyerek bir denetleyici sunucusu oluşturun.
- Birincil ve ikincil sunucular, bir ortak yönetici hesabı oluşturmayı ve güvenlik duvarı özel durumlarını yapılandırma hazırlayın.
- Sunucu grubu denetleyici sunucusunda IIS Yöneticisi'ni kullanarak yapılandırın.
- IIS uygulama isteği yönlendirme (ARR) veya bir alternatif Yük Dengeleme teknolojisi kullanılarak yük dengelemesini yapılandırın.

Görevler ve bu konudaki yönergeler Windows Server 2008 R2 çalıştıran temiz sunucu yapılarla başlatıyorsanız varsayılır. Her sunucu için başlamadan önce emin olun:

- Windows Server 2008 R2 Service Pack 1 ve tüm kullanılabilir güncelleştirmeler yüklenir.
- Etki alanına katılmış sunucusudur.
- Sunucuda bir statik IP adresi var.

> [!NOTE]
> Bilgisayarlarının bir etki alanına katılmasını sağlama hakkında daha fazla bilgi için bkz: [katılan bilgisayarların etki alanı ve günlüğe kaydetme üzerinde](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Statik IP adreslerini yapılandırma hakkında daha fazla bilgi için bkz. [statik bir IP adresi yapılandırın](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).


## <a name="create-the-wff-controller-server"></a>WFF denetleyicisi sunucusu oluşturma

WFF denetleyicisi sunucusu oluşturmak için IIS 7 veya üzeri hem WFF 2.0 veya sonraki sürümünü yüklemeniz gerekir. Perde WFF IIS Web Dağıtım Aracı (Web dağıtımı) kullanır, grubunuzdaki sunucuların eşitlenecek 2.x. WFF yüklemek için Web Platformu yükleyicisi kullanıyorsanız, yükleyici otomatik olarak indirin ve yükleyin, Web dağıtımı.

**WFF denetleyicisi sunucu oluşturmak için**

1. İndirme ve yükleme [Web Platformu yükleyicisi](https://go.microsoft.com/?linkid=9739157).
2. Üst kısmındaki **Web Platformu yükleyicisi 3.0** penceresinde tıklayın **ürünleri**.
3. Gezinti bölmesinde, pencerenin sol tarafındaki tıklayarak **sunucu**.
4. İçinde **IIS 7 önerilen Yapılandırması** satır, tıklayın **Ekle**.
5. İçinde <strong>Web Farm Framework 2.</strong> <em>x</em> satır, tıklayın <strong>Ekle</strong>.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image2.png)
6. **Yükle**'ye tıklatın. Web Platformu yükleyicisi çeşitli diğer bağımlılıkları yanı sıra Web dağıtım aracı yükleme listesine ekledi dikkat edin.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image3.png)
7. Lisans koşullarını gözden geçirin ve koşulları kabul ediyorsa **kabul ediyorum**.
8. Yükleme tamamlandığında, tıklayın **son**ve ardından kapatın **Web Platformu yükleyicisi 3.0** penceresi.

## <a name="configure-the-primary-and-secondary-servers"></a>Birincil ve ikincil sunucuları yapılandırma

WFF sunucu grubu oluşturmadan önce web sunucularındaki çiftliğinin olmanızı sağlayacak bazı hazırlama görevleri tamamlamanız:

- İzin vermek için güvenlik duvarı özel durumları ekleme **Çekirdek Ağ**, **Uzaktan Yönetim**, ve **dosya ve Yazıcı Paylaşımı** WFF denetleyicisi sunucusuyla iletişim kurmak için özellikleri .
- Bir etki alanı hesabı oluşturun (örneğin, **FABRIKAM\stagingfarm**) Active Directory'de ve her sunucuda yerel Yöneticiler grubuna ekleyin. Sunucu grubu oluşturduğunuzda bu hesap, sunucu grubu yönetici hesabı kullanacaksınız.

Windows Güvenlik Duvarı'nda bu güvenlik duvarı özel durumlarını yapılandırma hakkında daha fazla bilgi için bkz. [sistem ve IIS 7 için Web grubu Framework 2.0 Platform gereksinimlerini](https://go.microsoft.com/?linkid=9805128). Diğer güvenlik duvarı sistemleri için ürün belgelerinize başvurun.

Sonraki yordam, Windows Server 2008 R2'deki yerel administrators grubunun bir etki alanı hesabı eklemek için kullanabilirsiniz. Sunucu grubuna eklemek istediğiniz her sunucuda bu yordamı gerçekleştirmeniz gerekir&#x2014;başka bir deyişle, aynı etki alanı hesabını birincil sunucuda ve her ikincil sunucuda yerel Yöneticiler grubuna ekleyin.

**Bir etki alanı hesabı local administrators grubuna eklemek için**

1. Üzerinde **Başlat** menüsünde **Yönetimsel Araçlar**ve ardından **Sunucu Yöneticisi**.
2. İçinde **Sunucu Yöneticisi'ni** ağaç görünümü bölmesinde penceresini genişletin **yapılandırma**, genişletme **yerel kullanıcılar ve gruplar**ve ardından **grupları**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image4.png)
3. İçinde **grupları** bölmesinde çift **Yöneticiler**.
4. İçinde **Yöneticiler özellikleri** iletişim kutusu, tıklayın **Ekle**.
5. İçinde **kullanıcılar, bilgisayarlar, hizmet hesapları veya gruplar** iletişim kutusu, türü (veya göz atma) etki alanı hesabınıza (örneğin, **FABRIKAM\stagingfarm**) ve ardından **Tamam**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image5.png)
6. İçinde **Yöneticiler özellikleri** iletişim kutusu, tıklayın **Tamam**.

Sunucularınız bir sunucu grubuna eklenmesi artık hazırsınız. Birincil sunucu söz konusu olduğunda, önce veya sunucu grubu oluşturduktan sonra uygulama gereksinimlerinizi karşılamak üzere sunucuyu yapılandırabilirsiniz&#x2014;her iki durumda da, ürünleri, bileşenleri veya yapılandırma dağıtarak WFF sunucular eşitleme İkincil sunucuları için. Basitleştirmek amacıyla, Bu öğretici, sunucu grubu oluşturmayı tamamladığınızda, birincil sunucunun yapılandıracaksınız varsayar.

## <a name="create-the-wff-server-farm"></a>WFF sunucu grubu oluştur

Bu noktada, tüm sunucularınız WFF sunucu grubuna eklenecek hazırsınız:

- WFF denetleyicisi sunucusuna yüklediğiniz.
- Birincil ve ikincil web sunucularınız üzerinde güvenlik duvarı özel durumlarını yapılandırdınız.
- Bir etki alanı hesabı, birincil ve ikincil web sunucularında yerel administrators grubuna ekledik.

Sonraki adım, sunucu grubu içinde WFF oluşturmaktır. Bunu WFF denetleyicisi sunucusuna IIS Yöneticisi'nden yapabilirsiniz.

**WFF sunucu grubu oluşturmak için**

1. WFF denetleyicisi sunucuda üzerinde **Başlat** menüsünde **Yönetimsel Araçlar**ve ardından **Internet Information Services (IIS) Yöneticisi'ni**.
2. İçinde **bağlantıları** bölmesinde yerel sunucu düğümünü genişletin, sağ **sunucu grupları**ve ardından **sunucu grubu oluşturma**.
3. İçinde **sunucu grubu oluşturma** iletişim kutusuna, sunucu grubu için anlamlı bir ad yazın (örneğin, **hazırlama grubu**) ve ardından **sunucu grubu sağla**.
4. Kullanıcı adı ve her sunucuda yerel Yöneticiler grubuna eklenen etki alanı hesabının parolasını yazın.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image6.png)
5. **İleri**'ye tıklayın.
6. Üzerinde **sunucu ekleme** sayfasında, tam etki alanı adı (FQDN) yazın birincil sunucunun, select **birincil sunucu**ve ardından **Ekle**.
7. Bu noktada, WFF sağladığınız kimlik bilgilerini kullanarak birincil sunucunun sizinle iletişim kuracaktır. Bağlantı başarılı olursa, birincil sunucunun tabloya ekleneceğini **sunucu ekleme** sayfası.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image7.png)

    > [!NOTE]
    > Fark etmiş **sunucusu Yük Dengeleme için kullanılabilir** varsayılan olarak seçilidir. WFF Yük Dengelemesi uygulamak ve böylece server grubunuzdaki web sunucuları istekleri dağıtmak için IIS ARR modülü kullanır. Çoğu senaryoda, yalnızca temizler **sunucusu Yük Dengeleme için kullanılabilir** bir üçüncü taraf yük dengeleme çözümü bunun yerine kullanmak isterse seçeneği.
8. Üzerinde **sunucu ekleme** sayfasında ilk ikincil sunucunuzun FQDN'sini yazın ve ardından **Ekle**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image8.png)
9. Grubunuzdaki diğer ikincil sunucular için 7. adımı yineleyin ve ardından **son**.

WFF sunucu grubunuzu artık çalışır durumda. Herhangi bir web platformu ürünlerini veya birincil sunucu ve web uygulamaları veya birincil sunucuya, dağıttığınız içeriği Yükleme bileşenleri, tüm ikincil sunucularınız üzerinde otomatik olarak sağlanır.

WFF geniş çaplı ve karmaşık bir konu olduğu ve üzerinde daha fazla bilgi edinebilirsiniz [IIS 7 için Microsoft Web grubu çerçevesi 2.0](https://go.microsoft.com/?linkid=9805129) Web sitesi. Şimdilik, ancak dikkat edilmesi gereken iki özellikleri alanlar vardır:

- *Uygulama sağlama* sunucu grubundaki tüm ikincil sunucuya web uygulamaları ve yapılandırma ayarları gibi birincil sunucudan içerik çoğaltır işlemidir. Örneğin, birincil hazırlama sunucunuza örnek Kişi Yöneticisi çözümü dağıtırsanız, bu çözüm tüm ikincil hazırlama sunucularınıza WFF uygulama sağlama işlemini dağıtır. Varsayılan olarak, her 30 saniyede uygulama sağlama işlemi çalıştırır.
- *Platform sağlama* web platformu ürünleri ve sunucu grubundaki tüm ikincil sunucuları birincil sunucudan bileşenlerin eşitler işlemidir. ASP.NET MVC 3 birincil hazırlama sunucusunda yüklerseniz, örneğin, platform sağlama işlemini Web Platformu yükleyicisi tüm ikincil hazırlama sunucularınız üzerinde ASP.NET MVC 3 yüklemek için kullanır. Varsayılan olarak, platform sağlama işlemini beş dakikada bir çalışır.

Temel uygulama ve platform sağlama ayarları WFF denetleyicisi sunucunuzda IIS Yöneticisi'nden yönetebilirsiniz.

**Uygulama ve platform ayarları sağlama keşfedin**

1. IIS Yöneticisi'nde, **bağlantıları** bölmesinde, sunucu grubunuzu seçin.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image9.png)
2. İçinde **sunucu grubu** bölmesinde çift **uygulama sağlama**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image10.png)
3. Gördüğünüz gibi sunucu grubu şu anda her 30 saniyede birincil sunucunun ve ikincil sunucular arasında web içeriği ve yapılandırma ayarlarını eşitlemek için yapılandırılır.
4. Tıklayın **geri**ve çift tıklatarak **Platform sağlama**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image11.png)
5. Gördüğünüz gibi sunucu grubu şu anda web platformu ürünlerini ve bileşenleri birincil sunucunun ve ikincil sunucular arasında beş dakikada bir eşitlemek için yapılandırılır.
6. Tıklayın **geri**.
7. Sunucu grubu, Web Platformu ürünlerini hemen eşitlemek için zorlama içinde **eylemleri** bölmesinde tıklayın **Platform sağla**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image12.png)

    > [!NOTE]
    > Platform sağlama biraz zaman alabilir. Yükleyici işleminin ikincil sunucularda sunucu grubunuzu arka planda çalışır.
8. Sağlama işleminin tamamlanması için yeterli zaman izin verdik sonra ürün ve birincil sunucuya eklenmiş bileşenleri artık ikincil sunucularda çoğaltılmış olduğunu doğrulayabilirsiniz. Örneğin, bir ikincil sunucuya oturum açabilir ve kullanabilir **Sunucu Yöneticisi'ni** penceresinde web sunucusu rolü yüklü olduğunu doğrulayın.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image13.png)
9. Ayrıca, çeşitli web platformu bileşenlerini eklendiğini doğrulamak için yüklü programların listesini kontrol edebilirsiniz.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image14.png)

## <a name="configure-load-balancing"></a>Yük dengelemeyi yapılandırma

Bir web grubu oluşturduğunuzda, HTTP istekleri, web sunucuları arasında dağıtmak için yük dengelemeyi çeşit ayarlamanız gerekir. Bu, Windows Server 2008 Ağ Yükü Dengeleme, IIS ARR ya da üçüncü taraf yazılım veya donanım tabanlı yük dengeleme çözümü olabilir.

WFF yakından IIS arr ile tümleştirmek için tasarlanmıştır Bu tümleştirme yararlanmak için ARR modülü WFF denetleyicisi sunucusuna yüklemeniz gerekir. Ardından tüm, doğrudan web trafiği denetleyicisi sunucusuna genellikle etki alanı adı sistemi (DNS) kayıtlarınızı yapılandırarak. Denetleyici sunucusu ardından gelen istekler sunucu kullanılabilirliği ve çeşitli diğer ölçütlere göre grubunuzdaki bağlı olarak, sunucular arasında dağıtır.

> [!NOTE]
> ARR ile WFF kullanmak zorunda değilsiniz; WFF üçüncü taraf Yük Dengeleme çözümlerinde çalışacak şekilde yapılandırabilirsiniz. Daha fazla bilgi için [IIS 7 için Web grubu çerçevesi 2.0 genel bakış](https://go.microsoft.com/?linkid=9805126).


ARR kullanarak Yük Dengeleme, bu öğreticinin kapsamı dışındadır en olduğu karmaşık bir konu olduğu. Ancak, sonraki yordama ARR modülünü yüklemek ve Yük Dengeleme ile başlamak için kullanabilirsiniz.

**WFF denetleyicisi sunucusuna yük dengelemeyi ayarlamak için**

1. WFF denetleyicisi sunucusuna Web Platformu Yükleyicisi'ni başlatın.
2. Üst kısmındaki **Web Platformu yükleyicisi 3.0** penceresinde tıklayın **ürünleri**.
3. Gezinti bölmesinde, pencerenin sol tarafındaki tıklayarak **sunucu**.
4. İçinde **uygulama isteği yönlendirme 2.5** satır, tıklayın **Ekle**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image15.png)
5. Tıklayın **yükleme**ve ardından yönergeleri izleyin **Web Platformu yüklemesi** penceresi.
6. Yükleme tamamlandığında, IIS Yöneticisi'ni başlatın ve **bağlantıları** bölmesinde, sunucu grubu düğümünü tıklatın. Çeşitli yeni simgeler eklenmiş bildirimi **sunucu grubu** bölmesi.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image16.png)
7. İçinde **sunucu grubu** bölmesinde çift **Yük Dengeleme**.
8. İçinde **Yük Dengeleme** bölmesinde, bir Yük Dengeleme algoritması (örneğin, **az geçerli istek**).

    > [!NOTE]
    > Yük Dengeleme algoritmaları ve diğer yapılandırma ayarları hakkında daha fazla bilgi için bkz. [uygulama isteği yönlendirme Modülü](https://go.microsoft.com/?linkid=9805130).

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image17.png)
9. İçinde **eylemleri** bölmesinde tıklayın **Uygula**.

Sunucu grubunuzu sunucuları için temel yük dengelemeyi yapılandırdınız. Tüm web grubu trafiğiniz denetleyicisi sunucusuna doğrudan, istekleri seçtiğiniz Yük Dengeleme algoritmasını ve kullanılabilirlik göre grubunuzdaki sunucuları arasında dağıtılır.

ARR ile yük dengelemeyi yapılandırma hakkında daha fazla bilgi için bkz. [uygulama isteği yönlendirme Modülü](https://go.microsoft.com/?linkid=9805130).

## <a name="monitor-the-server-farm"></a>Sunucu grubu izleme

Herhangi bir zamanda IIS Yöneticisi'ni denetleyici sunucusunda sunucu grubunuzu durumunu izleyebilirsiniz. İçinde **bağlantıları** bölmesinde, sunucu grubunuzu genişletin ve ardından **sunucuları**. Orta bölmeye son etkinliğin izleme günlüğü birlikte gruptaki her sunucunun bir özeti gösterilir.

![](creating-a-server-farm-with-the-web-farm-framework/_static/image18.png)

## <a name="conclusion"></a>Sonuç

WFF sunucu grubunuzu artık ve çalışıyor olması gerekir. Tercih ettiğiniz dağıtım yaklaşımı desteklemek için birincil sunucuyu yapılandırabilirsiniz&#x2014;daha fazla bilgi için ayrıntıları bakın&#x2014;ve sunucu grubundaki her ikincil sunucuda yapılandırmanızı çoğaltılır.

## <a name="further-reading"></a>Daha Fazla Bilgi

Tüm yönlerini yapılandırma ve WFF kullanma hakkında daha fazla rehberlik için bkz [IIS 7 için Microsoft Web grubu çerçevesi 2.0](https://go.microsoft.com/?linkid=9805129) Web sitesi.

> [!div class="step-by-step"]
> [Önceki](configuring-a-database-server-for-web-deploy-publishing.md)
> [İleri](configuring-deployment-properties-for-a-target-environment.md)
