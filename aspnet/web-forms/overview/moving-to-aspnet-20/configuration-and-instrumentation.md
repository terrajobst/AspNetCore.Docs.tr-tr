---
uid: web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
title: Yapılandırma ve izleme | Microsoft Docs
author: microsoft
description: Yapılandırma önemli değişiklikler ve ASP.NET 2.0 araçları vardır. Yeni ASP.NET yapılandırma API çekme isteği yapılacak yapılandırma değişikliklerini sağlar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 21ebbaee-7ed8-45ae-b6c1-c27c88342e48
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
msc.type: authoredcontent
ms.openlocfilehash: dc4e75e8c97228bf14935d6bf4242a036d513816
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399311"
---
<a name="configuration-and-instrumentation"></a>Yapılandırma ve izleme
====================
tarafından [Microsoft](https://github.com/microsoft)

> Yapılandırma önemli değişiklikler ve ASP.NET 2.0 araçları vardır. Yeni ASP.NET yapılandırma API yapılandırma değişikliklerini programlı bir şekilde yapılmasına olanak sağlar. Ayrıca, birçok yeni yapılandırma ayarlarını kayıtlı yeni yapılandırmalar ve izleme için izin verilir.


Yapılandırma önemli değişiklikler ve ASP.NET 2.0 araçları vardır. Yeni ASP.NET yapılandırma API yapılandırma değişikliklerini programlı bir şekilde yapılmasına olanak sağlar. Ayrıca, birçok yeni yapılandırma ayarlarını kayıtlı yeni yapılandırmalar ve izleme için izin verilir.

Bu modülde ASP.NET API configuration okuma ve yazma için ASP.NET yapılandırma dosyaları ilgilidir ve biz de ASP.NET izleme kapsar olarak ele alınacaktır. Biz de ASP.NET izleme yeni özellikler ele alınacaktır.

## <a name="aspnet-configuration-api"></a>ASP.NET yapılandırma API'si

ASP.NET yapılandırma API'si, geliştirme, dağıtma ve uygulama yapılandırma verileri tek bir programlama arabirimi kullanarak yönetmek sağlar. API configuration geliştirin ve ASP.NET yapılandırmaları tamamlayın, program aracılığıyla doğrudan yapılandırma dosyalarını XML düzenleme olmadan değiştirmek için kullanabilirsiniz. Ayrıca, yapılandırma API'si konsol uygulamaları ve geliştirdiğiniz betikler, Web tabanlı yönetim araçları ve Microsoft Yönetim Konsolu (MMC) ek bileşenleri kullanabilirsiniz.

Aşağıdaki iki yapılandırma yönetimi araçları, API configuration kullanın ve .NET Framework sürüm 2.0 ile dahil edilir:

- ASP.NET MMC ek bileşeni yönetim görevlerini basitleştirmek için API configuration kullanan, tüm yapılandırma hiyerarşi düzeylerini yerel yapılandırma verilerinden tümleşik bir görünümünü sağlar.
- Yerel ve uzak uygulamalar için yapılandırma ayarlarını yönetmenize olanak sağlar, Web sitesi yönetim aracı barındırılan siteler dahil olmak üzere.

ASP.NET yapılandırma API'si ASP.NET Yönetim program aracılığıyla Web sitelerini ve uygulamaları yapılandırmak için kullanabileceğiniz bir nesne içerir. Yönetim nesneleri, bir .NET Framework sınıf kitaplığı uygulanır. API configuration programlama modeli, özel olarak derleme zamanında veri türleri zorlayarak kod tutarlılık ve güvenilirlik sağlamaya yardımcı olur. Uygulama yapılandırmaları yönetmeyi kolaylaştırmak için API configuration yapılandırma hiyerarşideki tüm noktalarından ayrı koleksiyonlar farklı olarak veri görüntülemek yerine tek bir koleksiyon olarak birleştirilir veri görüntülemenize olanak sağlar yapılandırma dosyaları. Ayrıca, API configuration doğrudan yapılandırma dosyalarını XML düzenleme olmadan tüm uygulama yapılandırmalarını yönetmenize olanak sağlar. Son olarak, API, Web sitesi yönetim aracı gibi Yönetimsel Araçlar destekleyerek yapılandırma görevleri basitleştirir. API configuration dağıtım yapılandırma dosyaları bir bilgisayarda oluşturulmasını destekleyen ve birden fazla bilgisayara yapılandırma betiklerini çalıştırarak basitleştirir.

> [!NOTE]
> API configuration IIS uygulamaları oluşturmayı desteklemez.


## <a name="working-with-local-and-remote-configuration-settings"></a>Yerel ve uzak yapılandırma ayarları ile çalışma

Bir yapılandırma nesnesi, bir bilgisayar gibi belirli bir fiziksel varlık veya bir uygulama veya bir Web sitesi gibi mantıksal bir varlığı geçerli yapılandırma ayarları birleştirilmiş görünümünü temsil eder. Belirtilen mantıksal varlık, yerel bilgisayarda veya uzak bir sunucuda bulunabilir. Belirtilen bir varlık için herhangi bir yapılandırma dosyası mevcut olduğunda, yapılandırma nesnesini varsayılan yapılandırma ayarlarından Machine.config dosyası tarafından tanımlanan temsil eder.

Aşağıdaki sınıflar açık yapılandırma yöntemlerinden birini kullanarak bir yapılandırma nesnesi alabilirsiniz:

1. Varlığınız bir istemci uygulaması ise ConfigurationManager sınıfı.
2. Varlığınız bir Web uygulamasıysa WebConfigurationManager sınıfı.

Bu yöntemler sırayla gerekli yöntemleri ve temel alınan yapılandırma dosyalarını işlemek için özellikler sağlayan bir yapılandırma nesnesi döndürür. Okuma veya yazma için bu dosyalara erişebilirsiniz.

### <a name="reading"></a>Okuma

Yapılandırma bilgilerinin okunacağı GetSection veya GetSectionGroup yöntemi kullanın. Kullanıcı veya okuyan işlem yapılandırma dosyalarını hiyerarşideki tüm okuma izinlerine sahip olmalıdır.

> [!NOTE]
> Bir yolu parametresi alan statik bir GetSection yöntemi kullanırsanız, yol parametresi kodun çalıştığı uygulama başvurmalıdır. Aksi takdirde, bu parametre yoksayılır ve şu anda çalışan bir uygulama için yapılandırma bilgilerini döndürülür.


### <a name="writing"></a>Yazma

Yapılandırma bilgilerini yazmak için bu kaydetme yöntemlerden birini kullanın. Bir kullanıcı ya da yazma gereken işlem yazma izinleri yapılandırma hiyerarşi düzeyi geçerli dizinde ve yapılandırma dosyası, hem de okuma izinleri yapılandırma dosyalarını hiyerarşideki tüm.

Belirtilen varlık için devralınan yapılandırma ayarlarını temsil eden bir yapılandırma dosyası oluşturmak için aşağıdaki yapılandırma Kaydet yöntemlerden birini kullanın:

1. Yeni bir yapılandırma dosyası oluşturmak için Kaydet'e yöntemi.
2. Başka bir konumda yeni bir yapılandırma dosyası oluşturmak için Farklı Kaydet yöntemi.

## <a name="configuration-classes-and-namespaces"></a>Yapılandırma sınıfları ve ad alanları

Birçok yapılandırma sınıflar ve yöntemler birbirine benzerdir. Aşağıdaki tabloda, en sık kullanılan yapılandırma sınıfları ve ad alanlarını açıklar.

| **Yapılandırma sınıfı veya ad alanı** | **Açıklama** |
| --- | --- |
| [System.Configuration](https://msdn.microsoft.com/library/system.configuration.aspx) ad alanı | Tüm .NET Framework uygulamaları için ana yapılandırma sınıfları içerir. Bölümü işleyici sınıfları, yöntemleri, GetSection ve GetSectionGroup gibi bir bölümü için yapılandırma verilerini almak için kullanılır. Bu iki yöntem statik olmayan. |
| System.Configuration.Configuration sınıfı | Yapılandırma verilerini bir bilgisayara, uygulama, Web dizini ya da başka bir kaynak kümesini temsil eder. Bu sınıf yapılandırma ayarları güncelleştirilirken ve bölümler ve bölüm grupları başvuruları elde etmek için kullanışlı yöntemler, GetSection ve GetSectionGroup, gibi içerir. Bu sınıf, WebConfigurationManager ve ConfigurationManager sınıflarının yöntemleri gibi tasarım zamanı yapılandırma verilerini elde yöntemleri için dönüş türü kullanılır. |
| System.Web.Configuration ad alanı | ASP.NET yapılandırma bölümlerini tanımlanan bölüm işleyici sınıflarını içerir [ASP.NET yapılandırma ayarlarını](https://msdn.microsoft.com/library/b5ysx397.aspx). Bölümü işleyici sınıfları, yöntemleri, GetSection ve GetSectionGroup gibi bir bölümü için yapılandırma verilerini almak için kullanılır. |
| System.Web.Configuration.WebConfigurationManager class | Çalışma zamanı ve tasarım zamanı yapılandırma ayarlarına yapılan başvuruları alma için kullanışlı yöntemler sağlar. Bu yöntemler bir dönüş türü olarak System.Configuration.Configuration sınıfını kullanın. Bu sınıfın statik GetSection yöntemi veya System.Configuration.ConfigurationManager sınıfının statik olmayan GetSection yöntemi birbirinin yerine kullanabilirsiniz. Web uygulama yapılandırmaları için System.Web.Configuration.WebConfigurationManager sınıfı yerine System.Configuration.ConfigurationManager sınıfı önerilir. |
| [System.Configuration.Provider](https://msdn.microsoft.com/library/system.configuration.provider.aspx) ad alanı | Özelleştirme ve genişletme yapılandırma sağlayıcısı için bir yol sağlar. Yapılandırma sistemdeki tüm sağlayıcısı sınıfları için temel sınıf budur. |
| [System.Web.Management](https://msdn.microsoft.com/library/system.web.management.aspx) ad alanı | Sınıfları ve arabirimleri yönetmek ve Web uygulamalarının durumunu izlemek için içerir. NET olarak söylemek gerekirse, bu ad alanı API yapılandırmasının bir parçası olarak kabul edilmez. Örneğin, izleme ve olay tetikleme gerçekleştirilir bu ad alanındaki sınıflar tarafından. |
| [System.Management.Instrumentation](https://msdn.microsoft.com/library/system.management.instrumentation.aspx) ad alanı | Gerekli yönetim bilgilerini ve olası tüketicileri olaylara Windows Yönetim Araçları (WMI) aracılığıyla kullanıma sunmak için uygulama izleme için sınıflar sağlar. ASP.NET durum izleme olayları teslim etmek için WMI kullanır. NET olarak söylemek gerekirse, bu ad alanı API yapılandırmasının bir parçası olarak kabul edilmez. |

## <a name="reading-from-aspnet-configuration-files"></a>ASP.NET yapılandırma dosyaları okuma

ASP.NET yapılandırma dosyaları okuma için temel sınıf WebConfigurationManager sınıftır. ASP.NET yapılandırma dosyaları okuma temelde üç adım vardır:

1. OpenWebConfiguration yöntemini kullanarak bir yapılandırma nesnesi Al.
2. İstenen bölüm yapılandırma dosyasındaki bir başvuru alın.
3. İstenen bilgilerin, yapılandırma dosyasından okuyun.

Yapılandırma nesnesini temsil eder, belirli bir yapılandırma dosyası göstermiyor. Bunun yerine, bir bilgisayar, uygulama veya Web sitesi yapılandırmasını birleştirilmiş bir görünümünü temsil eder. Aşağıdaki kod örneği, adında bir Web uygulama yapılandırmasını temsil eden bir yapılandırma nesnesi oluşturur *ifadesini*.

[!code-csharp[Main](configuration-and-instrumentation/samples/sample1.cs)]

> [!NOTE]
> /ProductInfo yol mevcut değilse, yukarıdaki kod machine.config dosyasında belirtildiği gibi varsayılan yapılandırmayı geri bildirdiğine dikkat edin.


Yapılandırma nesnesini oluşturduktan sonra yapılandırma ayarlarını gözden geçirmeye GetSection veya GetSectionGroup yöntemi kullanabilirsiniz. Aşağıdaki örnek, yukarıdaki ifadesini uygulaması için kimliğe bürünme ayarları için bir başvuru alır:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample2.cs)]

## <a name="writing-to-aspnet-configuration-files"></a>ASP.NET yapılandırma dosyaları yazma

Yapılandırma dosyaları okuma olduğu gibi Asp.NET yapılandırma dosyasına yazmak için çekirdek WebConfigurationManager sınıftır. ASP.NET yapılandırma dosyaları için yazma için üç adım vardır.

1. OpenWebConfiguration yöntemini kullanarak bir yapılandırma nesnesi Al.
2. İstenen bölüm yapılandırma dosyasındaki bir başvuru alın.
3. İstenen bilgilerin Kaydet veya farklı kaydet kullanarak yapılandırma dosyasından yazma yöntemi.

Aşağıdaki kod değişiklikleri **hata ayıklama** özniteliği &lt;derleme&gt; false öğesi:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample3.cs)]

Bu kod yürütüldüğünde, **hata ayıklama** özniteliği &lt;derleme&gt; öğesi için yanlış ayarlanacak *webApp* uygulamanın web.config dosyasında.

## <a name="systemwebmanagement-namespace"></a>System.Web.Management Namespace

Sınıfları ve arabirimleri yönetmek ve ASP.NET uygulamalarının durumunu izlemek için System.Web.Management ad alanı sağlar.

Günlük olayları bir sağlayıcı ile ilişkilendiren bir kural tanımlama tarafından gerçekleştirilir. Kural sağlayıcısına gönderilen olayların türünü tanımlar. Aşağıdaki temel olayları oturum için kullanılabilir:

| **WebBaseEvent** | Tüm olayları temel olay sınıfı. Gerekli içeren olay kodu, Olay Ayrıntısı kodu, tarih ve saat olayı tetiklendi, gibi tüm olayları özelliklerini sıra numarası, olay iletisi ve Olay Ayrıntıları. |
| --- | --- |
| **WebManagementEvent** | Uygulama yaşam süresi, istek, hata ve denetim olayları gibi yönetim olayları temel olay sınıfı. |
| **WebHeartbeatEvent** | Yararlı bir çalışma zamanı durum bilgilerini yakalamak için düzenli aralıklarla uygulama tarafından üretilen olay. |
| **WebAuditEvent** | Yetkilendirme başarısız oldu, şifre çözme hatası gibi koşullar işaretlemek için kullanılan güvenlik denetim olaylarını için temel sınıf *vs.* |
| **WebRequestEvent** | Tüm bilgi isteği olayları için temel sınıf. |
| **WebBaseErrorEvent** | Hata koşulları belirten tüm olaylar için taban sınıf. |

Kullanılabilir sağlayıcıları türlerini olay çıkış için Olay Görüntüleyicisi, SQL Server, Windows Yönetim Araçları (WMI) ve e-posta göndermek izin verin. Olay eşlemeleri ve önceden yapılandırılmış sağlayıcıları günlüğe olay çıktısını almak gerekli çalışma miktarını azaltın.

Başlatma ve durdurma yanı işlenmeyen özel durumların günlüğe kaydetme uygulama etki alanlarını temel alan olayları günlüğe kaydetmek, olay günlüğü sağlayıcısı,-hazır ASP.NET 2.0 kullanır. Bu, bazı temel senaryolar kapsayacak şekilde yardımcı olur. Örneğin, uygulamanızın bir istisna atarsa, ancak kullanıcı hatası kaydetmez ve yeniden oluşturulamıyor varsayalım. Varsayılan olay günlüğü kural ile hangi türde bir hata oluştu, daha iyi bir fikir edinmek için özel durum ve yığın bilgileri toplamak mümkün olacaktır. Başka bir örnek uygulama, oturum durumunu kaybetmeden geçerlidir. Bu durumda, uygulama etki alanı geri dönüştürme ve uygulama etki alanı ilk başta neden durduruldu olup olmadığını belirlemek için olay günlüğüne göz atabilirsiniz.

Ayrıca, sistem durumu izleme sistemi, genişletilebilir. Örneğin, özel Web olayları tanımlamak, bunları uygulamanızda yangın ve e-posta adresiniz gibi bir sağlayıcı için olay bilgileri göndermek için kural tanımlayabilirsiniz. Bu, sistem durumu izleme sağlayıcıları için izleme kolayca bağlamanın sağlar. Başka bir örnek olarak, her zaman bir sipariş işlenir ve her bir olay gönderir SQL Server veritabanına bir kuralı ayarlama bir olay harekete. Ayrıca, bir satırda birden çok kez oturum açmak ve e-posta tabanlı sağlayıcılar kullanmak için olay ayarlamak bir kullanıcı başarısız olduğunda olay harekete.

Yapılandırma olayları ve varsayılan sağlayıcıları için genel Web.config dosyasında depolanır. Genel Web.config dosyasına ASP.NET 1 Machine.config dosyasında depolanan tüm Web tabanlı ayarları depolar x. Genel Web.config dosyasını şu dizinde bulunur:

`%windir%\Microsoft.Net\Framework\v2.0.*\config\Web.config`

&lt;Ögesi&gt; Genel Web.config dosyasının varsayılan yapılandırma ayarlarını sağlar. Bu ayarı geçersiz kılmak veya kendi ayarlarını uygulayarak yapılandırma &lt;ögesi&gt; uygulamanız için Web.config dosyasının bir bölümünde.

&lt;Ögesi&gt; Genel Web.config dosyasının aşağıdaki öğeleri içerir:

| **sağlayıcıları** | Olay Görüntüleyicisi, WMI ve SQL Server için ayarlanan sağlayıcıları içerir. |
| --- | --- |
| **eventMappings** | Eşlemeler için çeşitli WebBase sınıfları içerir. Kendi olay sınıfı oluşturursanız, bu listeyi genişletebilirsiniz. Kendi olay sınıfı oluşturma daha iyi tanecikli üzerinden bilgi göndermek sağlayıcılar sunar. Örneğin, SQL Server için kendi özel olaylar için e-posta gönderilirken gönderilmek üzere işlenmeyen özel durumlar yapılandırabilirsiniz. |
| **kuralları** | Bağlantı sağlayıcısına eventMappings. |
| **arabelleğe alma** | SQL Server ve e-posta sağlayıcıları ile nasıl genellikle sağlayıcıya olayları temizleme belirlemek için kullanılır. |

Genel Web.config dosyasındaki bir kod örneği aşağıda verilmiştir.

[!code-xml[Main](configuration-and-instrumentation/samples/sample4.xml)]

## <a name="how-to-store-events-to-event-viewer"></a>Olay Görüntüleyicisi olayları depolamak nasıl

Sağlayıcı olayları günlüğe kaydetmeyi için daha önce belirtildiği Olay Görüntüleyicisi sizin için genel Web.config dosyasında yapılandırılmış. Varsayılan olarak, tüm olayları temel alarak **WebBaseErrorEvent** ve **WebFailureAuditEvent** kaydedilir. Olay günlüğüne ek bilgi için ek kuralları ekleyebilirsiniz. Örneğin, tüm olayları günlüğe kaydetmek isterseniz (*yani*, her olay temelinde **değeri**), aşağıdaki kural Web.config dosyasına ekleyebilirsiniz:

[!code-xml[Main](configuration-and-instrumentation/samples/sample5.xml)]

Bu kural bağlamanız **tüm olayları** olay eşlemesi için olay günlüğü sağlayıcısı. EventMapping hem sağlayıcı Genel Web.config dosyasına dahil edilir.

## <a name="how-to-store-events-to-sql-server"></a>SQL Server için olayları depolamak nasıl

Bu yöntemde **ASPNETDB** ASP.NET tarafından oluşturulan veritabanı\_regsql.exe aracı. Varsayılan sağlayıcıyı uygulamada ya da bir dosya tabanlı veritabanı'nı kullanan LocalSqlServer bağlantı dizesi kullanır,\_veri klasörü veya yerel bir SQLExpress örneği SQL Server. Hem değer LocalSqlServer bağlantı dizesi hem de SqlProvider Genel Web.config dosyasında yapılandırılır.

Genel Web.config dosyasında LocalSqlServer bağlantı dizesi şöyle görünür:

[!code-xml[Main](configuration-and-instrumentation/samples/sample6.xml)]

Başka bir SQL Server örneğini kullanmak istiyorsanız, ASP.NET kullanmanız gerekecektir\_% windir%\Microsoft.Net\Framework\v2.0. bulunabilir regsql.exe aracı\* klasör. ASP.NET kullanmak\_regsql.exe araç'özel oluşturmak için **ASPNETDB** veritabanı SQL Server örneğinde sonra bağlantı dizesini uygulama yapılandırma dosyasına ekleyin ve ardından yeni kullanarak bir sağlayıcı Ekle bağlantı dizesi. Yapılandırmasını tamamladıktan **ASPNETDB** oluşturulmuş bir veritabanı için sqlProvider bir eventMapping bağlamak için bir kural kümesi gerekir.

Varsayılan SqlProvider kullanın veya kendi sağlayıcınızı yapılandırın olsun, bağlama sağlayıcı bir olay eşlemesi ile bir kural eklemek üzere gerekir. Aşağıdaki kural için yukarıda oluşturduğunuz yeni sağlayıcı bağlantıları **tüm olayları** olay eşlemesi. Bu kural bağlı olan tüm olayları günlüğe kaydeder **değeri** ve MYASPNETDB bağlantı dizesini kullanan MySqlWebEventProvider gönderin. Aşağıdaki kod, bir olay eşlemesi sağlayıcıyla bağlamak için bir kural ekler:

[!code-xml[Main](configuration-and-instrumentation/samples/sample7.xml)]

Yalnızca SQL Server hataları göndermek istiyorsanız, aşağıdaki kural ekleyebilirsiniz:

[!code-xml[Main](configuration-and-instrumentation/samples/sample8.xml)]

## <a name="how-to-forward-events-to-wmi"></a>WMI olayları nasıl

Ayrıca, olayları için WMI iletebilirsiniz. WMI sağlayıcısı, genel Web.config dosyasında varsayılan olarak yapılandırılır.

Aşağıdaki kod örneği için WMI olayları iletmek için bir kural ekler:

[!code-xml[Main](configuration-and-instrumentation/samples/sample9.xml)]

Sağlayıcı ve olaylarını dinleyecek şekilde WMI dinleyici uygulamasına bir eventMapping ilişkilendirmek için bir kuralı eklemeniz gerekir. Aşağıdaki kod örneği WMI sağlayıcısına bağlamak için bir kural ekler **tüm olayları** olay eşlemesi:

[!code-xml[Main](configuration-and-instrumentation/samples/sample10.xml)]

## <a name="how-to-forward-events-to-email"></a>E-posta olayları nasıl

Ayrıca, e-posta olayları iletebilirsiniz. SQL Server veya olay günlüğü için daha uygun istemeden kendiniz birçok bilgi, gönderebilirsiniz gibi e-posta sağlayıcınız için harita hangi olay kuralları olabilir dikkatli olun. İki e-posta sağlayıcıları vardır; SimpleMailWebEventProvider ve TemplatedMailWebEventProvider. Her ikisi de yalnızca TemplatedMailWebEventProvider üzerinde kullanılabilir olan "Şablon" ve "detailedTemplateErrors" öznitelikleri dışında aynı configuration öznitelikleri vardır.

> [!NOTE]
> Bu e-posta sağlayıcılarının hiçbirine sizin için yapılandırılır. Web.config dosyanızı eklemeniz gerekir.


Bu iki e-posta sağlayıcıları arasındaki temel fark, SimpleMailWebEventProvider e-postaları değiştirilemez bir genel şablon göndermesidir. Örnek Web.config dosyası, aşağıdaki kural kullanarak, yapılandırılmış sağlayıcı listesine bu e-posta sağlayıcısı ekler:

[!code-xml[Main](configuration-and-instrumentation/samples/sample11.xml)]

E-posta sağlayıcısı için bağlamak için de aşağıdaki kural eklenir **tüm olayları** olay eşlemesi:

[!code-xml[Main](configuration-and-instrumentation/samples/sample12.xml)]

## <a name="aspnet-20-tracing"></a>ASP.NET 2.0 izleme

Üç önemli geliştirmeler, ASP.NET 2.0 izleme vardır.

1. Tümleşik izleme işlevi
2. İzleme iletileri için programlı erişim
3. Gelişmiş uygulama düzeyinde izleme

## <a name="integrated-tracing-functionality"></a>Tümleşik izleme işlevselliği

Şimdi, ASP.NET çıktı izleme System.Diagnostics.Trace sınıfı tarafından yayılan iletileri yönlendirmek ve System.Diagnostics.Trace ASP.NET izleme tarafından yayılan iletiler yönlendirebilirsiniz. Ayrıca, System.Diagnostics.Trace için ASP.NET ölçümlü izleme olaylarında iletebilirsiniz. Bu işlev yeni tarafından sağlanır **writeToDiagnosticsTrace** özniteliği &lt;izleme&gt; öğesi. Bu Boolean değeri true olduğunda, ASP.NET izleme iletilerini System.Diagnostics izleme altyapısına kullanmak için izleme iletilerini görüntülemek için kaydedilen herhangi bir dinleyicileri tarafından iletilir.

## <a name="programmatic-access-to-trace-messages"></a>İzleme iletileri için programlı erişim

ASP.NET 2.0 tüm izleme iletilerini programlı erişim sağlar **TraceContextRecord** sınıfı ve **TraceRecords** koleksiyonu. İzleme iletileri erişmenin en verimli şekilde kaydedilecek olan bir **TraceContextEventHandler** temsilci (aynı zamanda ASP.NET 2.0 sürümünde yeni) yeni işlemek için **TraceFinished** olay. İstediğiniz gibi sonra izleme iletilerini döngüye girer.

Aşağıdaki kod örneği bunu göstermektedir:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample13.cs)]

Yukarıdaki örnekte, ı TraceRecords toplulukta döngü ve ardından her ileti yanıt akışına yazmak.

## <a name="improved-application-level-tracing"></a>Gelişmiş uygulama düzeyinde izleme

Uygulama düzeyinde izleme yeni giriş geliştirilmiş **mostRecent** özniteliği &lt;izleme&gt; öğesi. Bu öznitelik, en son uygulama düzeyinde izleme çıktısı görüntülenir ve eski izleme verilerinin requestLimit tarafından belirtilen sınırları aşan atılır olup olmadığını belirtir. RequestLimit öznitelik ulaşılana kadar yanlışsa, istekleri izleme verileri görüntülenir.

## <a name="aspnet-command-line-tools"></a>ASP.NET komut satırı araçları

ASP.NET yapılandırmada yardımcı olmak için çeşitli komut satırı araçları vardır. ASP.NET geliştiricilerine ASP.NET ile tanıdık\_regiis.exe aracı. ASP.NET 2.0 yapılandırmasında yardımcı olması için üç diğer komut satırı araçları sağlar.

Aşağıdaki komut satırı araçları mevcuttur:

| **Aracı** | **Kullanın** |
| --- | --- |
| **aspnet\_regiis.exe** | ASP.NET IIS Kayıt olanak tanır. İki sürümü vardır, bu araçları biri (Framework klasöründe) 32-bit sistemler için diğeri (klasöründe Framework64.) 64 bit sistemler için ASP.NET 2.0 birlikte sevk Bir 32-bit işletim sisteminde 64-bit sürümü yüklü değil. |
| **aspnet\_regsql.exe** | ASP.NET SQL Server kayıt aracı, ASP.NET, SQL Server sağlayıcıları tarafından kullanılmak üzere bir Microsoft SQL Server veritabanı oluşturmak veya eklemek veya varolan bir veritabanından seçenekleri kaldırmak için kullanılır. ASP.NET\_regsql.exe dosya [drive:]\WINDOWS\Microsoft.NET\Framework\versionNumber klasöründe Web sunucunuzda bulunur. |
| **aspnet\_regbrowsers.exe** | ASP.NET tarayıcı kayıt aracını ayrıştırır ve tüm sistem genelinde tarayıcı tanımlarını bir derleme içine derler ve derlemeyi genel bütünleştirilmiş kod önbelleğine yükler. Aracı'nı tarayıcı tanım dosyalarını kullanır (. Tarayıcı dosyaları) .NET Framework tarayıcılar alt dizinden. Aracı %SystemRoot%\Microsoft.NET\Framework\version\ dizininde bulunabilir. |
| **aspnet\_compiler.exe** | ASP.NET derleme aracı, yerinde veya dağıtım bir üretim sunucusu gibi bir hedef konum için bir ASP.NET Web uygulaması derleme olanak tanır. Yerinde derleme, uygulama derlenirken son kullanıcılar uygulamanın ilk isteğe bir gecikme karşılaşmazsınız çünkü uygulama performansını yardımcı olur. |

Çünkü aspnet\_regiis.exe aracı ASP.NET 2.0 için yeni değil, bunu burada ele alınacaktır değil.

## <a name="aspnet-sql-server-registration-tool---aspnetregsqlexe"></a>ASP.NET SQL Server kayıt aracı - aspnet\_regsql.exe

Çeşitli seçenekleri ASP.NET SQL Server kayıt aracını kullanarak ayarlayabilirsiniz. SQL Bağlantısı belirttiğinden, SQL Server ASP.NET uygulama hizmetleri bilgilerini yönetmek için hangi veritabanı veya tablo SQL önbellek bağımlılık için kullanıldığını belirtmek belirtmek ve ekleyebilir veya kaldırabilirsiniz yordamları ve oturum durumunu depolamak için SQL Server'ı kullanma desteği.

Birden çok ASP.NET uygulama hizmetleri, depolama ve bir veri kaynağından veri almanın yönetmek için bir sağlayıcı dayanır. Her bir sağlayıcı veri kaynağına özeldir. ASP.NET, SQL Server sağlayıcısı için aşağıdaki özellikleri içerir:

- Üyelik ( [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) sınıfı).
- Rol yönetimi ( [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) sınıfı).
- Profil ( [SqlProfileProvider](https://msdn.microsoft.com/library/system.web.profile.sqlprofileprovider.aspx) sınıfı).
- Web Bölümleri kişiselleştirme ( [SqlPersonalizationProvider](https://msdn.microsoft.com/library/system.web.ui.webcontrols.webparts.sqlpersonalizationprovider.aspx) sınıfı).
- Web olayları ( [SqlWebEventProvider](https://msdn.microsoft.com/library/system.web.management.sqlwebeventprovider.aspx) sınıfı).

ASP.NET yüklediğinizde, sunucunuzun Machine.config dosyasının her üzerinde sağlayıcı güvenin ASP.NET özellikleri için SQL Server sağlayıcıları belirten yapılandırma öğeleri içerir. Bu sağlayıcılar, SQL Server Express 2005'in bir yerel kullanıcı örneğine bağlanmak için varsayılan olarak yapılandırılır. Sağlayıcıları tarafından kullanılan varsayılan bağlantı dizesini değiştirirseniz, makine yapılandırmasında yapılandırılan ASP.NET özelliklerinden herhangi birini kullanmadan önce daha sonra SQL Server veritabanını ve veritabanı öğelerini Aspnetkullanarak,seçilenözelliğinyüklemenizgerekir\_regsql.exe. SQL kayıt aracıyla belirttiğiniz veritabanı zaten mevcut değilse (aspnetdb olacaktır varsayılan veritabanı bir komut satırında belirtilmezse), sonra da geçerli kullanıcı SQL şeması e oluşturmak aynı zamanda sunucu veritabanı oluşturma haklarına sahip olmalıdır bir veritabanı içinde lements.

### <a name="sql-cache-dependency"></a>SQL önbellek bağımlılığı

SQL önbellek bağımlılık ASP.NET çıktı önbelleğini, Gelişmiş bir özelliktir. SQL önbellek bağımlılık iki farklı çalışma modunu destekler: bir ASP.NET uygulaması tablo yoklama ve SQL Server 2005 sorgu bildirim özelliklerini kullanan ikinci bir modu kullanan bir. SQL kayıt aracı, işlem tablo yoklama modunu yapılandırmak için kullanılabilir.

### <a name="session-state"></a>Oturum durumu

Varsayılan olarak, oturum durum değerlerini ve bilgi bellek ASP.NET'in işlem içinde depolanır. Alternatif olarak, burada birden çok Web sunucusu tarafından paylaşılabilen bir SQL Server veritabanında oturum verilerini depolayabilir. Ardından SQL Kayıt Aracı ile oturum durumu için belirttiğiniz veritabanı zaten mevcut değilse, geçerli kullanıcı SQL Server'da bir veritabanı içinde şema öğeleri oluşturmak için de veritabanı oluşturma hakları olmalıdır. Veritabanı yoksa, ardından geçerli kullanıcının varolan bir veritabanında şema öğeleri oluşturmak için haklarınız olmalıdır.

SQL Server'da oturum durumu veritabanını yüklemek için ASP.NET çalıştırma\_regsql.exe aracı ve tedarik komutu aşağıdaki bilgilerle:

- SQL Server adını kullanarak örnek **-S** seçeneği.
- SQL Server çalıştıran bir bilgisayarda bir veritabanı oluşturma izni olan bir hesap için oturum açma kimlik bilgileri. Kullanma **-E** seçeneği şu anda oturum açmış kullanıcı veya kullanın **- U** bir kullanıcı kimliği ile birlikte belirtmek için seçeneği **-P** bir parola belirtmek için seçeneği.
- **- Ssadd** oturum durumu veritabanı eklemek için komut satırı seçeneği.

Varsayılan olarak, ASP.NET kullanamazsınız\_regsql.exe aracı oturum durumu veritabanını SQL Server 2005 Express Edition çalıştıran bir bilgisayara yükleyin.

### <a name="the-aspnet-browser-registration-tool---aspnetregbrowsersexe"></a>ASP.NET tarayıcı kayıt aracı - aspnet\_regbrowsers.exe

ASP.NET sürüm 1.1 adlı bir bölüm Machine.config dosyasının içerdiği &lt;browserCaps&gt;. Bu bölümde, bir dizi yapılandırmaları bir normal ifadeye göre çeşitli tarayıcılar için tanımlanmış XML girdi içeriyor. ASP.NET sürüm 2.0 için yeni bir. Tarayıcı dosyası XML girişleri kullanarak belirli bir tarayıcı parametrelerini tanımlar. Yeni bir ekleyerek yeni bir tarayıcı üzerinde bilgi ekleyin. Tarayıcı dosyası %SystemRoot%\Microsoft.NET\Framework\version\CONFIG\Browsers sisteminize konumundaki klasörüne.

Tarayıcı bilgileri gerektiren her zaman uygulamanın .config dosyası okunurken değil çünkü yeni oluşturabilirsiniz. Tarayıcı dosyası ve çalışma Aspnet\_regbrowsers.exe gerekli değişiklikleri derlemeye eklenecek. Bu şekilde bilgilerini seçmek için uygulamaları kapatmanız gerekmez yeni tarayıcı bilgileri hemen erişmek sağlar. Bir uygulama tarayıcı yetenekleri geçerli HTTP isteği tarayıcı özelliği üzerinden erişebilirsiniz.

ASP.NET çalıştırırken aşağıdaki seçenekler kullanılabilir\_regbrowser.exe:

| **Seçeneği** | **Açıklama** |
| --- | --- |
| **-?** | ASP.NET görüntüler\_regbbrowsers.exe komut penceresinde Yardım metni. |
| **-i** | Çalışma zamanı tarayıcı yetenekleri derlemesi oluşturur ve genel bütünleştirilmiş kod önbelleğine yükler. |
| **-u** | Çalışma zamanı tarayıcı yetenekleri derlemeyi genel bütünleştirilmiş kod önbelleğinden kaldırır. |

## <a name="the-aspnet-compilation-tool---aspnetcompilerexe"></a>ASP.NET derleme aracı - aspnet\_compiler.exe

ASP.NET derleme aracı genel iki şekilde kullanılabilir: yerinde derleme ve dağıtım için hedef çıkış dizinine burada belirtilirse, derleme için.

### <a name="compiling-an-application-in-placehttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Yerinde bir uygulama derleme](https://msdn.microsoft.com/library/ms229863.aspx)

ASP.NET derleme Aracı'nı yerinde bir uygulamayı derleyebilirsiniz; diğer bir deyişle, bu nedenle normal derleme neden uygulamasına, birden çok istek yapan davranışını taklit eder. Önceden derlenmiş sitenin kullanıcıları ilk isteği sayfasında derlenerek neden bir gecikme başlatmayla karşılaşmazsınız.

Yerinde bir siteyi önceden derlemeniz, aşağıdaki maddeler geçerlidir:

- Site, dizin yapısını ve dosyalarını korur.
- Site sunucusundaki tarafından kullanılan tüm programlama dilleri için derleyiciler olması gerekir.
- Tüm site herhangi bir dosya, derleme başarısız olursa, derleme başarısız oluyor.

Ayrıca, bir uygulama için yeni kaynak dosyaları ekledikten sonra yerinde yeniden derleyebilirsiniz. Eklemediğiniz sürece, araç yalnızca yeni veya değiştirilmiş dosyaları derler **- c** seçeneği.

> [!NOTE]
> İç içe geçmiş bir uygulama içeren bir uygulamanın derlenmesini iç içe geçmiş bir uygulama için derlenmiyor. İç içe geçmiş uygulama ayrı olarak derlenmelidir.


### <a name="compiling-an-application-for-deploymenthttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Dağıtım için bir uygulama derleme](https://msdn.microsoft.com/library/ms229863.aspx)

Bir uygulama dağıtımı (derleme hedef konuma) için targetDir parametresi belirtilerek derleyin. Daha ayrıntılı derlenmiş uygulama dağıtılabilir veya Web uygulaması için son konum targetDir olabilir. Kullanarak **-u** seçeneği derler ve derlenmiş uygulamada belirli dosyalara yeniden derlemeden değişiklik yapabilirsiniz, şekilde uygulama. ASP.NET\_compiler.exe statik ve dinamik dosya türleri arasında bir ayrım yapar ve bunları sonuçta elde edilen uygulamayı oluştururken farklı işler.

- Statik dosya türleri: olanlar değil ilişkili bir derleyici veya sağlayıcısı, ayarlanmış dosyalar gibi yapı adlandırılmış .css, .gif, .htm, .html, .jpg, .js uzantıları vb. vardır. Bu dosyalar, yalnızca göreli konumlarına korunur dizin yapısına sahip bir hedef konuma kopyalanır.
- Dinamik türden ilişkili bir derleyici veya ASP.NET özel dosya adı uzantıları .asax, .ascx, .ashx, .aspx, .browser, .master vb. gibi dosyaları dahil olmak üzere sağlayıcı, yapı olanlardır. ASP.NET derleme aracı, bu dosyalara ilişkin derlemeleri oluşturur. Varsa **-u** seçeneği atlanırsa, aracın dosya adı uzantısı ile de dosyaları oluşturur. Kendi derlemesi için orijinal kaynak dosyalarına eşleme COMPILED. Uygulama kaynak dizin yapısını korunmasını sağlamak için hedef uygulama karşılık gelen konumlarda yer tutucu dosyalar araç oluşturur.

Kullanmalısınız **-u** derlenmiş uygulama içeriğini değiştirilebilir belirtmek için seçeneği. Aksi halde, sonraki değişiklikler göz ardı edilir veya çalışma zamanı hatalarına neden olabilir.

Aşağıdaki tabloda, ne zaman ASP.NET derleme aracı tanıtıcıları farklı dosya türleri açıklanmaktadır **-u** seçeneği dahildir.

| **Dosya türü** | **Derleme eylemi** |
| --- | --- |
| .ascx, .aspx, .master | Bu dosyalar arka plan kod dosyaları hem de içine herhangi bir kod içerir, biçimlendirme ve kaynak kodu ayrılır &lt;betik runat = "server"&gt; öğeleri. Derlemeler, bir karma algoritma türetilmiş adlarla kaynak kod derlenir ve derlemeleri Bin dizinine yerleştirilir. Herhangi bir satır içi kod, kod içine arasında **&lt; %** ve **% &gt;** köşeli ayraçlar ve derlenmemiş biçimlendirme ile eklenmiştir. Kaynak dosyaları aynı ada sahip yeni dosyaların işaretleme içerecek şekilde oluşturulan ve karşılık gelen çıkış dizinler yerleştirilir. |
| .ashx, .asmx | Bu dosyalar değil derlenir ve çıktı dizini olan ve derlenmemiş taşınır. Derlenmiş işleyici kodu olmasını istiyorsanız, kaynak kodu dosyaları uygulamadaki kodu yerleştirin\_kod dizini. |
| .cs, .vb, .jsl, .cpp (daha önce listelenen dosya türleri için arka plan kod dosyaları dahil değil) | Bu dosyalar derlenmiş ve onlara başvuran derlemeleri kaynak olarak dahil. Kaynak dosyalar çıkış dizinine kopyalanmaz. Bir kod dosyası başvurulmuyorsa derlenmedi. |
| Özel dosya türleri | Bu dosyalar derlenmiş değil. Bu dosyalar, karşılık gelen çıkış dizinlere kopyalanır. |
| Kaynak kodu dosyaları uygulamasında\_kod alt dizini | Bu dosyalar derlemeleri haline getirilebilen ve Bin dizinine yerleştirilir. |
| .resx ve .resource dosyaları uygulamasında\_GlobalResources alt dizini | Bu dosyalar derlemeleri haline getirilebilen ve Bin dizinine yerleştirilir. Hiçbir uygulama\_GlobalResources alt ana çıkış dizini altında oluşturulur ve hiçbir .resx veya .resources dosyaları kaynak dizininde bulunan çıkış dizinlere kopyalanır. |
| .resx ve .resource dosyaları uygulamasında\_LocalResources alt dizini | Bu dosyalar derlenmiş değil ve karşılık gelen çıkış dizinlere kopyalanır. |
| Uygulama dosyalarında .skin\_Temalar alt dizini | .Skin dosyaları ve statik bir tema dosyası değil derlenir ve karşılık gelen çıkış dizinlere kopyalanır. |
| Derlemeleri Bin dizininde zaten mevcut .browser Web.config statik dosya türleri | Bu dosyalar için çıktı dizini olarak kopyalanır. |

Aşağıdaki tabloda, ne zaman ASP.NET derleme aracı tanıtıcıları farklı dosya türleri açıklanmaktadır **-u** seçeneği atlanırsa.

| **Dosya türü** | **Derleme eylemi** |
| --- | --- |
| .aspx, .asmx, .ashx, .master | Bu dosyalar arka plan kod dosyaları hem de içine herhangi bir kod içerir, biçimlendirme ve kaynak kodu ayrılır &lt;betik runat = "server"&gt; öğeleri. Kaynak kodu, derlemeleri, bir karma algoritma türetilmiştir adları ile derlenir. Sonuçta elde edilen derlemeler Bin dizinine yerleştirilir. Herhangi bir satır içi kod, kod içine arasında **&lt; %** ve **% &gt;** köşeli ayraçlar ve derlenmemiş biçimlendirme ile eklenmiştir. Derleyici, biçimlendirme kaynak dosyaları aynı ada sahip içerecek yeni bir dosya oluşturur. Bu sonuç dosyaları Bin dizinine yerleştirilir. Derleyici, ayrıca kaynak dosyaları aynı ada sahip ancak uzantısına sahip dosyaları oluşturur. Eşleme bilgileri içeren COMPILED. . DERLENMİŞ dosyalar kaynak dosyaların özgün konuma karşılık gelen çıktı dizini yerleştirilir. |
| .ascx | Bu dosyalar, biçimlendirme ve kaynak koduna bölünür. Kaynak kodu derlemeleri haline getirilebilen ve karma algoritma türetilmiştir adlarla Bin dizinine yerleştirilir. Hiçbir işaretleme dosyalar oluşturulur. |
| .cs, .vb, .jsl, .cpp (daha önce listelenen dosya türleri için arka plan kod dosyaları dahil değil) | Kaynak kodu, .ascx, .ashx veya .aspx dosyaları oluşturulan derlemeler tarafından başvurulan derlemeleri haline getirilebilen ve Bin dizinine yerleştirilir. Hiçbir kaynak dosya kopyalanamadı. |
| Özel dosya türleri | Bu dosyalar gibi dinamik dosyaları derlenir. Temel dosya türüne bağlı olarak, derleyici çıkışı dizinler eşleme dosyaları yerleştirebilirsiniz. |
| Uygulama dosyalarında\_kod alt dizini | Bu alt kaynak kodu dosyaları derlemeleri haline getirilebilen ve Bin dizinine yerleştirilir. |
| Uygulama dosyalarında\_GlobalResources alt dizini | Bu dosyalar derlemeleri haline getirilebilen ve Bin dizinine yerleştirilir. Hiçbir uygulama\_GlobalResources alt ana çıkış dizini altında oluşturulur. Yapılandırma dosyası appliesTo belirtiyorsa = "All".resx ve .resources dosyaları için çıktı dizini kopyalanır. Tarafından başvurulan bunlar kopyalanmaz bir [BuildProvider](https://msdn.microsoft.com/library/system.web.configuration.buildprovider.aspx). |
| .resx ve .resource dosyaları uygulamasında\_LocalResources alt dizini | Bu dosyalar benzersiz adlara sahip derlemeler içine derlenmiş ve Bin dizinine yerleştirilir. Hiçbir .resx veya .resource dosyaları için çıktı dizini kopyalanır. |
| Uygulama dosyalarında .skin\_Temalar alt dizini | Temalar derlemeleri haline getirilebilen ve Bin dizinine yerleştirilir. Saplama dosyalarını .skin dosyaları oluşturulur ve karşılık gelen çıkış dizinine yerleştirilir. Çıktı dizini için (örneğin, .css) statik dosyalar kopyalanır. |
| Derlemeleri Bin dizininde zaten mevcut .browser Web.config statik dosya türleri | Bu dosyalar olarak çıkış dizinine kopyalanır. |

### <a name="fixed-assembly-nameshttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Sabit derleme adları](https://msdn.microsoft.com/library/ms229863.aspx##)

MSI Windows Installer kullanarak bir Web uygulaması dağıtımı gibi bazı senaryolar, derlemeleri veya güncelleştirmeleri için yapılandırma ayarlarını belirlemek için tutarlı dizin yapılarını yanı sıra tutarlı dosya adları ve içerikleri kullanımını gerektirir. Bu gibi durumlarda, kullandığınız **- fixednames** ASP.NET derleme aracı derleme derlemesi gerektiğini belirtmek için seçeneği nerede kullanmak yerine her kaynak dosyası için birden çok sayfa derlemelerine derlenir. Ölçeklenebilirliğiyle endişeniz varsa bu seçeneği dikkatli kullanmanız gerekir, bu çok sayıda derlemeler için neden olabilir.

### <a name="strong-name-compilationhttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Tanımlayıcı ad derleme](https://msdn.microsoft.com/library/ms229863.aspx##)

**- Aptca**, **- delaysıgn**, **- keycontaıner** ve **- keyfile** Aspnet kullanabilmesi için seçenekleri sağlanır\_ kesin oluşturmak için compiler.exe adlı derlemeler kullanmadan [tanımlayıcı ad Aracı (Sn.exe)](https://msdn.microsoft.com/library/k5b5tt23.aspx) ayrı olarak. Bu seçenekler, sırasıyla karşılık gelen, **AllowPartiallyTrustedCallersAttribute**, **AssemblyDelaySignAttribute**, **AssemblyKeyNameAttribute**ve  **AssemblyKeyFileAttribute**.

Bu kurs kapsamı dışında tartışmadır bu öznitelikler.

## <a name="labs"></a>Laboratuvarları

Her biri aşağıdaki laboratuvarlar, önceki Laboratuvarları oluşturur. Sırada yapmanız gerekir.

## <a name="lab-1-using-the-configuration-api"></a>Laboratuvar 1: yapılandırma API'si kullanma

1. Adlı yeni bir Web sitesi oluşturma *mod9lab*.
2. Site için yeni bir Web yapılandırma dosyası ekleyin.
3. Web.config dosyasına aşağıdakileri ekleyin:


[!code-xml[Main](configuration-and-instrumentation/samples/sample14.xml)]

Bu, web.config dosyasına değişiklikleri kaydetme izniniz olduğunu garanti eder.

1. Default.aspx için yeni bir etiket denetimi ekleyin ve Kimliğine değiştirme **lblDebugStatus**.
2. Default.aspx için yeni bir düğme denetimi ekleyin.
3. Değişiklik için düğme denetiminin kimliği **btnToggleDebug** ve metni **hata ayıklama durumunu değiştir**.
4. Arka plan kod dosyasının Default.aspx kod görünümü açın ve eklemek bir **kullanarak** bildirimi **System.Web.Configuration** gibi:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample15.cs)]

1. İki özel değişkeni sınıfı ve bir sayfa ekleme\_aşağıda gösterildiği gibi Init yöntemi:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample16.cs)]

1. Aşağıdaki kod sayfasına ekleme\_yük:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample17.cs)]

1. Kaydet ve default.aspx göz atın. Etiket denetimi geçerli hata ayıklama durumunu görüntülendiğine dikkat edin.
2. Tasarımcısı'nda düğme denetimini çift tıklayın ve düğme denetimi için tıklama olayı için aşağıdaki kodu ekleyin:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample18.cs)]

1. Kaydet ve default.aspx göz atın ve düğmeye tıklayın.
2. Her düğmesine tıklayıp gözlemleyin sonra web.config dosyasını açın **hata ayıklama** özniteliğini &lt;derleme&gt; bölümü.

## <a name="lab-2-logging-application-restarts"></a>Laboratuvar 2: Uygulama yeniden başlatılmadan günlüğe kaydetme

Bu laboratuvarda, uygulama kapatma, yeni işletmeler ve yeniden Olay Görüntüleyicisi'nde günlüğe geçiş sağlayacak bir kod oluşturur.

1. Bir DropDownList için default.aspx ekleyin ve ddlLogAppEvents için kodu değiştirin.
2. Ayarlama **AutoPostBack** DropDownList'e özelliği **true**.
3. Öğe koleksiyonunun DropDownList için üç öğe ekleyin. Olun **metin** birinci öğenin *Select Value* ve -1 değeri. Olun **metin** ve **değer** ikinci öğenin **True** ve **metin** ve **değer** üçüncü öğesi **False**.
4. Yeni bir etiket için default.aspx ekleyin. Kimliği değiştirme **lblLogAppEvents**.
5. Default.aspx için arka plan kod görünümünü açın ve aşağıda gösterildiği gibi bir değişken için yeni bir bildirim türü HealthMonitoringSection ekleyin:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample19.cs)]

1. Mevcut kod sayfasında aşağıdaki kodu ekleyin\_Init:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample20.cs)]

1. Üzerinde DropDownList çift tıklayın ve SelectedIndexChanged olayı için aşağıdaki kodu ekleyin:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample21.cs)]

1. Default.aspx göz atın.
2. Açılan kümesine **False**.
3. Olay Görüntüleyicisi'ni uygulama günlüğünde temizleyin.
4. Uygulama için hata ayıklama özniteliğini değiştirmek için düğmeye tıklayın.
5. Olay Görüntüleyicisi'ni uygulama günlüğünde yenileyin. 

    1. Hiçbir olay günlüğe kaydedilen?
    2. Neden veya neden değiştirilemez?
6. Açılan kümesine **True.**
7. Uygulama için hata ayıklama öznitelik geçiş yapmak için düğmeye tıklayın.
8. Uygulama oturum açma Olay Görüntüleyicisi'ni yenileyebilirsiniz. 

    1. Hiçbir olay günlüğe kaydedilen?
    2. Uygulama kapatma nedeni neydi?
9. Açma ve kapatma günlük kaydı ile denemeler yapın ve web.config dosyasına yapılan değişiklikleri Bakılacaklar.

## <a name="more-information"></a>Daha fazla bilgi:

ASP.NET 2.0'ın sağlayıcı modeli, kendi sağlayıcıları yalnızca uygulama izleme, ancak birçok diğer kullanımlar için de üyelik, vb. profilleri oluşturmanıza olanak tanır. Bir metin dosyasına uygulama olaylarını günlüğe kaydedecek şekilde özel bir sağlayıcı yazma ile ilgili ayrıntılı bilgi için ziyaret [bu bağlantıyı](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/ASPNETProvMod_Prt6.asp).
