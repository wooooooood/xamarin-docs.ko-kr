---
title: Xamarin.ios의 파일 시스템 액세스
description: 이 문서에서는 Xamarin.ios에서 파일 시스템을 사용 하는 방법을 설명 합니다. 디렉터리, 파일 읽기, XML 및 JSON serialization, 응용 프로그램 샌드박스, iTunes를 통한 파일 공유 등을 설명 합니다.
ms.prod: xamarin
ms.assetid: 37DF2F38-901E-8F8E-269A-5EE0CCD28C08
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 11/12/2018
ms.openlocfilehash: c9e5b2504fd8af8b4232eea0dcf39d9c4760b555
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73010610"
---
# <a name="file-system-access-in-xamarinios"></a>Xamarin.ios의 파일 시스템 액세스

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/ios-samples/filesystemsamplecode)

*.NET BCL (기본 클래스 라이브러리)* 의 xamarin.ios 및 `System.IO` 클래스를 사용 하 여 iOS 파일 시스템에 액세스할 수 있습니다. `File` 클래스를 사용하여 파일을 만들고, 삭제하고, 삭제할 수 있으며, `Directory` 클래스를 사용하여 디렉터리의 콘텐츠를 만들거나, 삭제하거나, 열거할 수 있습니다. 또한 파일 작업 (예: 파일 내의 압축 또는 위치 검색)에 대해 더 많은 제어를 제공할 수 있는 `Stream` 서브 클래스를 사용할 수 있습니다.

iOS는 응용 프로그램 데이터의 보안을 유지 하 고 malignant apps에서 사용자를 보호 하기 위해 응용 프로그램에서 파일 시스템으로 수행할 수 있는 작업에 대 한 몇 가지 제한 사항을 부과 합니다. 이러한 제한 사항은 응용 프로그램의 파일, 기본 설정, 네트워크 리소스, 하드웨어 등에 대 한 액세스를 제한 하는 규칙 집합인 *응용 프로그램 샌드박스의* 일부입니다. 응용 프로그램은 홈 디렉터리 (설치 된 위치) 내에서 파일을 읽고 쓰는 것으로 제한 됩니다. 다른 응용 프로그램의 파일에 액세스할 수 없습니다.

iOS에는 일부 파일 시스템 관련 기능도 있습니다. 특정 디렉터리에는 백업 및 업그레이드와 관련 하 여 특별 한 처리가 필요 하 고, 응용 프로그램은 다른 파일 및 **파일** 앱 (iOS 11 이후) 및 iTunes를 통해 파일을 공유할 수도 있습니다.

이 문서에서는 iOS 파일 시스템의 기능 및 제한 사항에 대해 설명 하며, Xamarin.ios를 사용 하 여 몇 가지 간단한 파일 시스템 작업을 실행 하는 방법을 보여 주는 샘플 응용 프로그램을 제공 합니다.

[몇 가지 간단한 파일 시스템 작업을 실행 하는 iOS 샘플![](file-system-images/01-sampleapp-sml.png)](file-system-images/01-sampleapp.png#lightbox)

## <a name="general-file-access"></a>일반 파일 액세스

Xamarin.ios를 사용 하면 iOS에서 파일 시스템 작업에 .NET `System.IO` 클래스를 사용할 수 있습니다.

다음 코드 조각에서는 몇 가지 일반적인 파일 작업을 보여 줍니다. 이 문서에 대 한 샘플 응용 프로그램의 **SampleCode.cs** 파일에서 아래의 모든 항목을 찾을 수 있습니다.

### <a name="working-with-directories"></a>디렉터리 작업

이 코드는 응용 프로그램 실행 파일의 위치인 "./" 매개 변수로 지정 된 현재 디렉터리의 하위 디렉터리를 열거 합니다.
출력은 응용 프로그램과 함께 배포 되는 모든 파일 및 폴더의 목록이 됩니다 (디버깅 하는 동안 콘솔 창에 표시 됨).

```csharp
var directories = Directory.EnumerateDirectories("./");
foreach (var directory in directories) {
      Console.WriteLine(directory);
}
```

### <a name="reading-files"></a>파일 읽기

텍스트 파일을 읽으려면 한 줄의 코드가 필요 합니다. 이 예제에서는 응용 프로그램 출력 창에 텍스트 파일의 내용을 표시 합니다.

```csharp
var text = File.ReadAllText("TestData/ReadMe.txt");
Console.WriteLine(text);
```

### <a name="xml-serialization"></a>XML serialization

전체 `System.Xml` 네임 스페이스를 사용 하는 작업은이 문서에서 다루지 않지만 다음 코드 조각과 같은 StreamReader를 사용 하 여 파일 시스템에서 XML 문서를 쉽게 deserialize 할 수 있습니다.

```csharp
using (TextReader reader = new StreamReader("./TestData/test.xml")) {
      XmlSerializer serializer = new XmlSerializer(typeof(MyObject));
      var xml = (MyObject)serializer.Deserialize(reader);
}
```

자세한 내용은 [system.xml](xref:System.Xml) 및 [serialization](xref:System.Xml.Serialization)에 대 한 설명서를 참조 하세요. 링커에 대 한 [xamarin.ios 설명서](~/ios/deploy-test/linker.md) 를 참조 하세요. 직렬화 하려는 클래스에 `[Preserve]` 특성을 추가 해야 하는 경우가 많습니다.

### <a name="creating-files-and-directories"></a>파일 및 디렉터리 만들기

이 샘플에서는 `Environment` 클래스를 사용 하 여 파일 및 디렉터리를 만들 수 있는 문서 폴더에 액세스 하는 방법을 보여 줍니다.

```csharp
var documents =
 Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments); 
var filename = Path.Combine (documents, "Write.txt");
File.WriteAllText(filename, "Write this text into a file");
```

디렉터리를 만드는 과정은 다음과 같습니다.

```csharp
var documents =
 Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var directoryname = Path.Combine (documents, "NewDirectory");
Directory.CreateDirectory(directoryname);
```

자세한 내용은 [SYSTEM.IO API 참조](xref:System.IO)를 참조 하세요.

### <a name="serializing-json"></a>JSON 직렬화

[Json.NET](http://www.newtonsoft.com/json) 는 xamarin.ios와 함께 작동 하는 고성능 Json 프레임 워크로, NuGet에서 사용할 수 있습니다. Mac용 Visual Studio에서 **Nuget 추가** 를 사용 하 여 응용 프로그램 프로젝트에 nuget 패키지를 추가 합니다.

[![](file-system-images/json01.png "Adding the NuGet package to the applications project")](file-system-images/json01.png#lightbox)

다음으로 직렬화/deserialization을 위한 데이터 모델 역할을 할 클래스를 추가 합니다 (이 경우 `Account.cs`).

```csharp
using System;
using System.Collections.Generic;
using Foundation; // for Preserve attribute, which helps serialization with Linking enabled

namespace FileSystem
{
    [Preserve]
    public class Account
    {
        public string Email { get; set; }
        public bool Active { get; set; }
        public DateTime CreatedDate { get; set; }
        public List<string> Roles { get; set; }

        public Account() {
        }
    }
}
```

마지막으로, `Account` 클래스의 인스턴스를 만들고, json 데이터로 serialize 하 고, 파일에 씁니다.

```csharp
// Create a new record
var account = new Account(){
    Email = "monkey@xamarin.com",
    Active = true,
    CreatedDate = new DateTime(2015, 5, 27, 0, 0, 0, DateTimeKind.Utc),
    Roles = new List<string> {"User", "Admin"}
};

// Serialize object
var json = JsonConvert.SerializeObject(account, Newtonsoft.Json.Formatting.Indented);

// Save to file
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var filename = Path.Combine (documents, "account.json");
File.WriteAllText(filename, json);
```

.NET 응용 프로그램에서 json 데이터를 사용 하는 방법에 대 한 자세한 내용은 참조. NET의 [설명서](https://www.newtonsoft.com/json/help)를 참조 하세요.

## <a name="special-considerations"></a>특별 고려 사항

Xamarin.ios와 .NET 파일 작업의 유사성에도 불구 하 고, iOS 및 Xamarin.ios는 몇 가지 중요 한 측면에서 .NET과 다릅니다.

### <a name="making-project-files-accessible-at-runtime"></a>런타임에 프로젝트 파일에 액세스할 수 있도록 설정

기본적으로 프로젝트에 파일을 추가 하는 경우이 파일은 최종 어셈블리에 포함 되지 않으므로 응용 프로그램에서 사용할 수 없습니다. 어셈블리에 파일을 포함 하려면 콘텐츠 라는 특수 한 빌드 작업으로 표시 해야 합니다.

파일에 포함할 파일을 표시 하려면 파일을 마우스 오른쪽 단추로 클릭 하 고 Mac용 Visual Studio에서 **콘텐츠 &gt; 빌드 작업** 을 선택 합니다. 또한 파일의 **속성** 시트에서 **빌드 작업** 을 변경할 수 있습니다.

### <a name="case-sensitivity"></a>대/소문자 구분

IOS 파일 시스템은 *대/소문자를 구분*한다는 것을 이해 하는 것이 중요 합니다. 대/소문자 구분은 파일 및 디렉터리 이름이 정확 하 게 일치 해야 **합니다. readme.txt** 와 **readme.txt** 는 다른 파일 이름으로 간주 됩니다.

이는 *대/소문자를 구분* 하지 않는 Windows 파일 시스템에 익숙한 .net 개발자에 게 혼란 스 러 울 수 있습니다. **파일**, **파일**및 **파일** 은 모두 동일한 디렉터리를 참조 합니다.

> [!WARNING]
> IOS 시뮬레이터는 대/소문자를 구분 하지 않습니다.
> 파일 이름에 대 한 대/소문자 구분이 파일 자체와 코드에서의 참조에 서로 다른 경우 코드는 시뮬레이터에서 계속 작동할 수 있지만 실제 장치에서는 실패 합니다. 이는 iOS를 개발 하는 동안에는 자주 실제 장치에서 배포 하 고 테스트 하는 것이 중요 한 이유 중 하나입니다.

### <a name="path-separator"></a>경로 구분 기호

iOS는 슬래시 '/'를 경로 구분 기호로 사용 합니다 .이는 백슬래시 ' \ '를 사용 하는 Windows와는 다릅니다.

이러한 혼동 때문에 특정 경로 구분 기호를 하드 코딩 하는 대신 현재 플랫폼에 맞게 조정 하는 `System.IO.Path.Combine` 메서드를 사용 하는 것이 좋습니다. 이는 코드를 다른 플랫폼으로 이식할 수 있게 해 주는 간단한 단계입니다.

## <a name="application-sandbox"></a>응용 프로그램 샌드박스

응용 프로그램의 파일 시스템 액세스 (및 네트워크 및 하드웨어 기능과 같은 기타 리소스)는 보안상의 이유로 제한 됩니다. 이러한 제한 사항을 *응용 프로그램 샌드박스*라고 합니다. 파일 시스템을 기준으로 응용 프로그램은 홈 디렉터리에서 파일 및 디렉터리를 만들고 삭제 하는 것으로 제한 됩니다.

홈 디렉터리는 응용 프로그램 및 모든 데이터가 저장 되는 파일 시스템의 고유한 위치입니다. 응용 프로그램에 대 한 홈 디렉터리의 위치를 선택 (또는 변경) 할 수 없습니다. 그러나 iOS 및 Xamarin.ios는 내에서 파일 및 디렉터리를 관리 하는 속성과 메서드를 제공 합니다.

## <a name="the-application-bundle"></a>응용 프로그램 번들

*응용 프로그램 번들* 은 응용 프로그램을 포함 하는 폴더입니다.
이 파일은 디렉터리 이름에. 앱 접미사를 추가 하 여 다른 폴더와 구분 됩니다. 응용 프로그램 번들에는 실행 파일과 프로젝트에 필요한 모든 콘텐츠 (파일, 이미지 등)가 포함 되어 있습니다.

Mac OS에서 응용 프로그램 번들로 이동 하면 다른 디렉터리에 표시 되는 것과 다른 아이콘을 사용 하 여 표시 됩니다. 즉, 앱 접미사는 숨겨집니다 **.** 그러나 운영 체제가 다르게 표시 되는 일반 디렉터리 일 뿐입니다.

샘플 코드에 대 한 응용 프로그램 번들을 보려면 **Mac용 Visual Studio** 에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **Finder에**표시를 선택 합니다. 그런 다음 응용 프로그램 아이콘을 찾을 **bin/** 디렉터리 (아래 스크린샷에 비슷함)로 이동 합니다.

![Bin 디렉터리를 탐색 하 여이 스크린샷에서 유사한 응용 프로그램 아이콘을 찾습니다.](file-system-images/40-bundle.png)

이 아이콘을 마우스 오른쪽 단추로 클릭 하 고 **패키지 내용 표시** 를 선택 하 여 응용 프로그램 번들 디렉터리의 내용을 검색 합니다. 콘텐츠는 다음과 같이 일반 디렉터리의 내용과 동일 하 게 표시 됩니다.

[앱 번들의 내용![](file-system-images/45-bundle-sml.png)](file-system-images/45-bundle.png#lightbox)

응용 프로그램 번들은 테스트 중에 시뮬레이터 나 장치에 설치 되는 것으로, 궁극적으로 앱 스토어에 포함 하기 위해 Apple에 제출 됩니다.

## <a name="application-directories"></a>응용 프로그램 디렉터리

응용 프로그램이 장치에 설치 되 면 운영 체제는 응용 프로그램의 홈 디렉터리를 만들고 응용 프로그램 루트 디렉터리 내에서 사용할 수 있는 여러 디렉터리를 만듭니다. IOS 8부터 사용자가 액세스할 수 있는 디렉터리는 응용 프로그램 루트 [에 없으므로 사용자](https://developer.apple.com/library/ios/technotes/tn2406/_index.html) 디렉터리에서 응용 프로그램 번들의 경로를 파생할 수 없으며 그 반대의 경우도 마찬가지입니다.

이러한 디렉터리, 경로를 확인 하는 방법 및 해당 용도는 아래에 나열 되어 있습니다.

&nbsp;

|디렉터리|설명|
|---|---|
|[ApplicationName]. 앱/|**IOS 7 및 이전 버전**에서는 응용 프로그램 실행 파일이 저장 되는 `ApplicationBundle` 디렉터리입니다. 앱에서 만드는 디렉터리 구조는이 디렉터리에 있습니다 (예: Mac용 Visual Studio 프로젝트에서 리소스로 표시 한 이미지 및 기타 파일 형식).<br /><br />응용 프로그램 번들 내에서 콘텐츠 파일에 액세스 해야 하는 경우이 디렉터리의 경로는 `NSBundle.MainBundle.BundlePath` 속성을 통해 사용할 수 있습니다.|
|문서|이 디렉터리를 사용 하 여 사용자 문서 및 응용 프로그램 데이터 파일을 저장 합니다.<br /><br />이 디렉터리의 콘텐츠는 iTunes 파일 공유를 통해 사용자가 사용할 수 있도록 설정할 수 있습니다 (기본적으로 사용 하지 않도록 설정 됨). Info.plist 파일에 `UIFileSharingEnabled` 부울 키를 추가 하 여 사용자가 이러한 파일에 액세스할 수 있도록 합니다.<br /><br />응용 프로그램에서 파일 공유를 즉시 사용 하도록 설정 하지 않는 경우에도이 디렉터리의 사용자에 게 서 숨겨지는 파일 (공유 하지 않는 한 데이터베이스 파일)을 배치 하지 않아야 합니다. 중요 한 파일이 숨겨진 상태로 유지 되는 한, 이후 버전에서 파일 공유를 사용 하는 경우 이러한 파일은 노출 되지 않고 iTunes에 의해 이동, 수정 또는 삭제 될 수 있습니다.<br /><br /> `Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments)` 메서드를 사용 하 여 응용 프로그램에 대 한 문서 디렉터리의 경로를 가져올 수 있습니다.<br /><br />이 디렉터리의 콘텐츠는 iTunes에 의해 백업 됩니다.|
|라이브러리|라이브러리 디렉터리는 사용자가 직접 만들지 않은 파일 (예: 데이터베이스 또는 다른 응용 프로그램 생성 파일)을 저장할 수 있는 좋은 장소입니다. 이 디렉터리의 콘텐츠는 iTunes를 통해 사용자에 게 노출 되지 않습니다.<br /><br />라이브러리에서 고유한 하위 디렉터리를 만들 수 있습니다. 그러나 여기에는 기본 설정 및 캐시를 비롯 하 여 알고 있어야 하는 시스템 생성 디렉터리가 이미 있습니다.<br /><br />이 디렉터리의 콘텐츠 (캐시 하위 디렉터리 제외)는 iTunes에 의해 백업 됩니다. 라이브러리에서 만든 사용자 지정 디렉터리를 백업 합니다.|
|라이브러리/기본 설정/|응용 프로그램별 기본 설정 파일은이 디렉터리에 저장 됩니다. 이러한 파일을 직접 만들지 마십시오. 대신 `NSUserDefaults` 클래스를 사용 합니다.<br /><br />이 디렉터리의 콘텐츠는 iTunes에 의해 백업 됩니다.|
|라이브러리/캐시/|캐시 디렉터리는 응용 프로그램을 실행 하는 데 도움이 되는 데이터 파일을 저장 하는 데 유용 하지만 쉽게 다시 만들 수 있습니다. 응용 프로그램은 필요에 따라 이러한 파일을 만들고 삭제 하며 필요한 경우 이러한 파일을 다시 만들 수 있습니다. iOS 5는 이러한 파일을 삭제할 수도 있습니다 (낮은 저장소 상황에서). 그러나 응용 프로그램이 실행 되는 동안에는이 작업을 수행 하지 않습니다.<br /><br />이 디렉터리의 콘텐츠는 iTunes에서 백업 되지 않습니다. 즉, 사용자가 장치를 복원 하는 경우 표시 되지 않으며 업데이트 된 버전의 응용 프로그램을 설치한 후에는 나타나지 않습니다.<br /><br />예를 들어 응용 프로그램에서 네트워크에 연결할 수 없는 경우에는 캐시 디렉터리를 사용 하 여 데이터 또는 파일을 저장 하 여 적절 한 오프 라인 환경을 제공할 수 있습니다. 네트워크 응답을 기다리는 동안 응용 프로그램에서이 데이터를 신속 하 게 저장 하 고 검색할 수 있지만 백업 하지 않아도 되며 복원 또는 버전 업데이트 후에 쉽게 복구 하거나 다시 만들 수 있습니다.|
|임시|응용 프로그램은이 디렉터리의 짧은 기간 동안만 필요한 임시 파일을 저장할 수 있습니다. 공간을 절약 하려면 더 이상 필요 하지 않은 파일을 삭제 해야 합니다. 응용 프로그램이 실행 되 고 있지 않은 경우 운영 체제는이 디렉터리에서 파일을 삭제할 수도 있습니다.<br /><br />이 디렉터리의 콘텐츠는 iTunes에서 백업 되지 않습니다.<br /><br />예를 들어, 사용자에 게 표시 하기 위해 다운로드 한 임시 파일 (예: Twitter 아바타 또는 전자 메일 첨부 파일)을 저장 하는 데 tmp 디렉터리를 사용할 수 있지만, 표시 된 후에 삭제할 수 있으며 나중에 필요할 경우 다시 다운로드할 수 있습니다. .|

이 스크린샷은 Finder 창의 디렉터리 구조를 보여줍니다.

[![](file-system-images/08-library-directory.png "This screenshot shows the directory structure in a Finder window")](file-system-images/08-library-directory.png#lightbox)

### <a name="accessing-other-directories-programmatically"></a>프로그래밍 방식으로 다른 디렉터리 액세스

이전 디렉터리 및 파일 예제는 `Documents` 디렉터리에 액세스 했습니다. 다른 디렉터리에 쓰려면 다음과 같이 "..." 구문을 사용 하 여 경로를 생성 해야 합니다.

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var library = Path.Combine (documents, "..", "Library");
var filename = Path.Combine (library, "WriteToLibrary.txt");
File.WriteAllText(filename, "Write this text into a file in Library");
```

디렉터리 만들기는 다음과 유사 합니다.

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var library = Path.Combine (documents, "..", "Library");
var directoryname = Path.Combine (library, "NewLibraryDirectory");
Directory.CreateDirectory(directoryname);
```

`Caches` 및 `tmp` 디렉터리에 대 한 경로는 다음과 같이 구성할 수 있습니다.

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var cache = Path.Combine (documents, "..", "Library", "Caches");
var tmp = Path.Combine (documents, "..", "tmp");
```

## <a name="sharing-with-the-files-app"></a>파일 앱과 공유

iOS 11에는 사용자가 iCloud에서 파일을 보고 상호 작용 하 고이를 지 원하는 응용 프로그램 에서도 저장할 수 있도록 하는 iOS 용 파일 **브라우저 파일이 도입** 되었습니다. 사용자가 앱의 파일에 직접 액세스할 수 있도록 하려면 **info.plist** 파일 `LSSupportsOpeningDocumentsInPlace`에 새 부울 키를 만들고 다음과 같이 `true`으로 설정 합니다.

![Info.plist에 LSSupportsOpeningDocumentsInPlace를 설정 합니다.](file-system-images/51-supports-opening.png)

이제 앱 **문서** 디렉터리를 **파일** 앱에서 찾아볼 수 있습니다. **파일** 앱에서 **내 iPhone** 로 이동 하면 공유 파일이 있는 각 앱이 표시 됩니다. 아래 스크린샷에는 [FileSystem 샘플 앱](https://docs.microsoft.com/samples/xamarin/ios-samples/filesystemsamplecode) 이 표시 됩니다.

![iOS 11 파일 앱](file-system-images/50-files-app-1-sml.png) ![내 iPhone 파일 찾아보기](file-system-images/50-files-app-2-sml.png) ![샘플 앱 파일](file-system-images/50-files-app-3-sml.png)

## <a name="sharing-files-with-the-user-through-itunes"></a>ITunes를 통해 사용자와 파일 공유

사용자는 다음과 같이 `Info.plist` 편집 하 고 **원본** 뷰에서 iTunes 공유 (`UIFileSharingEnabled`) 항목을 **지 원하는 응용** 프로그램을 만들어 응용 프로그램의 문서 디렉터리에 있는 파일에 액세스할 수 있습니다.

[응용 프로그램을 추가 하는![iTunes 공유 속성 지원](file-system-images/09-uifilesharingenabled-plist-sml.png)](file-system-images/09-uifilesharingenabled-plist.png#lightbox)

이러한 파일은 장치가 연결 되어 있고 사용자가 `Apps` 탭을 선택할 때 iTunes에서 액세스할 수 있습니다. 예를 들어 다음 스크린샷은 iTunes를 통해 공유 되는 선택 된 앱의 파일을 보여 줍니다.

[![이 스크린샷은 iTunes를 통해 공유 되는 선택한 앱의 파일을 보여줍니다.](file-system-images/10-itunes-file-sharing-sml.png)](file-system-images/10-itunes-file-sharing.png#lightbox)

사용자는 iTunes를 통해이 디렉터리의 최상위 항목에만 액세스할 수 있습니다. 하위 디렉터리의 콘텐츠를 볼 수 없습니다 (컴퓨터에 복사 하거나 삭제할 수는 있음). 예를 들어 GoodReader를 사용 하면 사용자가 iOS 장치에서 파일을 읽을 수 있도록 PDF 및 EPUB 파일을 응용 프로그램과 공유할 수 있습니다.

문서 폴더의 내용을 수정 하는 사용자가 주의 하지 않으면 문제를 일으킬 수 있습니다. 응용 프로그램은이를 고려 하 고 문서 폴더의 파괴적인 업데이트를 복원 해야 합니다.

이 문서의 샘플 코드는 Documents 폴더 ( **SampleCode.cs**)에 파일과 폴더를 모두 만들며 **info.plist** 파일에서 파일 공유를 사용 하도록 설정 합니다. 이 스크린샷에서는 iTunes에 표시 되는 방법을 보여 줍니다.

[이 스크린샷에서는 파일이 iTunes에 표시 되는 방식을 보여![](file-system-images/15-itunes-file-sharing-example-sml.png)](file-system-images/15-itunes-file-sharing-example.png#lightbox)

응용 프로그램에 대 한 아이콘을 설정 하는 방법 및 사용자가 만드는 모든 사용자 지정 문서 형식에 대 한 자세한 내용은 [이미지 작업](~/ios/app-fundamentals/images-icons/index.md) 문서를 참조 하세요.

`UIFileSharingEnabled` 키가 false 이거나 없는 경우 파일 공유는 기본적으로 사용 하지 않도록 설정 되 고 사용자는 Documents 디렉터리와 상호 작용할 수 없습니다.

## <a name="backup-and-restore"></a>백업 및 복원

ITunes에서 장치를 백업 하는 경우 응용 프로그램의 홈 디렉터리에 생성 된 모든 디렉터리는 다음 디렉터리를 제외 하 고 저장 됩니다.

- **[ApplicationName]. 앱** – 서명 되었으므로이 디렉터리에 쓰지 않으므로 설치 후에는 변경 되지 않은 상태로 유지 해야 합니다. 코드에서 액세스 하는 리소스를 포함할 수 있지만 앱을 다시 다운로드 하 여 복원 되므로 백업이 필요 하지 않습니다.
- **라이브러리/캐시** – 캐시 디렉터리는 백업 하지 않아도 되는 작업 파일을 위한 것입니다.
- **tmp** –이 디렉터리는 더 이상 필요 하지 않을 때 만들어지거나 삭제 된 임시 파일 또는 공간이 필요할 때 iOS에서 삭제 하는 파일에 사용 됩니다.

많은 양의 데이터를 백업 하는 데 시간이 오래 걸릴 수 있습니다. 특정 문서나 데이터를 백업 해야 하는 경우 응용 프로그램에서 문서 및 라이브러리 폴더를 사용 해야 합니다. 네트워크에서 쉽게 검색할 수 있는 임시 데이터 또는 파일의 경우 캐시 또는 tmp 디렉터리를 사용 합니다.

> [!NOTE]
> 장치에서 디스크 공간이 매우 부족 하면 iOS에서 파일 시스템을 ' 정리 ' 합니다.
> 이 프로세스는 현재 실행 되 고 있지 않은 응용 프로그램의 라이브러리/캐시 및 tmp 폴더에서 모든 파일을 제거 합니다.

## <a name="complying-with-ios-5-icloud-backup-restrictions"></a>IOS 5 iCloud 백업 제한 준수

> [!NOTE]
> 이 정책은 iOS 5에 처음 도입 되었지만 (오랜 시간 전 처럼 보이지만), 현재 앱에 대 한 지침은 계속 제공 됩니다.

Apple에는 iOS 5와 함께 *ICloud 백업* 기능이 도입 되었습니다. ICloud 백업을 사용 하도록 설정 하면 응용 프로그램의 홈 디렉터리에 있는 모든 파일 (일반적으로 백업 되지 않는 디렉터리 (예: 앱 번들, `Caches`및 `tmp`)이 iCloud 서버에 백업 됩니다. 이 기능을 통해 사용자는 장치를 분실 하거나 도난당 하거나 손상 한 경우 전체 백업을 수행할 수 있습니다.

ICloud는 각 사용자에 게 5gb의 사용 가능한 공간만 제공 하 고 대역폭을 불필요 하 게 사용 하지 않도록 하기 때문에 Apple에서는 응용 프로그램이 필수적인 사용자 생성 데이터를 백업 해야 합니다. IOS 데이터 저장소 지침을 준수 하려면 다음 항목을 준수 하 여 백업 되는 데이터의 양을 제한 해야 합니다.

- 사용자가 생성 한 데이터 또는 다시 만들 수 없는 데이터를 문서 디렉터리 (백업 됨)에만 저장 합니다.
- `Library/Caches` 또는 `tmp`에서 쉽게 다시 만들거나 다시 다운로드할 수 있는 다른 데이터를 저장 합니다 .이 데이터는 백업 되지 않으며 ' 치료 ' 될 수 있습니다.
- `Library/Caches` 또는 `tmp` 폴더에 적합 한 파일이 있지만 ' 정리 ' 하지 않으려는 경우 다른 곳 (예: `Library/YourData`)에 저장 하 고 ' 백업 안 함 ' 특성을 적용 하 여 파일이 iCloud 백업 대역폭 및 stora를 사용 하지 않도록 합니다. ge 공간. 이 데이터는 장치에서 공간을 계속 사용 하므로 신중 하 게 관리 하 고 가능한 경우 삭제 해야 합니다.

' 백업 안 함 ' 특성은 `NSFileManager` 클래스를 사용 하 여 설정 됩니다. 클래스가 `using Foundation` 인지 확인 하 고 다음과 같이 `SetSkipBackupAttribute`를 호출 합니다.

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var filename = Path.Combine (documents, "LocalOnly.txt");
File.WriteAllText(filename, "This file will never get backed-up. It would need to be re-created after a restore or re-install");
NSFileManager.SetSkipBackupAttribute (filename, true); // backup will be skipped for this file
```

`SetSkipBackupAttribute` `true` 되는 경우 파일이 저장 되는 디렉터리 (`Documents` 디렉터리)에 관계 없이 파일이 백업 되지 않습니다. `GetSkipBackupAttribute` 메서드를 사용 하 여 특성을 쿼리 하 고 다음과 같이 `false`를 사용 하 여 `SetSkipBackupAttribute` 메서드를 호출 하 여이 특성을 다시 설정할 수 있습니다.

```csharp
NSFileManager.SetSkipBackupAttribute (filename, false); // file will be backed-up
```

## <a name="sharing-data-between-ios-apps-and-app-extensions"></a>IOS 앱과 앱 확장 간 데이터 공유

앱 확장은 포함 하는 앱과는 달리 호스트 응용 프로그램의 일부로 실행 되므로 데이터 공유가 자동으로 포함 되지 않으므로 추가 작업이 필요 합니다. 앱 그룹은 다양 한 앱이 데이터를 공유할 수 있도록 하기 위해 iOS에서 사용 하는 메커니즘입니다. 올바른 자격 및 프로 비전을 사용 하 여 응용 프로그램이 제대로 구성 된 경우에는 일반 iOS 샌드박스 외부의 공유 디렉터리에 액세스할 수 있습니다.

### <a name="configure-an-app-group"></a>앱 그룹 구성

공유 위치는 [IOS 개발자 센터](https://developer.apple.com/devcenter/ios/)의 **인증서, 식별자 & 프로필** 섹션에서 구성 된 [앱 그룹](https://developer.apple.com/library/archive/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19)을 사용 하 여 구성 됩니다. 이 값은 각 프로젝트의 **info.plist**에서도 참조 되어야 합니다.

앱 그룹을 만들고 구성 하는 방법에 대 한 자세한 내용은 [앱 그룹 기능](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) 가이드를 참조 하세요.

### <a name="files"></a>파일

IOS 앱 및 확장은 일반적인 파일 경로를 사용 하 여 파일을 공유할 수도 있습니다 (올바른 자격 및 프로 비전을 사용 하 여 제대로 구성 된 경우).

```csharp
var FileManager = new NSFileManager ();
var appGroupContainer =FileManager.GetContainerUrl ("group.com.xamarin.WatchSettings");
var appGroupContainerPath = appGroupContainer.Path

Console.WriteLine ("Group Path: " + appGroupContainerPath);

// use the path to create and update files
...
```

> [!IMPORTANT]
> 반환 된 그룹 경로가 `null`이면 자격 및 프로 비전 프로필의 구성이 올바른지 확인 하 고 올바른지 확인 합니다.

## <a name="application-version-updates"></a>응용 프로그램 버전 업데이트

새 버전의 응용 프로그램을 다운로드 하면 iOS에서 새 홈 디렉터리를 만들고 새 응용 프로그램 번들을 저장 합니다. 그런 다음 iOS는 이전 버전의 응용 프로그램 번들에서 다음 폴더를 새 홈 디렉터리로 이동 합니다.

- **문서**
- **라이브러리**

다른 디렉터리는 다른 디렉터리에 복사 하 여 새 홈 디렉터리에 저장할 수도 있지만 복사 되지 않으므로 응용 프로그램에서이 시스템 동작을 사용 하지 않아야 합니다.

## <a name="summary"></a>요약

이 문서에서는 Xamarin.ios를 사용 하는 파일 시스템 작업이 다른 .NET 응용 프로그램과 유사 하다는 것을 보여 주었습니다. 또한 응용 프로그램 샌드박스를 도입 하 고 발생 하는 보안 문제를 검사 했습니다. 다음으로 응용 프로그램 번들의 개념을 탐색 했습니다. 마지막으로 응용 프로그램에서 사용할 수 있는 특수 디렉터리를 열거 하 고 응용 프로그램 업그레이드 및 백업 중에 해당 역할에 대해 설명 했습니다.

## <a name="related-links"></a>관련 링크

- [FileSystem 샘플 코드](https://docs.microsoft.com/samples/xamarin/ios-samples/filesystemsamplecode)
- [파일 시스템 프로그래밍 가이드](https://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/FileSystemProgrammingGUide/Introduction/Introduction.html)
- [앱에서 지 원하는 파일 형식 등록](https://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html#/apple_ref/doc/uid/TP40010411-SW1)
