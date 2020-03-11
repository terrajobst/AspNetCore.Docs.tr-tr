# <a name="upload-files-sample-app"></a>Dosyaları karşıya yükleme örnek uygulaması

Bu örnek uygulama, [ASP.NET Core içindeki dosyaları karşıya yükle](https://docs.microsoft.com/aspnet/core/mvc/models/file-uploads) konusunda açıklanan kavramları gösterir.

## <a name="security-considerations"></a>Güvenlikle ilgili dikkat edilmesi gerekenler

Kullanıcılara bir sunucuya dosya yükleme yeteneği sağlarken dikkatli olun. Saldırganlar [hizmet reddi](/windows-hardware/drivers/ifs/denial-of-service) saldırıları yürütebilir, virüs veya kötü amaçlı yazılım yüklemeye veya diğer yollarla ağların ve sunucuların güvenliğinin aşılmasına çalışabilir.

Başarılı bir saldırının olasılığını azaltan güvenlik adımları şunlardır:

* Dosyaları, tercihen sistem dışı bir sürücüye sahip bir özel dosya yükleme alanına yükleyin. Adanmış bir konumun kullanılması, karşıya yüklenen dosyalar üzerinde güvenlik önlemleri yapmayı kolaylaştırır. Dosya yükleme konumunda yürütme izinlerini devre dışı bırakın.&dagger;
* Karşıya yüklenen dosyaları uygulamayla aynı dizin ağacında hiçbir şekilde kalıcı hale getirme.&dagger;
* Uygulama tarafından belirlenen bir güvenli dosya adı kullanın. Kullanıcı girişi veya karşıya yüklenen dosyanın güvenilmeyen dosya adı tarafından belirtilen bir dosya adı kullanmayın.&dagger;
* Yalnızca belirli bir onaylanan dosya uzantıları kümesine izin verin.&dagger;
* Kullanıcının bir kendini gizleyen dosya yüklemesini engellemek için dosya biçimi imzasını denetleyin (örneğin,. *exe* dosyasını *. txt* uzantısıyla karşıya yükleme).&dagger;
* Sunucu üzerinde istemci tarafı denetimlerinin de gerçekleştirildiğinden emin olun. İstemci tarafı denetimleri kolayca atmayı kolaylaştırır.&dagger;
* Karşıya yükleme işleminin boyutunu denetleyin ve beklenenden daha büyük olan karşıya yüklemeleri önleyin.&dagger;
* Aynı ada sahip karşıya yüklenen bir dosya tarafından dosyaların üzerine yazılmaması gerektiğinde, dosyayı karşıya yüklemeden önce dosya adını veritabanına veya fiziksel depolamaya göre denetleyin.
* **Dosya depolanmadan önce karşıya yüklenen içerik üzerinde bir virüs/kötü amaçlı yazılım tarayıcısı çalıştırın.**

örnek uygulama &dagger;ölçütleri karşılayan bir yaklaşımı gösterir.

> [!WARNING]
> Kötü amaçlı bir kodun bir sisteme karşıya yükleme için kod yürütme için ilk adımı sık şöyledir:
>
> * Tamamen devralma sistemin.
> * Sistemin kilitlenme sonucuyla bir sistemi aşırı yükleme.
> * Kullanıcı veya sistem verilerini tehlikeye.
> * Genel Kullanıcı arabirimine Graffiti uygulayın.
>
> Kullanıcıların dosyaları kabul ederken saldırı yüzey alanı azaltma hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:
>
> * [Kısıtlanmamış dosya yükleme](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * [Azure güvenliği: kullanıcılardan dosya kabul edilirken uygun denetimlerin yerinde olduğundan emin olun](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

Daha fazla bilgi için bkz. [ASP.NET Core dosyaları karşıya yükleme](https://docs.microsoft.com/aspnet/core/mvc/models/file-uploads).

## <a name="how-to-use-the-sample"></a>Örneği kullanma

*AppSettings. JSON* dosyasında:

1. Depolanan dosyalar için yolu ayarlayın (`StoredFilesPath`).

   * Örnek uygulama, değeri, sistem C: sürücü kökünde *Dosya* adında bir klasör olduğunu varsayan `c:\\files`olarak ayarlar.
   * Yolun mevcut olması gerekir. Sistemin C: sürücüsünde bir *dosyalar* klasörü oluşturun veya yolu uygun bir konum olarak ayarlayın.
   * Uygulamanın işlemi, yola okuma/yazma izinleri gerektiriyor.
   * **ÖNEMLI!** Yoldaki tüm kullanıcılar için yürütme izinlerini devre dışı bırakın.

1. Dosya boyutu sınırını (`FileSizeLimit`) bayt cinsinden ayarlayın. Örnek uygulamanın varsayılan değer olan `2097152` (2.097.152 bayt), dosya yüklemelerine 2 MB 'a kadar izin verir.
