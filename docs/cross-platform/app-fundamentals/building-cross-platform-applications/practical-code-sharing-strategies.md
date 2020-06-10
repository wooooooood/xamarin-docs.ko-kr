---
title: 5부 - 실제 코드 공유 전략
description: 이 문서에서는 데이터베이스, 파일 액세스, 네트워크 작업, 비동기 코드 등의 시나리오에 대 한 실용적인 코드 공유 전략을 설명 합니다.
ms.prod: xamarin
ms.assetid: 328D042A-FF78-A7B6-1574-B5AF49A1AADB
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: fd0e48c8f954ba926c5e1b5dc3a1c9bf6aab8c54
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571196"
---
# <a name="part-5---practical-code-sharing-strategies"></a>5부 - 실제 코드 공유 전략

이 섹션에서는 일반적인 응용 프로그램 시나리오에 대 한 코드를 공유 하는 방법의 예를 제공 합니다.

## <a name="data-layer"></a>데이터 계층

데이터 계층은 정보를 읽고 쓰기 위한 저장소 엔진과 메서드로 구성 됩니다. 성능, 유연성 및 플랫폼 간 호환성을 위해 SQLite 데이터베이스 엔진은 Xamarin 플랫폼 간 응용 프로그램에 권장 됩니다.
Windows, Android, iOS 및 Mac을 비롯 한 다양 한 플랫폼에서 실행 됩니다.

### <a name="sqlite"></a>SQLite

SQLite는 오픈 소스 데이터베이스 구현입니다. 원본 및 설명서는 [SQLite.org](https://www.sqlite.org/)에서 찾을 수 있습니다. SQLite 지원은 각 모바일 플랫폼에서 사용할 수 있습니다.

- **iOS** – 운영 체제에 기본 제공 됩니다.
- **Android** – android 2.2 이후 운영 체제에 기본 제공 됩니다 (API 수준 10).
- **Windows** – [유니버설 Windows 플랫폼 확장은 SQLite](https://visualstudiogallery.msdn.microsoft.com/4913e7d5-96c9-4dde-a1a1-69820d615936)를 참조 하세요.

모든 플랫폼에서 데이터베이스 엔진을 사용할 수 있는 경우에도 데이터베이스에 액세스 하는 네이티브 메서드는 다릅니다. IOS와 Android는 모두 Xamarin.ios 또는 Xamarin.ios에서 사용할 수 있는 SQLite에 액세스 하는 기본 제공 Api를 제공 하지만, 네이티브 SDK 메서드를 사용 하 여 코드를 공유 하는 기능을 제공 하지 않습니다. 단, SQL 쿼리 자체가 문자열로 저장 되었다고 가정 합니다. 네이티브 데이터베이스 기능에 대 한 자세한 내용은 `CoreData` iOS 또는 Android의 클래스에서를 검색 `SQLiteOpenHelper` 합니다. 이러한 옵션은 플랫폼 간이 아니기 때문에이 문서에서는 다루지 않습니다.

### <a name="adonet"></a>ADO.NET

Xamarin.ios와 Xamarin.ios는 모두 및를 지원 `System.Data` `Mono.Data.Sqlite` 합니다. 자세한 내용은 xamarin.ios [설명서](~/ios/data-cloud/system.data.md) 를 참조 하세요.
이러한 네임 스페이스를 사용 하 여 두 플랫폼 모두에서 작동 하는 ADO.NET 코드를 작성할 수 있습니다. 및를 포함 하도록 프로젝트의 참조를 편집 하 `System.Data.dll` `Mono.Data.Sqlite.dll` 고 다음 using 문을 코드에 추가 합니다.

```csharp
using System.Data;
using Mono.Data.Sqlite;
```

그러면 다음 샘플 코드가 작동 합니다.

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

ADO.NET의 실제 구현은 다양 한 메서드와 클래스에 걸쳐 분리 됩니다 (이 예제는 데모용 으로만 사용 됨).

### <a name="sqlite-net--cross-platform-orm"></a>SQLite-NET – 플랫폼 간 ORM

ORM (또는 개체 관계형 매퍼)은 클래스에서 모델링 된 데이터의 저장소를 단순화 하려고 합니다. 테이블을 만들거나 클래스 필드 및 속성에서 수동으로 추출 된 데이터를 선택, 삽입 및 삭제 하는 SQL 쿼리를 수동으로 작성 하는 대신 ORM은이를 수행 하는 코드 계층을 추가 합니다. ORM을 사용 하 여 클래스의 구조를 검사 하면 ORM에서 클래스와 일치 하는 테이블과 열을 자동으로 만들고 쿼리를 생성 하 여 데이터를 읽고 쓸 수 있습니다. 이를 통해 응용 프로그램 코드는 단지 ORM으로 개체 인스턴스를 보내고 검색할 수 있으며,이는 내부적으로 모든 SQL 작업을 처리 합니다.

SQLite-NET은 SQLite에 클래스를 저장 하 고 검색할 수 있는 간단한 ORM 역할을 합니다. 컴파일러 지시문 및 기타 트릭의 조합을 사용 하 여 플랫폼 간 SQLite 액세스의 복잡성을 숨깁니다.

SQLite의 기능:

- 테이블은 모델 클래스에 특성을 추가 하 여 정의 됩니다.
- 데이터베이스 인스턴스는 SQLite 라이브러리의 기본 클래스인의 서브 클래스로 표현 됩니다 `SQLiteConnection` .
- 개체를 사용 하 여 데이터를 삽입, 쿼리 및 삭제할 수 있습니다. 필요한 경우 sql 문을 작성할 수 있지만 SQL 문은 필요 하지 않습니다.
- SQLite-NET에서 반환 된 컬렉션에 대해 기본 Linq 쿼리를 수행할 수 있습니다.

SQLite의 소스 코드 및 설명서는 [github의 sqlite](https://github.com/praeclarum/sqlite-net) 에서 사용할 수 있으며 사례 연구에서 구현 되었습니다. 예를 들어 *Tasky Pro* 사례 연구에서 SQLITE-NET 코드의 간단한 예를 볼 수 있습니다.

먼저 클래스는 `TodoItem` 특성을 사용 하 여 필드를 데이터베이스 기본 키로 정의 합니다.

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

이를 통해 `TodoItem` 인스턴스에 다음 코드 줄 (SQL 문 없음)을 사용 하 여 테이블을 만들 수 있습니다 `SQLiteConnection` .

```csharp
CreateTable<TodoItem> ();
```

테이블의 데이터는 `SQLiteConnection` SQL 문을 요구 하지 않고에서 다른 메서드를 사용 하 여 조작할 수도 있습니다.

```csharp
Insert (TodoItem); // 'task' is an instance with data populated in its properties
Update (TodoItem); // Primary Key field must be populated for Update to work
Table<TodoItem>.ToList(); // returns all rows in a collection
```

전체 예제는 사례 연구 소스 코드를 참조 하세요.

## <a name="file-access"></a>파일 액세스

파일 액세스는 모든 응용 프로그램의 주요 부분입니다. 응용 프로그램에 포함 될 수 있는 파일의 일반적인 예는 다음과 같습니다.

- SQLite 데이터베이스 파일.
- 사용자가 생성 한 데이터 (텍스트, 이미지, 소리, 비디오)
- 캐싱에 대 한 데이터 (이미지, html 또는 PDF 파일)를 다운로드 합니다.

### <a name="systemio-direct-access"></a>System.IO 직접 액세스

Xamarin.ios와 Xamarin.ios는 모두 네임 스페이스의 클래스를 사용 하 여 파일 시스템에 액세스할 수 `System.IO` 있습니다.

각 플랫폼에는 고려해 야 할 다른 액세스 제한이 있습니다.

- iOS 응용 프로그램은 파일 시스템 액세스가 매우 제한 된 샌드박스에서 실행 됩니다. Apple은 백업 된 특정 위치 (및 기타 다른 위치)를 지정 하 여 파일 시스템을 사용 하는 방법을 추가로 지정 합니다. 자세한 내용은 [xamarin.ios에서 파일 시스템 사용](~/ios/app-fundamentals/file-system.md) 가이드를 참조 하세요.
- 또한 Android는 응용 프로그램과 관련 된 특정 디렉터리에 대 한 액세스를 제한 하지만 외부 미디어도 지원 합니다 (예: SD 카드)를 만들고 공유 데이터에 액세스 합니다.
- Windows Phone 8 (Silverlight)는 직접 파일 액세스를 허용 하지 않습니다. 파일은를 사용 하는 경우에만 조작할 수 있습니다 `IsolatedStorage` .
- Windows 8.1 WinRT 및 Windows 10 UWP 프로젝트는 `Windows.Storage` 다른 플랫폼과 다른 api를 통해서만 비동기 파일 작업을 제공 합니다.

#### <a name="example-for-ios-and-android"></a>IOS 및 Android에 대 한 예제

텍스트 파일을 쓰고 읽는 간단한 예제는 다음과 같습니다.
를 사용 `Environment.GetFolderPath` 하면 동일한 코드를 iOS 및 Android에서 실행할 수 있으며, 각각은 파일 시스템 규칙에 따라 올바른 디렉터리를 반환 합니다.

```csharp
string filePath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "MyFile.txt");
System.IO.File.WriteAllText (filePath, "Contents of text file");
Console.WriteLine (System.IO.File.ReadAllText (filePath));
```

IOS 관련 파일 시스템 기능에 대 한 자세한 내용은 [파일 시스템](~/ios/app-fundamentals/file-system.md) 문서를 참조 하세요. 플랫폼 간 파일 액세스 코드를 작성할 때 일부 파일 시스템은 대/소문자를 구분 하 고 다른 디렉터리 구분 기호를 사용 한다는 점에 주의 해야 합니다. `Path.Combine()`파일 또는 디렉터리 경로를 생성할 때 파일 이름 및 메서드에 대해 항상 동일한 대/소문자를 사용 하는 것이 좋습니다.

### <a name="windowsstorage-for-windows-8-and-windows-10"></a>Windows 8 및 Windows 10 용 windows 저장소

*Xamarin.ios 설명서를 사용 하 여 Mobile Apps를 만듭니다* [book](https://developer.xamarin.com/r/xamarin-forms/book/) 
 [. 20. Async 및 File i/o](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf) [에는 Windows 8.1 및 Windows 10에 대 한 샘플이](https://github.com/xamarin/xamarin-forms-book-preview-2/tree/master/Chapter20)포함 되어 있습니다.

를 사용 하면 [`DependencyService`](~/xamarin-forms/app-fundamentals/dependency-service/index.md) 지원 되는 api를 사용 하 여 이러한 플랫폼의 파일을 읽고 파일을 읽을 수 있습니다.

```csharp
StorageFolder localFolder = ApplicationData.Current.LocalFolder;
IStorageFile storageFile = await localFolder.CreateFileAsync("MyFile.txt",
                                        CreationCollisionOption.ReplaceExisting);
await FileIO.WriteTextAsync(storageFile, "Contents of text file");
```

자세한 내용은 [책 장](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf) 을 참조 하세요.

<a name="Isolated_Storage"></a>

### <a name="isolated-storage-on-windows-phone-7--8-silverlight"></a>Windows Phone 7 & 8 (Silverlight)의 격리 된 저장소

격리 된 저장소는 모든 iOS, Android 및 이전 Windows Phone 플랫폼에서 파일을 저장 하 고 로드 하는 일반적인 API입니다.

공용 파일 액세스 코드를 작성할 수 있도록 Xamarin.ios 및 Xamarin.ios에서 구현 된 Windows Phone (Silverlight)의 파일 액세스에 대 한 기본 메커니즘입니다. 이 `System.IO.IsolatedStorage` 클래스는 [공유 프로젝트](~/cross-platform/app-fundamentals/shared-projects.md)의 세 플랫폼에서 모두 참조할 수 있습니다.

자세한 내용은 [격리 된 저장소 Windows Phone 개요](https://msdn.microsoft.com/library/windowsphone/develop/ff402541(v=vs.105).aspx) 를 참조 하세요.

격리 된 저장소 Api는 [이식 가능한 클래스 라이브러리](~/cross-platform/app-fundamentals/pcl.md)에서 사용할 수 없습니다. PCL의 한 가지 대안은 [Pclstorage NuGet](https://pclstorage.codeplex.com/) 입니다.

### <a name="cross-platform-file-access-in-pcls"></a>PCLs에서 플랫폼 간 파일 액세스

또한 Xamarin 지원 플랫폼 및 최신 Windows Api에 대 한 플랫폼 간 파일 액세스 기능을 지 원하는 PCL 호환 NuGet – [Pclstorage](https://www.nuget.org/packages/PCLStorage/) 가 있습니다.

## <a name="network-operations"></a>네트워크 작업

대부분의 모바일 응용 프로그램에는 다음과 같은 네트워킹 구성 요소가 있습니다.

- 이미지, 비디오 및 오디오를 다운로드 합니다 (예: 축소판 그림, 사진, 음악).
- 문서 다운로드 (예: HTML, PDF).
- 사용자 데이터 (예: 사진 또는 텍스트)를 업로드 합니다.
- 웹 서비스 또는 타사 Api (SOAP, XML 또는 JSON 포함)에 액세스 합니다.

.NET Framework는 네트워크 리소스에 액세스 하기 위한 몇 가지 다른 클래스 `HttpClient` , `WebClient` 및를 제공 `HttpWebRequest` 합니다.

### <a name="httpclient"></a>HttpClient

`HttpClient`네임 스페이스의 클래스는 `System.Net.Http` Xamarin.ios, xamarin Android 및 대부분의 Windows 플랫폼에서 사용할 수 있습니다. 이 API를 이식 가능한 클래스 라이브러리 (및 Windows Phone 8 Silverlight)로 가져오는 데 사용할 수 있는 [MICROSOFT HTTP 클라이언트 라이브러리 NuGet](https://www.nuget.org/packages/Microsoft.Net.Http/) 이 있습니다.

```csharp
var client = new HttpClient();
var request = new HttpRequestMessage(HttpMethod.Get, "https://xamarin.com");
var response = await myClient.SendAsync(request);
```

### <a name="webclient"></a>WebClient

`WebClient`클래스는 원격 서버에서 원격 데이터를 검색 하는 간단한 API를 제공 합니다.

Xamarin.ios 및 Xamarin이 동기 작업 (백그라운드 스레드에서 수행할 수 있음)을 지원 하더라도 유니버설 Windows 플랫폼 작업은 비동기적 이어야 *합니다* .

간단한 보조적 작업에 대 한 코드는 `WebClient` 다음과 같습니다.

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

 `WebClient`또한에 `DownloadFileCompleted` 는 `DownloadFileAsync` 이진 데이터를 검색 하기 위한 및가 있습니다.

<a name="HttpWebRequest"></a>

### <a name="httpwebrequest"></a>HttpWebRequest

`HttpWebRequest`에는 보다 많은 사용자 지정이 제공 되므로 더 많은 `WebClient` 코드를 사용 해야 합니다.

간단한 동기 작업의 코드는 `HttpWebRequest` 다음과 같습니다.

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

[웹 서비스 설명서](~/cross-platform/data-cloud/web-services/index.md)에는 예제가 있습니다.

 <a name="Reachability"></a>

### <a name="reachability"></a>연결 가능성

모바일 장치는 fast wi-fi 또는 4G 연결에서 잘못 된 수신 영역 및 느린 데이터 연결에 이르는 다양 한 네트워크 조건으로 작동 합니다. 따라서 원격 서버에 연결을 시도 하기 전에 네트워크를 사용할 수 있는지 여부와 사용 가능한 네트워크 유형을 검색 하는 것이 좋습니다.

모바일 앱에서 수행할 수 있는 작업은 다음과 같습니다.

- 네트워크를 사용할 수 없는 경우 사용자에 게 알려 주십시오. 수동으로 비활성화 한 경우 (예: 비행기 모드를 설정 하거나 Wi-fi를 끌 때 문제를 해결할 수 있습니다.
- 연결이 3G 인 경우 응용 프로그램은 다르게 동작할 수 있습니다. 예를 들어 Apple에서는 20Mb 보다 큰 앱을 3G를 통해 다운로드할 수 없습니다. 응용 프로그램은이 정보를 사용 하 여 많은 파일을 검색할 때 과도 한 다운로드 시간을 사용자에 게 경고할 수 있습니다.
- 네트워크를 사용할 수 있는 경우에도 다른 요청을 시작 하기 전에 대상 서버와의 연결을 확인 하는 것이 좋습니다. 이렇게 하면 앱의 네트워크 작업이 반복적으로 시간 초과 되는 것을 방지 하 고 더 많은 정보를 제공 하는 오류 메시지가 사용자에 게 표시 될 수 있습니다.

## <a name="webservices"></a>WebServices

Xamarin.ios를 사용 하 여 REST, SOAP 및 WCF 끝점에 액세스 하는 [웹 서비스 작업](~/cross-platform/data-cloud/web-services/index.md)에 대 한 설명서를 참조 하세요. 웹 서비스 요청을 직접 작성 하 고 응답을 구문 분석할 수는 있지만 Azure, RestSharp 및 ServiceStack를 비롯 하 여 훨씬 간단한 라이브러리를 사용할 수 있습니다. Xamarin 앱에서 기본 WCF 작업에도 액세스할 수 있습니다.

### <a name="azure"></a>Azure

Microsoft Azure는 데이터 저장 및 동기화, 푸시 알림 등 모바일 앱에 대 한 다양 한 서비스를 제공 하는 클라우드 플랫폼입니다.

[Azure.microsoft.com](https://azure.microsoft.com/) 을 방문 하 여 무료로 사용해 보세요.

### <a name="restsharp"></a>RestSharp

RestSharp은 웹 서비스에 대 한 액세스를 간소화 하는 REST 클라이언트를 제공 하기 위해 모바일 응용 프로그램에 포함 될 수 있는 .NET 라이브러리입니다. 이를 통해 데이터를 요청 하 고 REST 응답을 구문 분석 하는 간단한 API를 제공할 수 있습니다. RestSharp은 유용할 수 있습니다.

[RestSharp 웹 사이트](http://restsharp.org/) 는 RestSharp를 사용 하 여 REST 클라이언트를 구현 하는 방법에 대 한 [설명서](https://github.com/restsharp/RestSharp/wiki) 를 포함 합니다.
RestSharp는 [github](https://github.com/restsharp/RestSharp/)에서 Xamarin.ios 및 xamarin Android 예제를 제공 합니다.

[웹 서비스 설명서](~/cross-platform/data-cloud/web-services/index.md)에는 xamarin.ios 코드 조각도 있습니다.

 <a name="ServiceStack"></a>

### <a name="servicestack"></a>ServiceStack

RestSharp와 달리 ServiceStack는 웹 서비스를 호스트 하는 서버 쪽 솔루션 뿐 아니라 모바일 응용 프로그램에서 해당 서비스에 액세스 하기 위해 구현할 수 있는 클라이언트 라이브러리입니다.

[Servicestack 웹 사이트](http://servicestack.net/) 는 프로젝트의 목적과 문서 및 코드 샘플에 대 한 링크를 설명 합니다. 이 예에는 웹 서비스의 전체 서버 쪽 구현과이를 액세스할 수 있는 다양 한 클라이언트 쪽 응용 프로그램이 포함 됩니다.

### <a name="wcf"></a>WCF

Xamarin 도구는 WCF (일부 Windows Communication Foundation) 서비스를 사용 하는 데 도움이 될 수 있습니다. 일반적으로 Xamarin은 Silverlight 런타임과 함께 제공 되는 동일한 클라이언트 쪽 WCF의 하위 집합을 지원 합니다. 여기에는를 사용 하 여 HTTP 전송 프로토콜을 통해 텍스트 인코딩된 SOAP 메시지의 가장 일반적인 인코딩 및 프로토콜 구현이 포함 됩니다 `BasicHttpBinding` .

WCF 프레임 워크의 크기와 복잡성 때문에 Xamarin의 클라이언트 하위 집합 도메인에서 지원 되는 범위를 벗어나는 현재 및 향후 서비스 구현이 있을 수 있습니다. 또한 WCF 지원에는 프록시를 생성 하는 Windows 환경 에서만 사용할 수 있는 도구를 사용 해야 합니다.

 <a name="Threading"></a>

## <a name="threading"></a>스레딩

응용 프로그램 응답성은 모바일 응용 프로그램에 중요 합니다. 즉, 사용자가 응용 프로그램을 로드 하 고 빠르게 수행할 수 있습니다. 사용자 입력 수신을 중지 하는 ' 고정 ' 화면은 응용 프로그램이 충돌 했음을 나타내는 것 처럼 보이지만, 네트워크 요청 또는 저속 로컬 작업 (예: 파일 압축 풀기)과 같은 장기 실행 차단 호출에 UI 스레드를 연결 하지 않는 것이 중요 합니다. 특히 시작 프로세스는 장기 실행 작업을 포함 해서는 안 됩니다. 모든 모바일 플랫폼은 로드 하는 데 너무 오래 걸리는 앱을 중지 합니다.

즉, 사용자 인터페이스에서 ' 진행률 표시기 '를 구현 하거나, 작업을 빠르게 수행할 수 있는 ' 사용 가능한 ' UI를 구현 하 고 백그라운드 작업을 수행할 비동기 작업을 구현 해야 합니다. 백그라운드 작업을 실행 하려면 스레드를 사용 해야 합니다. 즉, 백그라운드 작업에서 주 스레드를 다시 통신 하 여 진행 상태를 표시 하거나 완료 된 시간을 나타내는 방법이 필요 합니다.

 <a name="Parallel_Task_Library"></a>

### <a name="parallel-task-library"></a>병렬 작업 라이브러리

병렬 작업 라이브러리를 사용 하 여 만든 작업을 비동기적으로 실행 하 고 호출 스레드에서 반환할 수 있으므로 사용자 인터페이스를 차단 하지 않고 장기 실행 작업을 트리거하는 데 매우 유용 합니다.

간단한 병렬 작업 작업은 다음과 같습니다.

```csharp
using System.Threading.Tasks;
void MainThreadMethod ()
{
    Task.Factory.StartNew (() => wc.DownloadString ("http://...")).ContinueWith (
        t => label.Text = t.Result, TaskScheduler.FromCurrentSynchronizationContext()
    );
}
```

키는 메서드를 호출 하는 `TaskScheduler.FromCurrentSynchronizationContext()` 스레드 (여기서는를 실행 하는 주 스레드)를 호출 하는 스레드의 SynchronizationContext를 다시 사용 하는 것입니다. `MainThreadMethod` 즉, UI 스레드에서 메서드가 호출 되 면 `ContinueWith` ui 스레드에서 작업을 다시 실행 합니다.

코드가 다른 스레드에서 작업을 시작 하는 경우 다음 패턴을 사용 하 여 UI 스레드에 대 한 참조를 만들고 작업에서이를 다시 호출할 수 있습니다.

```csharp
static Context uiContext = TaskScheduler.FromCurrentSynchronizationContext();
```

 <a name="Invoking_on_the_UI_Thread"></a>

### <a name="invoking-on-the-ui-thread"></a>UI 스레드에서 호출

병렬 작업 라이브러리를 사용 하지 않는 코드의 경우 각 플랫폼에는 UI 스레드로 다시 마샬링 작업을 위한 자체 구문이 있습니다.

- **iOS** –`owner.BeginInvokeOnMainThread(new NSAction(action))`
- **Android** –`owner.RunOnUiThread(action)`
- **Xamarin.ios** –`Device.BeginInvokeOnMainThread(action)`
- **Windows** –`Deployment.Current.Dispatcher.BeginInvoke(action)`

IOS와 Android 구문을 모두 사용 하려면 ' context ' 클래스를 사용 해야 합니다 .이는 코드에서이 개체를 UI 스레드에서 콜백이 필요한 메서드에 전달 해야 함을 의미 합니다.

공유 코드에서 UI 스레드를 호출 하려면 [IDispatchOnUIThread 예제](https://www.slideshare.net/follesoe/cross-platform-mobile-apps-using-net) (의 경우)를 따릅니다 [@follesoe](https://twitter.com/follesoe) . 공유 코드에서 인터페이스에 대 한 및 프로그램을 선언 하 고 다음과 같이 `IDispatchOnUIThread` 플랫폼별 클래스를 구현 합니다.

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

Xamarin Forms 개발자는 [`Device.BeginInvokeOnMainThread`](~/xamarin-forms/platform/device.md#interact-with-the-ui-from-background-threads) 공용 코드 (공유 프로젝트 또는 PCL)에서를 사용 해야 합니다.

 <a name="Platform_and_Device_Capabilities_and_Degradation"></a>

## <a name="platform-and-device-capabilities-and-degradation"></a>플랫폼 및 장치 기능 및 성능 저하

다양 한 기능을 다루는 구체적인 예제는 플랫폼 기능 설명서에 제공 됩니다. 응용 프로그램의 잠재력을 최대한 활용할 수 없는 경우에도 다양 한 기능을 검색 하 고 응용 프로그램을 정상적으로 저하 하 여 적절 한 사용자 환경을 제공 하는 방법을 다룹니다.
