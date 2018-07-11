# <a name="how-to-buildrun-secure-user-data-sample"></a>Nasıl derleme/güvenli kullanıcı veri örneği çalıştırma

* Gizli dizi Yöneticisi Aracı ile parola ayarlayın:

  `dotnet user-secrets set SeedUserPW <pw>`

* Veritabanını Güncelleştir:

    `dotnet ef database update`

* Projede SSL'i etkinleştirin
