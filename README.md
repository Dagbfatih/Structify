
# Structify

## Giriş
Web alanının çok popüler olduğu bu dönemlerde her gün belki binlerce yeni yazılım projesi hayata geçiriliyor veya ölüyor. Özellikle web projelerinin birçoğu en başta geliştirilirken tecrübesizlik ve kötü kod yazımından kaynaklı birçok sorun meydana gelmekte ve bu sorunlar genelde proje oldukça ciddi bir seviyede ilerlemiş iken ortaya çıkmaktadır. Dolayısıyla zaten çalışan veya çok fazla ilerletilmiş olan bu projelerin bozuk temelden kaynaklı oluşan sorunlarının onarımı da zahmetli olmaktadır. Birçok durumda zaten dükkan kapatılmaktadır. Farklı bir açıdan bakıldığında tecrübeli yazılımcılar için temelin düzgün inşa edilmemesinden kaynaklı bu sorunlar birer iş fırsatıdır. Bu sorunları tecrübeli yazılımcılar veya firmalar çözebilir veya bu sorunların en baştan önüne geçmek için sağlam mimariler geliştirip satabilirler.

## Fikir
Structify projesinin ana fikri, basit bir şekilde **"Kullanıma Hazır Kapsamlı Yazılım Mimarileri"** şeklinde açıklanabilir. Zaten ismi de "struct" ve "beautify" kelimelerinin birleşiminden gelmektedir, bir yazılım mimarisi nasıl bir sanat eseri olabilir'i göstermek istedim. Girişim seviyesindeki birçok girişimci firma geliştirdikleri projelerde bir standart yakalamakta zorlanır. Eğer biz pazarlamayı başarırsak onlara özel tasarladığımız yazılım mimarilerini bu firmalara satabiliriz. 3-4 aylık işi bir haftada bitirme ve yıllar sonra da oluşabilecek sorunların yaşanmayacağı garantisi bir firma için çok önemlidir. Ayrıca ileride yazılımcı sayılarının artması, sürekli işe alımların ve çıkışların gerçekleştiği bu piyasada projenin standartlarını korumak açısından çok zordur. Bu temel mimari ve sağlam dökümantasyon ile birçok standartizasyon sorunu çözülmüş olacaktır.

## Başlıklar
1. **İlk Temel Mimari**
    * Ana Mimari(ler)
    * Test eklentisi
2. **Pazarlama**
3. **Servis ve Bakım**
4. **Projenin gelecekteki seyri**

---

### 1. İlk Temel Mimari

#### 1.1 Ana Mimari(ler)
Piyasada kullanılan en yaygın yazılım mimarileri başlıca: Mikroservisler, katmanlı mimariler: onion, hexagonal; event driven, mvc... Bu mimarilerden birisi başlangıç projesi olarak seçilmeli ve tam kullanıma hazır hale getirilmelidir. Daha sonra pazarlama süreci de başarıya ulaşırsa diğer mimariler için de temel mimari oluşturulur.

Github veya başka bir version-control sistemi ile uyumlu çalışılmalıdır ve ürün teslim edilmeden önce tüm gerekli github altyapısı oluşturulmalıdır.

Genelde projeler bir backend (Server) ve bir veya birkaç client'ten oluşur (React, flutter, vue, swift...).

**1.1.1 Server**

Server tarafında ana mimari olarak olarak onion seçtiğimizi varsayalım. Bu noktada birden fazla ecosystem ile çalışmak gerekecek. Başlangıçta .NET ve Spring olabilir. Bu ecosystem'ler ile iki tane onion architecture projesi geliştirilecek. Bu projeler özellikle kendi ecosystem'lerinin eksikliklerine odaklı olarak tasarlanacak. Gelin biraz derine inelim:

>**.NET :** Bir .NET web projesi inşa edeceğinizde kendi routing yapınızı oluşturmaz, onun yerine attribute'lar ile http endpointlerini oluşturursunuz. Ayrıca gRPC gibi farklı protokollere kolay geçiş imkanınız da yoktur.

> **.NET :** Endpoint'ler için bir isimlendirme standardı yoktur. CRUD gibi çok yaygın işlemler için bile standart yoktur ve bu client tarafında işi zorlaştırmaktadır.

> **.NET :** Caching, logging, exception management gibi feature'lar için attribute gibi kolay kullanım altyapıları yoktur.

> **.NET :** EF Core sağlam bir ORM'dir ancak seeder ve faker gibi özelliklerden yoksundur.

> **.NET :** Security için hazır altyapıların yeterli olmaması

gibi sorunlar, temelde tüm bu özellikleri destekleyen, kullanımı çok kolay ve standart bir mimari ihtiyacının olduğunu bize anlatır.

Ayrıca sorting, paging ve filtering için hazır altyapılar kurulmalıdır. File management için tamamen kullanıma hazır bir yapı oluşturulmalıdır. CQRS için pipeline'lar, repository pattern için interceptor'lar ile authorization, logging, caching, validation gibi altyapılar kurulmalıdır. Dinamik data için çok gelişmiş bir translation yapısı kurulmalıdır. Manuel, External Translation API gibi farklı birkaç seçenek eklenmeli ve çok kolay bir şekilde elle değiştirilebilmelidir.

**1.1.2 Clients**

Büyük girişim projeleri genelde bir mobil uygulama ve birkaç web clienti ihtiyacı duyar. CMS, CDN, Landing, Dashboard, Mobile olmak üzere 5 farklı client altyapısının bir haftada firma için hazırlanıp verilmesi gerçekten de firma için maliyet açısından çok büyük artıdır. Neler sağlayacağız? Frontend tarafında firmanın seçeceği CSS framework, API standartlarına tam uygun temel api servisleri, responsive design için altyapı, hazır temalar, standardizasyon için lint konfigürasyonu ve dahası...

#### 1.2 Test eklentisi

Hem server hem client'ler için sağlam test'lerin yazılması, bunların eğitimi ve detaylı örneklendirilmesi çok önemlidir. Bu noktada testlerin bizin tarafımızdan bizim mimarimiz üzerinde yazılması için ayrı ücret teklif edilebilir.

### 2. Pazarlama
Bu projenin aslında en önemli kısmı pazarlamasıdır. Çünkü piyasada böyle bir projenin ün edinmesi, firmalar tarafından yatırıma değer bulunması için çok iyi reklam ve pazarlama yapılması gerekmektedir. Bu noktada girişim projelerinin bulunması ve reklamın onlara yönelik gerçekleştirilmesi de çok önemlidir çünkü bizim asıl müşterilerimiz girişim projelerinden olacaktır.

### 3. Servis ve Bakım
Proje teslim edildikten sonra belli bir süreye kadar servis ve bakım hizmeti verilmeli, daha sonra da olası buglar, versiyon güncellemeleri için de bakım sağlanmalıdır.  

---

### 4. Projenin gelecekteki seyri
Bu projenin pazarlanması başarılı olursa ve birkaç sağlam müşteri ile sağlam işler çıkarılırsa proje gerçekten çok büyüyecektir çünkü piyasada işlerinde tam profesyonel, birsürü farklı ecosystem ile çalışan ve çok büyük projelere destek olabilecek yetkinlikle yazılımcıların olduğu gerçeği fark edilecek ve bu projenin önceki referanslarına bakıldığında verdiği güven üst seviyelerde olacaktır.
