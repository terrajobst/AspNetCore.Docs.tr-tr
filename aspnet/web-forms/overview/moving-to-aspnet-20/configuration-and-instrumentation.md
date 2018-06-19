---
uid: web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
title: Yapılandırma ve izleme | Microsoft Docs
author: microsoft
description: Yapılandırmasındaki önemli değişiklikler ve ASP.NET 2.0 de araçları vardır. Yeni ASP.NET yapılandırma API'si pr yapılması için yapılandırma değişiklikleri sağlar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 21ebbaee-7ed8-45ae-b6c1-c27c88342e48
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
msc.type: authoredcontent
ms.openlocfilehash: 16dfe3c899dfa028d8a52b4b5f9c2868887e8fa9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2018
ms.locfileid: "28886023"
---
<a name="configuration-and-instrumentation"></a>Yapılandırma ve izleme
====================
tarafından [Microsoft](https://github.com/microsoft)

> Yapılandırmasındaki önemli değişiklikler ve ASP.NET 2.0 de araçları vardır. Yeni ASP.NET yapılandırma API'si programlı olarak yapılması için yapılandırma değişiklikleri sağlar. Ayrıca, birçok yeni yapılandırma ayarları mevcut yeni yapılandırmaları ve izleme için izin verilir.


Yapılandırmasındaki önemli değişiklikler ve ASP.NET 2.0 de araçları vardır. Yeni ASP.NET yapılandırma API'si programlı olarak yapılması için yapılandırma değişiklikleri sağlar. Ayrıca, birçok yeni yapılandırma ayarları mevcut yeni yapılandırmaları ve izleme için izin verilir.

Bu modülde ASP.NET yapılandırma API'sı olarak okuma ve yazma ASP.NET yapılandırma dosyalarını ilgilidir ve biz de ASP.NET araçları ele alınacaktır aşağıdakiler ele alınacaktır. Biz de ASP.NET izleme içinde kullanılabilen yeni özellikleri kapsar.

## <a name="aspnet-configuration-api"></a>ASP.NET yapılandırma API'si

ASP.NET yapılandırma API'si geliştirmenize, dağıtmanıza ve tek bir programlama arabirimi kullanarak uygulama yapılandırma verileri yönetmek sağlar. Yapılandırma API'si geliştirmek ve XML yapılandırma dosyalarını doğrudan düzenleyerek olmadan ASP.NET yapılandırmaları tamamlayın programlı olarak değiştirmek için kullanabilirsiniz. Ayrıca, yapılandırma API'si konsol uygulamaları ve geliştirme komut dosyaları, Web tabanlı yönetim araçları ve Microsoft Yönetim Konsolu (MMC) ek bileşenleri kullanabilirsiniz.

Aşağıdaki iki yapılandırma yönetimi araçları yapılandırma API'si kullanın ve .NET Framework sürüm 2.0 ile eklenmiştir:

- Yönetim görevlerini basitleştirmek için yapılandırma API'si kullanan ASP.NET MMC ek bileşenini, tümleşik bir görünümü tüm yapılandırma hiyerarşi düzeylerini yerel yapılandırma verilerini sağlama.
- Yerel ve uzak uygulamalar için yapılandırma ayarlarını yönetmenize olanak sağlar, Web sitesi yönetim aracı barındırılan siteleri de dahil olmak üzere.

ASP.NET yapılandırma API'si ASP.NET Yönetim Web siteleri ve uygulamaları programlı olarak yapılandırmak için kullanabileceğiniz bir nesne oluşur. Yönetim nesneleri, bir .NET Framework sınıf kitaplığı uygulanır. Yapılandırma API'si programlama modeli, derleme zamanında veri türleri zorlayarak kod tutarlılık ve güvenilirlik sağlamaya yardımcı olur. Uygulama yapılandırmaları yönetmeyi kolaylaştırmak için yapılandırma API'si, yapılandırma hiyerarşideki tüm noktalarından ayrı koleksiyonlar farklı olarak veri görüntülemek yerine tek bir koleksiyon olarak birleştirilir verileri görüntülemek izin verir yapılandırma dosyaları. Ayrıca, yapılandırma API'si XML yapılandırma dosyalarını doğrudan düzenleyerek olmadan tüm uygulama yapılandırmaları yönetmenize olanak sağlar. Son olarak, API, Web sitesi yönetim aracı gibi Yönetimsel Araçlar destekleyerek yapılandırma görevlerini basitleştirir. Yapılandırma API'si dağıtım yapılandırma dosyaları bir bilgisayarda oluşturmayı destekleyen ve birden fazla bilgisayara yapılandırma komut dosyaları çalıştırarak basitleştirir.

> [!NOTE]
> Yapılandırma API'si IIS uygulamaların oluşturulmasını desteklemiyor.


## <a name="working-with-local-and-remote-configuration-settings"></a>Yerel ve uzak yapılandırma ayarlarıyla çalışma

Bir yapılandırma nesnesi bir bilgisayar gibi belirli bir fiziksel varlık veya bir uygulama veya bir Web sitesi gibi mantıksal bir varlığı geçerli yapılandırma ayarlarını birleştirilmiş görünümünü temsil eder. Belirtilen mantıksal varlık, yerel bilgisayarda veya uzak bir sunucuda bulunabilir. Belirtilen varlık için herhangi bir yapılandırma dosyası mevcut olduğunda, yapılandırma nesnesi varsayılan yapılandırma ayarlarını Machine.config dosyası tarafından tanımlanan temsil eder.

Şu sınıflardan açık yapılandırma yöntemlerden birini kullanarak bir yapılandırma nesnesi elde edebilirsiniz:

1. Bir istemci uygulaması varlığınız olması durumunda ConfigurationManager sınıfı.
2. Bir Web uygulaması varlığınız olması durumunda WebConfigurationManager sınıfı.

Bu yöntemler sırayla gerekli yöntemleri ve temel yapılandırma dosyalarını işlemek için özellikleri sağlayan bir yapılandırma nesnesi döndürür. Okuma veya yazma için bu dosyalara erişebilir.

### <a name="reading"></a>Okuma

Yapılandırma bilgilerini okumak için GetSection veya GetSectionGroup yöntemini kullanın. Kullanıcı ya da okur işlem yapılandırma dosyalarını hiyerarşideki tüm okuma iznine sahip olmalıdır.

> [!NOTE]
> Bir yol parametresi alan statik bir GetSection yöntemi kullanırsanız, path parametresi kodu çalıştığı uygulama başvurmalıdır. Aksi takdirde, parametre yoksayılır ve şu anda çalışan uygulama yapılandırma bilgilerini döndürülür.


### <a name="writing"></a>Yazma

Yapılandırma bilgilerini yazmak için Kaydet yöntemlerden birini kullanın. Kullanıcı ya da yazma olmalıdır işlem yazma izinleri yapılandırma dosyası ve geçerli yapılandırma hiyerarşi düzeyi dizininde, aynı zamanda Okuma izinleri yapılandırma dosyalarını hiyerarşideki tüm.

Belirtilen bir varlık için devralınan yapılandırma ayarlarını temsil eden bir yapılandırma dosyası oluşturmak için aşağıdaki Kaydet-yapılandırma yöntemlerden birini kullanın:

1. Yeni bir yapılandırma dosyası oluşturmak için Kaydet yöntemi.
2. Başka bir konumda yeni bir yapılandırma dosyası oluşturmak için Farklı Kaydet yöntemi.

## <a name="configuration-classes-and-namespaces"></a>Yapılandırma sınıfları ve ad alanları

Birçok yapılandırma sınıflar ve yöntemler birbirine benzer. Aşağıdaki tabloda, en sık kullanılan yapılandırma sınıfları ve ad alanlarını açıklar.

| **Yapılandırma sınıfı veya ad alanı** | **Açıklama** |
| --- | --- |
| [System.Configuration](https://msdn.microsoft.com/library/system.configuration.aspx) ad alanı | Tüm .NET Framework uygulamaları için ana yapılandırma sınıfları içerir. Bölümü işleyici sınıfları, yöntemleri, GetSection ve GetSectionGroup gibi bir bölüm için yapılandırma verilerini almak için kullanılır. Bu iki yöntem statik olmayan. |
| System.Configuration.Configuration class | Yapılandırma verilerini bir bilgisayara, uygulama, Web dizini veya diğer kaynak kümesini temsil eder. Bu sınıf yapılandırma ayarları güncelleştirilirken ve bölümler ve bölüm grupları yapılan başvuruları elde etmek için kullanışlı yöntemler, GetSection ve GetSectionGroup, gibi içerir. Bu sınıf, dönüş türü olarak WebConfigurationManager ve ConfigurationManager sınıflarının yöntemleri gibi tasarım zamanı yapılandırma verilerini almak yöntemleri için kullanılır. |
| System.Web.Configuration ad alanı | ASP.NET yapılandırma bölümlerinin tanımlanan bölümü işleyici sınıfları içerir [ASP.NET yapılandırma ayarlarını](https://msdn.microsoft.com/library/b5ysx397.aspx). Bölümü işleyici sınıfları, yöntemleri, GetSection ve GetSectionGroup gibi bir bölüm için yapılandırma verilerini almak için kullanılır. |
| System.Web.Configuration.WebConfigurationManager class | Çalışma zamanı ve tasarım zamanı yapılandırma ayarlarına yapılan başvuruları alma için kullanışlı yöntemler sağlar. Bu yöntemler System.Configuration.Configuration sınıfı bir dönüş türü olarak kullanın. Bu sınıfın statik GetSection yöntemini veya System.Configuration.ConfigurationManager sınıfının statik olmayan GetSection yöntemi birbirinin yerine kullanabilirsiniz. Web uygulama yapılandırmaları için System.Web.Configuration.WebConfigurationManager sınıfı yerine System.Configuration.ConfigurationManager sınıfı önerilir. |
| [System.Configuration.Provider](https://msdn.microsoft.com/library/system.configuration.provider.aspx) ad alanı | Özelleştirme ve yapılandırma sağlayıcısı genişletmek için bir yol sağlar. Yapılandırma sistemi tüm sağlayıcı sınıflar için temel sınıf budur. |
| [System.Web.Management](https://msdn.microsoft.com/library/system.web.management.aspx) ad alanı | Sınıflar ve arabirimler yönetmek ve Web uygulamalarının durumunu izlemek için içerir. Kesinlikle olarak bakıldığında, bu ad alanı API yapılandırmasının bir parçası olarak kabul edilmez. Örneğin, izleme ve olay tetikleme gerçekleştirilir bu ad alanındaki sınıflar tarafından. |
| [System.Management.Instrumentation](https://msdn.microsoft.com/library/system.management.instrumentation.aspx) ad alanı | Gerekli yönetim bilgilerini ve olası tüketicileri olaylara Windows Yönetim Araçları (WMI) aracılığıyla kullanıma sunmak için uygulama araçları için sınıflar sağlar. ASP.NET sistem durumu izleme olayları teslim etmek için WMI kullanır. Kesinlikle olarak bakıldığında, bu ad alanı API yapılandırmasının bir parçası olarak kabul edilmez. |

## <a name="reading-from-aspnet-configuration-files"></a>ASP.NET yapılandırma dosyaları okuma

ASP.NET yapılandırma dosyaları okuma için çekirdek sınıf WebConfigurationManager sınıftır. ASP.NET yapılandırma dosyalarını okuma temelde üç adım vardır:

1. OpenWebConfiguration yöntemi kullanarak bir yapılandırma nesnesini alın.
2. Yapılandırma dosyasında istenen bölüm için bir başvuru alır.
3. İstenen bilgilerin yapılandırma dosyasından okunur.

Nesneyi temsil ediyor yapılandırma belirli yapılandırma dosyasını temsil etmiyor. Bunun yerine, bir bilgisayar, uygulama veya Web sitesi yapılandırmasını birleştirilmiş bir görünümünü temsil eder. Aşağıdaki kod örneği olarak adlandırılan bir Web uygulaması yapılandırmasını temsil eden bir yapılandırma nesnesi başlatır *ifadesini*.

[!code-csharp[Main](configuration-and-instrumentation/samples/sample1.cs)]

> [!NOTE]
> /ProductInfo yol yoksa, yukarıdaki kod machine.config dosyasında belirtildiği gibi varsayılan yapılandırmayı geri bildirdiğine dikkat edin.


Yapılandırma nesnesini oluşturduktan sonra yapılandırma ayarlarını incelemek için GetSection veya GetSectionGroup yöntemini kullanabilirsiniz. Aşağıdaki örnek, yukarıdaki ifadesini uygulaması için kimliğe bürünme ayarlarını bir başvuru alır:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample2.cs)]

## <a name="writing-to-aspnet-configuration-files"></a>ASP.NET yapılandırma dosyalara yazma

Yapılandırma dosyaları okuma olduğu gibi Asp.NET yapılandırma dosyalarını yazmak için çekirdek WebConfigurationManager sınıftır. ASP.NET yapılandırma dosyalara yazma için üç adım vardır.

1. OpenWebConfiguration yöntemi kullanarak bir yapılandırma nesnesini alın.
2. Yapılandırma dosyasında istenen bölüm için bir başvuru alır.
3. İstenen bilgi kaydetme veya farklı kaydet kullanarak yapılandırma dosyasından yazma yöntemi.

Aşağıdaki kod değişiklikleri **hata ayıklama** özniteliği &lt;derleme&gt; öğesi false olarak ayarlayın:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample3.cs)]

Bu kod çalıştırıldığında **hata ayıklama** özniteliği &lt;derleme&gt; öğesi için yanlış ayarlanacak *webApp* uygulamanın web.config dosyasına.

## <a name="systemwebmanagement-namespace"></a>System.Web.Management Namespace

System.Web.Management ad alanı, sınıflar ve arabirimler yönetmek ve ASP.NET uygulamalarının durumunu izlemek için sağlar.

Günlük olayları bir sağlayıcı ile ilişkilendiren bir kural tanımlayarak gerçekleştirilir. Kural sağlayıcısına gönderilen olayların türünü tanımlar. Aşağıdaki temel olayları oturum için kullanılabilir:

| **WebBaseEvent** | Tüm olaylar için temel olay sınıfı. Gerekli içermektedir özelliklerini olay kodu, Olay Ayrıntısı kodu, tarih ve saat olay yükseltildi gibi tüm olaylar için sıra numarası, olay iletisi ve Olay Ayrıntıları. |
| --- | --- |
| **WebManagementEvent** | Yönetimi olayları, uygulama yaşam süresi, istek, hata ve denetim olayları gibi temel olay sınıfı. |
| **WebHeartbeatEvent** | Yararlı çalışma zamanı durumu bilgileri yakalamak için düzenli aralıklarla uygulama tarafından üretilen olay. |
| **WebAuditEvent** | Yetkilendirme hatası, şifre çözme hatası gibi koşullar işaretlemek için kullanılan güvenlik denetim olaylarını için temel sınıf *vs.* |
| **WebRequestEvent** | Tüm bilgi isteği olayları için temel sınıf. |
| **WebBaseErrorEvent** | Hata koşulları belirten tüm olaylar için temel sınıf. |

Kullanılabilir sağlayıcı türü, olay çıkış Olay Görüntüleyicisi'ni, SQL Server, Windows Yönetim Araçları (WMI) ve e-posta göndermek izin verin. Önceden yapılandırılmış sağlayıcıları ve olay eşlemeleri günlüğe olay çıkış almak gerekli iş miktarını azaltın.

ASP.NET 2.0 olay günlüğü sağlayıcısı out-of--box başlatma ve durdurma yanı işlenmeyen özel durumlar günlüğü uygulama etki alanlarını temel alan olaylarını günlüğe kaydedecek şekilde kullanır. Bu, bazı temel senaryolarını kapsamak üzere yardımcı olur. Örneğin, uygulamanızın bir özel durum oluşturur, ancak kullanıcı hata kaydetmez ve yeniden oluşturamaz varsayalım. Varsayılan olay günlüğü kuralla ne tür bir hata oluştu hakkında ile ilgili daha iyi bir fikir edinmek için özel durumu ve yığın bilgileri toplama gerçekleştirebilir. Başka bir örnek uygulamanızın oturum durumunu kaybetmeden geçerlidir. Bu durumda, uygulama etki alanı geri dönüştürme ve neden uygulama etki alanı ilk başta durduruldu olup olmadığını belirlemek için olay günlüğüne bakabilirsiniz.

Ayrıca, sistem durumu sistem izleme genişletilebilir. Örneğin, özel Web olayları tanımlayın, uygulamanızdaki yangın ve e-posta gibi bir sağlayıcı için olay bilgilerini göndermek üzere bir kural tanımlayın. Bu, sistem durumu izleme sağlayıcıları için izleme kolayca bağlamanın sağlar. Başka bir örnek olarak, bir sırada işlenir ve SQL Server veritabanına her olay gönderen bir kural ayarlama her zaman bir olay tetikleyin. Ayrıca bir satırda birden çok kez oturum açmak ve e-posta tabanlı sağlayıcıları kullanabilmek için olay ayarlamak bir kullanıcı başarısız olduğunda bir olay tetikleyin.

Varsayılan sağlayıcıları ve olayları yapılandırma genel Web.config dosyasında depolanır. ASP.NET 1 Machine.config dosyasında depolanan tüm Web tabanlı ayarları Genel Web.config dosyasını depolayan x. Genel Web.config dosyası şu dizinde bulunur:

`%windir%\Microsoft.Net\Framework\v2.0.*\config\Web.config`

&lt;Ögesi&gt; Genel Web.config dosyasının varsayılan yapılandırma ayarlarını sağlar. Bu ayar geçersiz kılabilir veya kendi ayarlarınızı uygulayarak yapılandırın &lt;ögesi&gt; uygulamanız için Web.config dosyasındaki bölümünü.

&lt;Ögesi&gt; Genel Web.config dosyasının bölümü aşağıdaki öğeleri içerir:

| **sağlayıcıları** | Olay Görüntüleyicisi'ni, WMI ve SQL Server için ayarlanmış sağlayıcıları içerir. |
| --- | --- |
| **eventMappings** | Eşlemeleri için çeşitli WebBase sınıfları içerir. Kendi olay sınıfı oluşturursanız, bu listeyi genişletebilirsiniz. Kendi olay sınıfı oluşturmak daha hassas ayrıntı düzeyi için bilgi gönder sağlayıcıları üzerinden sağlar. Örneğin, SQL Server için kendi özel olaylar için e-posta gönderilirken gönderilmek üzere işlenmeyen özel durumlar yapılandırabilirsiniz. |
| **Kuralları** | EventMappings sağlayıcıya bağlar. |
| **Arabelleğe alma** | SQL Server ve e-posta sağlayıcıları ile genellikle sağlayıcıya olayları temizlemek nasıl belirlemek için kullanılır. |

Genel Web.config dosyasındaki bir kod örneği aşağıdadır.

[!code-xml[Main](configuration-and-instrumentation/samples/sample4.xml)]

## <a name="how-to-store-events-to-event-viewer"></a>Olay Görüntüleyicisi olayları depolamak nasıl

Sağlayıcı olayları günlüğe kaydetmeyi için daha önce belirtildiği gibi Olay Görüntüleyicisi sizin için genel Web.config dosyasında yapılandırılmış. Varsayılan olarak, tüm olayları temel alarak **WebBaseErrorEvent** ve **WebFailureAuditEvent** kaydedilir. Olay günlüğüne ek bilgi için ek kurallar ekleyebilirsiniz. Örneğin, tüm olayları günlüğe istiyorsanız (*yani*, her olay temel alarak **değeri**), aşağıdaki kural Web.config dosyasına ekleyebilirsiniz:

[!code-xml[Main](configuration-and-instrumentation/samples/sample5.xml)]

Bu kural bağlayabilirsiniz **tüm olayları** olay günlüğü sağlayıcısı olay eşlenir. EventMapping ve sağlayıcı Genel Web.config dosyasına dahil edilir.

## <a name="how-to-store-events-to-sql-server"></a>SQL Server olayları depolamak nasıl

Bu yöntem **ASPNETDB** Aspnet tarafından oluşturulan veritabanı\_regsql.exe aracı. Varsayılan sağlayıcı uygulamada ya da bir dosya tabanlı veritabanı kullanır LocalSqlServer bağlantı dizesini kullanır\_veri klasörü veya SQL Server yerel SQLExpress örneği. LocalSqlServer bağlantı dizesi ve SqlProvider Genel Web.config dosyasında yapılandırılır.

Genel Web.config dosyasındaki LocalSqlServer bağlantı dizesi şu şekildedir:

[!code-xml[Main](configuration-and-instrumentation/samples/sample6.xml)]

Başka bir SQL Server örneği kullanmak istiyorsanız, Aspnet kullanmanız gerekecektir\_% windir%\Microsoft.Net\Framework\v2.0. bulunabilir regsql.exe aracı\* klasör. Aspnet kullanmak\_özel oluşturmak için regsql.exe aracını **ASPNETDB** veritabanı SQL Server örneğinde sonra bağlantı dizesini uygulama yapılandırma dosyasına ekleyin ve sonra yeni kullanarak bir sağlayıcı ekleyin bağlantı dizesi. Bulduktan sonra **ASPNETDB** oluşturulan veritabanı için sqlProvider bir eventMapping bağlamak için bir kural kümesi gerekir.

Varsayılan SqlProvider kullanabilir veya kendi sağlayıcınızı yapılandırmak isteyip sağlayıcı olay eşlemesi ile bağlama kural eklemeniz gerekir. Aşağıdaki kural için yukarıda oluşturduğunuz yeni sağlayıcı bağlantıları **tüm olayları** olay eşlemesi. Bu kural bağlı olan tüm olayları günlüğe kaydeder **değeri** ve MYASPNETDB bağlantı dizesi kullanacağı MySqlWebEventProvider gönderin. Aşağıdaki kod, sağlayıcı olay eşlemesi ile bağlamak için bir kuralı ekler:

[!code-xml[Main](configuration-and-instrumentation/samples/sample7.xml)]

Yalnızca SQL Server hataları göndermek istiyorsanız, aşağıdaki kural ekleyebilirsiniz:

[!code-xml[Main](configuration-and-instrumentation/samples/sample8.xml)]

## <a name="how-to-forward-events-to-wmi"></a>WMI olayları iletmek nasıl

Ayrıca WMI olayları iletebilirsiniz. WMI sağlayıcısı sizin için genel Web.config dosyasında varsayılan olarak yapılandırılır.

Aşağıdaki kod örneğinde WMI olayları iletmek için bir kuralı ekler:

[!code-xml[Main](configuration-and-instrumentation/samples/sample9.xml)]

Sağlayıcı ve olaylarını dinleyecek şekilde WMI dinleyicisi uygulama için bir eventMapping ilişkilendirmek için bir kural eklemeniz gerekir. Aşağıdaki kod örneğinde WMI sağlayıcısına bağlanmak için bir kuralı ekler **tüm olayları** olay eşlemesi:

[!code-xml[Main](configuration-and-instrumentation/samples/sample10.xml)]

## <a name="how-to-forward-events-to-email"></a>E-posta olayları iletmek nasıl

Ayrıca, e-posta olayları iletebilirsiniz. SQL Server veya olay günlüğü için daha uygun kasıtsız olarak kendiniz bir çok sayıda bilgi, gönderebilirsiniz gibi e-posta sağlayıcınıza eşleme hangi olay kuralları olabilir dikkatli olun. İki e-posta sağlayıcısı vardır; SimpleMailWebEventProvider ve TemplatedMailWebEventProvider. Her ikisi de yalnızca TemplatedMailWebEventProvider üzerinde kullanılabilir "Şablon" ve "detailedTemplateErrors" öznitelikleri dışında aynı yapılandırma öznitelikleri vardır.

> [!NOTE]
> Bu e-posta sağlayıcısı hiçbiri sizin için yapılandırılır. Web.config dosyanızı eklemeniz gerekir.


Bu iki e-posta sağlayıcısı arasındaki temel fark, SimpleMailWebEventProvider değiştirilemez genel bir şablon e-posta gönderir ' dir. Örnek Web.config dosyası, aşağıdaki kural kullanarak, yapılandırılmış sağlayıcılar listesi için bu e-posta sağlayıcısı ekler:

[!code-xml[Main](configuration-and-instrumentation/samples/sample11.xml)]

Aşağıdaki kural, ayrıca e-posta Sağlayıcısı'na bağlayın eklenme **tüm olayları** olay eşlemesi:

[!code-xml[Main](configuration-and-instrumentation/samples/sample12.xml)]

## <a name="aspnet-20-tracing"></a>ASP.NET 2.0 izleme

ASP.NET 2.0 izleme üç önemli geliştirmeler vardır.

1. Tümleşik izleme işlevselliği
2. İzleme iletileri için programlı erişim
3. Geliştirilmiş uygulama düzeyinde izleme

## <a name="integrated-tracing-functionality"></a>Tümleşik izleme işlevselliği

Şimdi, ASP.NET çıktı izleme System.Diagnostics.Trace sınıfı tarafından gösterilen iletileri yönlendirmek ve System.Diagnostics.Trace için ASP.NET izleme tarafından gösterilen iletileri yönlendirmek. Ayrıca, System.Diagnostics.Trace için ASP.NET izleme olayları iletebilirsiniz. Bu işlev yeni tarafından sağlanan **writeToDiagnosticsTrace** özniteliği &lt;izleme&gt; öğesi. Bu Boole değeri true olduğunda, ASP.NET izleme iletilerini kullanmak için System.Diagnostics izleme altyapı izleme iletilerini görüntülemek için kayıtlı tüm dinleyiciler tarafından iletilir.

## <a name="programmatic-access-to-trace-messages"></a>İleti izleme için programlı erişim

ASP.NET 2.0 verir programlı erişim aracılığıyla tüm izleme iletileri için **TraceContextRecord** sınıfı ve **TraceRecords** koleksiyonu. İzleme iletileri erişme en verimli şekilde kaydetmek için olan bir **TraceContextEventHandler** temsilci (ASP.NET 2. 0'da yeni) yeni işlemek için **TraceFinished** olay. İstediğiniz gibi sonra izleme iletilerini döngüye girer.

Aşağıdaki kod örneği bu gösterilmektedir:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample13.cs)]

Yukarıdaki örnekte, t TraceRecords toplulukta döngü ve ardından yanıt akışına her ileti yazın.

## <a name="improved-application-level-tracing"></a>Geliştirilmiş uygulama düzeyinde izleme

Uygulama düzeyi izleme, yeni giriş geliştirildi **mostRecent** özniteliği &lt;izleme&gt; öğesi. Bu öznitelik, en son uygulama düzeyinde izleme çıktısı görüntülenir ve requestLimit tarafından belirtilen sınırları aşan eski izleme verilerinin atılır olup olmadığını belirtir. RequestLimit özniteliğine ulaşılana kadar false ise, izleme verilerini istekleri için görüntülenir.

## <a name="aspnet-command-line-tools"></a>ASP.NET komut satırı araçları

ASP.NET yapılandırmasında yardımcı olmak için çeşitli komut satırı araçları vardır. ASP.NET geliştiricilerinin ile aspnet tanıdık\_regiis.exe aracı. ASP.NET 2.0 yapılandırmasında yardımcı olmak için üç diğer komut satırı araçları sağlar.

Aşağıdaki komut satırı araçları kullanılabilir:

| **Aracı** | **Kullanın** |
| --- | --- |
| **aspnet\_regiis.exe** | ASP.NET IIS Kayıt sağlar. ASP.NET 2.0 ile bu sevk, (Framework klasöründe) 32 bit sistemler için diğeri (klasöründe Framework64.) 64 bit sistemler için bu araçları iki sürümü bulunmaktadır 32-bit işletim sisteminde 64-bit sürümü yüklü değil. |
| **aspnet\_regsql.exe** | ASP.NET SQL Server kayıt aracı, ASP.NET, SQL Server sağlayıcıları tarafından kullanılmak üzere bir Microsoft SQL Server veritabanı oluşturmak veya eklemek veya varolan bir veritabanından seçenekleri kaldırmak için kullanılır. Aspnet\_regsql.exe dosya [drive:]\WINDOWS\Microsoft.NET\Framework\versionNumber klasöründe Web sunucunuzda bulunur. |
| **aspnet\_regbrowsers.exe** | ASP.NET tarayıcı kayıt aracını ayrıştırır ve tüm sistem genelinde tarayıcı tanımları bir bütünleştirilmiş derler ve derleme genel derleme önbelleğine yükler. Aracı tarayıcı tanım dosyalarını kullanır (. Tarayıcı dosyaları) .NET Framework tarayıcılar alt gelen. Aracı %SystemRoot%\Microsoft.NET\Framework\version\ dizininde bulunabilir. |
| **aspnet\_compiler.exe** | ASP.NET derleme aracı yerinde veya dağıtım bir üretim sunucusu gibi bir hedef konum için bir ASP.NET Web uygulaması derlemek sağlar. Son kullanıcılar uygulamayı derlenirken bir gecikme uygulamaya yapılan ilk istekte karşılaşmazsınız çünkü yerinde derleme uygulama performansı yardımcı olur. |

Çünkü aspnet\_regiis.exe aracı ASP.NET 2.0 yeni değil, bunu burada aşağıdakiler ele alınacaktır değil.

## <a name="aspnet-sql-server-registration-tool---aspnetregsqlexe"></a>ASP.NET SQL Server kayıt aracı - aspnet\_regsql.exe

Çeşitli ASP.NET SQL Server kayıt aracını kullanarak seçeneklerini ayarlayabilirsiniz. Bir SQL bağlantı belirtin, hangi ASP.NET uygulama hizmetleri SQL Server bilgilerini yönetmek, hangi veritabanı veya tablo SQL önbellek bağımlılığı için kullanıldığını belirtmek için kullanın belirtin ve Ekle veya yordamları ve oturum durumunu depolamak için SQL Server'ı kullanma desteği kaldırın.

Bir veri kaynağından veri alma ve depolamada yönetmek için bir sağlayıcı birkaç ASP.NET uygulama hizmetleri kullanır. Her sağlayıcı için veri kaynağı özeldir. ASP.NET aşağıdaki ASP.NET özellikleri için bir SQL Server sağlayıcısı içerir:

- Üyelik ( [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) sınıfı).
- Rol yönetimi ( [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) sınıfı).
- Profil ( [SqlProfileProvider](https://msdn.microsoft.com/library/system.web.profile.sqlprofileprovider.aspx) sınıfı).
- Web Bölümleri kişiselleştirme ( [SqlPersonalizationProvider](https://msdn.microsoft.com/library/system.web.ui.webcontrols.webparts.sqlpersonalizationprovider.aspx) sınıfı).
- Web olayları ( [SqlWebEventProvider](https://msdn.microsoft.com/library/system.web.management.sqlwebeventprovider.aspx) sınıfı).

ASP.NET yüklediğinizde, sunucunuz için Machine.config dosyasının bir sağlayıcısında kullanan ASP.NET özelliklerin her biri için SQL Server sağlayıcıları belirtmek yapılandırma öğeleri içerir. Bu sağlayıcılar için yerel kullanıcı bir SQL Server Express 2005 örneğine bağlanmak için varsayılan olarak yapılandırılır. Sağlayıcıları tarafından kullanılan varsayılan bağlantı dizesini değiştirirseniz, makine yapılandırmasında yapılandırılan ASP.NET özelliklerinden herhangi birini kullanmadan önce daha sonra SQL Server veritabanı ve veritabanı öğelerini Aspnetkullanarak,seçiliözellikiçinyüklemenizgerekir\_regsql.exe. SQL kayıt aracıyla belirttiğiniz veritabanı zaten mevcut değilse (aspnetdb olacaktır varsayılan veritabanı bir komut satırında belirtilmezse), sonra da geçerli kullanıcı SQL şeması e oluşturmak için aynı zamanda sunucu veritabanı oluşturma hakkına sahip olmalıdır bir veritabanı içinde lements.

### <a name="sql-cache-dependency"></a>SQL önbellek bağımlılığı

SQL önbellek bağımlılığı ASP.NET çıktı önbelleği, Gelişmiş bir özelliktir. SQL önbellek bağımlılığı iki farklı çalışma modunu destekler: bir ASP.NET uygulaması tabloyu yoklama ve SQL Server 2005 sorgu bildirim özelliklerini kullanır ikinci bir modu kullanan bir. SQL kayıt aracı, işlem tabloyu yoklama modunu yapılandırmak için kullanılabilir.

### <a name="session-state"></a>Oturum durumu

Varsayılan olarak, oturum durumu değerlerini ve bilgileri bellek ASP.NET işlemi içinde depolanır. Alternatif olarak, burada birden çok Web sunucusu tarafından paylaşılabilen bir SQL Server veritabanında oturum verilerini depolayabilir. Oturum durumu SQL kayıt aracıyla için belirttiğiniz veritabanı zaten mevcut değilse, geçerli kullanıcı SQL Server'da bir veritabanı içinde şema öğeleri oluşturmak için de veritabanı oluşturma hakkına olması gerekir. Veritabanı mevcut değilse, geçerli kullanıcının varolan bir veritabanında şema öğeleri oluşturmak için haklarınız olmalıdır.

SQL Server'da oturum durumu veritabanını yüklemek için ASP.NET çalıştıran\_regsql.exe aracı ve komutunu aşağıdaki bilgileri sağlayın:

- SQL Server adını kullanarak örnek **-S** seçeneği.
- SQL Server çalıştıran bir bilgisayarda bir veritabanı oluşturmak için izne sahip bir hesap için oturum açma kimlik bilgileri. Kullanmak **-E** şu anda oturum açmış kullanıcının kullanın veya kullanmak için seçeneği **- U** bir kullanıcı kimliği ile birlikte belirtmek için seçeneği **-P** seçeneği bir parola belirtin.
- **- Ssadd** oturum durumu veritabanı eklemek için komut satırı seçeneği.

Varsayılan olarak, ASP.NET kullanamazsınız\_regsql.exe aracı oturum durumu veritabanını SQL Server 2005 Express Edition çalıştıran bir bilgisayara yükleyin.

### <a name="the-aspnet-browser-registration-tool---aspnetregbrowsersexe"></a>ASP.NET tarayıcı kayıt aracı - aspnet\_regbrowsers.exe

ASP.NET sürüm 1.1 Machine.config dosyasının adlı bir bölümde yer alan &lt;browserCaps&gt;. Bu bölümde yer alan bir dizi yapılandırmaları bir normal ifadeye dayalı çeşitli tarayıcılar için tanımlanan XML girdi. ASP.NET sürüm 2.0, yeni bir. Tarayıcı dosyası XML girişleri kullanarak belirli bir tarayıcı parametreleri tanımlar. Yeni bir ekleyerek üzerinde yeni bir tarayıcı bilgileri ekleyin. Sisteminizde %SystemRoot%\Microsoft.NET\Framework\version\CONFIG\Browsers konumunda bulunan klasörüne tarayıcı dosyası.

Tarayıcı bilgileri gerektiren her zaman uygulamanın .config dosyası okunurken değil çünkü bir yeni oluşturabilirsiniz. Tarayıcı dosyası ve çalıştırma Aspnet\_gerekli değişiklikleri derlemeye eklemek için regbrowsers.exe. Bu bilgileri almak için uygulamalarınızın herhangi birinin kapatmak zorunda değilsiniz şekilde yeni tarayıcı bilgileri hemen erişmek sağlar. Bir uygulama tarayıcı yetenekleri geçerli HttpRequest tarayıcı özelliği üzerinden erişebilirsiniz.

Aspnet çalıştırırken aşağıdaki seçenekler kullanılabilir\_regbrowser.exe:

| **Seçeneği** | **Açıklama** |
| --- | --- |
| **-?** | Aspnet görüntüler\_regbbrowsers.exe komut penceresinde Yardım metni. |
| **-i** | Çalışma zamanı tarayıcı yetenekleri derlemesi oluşturur ve genel derleme önbelleğinde yükler. |
| **-u** | Çalışma zamanı tarayıcı yetenekleri derlemesi Genel Derleme Önbelleği'nden kaldırır. |

## <a name="the-aspnet-compilation-tool---aspnetcompilerexe"></a>ASP.NET derleme aracı - aspnet\_compiler.exe

ASP.NET derleme aracı iki genel şekillerde kullanılabilir: yerinde derleme ve dağıtım için bir hedef çıktı dizini belirtilen Burada, derleme için.

### <a name="compiling-an-application-in-placehttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Bir uygulama yerinde derleme](https://msdn.microsoft.com/library/ms229863.aspx)

ASP.NET derleme aracı yerinde bir uygulaması derleyebilir, diğer bir deyişle, bu nedenle normal derleme neden olan uygulama için birden çok istek yapma davranışını taklit eder. Önceden derlenmiş sitenin kullanıcıları ilk istek sayfasında derleme tarafından neden bir gecikme karşılaşmazsınız.

Bir site yerinde yapmanızın aşağıdaki öğeleri Uygula:

- Site, dizin yapısını ve dosyalarını korur.
- Sunucusunda site tarafından kullanılan tüm programlama dili için derleyicileri olması gerekir.
- Tüm site herhangi bir dosyanın derleme başarısız olursa, derleme başarısız oluyor.

Yeni kaynak dosyaları için ekledikten sonra uygulamanın yerinde yeniden derleyin. Eklemediğiniz sürece aracı yalnızca yeni veya değiştirilmiş dosyaları derler **- c** seçeneği.

> [!NOTE]
> İç içe geçmiş bir uygulama içeren bir uygulamanın derlenmesini iç içe geçmiş uygulama derlenmiyor. İç içe geçmiş uygulama ayrı olarak derlenmiş olmalıdır.


### <a name="compiling-an-application-for-deploymenthttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Bir uygulama dağıtımı için derleme](https://msdn.microsoft.com/library/ms229863.aspx)

TargetDir parametresini belirterek deployment (derleme için bir hedef konum) için bir uygulama derleyin. TargetDir Web uygulaması için son konumu olabilir veya derlenmiş uygulama daha fazla dağıtılabilir. Kullanarak **-u** seçeneği belirli derlenmiş uygulama dosyalarında onu derlemeden değişiklik yapabilirsiniz, şekilde uygulama derler. ASPNET\_compiler.exe statik ve dinamik dosya türleri arasında bir ayrım haline getirir ve bunları elde edilen uygulama oluştururken farklı şekilde işler.

- Statik dosya türleridir değil ilişkili derleyici olması ya da sağlayıcı, özelliği dosyaları gibi yapı adlandırılmış .css, .gif, .htm, .html, .jpg, .js gibi uzantıları vb. vardır. Bu dosyalar yalnızca göreli konumlarına korunur dizin yapısına sahip bir hedef konuma kopyalanır.
- Dinamik dosya türlerini ilişkili derleyici veya .asax, .ascx, .ashx, .aspx, .browser, .master ve benzerleri gibi belirli ASP.NET dosya adı uzantılarına sahip dosyaları dahil olmak üzere sağlayıcısı, yapı olanlardır. ASP.NET derleme aracı derlemeler bu dosyaları oluşturur. Varsa **-u** seçeneği atlanırsa, aracı dosya adı uzantısı ile de dosyaları oluşturur. Kendi derlemesi için orijinal kaynak dosyalarına eşleme COMPILED. Uygulama kaynağı dizin yapısını korunduğundan emin olmak için aracı hedef uygulamada karşılık gelen konumlarda yer tutucu dosyaları oluşturur.

Kullanmalısınız **-u** derlenmiş uygulama içeriğini değiştirilebilir belirtmek için seçenek. Aksi halde, sonraki değişiklikler göz ardı edilir veya çalışma zamanı hataları neden olabilir.

Aşağıdaki tabloda nasıl ne zaman ASP.NET derleme aracı tanıtıcıları farklı dosya türleri açıklanmaktadır **-u** seçeneği bulunur.

| **Dosya türü** | **Derleyici eylemi** |
| --- | --- |
| .ascx, .aspx, .master | Bu dosyalar arka plan kod dosyaları ve sınırlanan herhangi bir kod içerir, biçimlendirme ve kaynak kodu olarak bölünür &lt;komut dosyası runat = "server"&gt; öğeleri. Kaynak kodu bir karma algoritması türetilen adlarıyla derlemeleri derlenir ve derlemeler Bin dizinindeki yerleştirilir. Tüm satır içi kod olduğundan, kod içine arasında **&lt; %** ve **% &gt;** köşeli ayraçlar, biçimlendirme ile birlikte verilir ve derlenmemiş. Kaynak dosyaları ile aynı ada sahip yeni dosyalar biçimlendirme içerecek şekilde oluşturulur ve karşılık gelen çıktı dizinlerde yerleştirilir. |
| .ashx, .asmx | Bu dosyalar değil derlenir ve çıktı dizinlere olduğu ve derlenmemiş taşınır. Derlenmiş işleyici kod istiyorsanız, kaynak kodu dosyaları için uygulama kodu yerleştirin\_kod dizini. |
| .cs, .vb, .jsl, .cpp (daha önce listelenen dosya türleri için arka plan kod dosyaları dahil değil) | Bu dosyalar derlenmiş ve onları başvuru derlemeleri kaynak olarak dahil. Kaynak dosyalar çıkış dizinine kopyalanmaz. Bir kod dosyası başvurulmuyorsa derlenmiş değil. |
| Özel dosya türleri | Bu dosyalar derlenmemiş. Bu dosyalar ilgili çıktı dizinleri kopyalanır. |
| Kaynak kodu dosyaları için uygulama\_kod alt | Bu dosyalar derlemelerine derlenmiş ve Bin dizinindeki yerleştirilir. |
| Uygulama .resx ve .resource dosyalarında\_GlobalResources alt | Bu dosyalar derlemelerine derlenmiş ve Bin dizinindeki yerleştirilir. Hiçbir uygulama\_GlobalResources alt ana çıktı dizini altında oluşturulur ve kaynak dizininde bulunan hiçbir .resx veya .resources dosyaları çıkış dizinlere kopyalanır. |
| Uygulama .resx ve .resource dosyalarında\_LocalResources alt | Bu dosyalar değil derlenir ve ilgili çıktı dizinleri kopyalanır. |
| Uygulama .skin dosyalarında\_Temalar alt | .Skin dosyalar ve statik tema dosyaları değil derlenir ve ilgili çıktı dizinleri kopyalanır. |
| Derlemeleri Bin dizinindeki zaten mevcut .browser Web.config statik dosya türleri | Bu dosyalar çıkış dizinlere olarak kopyalanır. |

Aşağıdaki tabloda nasıl ne zaman ASP.NET derleme aracı tanıtıcıları farklı dosya türleri açıklanmaktadır **-u** seçeneği atlanmış.

| **Dosya türü** | **Derleyici eylemi** |
| --- | --- |
| .aspx, .asmx, .ashx, .master | Bu dosyalar arka plan kod dosyaları ve sınırlanan herhangi bir kod içerir, biçimlendirme ve kaynak kodu olarak bölünür &lt;komut dosyası runat = "server"&gt; öğeleri. Kaynak kodu bir karma algoritması türetilen adlarıyla derlemeleri derlenir. Sonuçta elde edilen derlemeleri Bin dizinindeki yerleştirilir. Tüm satır içi kod olduğundan, kod içine arasında **&lt; %** ve **% &gt;** köşeli ayraçlar, biçimlendirme ile birlikte verilir ve derlenmemiş. Derleyici kaynak dosyaları ile aynı adda biçimlendirme içerecek şekilde yeni dosyaları oluşturur. Bu sonuç dosyaları Bin dizinindeki yerleştirilir. Derleyici de kaynak dosyaları ile aynı ada sahip ancak uzantılı dosyaları oluşturur. Eşleme bilgilerini içerir COMPILED. . DERLENMİŞ dosyalar kaynak dosyalarını özgün konumuna karşılık gelen çıktı dizinleri yerleştirilir. |
| .ascx | Bu dosyalar, biçimlendirme ve kaynak koduna bölünür. Kaynak kodu derlemelerine derlenmiş ve karma algoritma türetilen adlarıyla Bin dizinine yerleştirilir. Hiçbir biçimlendirme dosya oluşturulur. |
| .cs, .vb, .jsl, .cpp (daha önce listelenen dosya türleri için arka plan kod dosyaları dahil değil) | .Ascx, .ashx veya .aspx dosyaları oluşturulan derlemeleri tarafından başvurulan kaynak kodu derlemelerine derlenmiş ve Bin dizinindeki yerleştirilir. Kaynak dosyalar kopyalanır. |
| Özel dosya türleri | Bu dosyalar gibi dinamik dosyaları derlenir. Esas alan dosya türüne bağlı olarak, derleyici çıktı dizinlerde eşleme dosyaları yerleştirebilirsiniz. |
| Uygulama dosyalarında\_kod alt | Kaynak kodu dosyaları bu alt derlemelerine derlenir ve Bin dizinindeki yerleştirilir. |
| Uygulama dosyalarında\_GlobalResources alt | Bu dosyalar derlemelerine derlenmiş ve Bin dizinindeki yerleştirilir. Hiçbir uygulama\_GlobalResources alt ana çıktı dizini altında oluşturulur. Yapılandırma dosyası appliesTo belirtiyorsa = "Tümü", .resx ve .resources dosyaları çıkış dizinlere kopyalanır. Tarafından başvurulan varsa bunlar kopyalanmaz bir [BuildProvider](https://msdn.microsoft.com/library/system.web.configuration.buildprovider.aspx). |
| Uygulama .resx ve .resource dosyalarında\_LocalResources alt | Bu dosyalar benzersiz adlara sahip derlemeler içine derlenmiş ve Bin dizinindeki yerleştirilir. Hiçbir .resx veya .resource dosyalar çıkış dizinlere kopyalanır. |
| Uygulama .skin dosyalarında\_Temalar alt | Temalar derlemelerine derlenmiş ve Bin dizinindeki yerleştirilir. Saplama dosyaları .skin dosyaları oluşturulur ve karşılık gelen çıktı dizininde yerleştirilir. Statik dosyalar (örneğin, .css) çıkış dizinlere kopyalanır. |
| Derlemeleri Bin dizinindeki zaten mevcut .browser Web.config statik dosya türleri | Bu dosyalar çıkış dizinine olarak kopyalanır. |

### <a name="fixed-assembly-nameshttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Sabit derleme adları](https://msdn.microsoft.com/library/ms229863.aspx##)

MSI Windows Yükleyicisi'ni kullanarak bir Web uygulamasını dağıtma gibi bazı senaryolar tutarlı dosya adları ve içeriği yanı sıra, derlemeler veya güncelleştirmeleri için yapılandırma ayarlarını tanımlamak için tutarlı dizin yapıları kullanılmasını gerektirir. Bu durumda, kullandığınız **- fixednames** ASP.NET derleme aracı bütünleştirilmiş derleme belirtmek için seçeneği nerede kullanmak yerine her kaynak dosya için birden çok sayfa derlemelerine derlenir. Ölçeklenebilirlik ile endişeniz varsa bu seçeneği dikkatli kullanmanız gerekir bu çok sayıda derlemeler için neden olabilir.

### <a name="strong-name-compilationhttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Tanımlayıcı ad derleme](https://msdn.microsoft.com/library/ms229863.aspx##)

**- Aptca**, **- delaysign**, **- keycontainer** ve **- keyfile** seçenekleri Aspnet kullanabilmesi için sağlanan\_ kesinlikle oluşturmak için compiler.exe adlı derlemeler kullanmadan [tanımlayıcı ad Aracı (Sn.exe)](https://msdn.microsoft.com/library/k5b5tt23.aspx) ayrı olarak. Bu seçenekler, sırasıyla karşılık gelen, **AllowPartiallyTrustedCallersAttribute**, **AssemblyDelaySignAttribute**, **AssemblyKeyNameAttribute**ve  **AssemblyKeyFileAttribute**.

Bu öznitelikler tartışması bu kapsamı dışında ' dir.

## <a name="labs"></a>Labs

Her biri aşağıdaki labs önceki laboratuvarlarda oluşturur. Sırayla yapmanız gerekir.

## <a name="lab-1-using-the-configuration-api"></a>Laboratuvar 1: yapılandırma API'si kullanma

1. Adlı yeni bir Web sitesi oluşturma *mod9lab*.
2. Yeni bir Web yapılandırma dosyası siteye ekleyin.
3. Aşağıdaki web.config dosyasına ekleyin:


[!code-xml[Main](configuration-and-instrumentation/samples/sample14.xml)]

Bu, web.config dosyasında değişiklikleri kaydetme izniniz olduğunu güvence altına alır.

1. Yeni bir etiket denetimi için Default.aspx ekleyip Kimliğine değiştirirseniz **lblDebugStatus**.
2. Yeni bir düğme denetimi için Default.aspx ekleyin.
3. Düğme denetiminin Kimliğini değiştirmek **btnToggleDebug** ve metne **geçiş hata ayıklama durum**.
4. Default.aspx arka plan kod dosyasının kod görünümünü açın ve eklemek bir **kullanarak** bildirimi **System.Web.Configuration** gibi:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample15.cs)]

1. İki özel değişken sınıfı ve bir sayfa ekleyin\_aşağıda gösterildiği gibi Init yöntemi:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample16.cs)]

1. Aşağıdaki kod sayfasına ekleme\_yük:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample17.cs)]

1. Kaydet ve default.aspx göz atın. Etiket denetimi geçerli hata ayıklama durum görüntülendiğine dikkat edin.
2. Düğme denetimi Tasarımcısı'nda çift tıklayın ve Click olayını düğme denetimi için aşağıdaki kodu ekleyin:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample18.cs)]

1. Kaydedip default.aspx göz atın ve düğmesini tıklatın.
2. Her düğmesini tıklatın ve uyun sonra web.config dosyasını açın **hata ayıklama** özniteliğini &lt;derleme&gt; bölümü.

## <a name="lab-2-logging-application-restarts"></a>Laboratuvar 2: Uygulama yeniden başlatılmadan günlüğe kaydetme

Bu laboratuvarda, uygulama kapatmalar, başlatmalar ve Olay Görüntüleyicisi'nde yeniden derlemelerinde günlüğe geçiş sağlayacak kodu oluşturur.

1. Bir DropDownList için default.aspx ekleyin ve kimliği için ddlLogAppEvents değiştirin.
2. Ayarlama **AutoPostBack** DropDownList özelliği **doğru**.
3. Üç öğe DropDownList öğeler koleksiyonuna ekleyin. Olun **metin** ilk öğesinin *Select Value* ve -1 değeri. Olun **metin** ve **değeri** ikinci öğesinin **True** ve **metin** ve **değeri** üçüncü öğesi **False**.
4. Yeni bir etiket için default.aspx ekleyin. Değişiklik kimliği **lblLogAppEvents**.
5. Default.aspx arka plan kodu görünümünü açın ve bir değişken için yeni bir bildirim türü HealthMonitoringSection aşağıda gösterildiği gibi ekleyin:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample19.cs)]

1. Varolan bir koda sayfasında aşağıdaki kodu ekleyin\_Init:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample20.cs)]

1. Üzerinde DropDownList çift tıklayın ve SelectedIndexChanged olay aşağıdaki kodu ekleyin:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample21.cs)]

1. Default.aspx göz atın.
2. Aşağı açılır kümesine **False**.
3. Olay Görüntüleyicisi'nde Uygulama günlüğünü temizleyin.
4. Uygulama için hata ayıklama özniteliğini değiştirmek için düğmesini tıklatın.
5. Olay Görüntüleyicisi'ndeki uygulama günlüğünde yenileyin. 

    1. Tüm olayları kaydedilmiş?
    2. Neden veya dersiniz?
6. Aşağı açılır kümesine **True.**
7. Uygulama için hata ayıklama özniteliği geçiş yapmak için düğmesini tıklatın.
8. Uygulama oturum açma Olay Görüntüleyicisi'ni yenileyin. 

    1. Tüm olayları kaydedilmiş?
    2. Uygulama kapanmanın nedeni neydi?
9. Açma ve kapatma günlük deneme ve web.config dosyasında yapılan değişiklikler bakın.

## <a name="more-information"></a>Daha fazla bilgi:

ASP.NET 2.0'ın sağlayıcı modeli kendi sağlayıcıları değil yalnızca uygulama izleme, ancak birçok diğer kullanımlar için de üyelik, vb. profilleri oluşturmanıza olanak sağlar. Bir metin dosyasına uygulama olayları günlüğe kaydetmek için özel bir sağlayıcı yazma ile ilgili ayrıntılı bilgi için ziyaret [bu bağlantıyı](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/ASPNETProvMod_Prt6.asp).
