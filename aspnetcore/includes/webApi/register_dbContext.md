## <a name="register-the-database-context"></a>Veritabanı bağlamı kaydetme

Bu adımda, veritabanı bağlamı kayıtlı [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcı. Bağımlılık ekleme (dı) kapsayıcı ile kaydedilen Hizmetleri (örneğin, DB bağlamı) denetleyicileri için kullanılabilir.

DB bağlamı için yerleşik destek kullanarak hizmet kapsayıcısı kaydetme [bağımlılık ekleme](xref:fundamentals/dependency-injection). Değiştir *haline* aşağıdaki kod ile dosya:

[!code-csharp[](../../tutorials/first-web-api/sample/TodoApi/Startup.cs?highlight=2,4,12)]

Önceki kod:

* Kullanılmayan kodunu kaldırır.
* Hizmet kapsayıcıya bir bellek içi veritabanına eklenen belirtir.