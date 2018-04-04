---
title: 5 부-전략을 공유 하는 실제 코드
ms.prod: xamarin
ms.assetid: 328D042A-FF78-A7B6-1574-B5AF49A1AADB
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: d5f639cffc8ff2d134731374bd72663fec81c6a0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="part-5---practical-code-sharing-strategies"></a>5 부-전략을 공유 하는 실제 코드

이 섹션에서는 일반적인 응용 프로그램 시나리오에 대 한 코드를 공유 하는 방법의 예를 제공 합니다.



## <a name="data-layer"></a>데이터 계층

데이터 계층 읽고 정보를 기록 하는 메서드 및 저장소 엔진으로 구성 됩니다. SQLite는 성능, 유연성 및 플랫폼 호환성에 대 한 데이터베이스 엔진 Xamarin 플랫폼 간 응용 프로그램에 권장 됩니다.
다양 한 Windows, Android, iOS 및 Mac. 포함 하는 플랫폼에서 실행


### <a name="sqlite"></a>SQLite

SQLite는 오픈 소스 데이터베이스 구현입니다. 소스 및 설명서에서 찾을 수 있습니다 [SQLite.org](http://www.sqlite.org/)합니다. SQLite 지원은 각 모바일 플랫폼에서 제공 됩니다.

-  **iOS** – 운영 체제에서 기본적으로 제공 합니다.
- **Android** – Android 2.2 (API 수준 10) 이후 운영 체제에서 기본적으로 제공 합니다.
- **Windows** – 참조는 [유니버설 Windows 플랫폼 확장에 대 한 SQLite](https://visualstudiogallery.msdn.microsoft.com/4913e7d5-96c9-4dde-a1a1-69820d615936)합니다.


모든 플랫폼에서 사용할 수 있는 데이터베이스 엔진, 된 경우에 데이터베이스에 액세스 하는 네이티브 메서드는 다릅니다. 하지만 두 iOS 및 Android Xamarin.iOS 또는 Xamarin.Android에서 사용할 수 있는 SQLite 액세스 하기 위한 기본 제공 Api를 제공 하며 (아마도 SQL 쿼리 자체를 문자열로 저장 하는 것으로 가정)이 아닌 코드를 공유 하는 기능을 제공 네이티브 SDK 메서드를 사용 하 여 . 에 대 한 기본 데이터베이스 기능 검색에 대 한 내용은 `CoreData` iOS 또는 Android의 `SQLiteOpenHelper` 이 문서에서 다루지 않으며 이러한 옵션은 플랫폼 간 때문에, 클래스입니다.



### <a name="adonet"></a>ADO.NET

Xamarin.iOS 및 Xamarin.Android를 모두 지원 `System.Data` 및 `Mono.Data.Sqlite` (참조는 Xamarin.iOS [설명서](~/ios/data-cloud/system.data.md) 자세한 정보에 대 한).
이러한 네임 스페이스를 사용 하 여 두 플랫폼 모두에서 작동 하는 ADO.NET 코드를 작성할 수 있습니다. 프로젝트의 참조를 포함 하도록 편집 `System.Data.dll` 및 `Mono.Data.Sqlite.dll` 코드에 다음 using 문을 추가 합니다.

```csharp
using System.Data;
using Mono.Data.Sqlite;
```

다음 샘플 코드 작동 합니다.

```csharp
string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "items.db3");
bool exists = File.Exists (dbPath);
if (!exists)
    SqliteConnection.CreateFile (dbPath);
var connection = new SqliteConnection ("Data Source=" + dbPath);
connection.Open ();
if (!exists) {
    // This is the first time the app has run and/or that we need the DB.
    // Copy a "template" DB from your assets, or programmatically create one like this:
    var commands = new[]{
        "CREATE TABLE [Items] (Key ntext, Value ntext);",
        "INSERT INTO [Items] ([Key], [Value]) VALUES ('sample', 'text')"
    };
    foreach (var command in commands) {
        using (var c = connection.CreateCommand ()) {
            c.CommandText = command;
            c.ExecuteNonQuery ();
        }
    }
}
// use `connection`... here, we'll just append the contents to a TextView
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT [Key], [Value] from [Items]";
    var r = contents.ExecuteReader ();
    while (r.Read ())
        Console.Write("\n\tKey={0}; Value={1}",
                r ["Key"].ToString (),
                r ["Value"].ToString ());
}
connection.Close ();
```

ADO.NET의 실제 구현은 분명히 다른 메서드 및 클래스 (이 예제는 예시용 으로만) 간에 분할 됩니다.



### <a name="sqlite-net--cross-platform-orm"></a>SQLite-NET – 플랫폼 간 ORM

클래스에서 모델링 된 데이터의 저장소를 단순화 하는 ORM (개체 관계형 매퍼 또는) 시도 합니다. 대신 수동으로 해당 테이블 만들기 또는 선택에는 SQL 쿼리를 작성, 수동으로에서 추출 된 데이터를 삽입 및 삭제 클래스 필드 및 속성, ORM을 수행 하는 코드의 계층을 추가 합니다. 리플렉션을 사용 하 여 클래스의 구조를 검사, ORM 자동으로 만들 수 테이블 및 열과 일치 하는 클래스 고 쿼리 된 데이터 읽기 및 쓰기를 생성 합니다. 따라서 응용 프로그램 코드를를 단순히 보내고 개체 인스턴스 내부에서 모든 SQL 작업 하는 ORM 검색할 수 있습니다.

SQLite NET 저장 하 고 SQLite의 클래스를 검색할 수 있는 간단한 ORM 역할을 합니다. 크로스 플랫폼 SQLite 액세스 컴파일러 지시문 및 다른 유용한 정보를 조합 하의 복잡성을 숨깁니다.

SQLite NET의 기능은 다음과 같습니다.

-  테이블 모델 클래스에 특성을 추가 하 여 정의 됩니다.
-  데이터베이스 인스턴스의 서브 클래스를 나타내는 `SQLiteConnection` , SQLite Net 라이브러리의 기본 클래스입니다.
-  쿼리 및 개체를 사용 하 여 삭제 된 데이터를 삽입할 수 있습니다. SQL 문이 없습니다는 (하지만 필요한 경우 SQL 문을 작성할 수 있습니다) 필요 합니다.
-  SQLite NET에서 반환 된 컬렉션에 대해 기본 Linq 쿼리를 수행할 수 있습니다.


소스 코드와 SQLite-순익에 대 한 설명서에서 제공 됩니다. [SQLite-github에 Net](https://github.com/praeclarum/sqlite-net) 두 사례 연구에서 구현 되었습니다. SQLite NET 코드의 간단한 예 (에서 *Tasky Pro* 사례 연구)는 다음과 같습니다.

첫째, 고 `TodoItem` 클래스 특성을 사용 하 여 데이터베이스의 기본 키 필드를 정의 하려면:

```csharp
public class TodoItem : IBusinessEntity
{
    public TodoItem () {}
    [PrimaryKey, AutoIncrement]
    public int ID { get; set; }
    public string Name { get; set; }
    public string Notes { get; set; }
    public bool Done { get; set; }
}
```

이 통해 한 `TodoItem` 테이블에 다음 코드 (및 없는 SQL 문)를 사용 하 여 생성 한 `SQLiteConnection` 인스턴스:

```csharp
CreateTable<TodoItem> ();
```

테이블의 데이터도에서 조작할 수 있습니다 다른 방법으로는 `SQLiteConnection` (다시 요구 하지 않고 SQL 문):

```csharp
Insert (TodoItem); // 'task' is an instance with data populated in its properties
Update (TodoItem); // Primary Key field must be populated for Update to work
Table<TodoItem>.ToList(); // returns all rows in a collection
```

전체 예제에 대 한 사례 연구 소스 코드를 참조 하십시오.



## <a name="file-access"></a>파일 액세스

파일 액세스는 특정 응용 프로그램의 핵심 부분입니다. 응용 프로그램 포함의 일부가 될 수 있는 파일의 일반적인 예:

-  SQLite 데이터베이스 파일입니다.
-  사용자에서 생성 된 데이터 (예: 텍스트, 이미지, 사운드, 비디오)입니다.
-  캐싱 (이미지, html 또는 PDF 파일)에 대 한 다운로드 한 데이터입니다.




### <a name="systemio-direct-access"></a>System.IO 직접 액세스

Xamarin.iOS 및 Xamarin.Android를 둘 다 수의 클래스를 사용 하는 파일 시스템 액세스는 `System.IO` 네임 스페이스입니다.

각 플랫폼에는 고려 사항에는 고려해 야 하는 다양 한 액세스 제한 사항을 갖습니다.

-  iOS 응용 프로그램은 매우 제한 된 파일 시스템 액세스 권한이 있는 샌드박스에서 실행 합니다. Apple 추가 기능을 특정 위치에 백업 (및 없는 다른)를 지정 하 여 파일 시스템을 사용 해야 합니다. 참조는 [Xamarin.iOS에서 파일 시스템 작업](~/ios/app-fundamentals/file-system.md) 자세한 내용은 가이드입니다.
-  Android 응용 프로그램에 관련 된 특정 디렉터리에도 액세스를 제한 하지만 지원 합니다. 외부 미디어 (예:. SD 카드) 공유 데이터에 액세스 합니다.
-  Windows Phone 8 (Silverlight) 직접 파일 액세스를 허용 하지 않습니다 – 파일을 다시 처리를 사용 하 여 `IsolatedStorage`합니다.
-  WinRT Windows 8.1 및 Windows 10 UWP 프로젝트를 통해 비동기 파일 작업은 제공 `Windows.Storage` 다른 플랫폼에서 다른 Api입니다.

#### <a name="example-for-ios-and-android"></a>IOS 및 Android에 대 한 예제

쓰고 텍스트 파일을 읽고는 간단한 예는 다음과 같습니다.
사용 하 여 `Environment.GetFolderPath` 동일한 코드가 반환 하는 각 해당 파일 시스템 규칙에 따라 올바른 디렉터리 iOS 및 Android에서 실행 되도록 허용 합니다.

```csharp
string filePath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "MyFile.txt");
System.IO.File.WriteAllText (filePath, "Contents of text file");
Console.WriteLine (System.IO.ReadAllText (filePath));
```

참조는 xamarin.ios [파일 시스템 작업](~/ios/app-fundamentals/file-system.md) iOS 관련 파일 시스템 기능에 대 한 자세한 내용은 문서입니다. 플랫폼 간 파일 액세스 코드를 작성할 때 일부 파일 시스템 대/소문자와 다른 디렉터리 구분 기호가 기억 합니다. 것이 좋습니다 항상 파일 이름에 대 한 동일한 대/소문자를 사용 하 고 `Path.Combine()` 파일 또는 디렉터리 경로 생성할 때 메서드.



### <a name="windowsstorage-for-windows-8-and-windows-10"></a>Windows 8 및 Windows 10에 대 한 Windows.Storage

*Xamarin.Forms 사용 하 여 모바일 앱 만들기* [책](https://developer.xamarin.com/r/xamarin-forms/book/)
[장 20입니다. Async 및 파일 I/O](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf) 포함 [Windows 8.1 및 Windows 10에 대 한 샘플](https://github.com/xamarin/xamarin-forms-book-preview-2/tree/master/Chapter20)합니다.

사용 하는 [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) 와 지원 되는 Api를 사용 하 여 이러한 플랫폼에서 파일을 파일 수:

```csharp
StorageFolder localFolder = ApplicationData.Current.LocalFolder;
IStorageFile storageFile = await localFolder.CreateFileAsync("MyFile.txt",
                                        CreationCollisionOption.ReplaceExisting);
await FileIO.WriteTextAsync(storageFile, "Contents of text file");
```

참조는 [설명서 장](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf) 자세한 세부 정보에 대 한 합니다.


<a name="Isolated_Storage" />

### <a name="isolated-storage-on-windows-phone-7--8-silverlight"></a>Windows Phone 7 및 8 (Silverlight)에서 격리 된 저장소

격리 된 저장소는 일반적인 API 저장 하 고 모든 iOS, Android, 및 이전 Windows Phone 플랫폼 간에 파일을 로드 합니다.

그는 Xamarin.iOS 및 Xamarin.Android 쓸 일반적인 가져온 파일 액세스 코드에서 구현 된 Windows Phone (Silverlight)에서 파일 액세스를 위한 기본 메커니즘입니다. `System.IO.IsolatedStorage` 클래스에는 세 플랫폼 모두에서 참조할 수 있습니다는 [공유 프로젝트](~/cross-platform/app-fundamentals/shared-projects.md)합니다.

참조는 [Windows Phone 대 한 격리 된 저장소 개요](http://msdn.microsoft.com/en-us/library/windowsphone/develop/ff402541(v=vs.105).aspx) 자세한 정보에 대 한 합니다.

격리 된 저장소 Api에서 사용할 수 없는 [이식 가능한 클래스 라이브러리](~/cross-platform/app-fundamentals/pcl.md)합니다. PCL에 대 한 대체 방법 중 하나로 [PCLStorage NuGet](https://pclstorage.codeplex.com/)



### <a name="cross-platform-file-access-in-pcls"></a>PCLs에서 플랫폼 간 파일 액세스

PCL 호환 Nuget – 이기도 [PCLStorage](https://www.nuget.org/packages/PCLStorage/) – Xamarin 지원 되는 플랫폼 및 최신 Windows Api에 대 한 해당 시설을 운영 플랫폼 간 파일 액세스 합니다.


## <a name="network-operations"></a>네트워크 작업

대부분의 모바일 응용 프로그램에는 예를 들어 네트워킹 구성 요소를 포함 됩니다.

-  이미지를 다운로드, 비디오 및 오디오 않음 (예: 미리 보기, 사진, 음악)입니다.
-  않음 (예: 문서를 다운로드합니다. HTML, PDF)입니다.
-  사용자 데이터 (예: 사진 또는 텍스트)를 업로드 합니다.
-  웹 서비스 또는 제 3 자 (포함 하 여 SOAP, XML 또는 JSON) Api에 액세스합니다.


네트워크 리소스에 액세스 하기 위한 몇 가지 다른 클래스를 제공 하는.NET Framework: `HttpClient`, `WebClient`, 및 `HttpWebRequest`합니다.

### <a name="httpclient"></a>HttpClient

`HttpClient` 클래스에 `System.Net.Http` 네임 스페이스는 Xamarin.iOS, Xamarin.Android, 및 대부분의 Windows 플랫폼에서 사용할 수 있습니다. 한 [Microsoft HTTP 클라이언트 라이브러리 Nuget](https://www.nuget.org/packages/Microsoft.Net.Http/) 이식 가능한 클래스 라이브러리 (및 Windows Phone 8 Silverlight)으로이 API를 가져오는 데 사용할 수 있는 합니다.

```csharp
var client = new HttpClient();
var request = new HttpRequestMessage(HttpMethod.Get, "https://xamarin.com");
var response = await myClient.SendAsync(request);
```

### <a name="webclient"></a>WebClient

`WebClient` 클래스는 원격 서버에서 원격 데이터를 검색 하는 단순 API를 제공 합니다.

유니버설 Windows Platofrm 작업 *해야* Xamarin.iOS 및 Xamarin.Android (을 사용할 수 있도록 백그라운드 스레드에서) 동기 작업을 지원 하기는 하지만, 비동기 수 있습니다.

간단한 비동기에 대 한 코드 `WebClient` 작업은:

```csharp
var webClient = new WebClient ();
webClient.DownloadStringCompleted += (sender, e) =>
{
    var resultString = e.Result;
    // do something with downloaded string, do UI interaction on main thread
};
webClient.Encoding = System.Text.Encoding.UTF8;
webClient.DownloadStringAsync (new Uri ("http://some-server.com/file.xml"));
```

 `WebClient` 또한 `DownloadFileCompleted` 및 `DownloadFileAsync` 이진 데이터를 검색 합니다.

<a name="HttpWebRequest" />

### <a name="httpwebrequest"></a>HttpWebRequest

`HttpWebRequest` 보다 더 많은 사용자 지정 제공 `WebClient` 결과적으로 사용 하 여 더 많은 코드가 필요 합니다.

동기 간단한에 대 한 코드 `HttpWebRequest` 작업은:

```csharp
var request = HttpWebRequest.Create(@"http://some-server.com/file.xml ");
request.ContentType = "text/xml";
request.Method = "GET";
using (HttpWebResponse response = request.GetResponse() as HttpWebResponse)
{
    if (response.StatusCode != HttpStatusCode.OK)
        Console.WriteLine("Error fetching data. Server returned status code: {0}", response.StatusCode);
    using (StreamReader reader = new StreamReader(response.GetResponseStream()))
    {
        var content = reader.ReadToEnd();
        // do something with downloaded string, do UI interaction on main thread
    }
}
```

한 예로 우리의 [웹 서비스 설명서](~/cross-platform/data-cloud/web-services/index.md)합니다.

 <a name="Reachability" />


### <a name="reachability"></a>연결 가능성

모바일 장치에서 다양 한 네트워크 조건에서 빠른 Wi-fi 또는 4g 연결 आ स ा 불량 영역 및 느린 가장자리 데이터 연결을 작동합니다. 이 인해 것이 좋습니다 네트워크를 사용할 수 있는지 및 그렇다면 어떤 유형의 네트워크는 경우 원격 서버에 연결 하기 전에 감지 합니다.

이러한 경우에는 모바일 앱 걸릴 수 있습니다 하는 동작은 다음과 같습니다.

-  네트워크를 사용할 수 없는 경우에 사용자 하시기 바랍니다. 그리고가 않음 (예: 사용할 수동으로 있어야 하는 경우 비행기 모드 또는 Wi-fi 해제) 다음 문제를 해결할 수 있습니다.
-  3g 연결을 사용 하는 경우 응용 프로그램이 다르게 동작할 수 있습니다 (예를 들어, 사과 허용 하지 않음 앱 3g 이상 다운로드할 20mb 보다 큰). 응용 프로그램 과도 한 다운로드에 대 한 사용자에 게 경고 하기 위한이 정보를 사용할 수 큰 파일을 검색할 때 제한 시간이 있습니다.
-  네트워크를 사용할 수 있는 경우에 것이 좋습니다 다른 요청을 시작 하기 전에 대상 서버와 연결을 확인 합니다. 이 시간을 초과 하는 응용 프로그램의 네트워크 작업을 반복 해 서 작업이 방지 되 고 또한 사용자에 게 표시할 더 자세한 오류 메시지를 허용 합니다.


한 [Xamarin.iOS 샘플](https://github.com/xamarin/monotouch-samples/tree/master/ReachabilitySample) 사용 가능한 (Apple의 기준으로 하는 [연결성 샘플 코드](http://developer.apple.com/library/ios/#samplecode/Reachability/Introduction/Intro.html) ) 네트워크 가용성을 발견할 수 있도록 합니다.


## <a name="webservices"></a>웹 서비스

설명서를 참조 하십시오. [웹 서비스 작업](~/cross-platform/data-cloud/web-services/index.md)는에서는 나머지 부분에서는 액세스 Xamarin.iOS를 사용 하 여 SOAP 및 WCF 끝점입니다. 하지만 직접 만든 웹 서비스 요청에 가능한 이며이 훨씬 더 간단 하 게, Azure, RestSharp, ServiceStack 등을 사용할 수 있는 라이브러리는 응답을 구문 분석 합니다. Xamarin 앱에도 기본 WCF 작업을 액세스할 수 있습니다.

### <a name="azure"></a>Azure

Microsoft Azure는 다양 한 데이터 저장소 및 동기화, 푸시 알림을 포함 하는 모바일 앱에 대 한 서비스를 제공 하는 클라우드 플랫폼입니다.

방문 [azure.microsoft.com](https://azure.microsoft.com/) 무료로 시도 합니다.

### <a name="restsharp"></a>RestSharp

RestSharp는 웹 서비스에 대 한 액세스를 간소화 하는 REST 클라이언트를 제공 하는 모바일 응용 프로그램에 포함 될 수 있는.NET 라이브러리입니다. 데이터를 요청 하 고 나머지 응답을 구문 분석 하는 단순 API를 제공 하 여는 데 도움이 됩니다. RestSharp 유용할 수 있습니다.

[RestSharp 웹 사이트](http://restsharp.org/) 포함 [설명서](https://github.com/restsharp/RestSharp/wiki) RestSharp를 사용 하 여 REST 클라이언트를 구현 하는 방법에 있습니다.
에 Xamarin.iOS 및 Xamarin.Android 예제를 제공 하는 RestSharp [github](https://github.com/restsharp/RestSharp/)합니다.

또한에 Xamarin.iOS 코드 조각 우리의 [웹 서비스 설명서](~/cross-platform/data-cloud/web-services/index.md)합니다.

 <a name="ServiceStack" />


### <a name="servicestack"></a>ServiceStack

RestSharp, 달리 ServiceStack를 해당 서비스에 액세스 하는 모바일 응용 프로그램에서 구현 될 수 있는 클라이언트 라이브러리 뿐 아니라 웹 서비스를 호스팅하는 서버 쪽 솔루션 모두는 합니다.

[ServiceStack 웹 사이트](http://servicestack.net/) 문서와 코드 샘플에 있는 프로젝트 및 링크의 용도 설명 합니다. 예에는 액세스할 수 있는 다양 한 클라이언트 쪽 응용 프로그램 뿐 아니라 웹 서비스의 완전 한 서버 쪽 구현을 포함 합니다.

한 [Xamarin.iOS 예제](http://www.servicestack.net/monotouch/remote-info/) ServiceStack 웹 사이트 및의 코드 조각에 우리의 [웹 서비스 설명서](~/cross-platform/data-cloud/web-services/index.md)합니다.


### <a name="wcf"></a>WCF

Xamarin 도구 일부 Windows Communication Foundation (WCF) 서비스를 사용할 수 있습니다. 일반적으로 Xamarin Silverlight 런타임과 함께 제공 되는 WCF의 동일한 클라이언트 하위 집합을 지원 합니다. WCF의 가장 일반적인 인코딩 및 프로토콜 구현을 여기에:는 HTTP 통해 텍스트 인코딩된 SOAP 메시지의 전송 프로토콜을 사용 하는 `BasicHttpBinding`합니다.

크기와 WCF 프레임 워크의 복잡성 때문에 Xamarin 클라이언트 하위 집합 도메인에서 지 원하는 범위 밖에 서 해당 하는 현재와 미래의 서비스 구현 있을 수 있습니다. 또한 WCF 지원 하려면 프록시를 생성 하는 Windows 환경에만 사용할 수 있는 도구를 사용 합니다.

 <a name="Threading" />


## <a name="threading"></a>스레딩

모바일 응용 프로그램에 대 한 중요 한 응용 프로그램의 응답성은 사용자는 응용 프로그램이 로드 하 고 좀 더 빠르게 수행 됩니다. 사용자 입력을 적용 하는 중지 것이 중요 하지 네트워크 요청과 같은 장기 실행 차단 호출 또는 느린 로컬 작업 (예: 파일 압축을 푼) UI 스레드를 계속 사용 하려면 응용 프로그램에서 충돌이 발생을 나타내기 위해 표시 되는 '고정된' 화면입니다. 특히 시작 프로세스에는 장기 실행 작업 없어야 합니다.-모든 모바일 플랫폼 로드 시간이 너무 오래 걸리는 하는 응용 프로그램을 중지 합니다.

즉, '진행률 표시기' 또는 그렇지 않은 경우 '사용 가능한' 되는 UI를 표시 하려면 빠른 및 비동기 작업 백그라운드 작업을 수행할 사용자 인터페이스를 구현 해야 합니다. 백그라운드 작업 실행을 의미 하는 백그라운드 작업 요구 하는 진행률을 나타내는 주 스레드 알릴 수 있도록 하거나 완료 될 때 스레드를 사용을 해야 합니다.

 <a name="Parallel_Task_Library" />


### <a name="parallel-task-library"></a>병렬 작업 라이브러리

병렬 작업 라이브러리를 사용 하 여 만든 작업 비동기적으로 실행 하 고 해당 사용자 인터페이스를 차단 하지 않고 장기 실행 작업을 트리거하는 데 매우 유용 하므로 호출 스레드에서 반환 수 있습니다.

간단한 병렬 작업 작업을 다음과 같이 표시 될 수 있습니다.

```csharp
using System.Threading.Tasks;
void MainThreadMethod ()
{
    Task.Factory.StartNew (() => wc.DownloadString ("http://...")).ContinueWith (
        t => label.Text = t.Result, TaskScheduler.FromCurrentSynchronizationContext()
    );
}
```

키가 `TaskScheduler.FromCurrentSynchronizationContext()` 하는 메서드를 호출 하는 스레드의 SynchronizationContext.Current 다시 사용 (여기서 실행 하는 주 스레드에 `MainThreadMethod`)를 해당 스레드로 백 호출을 마샬링하는 방법으로 합니다. 즉, 실행할는 메서드가 UI 스레드에서 호출 되는 경우는 `ContinueWith` UI 스레드에서 작업 합니다.

코드가 다른 스레드에서 작업의 시작을 UI 스레드에 대 한 참조를 만들려면 다음과 같은 패턴을 사용 하 고 작업을 다시 호출할 여전히 수 있습니다.

```csharp
static Context uiContext = TaskScheduler.FromCurrentSynchronizationContext();
```

 <a name="Invoking_on_the_UI_Thread" />


### <a name="invoking-on-the-ui-thread"></a>UI 스레드에서 호출

병렬 작업 라이브러리를 사용 하지 않는 하는 코드의 경우 각 플랫폼에는을 UI 스레드에 다시 마샬링 작업에 대 한 구문:

-  **iOS** – `owner.BeginInvokeOnMainThread(new NSAction(action))`
-  **Android** – `owner.RunOnUiThread(action)`
-  **Xamarin.Forms** – `Device.BeginInvokeOnMainThread(action)`
-  **Windows** – `Deployment.Current.Dispatcher.BeginInvoke(action)`



IOS 및 Android 구문을 사용할 즉, 코드를 UI 스레드에서 콜백 하는 메서드를이 개체를 전달 해야 할 수 있도록 '컨텍스트' 클래스가 필요 합니다.

공유 코드에 UI 스레드 호출 하려면 수행는 [IDispatchOnUIThread 예제](http://www.slideshare.net/follesoe/cross-platform-mobile-apps-using-net) (의 [ @follesoe ](http://jonas.follesoe.no/)). 선언 하 고 프로그램을 `IDispatchOnUIThread` 공유 코드에서 인터페이스를 다음 아래 그림과 같이 플랫폼 특정 클래스를 구현 합니다.

```csharp
// program to the interface in shared code
public interface IDispatchOnUIThread {
    void Invoke (Action action);
}
// iOS
public class DispatchAdapter : IDispatchOnUIThread {
    public readonly NSObject owner;
    public DispatchAdapter (NSObject owner) {
        this.owner = owner;
    }
    public void Invoke (Action action) {
        owner.BeginInvokeOnMainThread(new NSAction(action));
    }
}
// Android
public class DispatchAdapter : IDispatchOnUIThread {
    public readonly Activity owner;
    public DispatchAdapter (Activity owner) {
        this.owner = owner;
    }
    public void Invoke (Action action) {
        owner.RunOnUiThread(action);
    }
}
// WP7
public class DispatchAdapter : IDispatchOnUIThread {
    public void Invoke (Action action) {
        Deployment.Current.Dispatcher.BeginInvoke(action);
    }
}
```

Xamarin.Forms 사용 해야 [ `Device.BeginInvokeOnMainThread` ](~/xamarin-forms/platform/device.md#Device_BeginInvokeOnMainThread) 공통 코드 (공유 프로젝트 또는 PCL).

 <a name="Platform_and_Device_Capabilities_and_Degradation" />


## <a name="platform-and-device-capabilities-and-degradation"></a>플랫폼 및 장치 기능 및 성능 저하

더 구체적인 예제는 다양 한 기능을 다루는 플랫폼 기능 설명서에 제공 됩니다. 다양 한 기능 및 응용 프로그램을 최대한 사용할 수 없지만 경우에 유용한 사용자 환경을 제공 하는 응용 프로그램을 적절 하 게 저하 하는 방법을 검색를 다룹니다.
