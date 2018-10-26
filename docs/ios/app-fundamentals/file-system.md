---
title: Xamarin.iOS에서 파일 시스템 작업
description: 이 문서에서는 Xamarin.iOS에서 파일 시스템을 사용 하는 방법을 설명 합니다. 디렉터리, 파일, XML 및 JSON serialization, iTunes를 통해 파일을 공유 하는 응용 프로그램 샌드박스 읽기에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 37DF2F38-901E-8F8E-269A-5EE0CCD28C08
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: cf595b57906cf1c47acdcdbcddf04bfbdc963393
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50113432"
---
# <a name="working-with-the-file-system-in-xamarinios"></a>Xamarin.iOS에서 파일 시스템 작업

Xamarin.iOS를 사용할 수 있습니다 및 `System.IO` 의 클래스를 *.NET 클래스 라이브러리 (BCL (기본)* iOS 파일 시스템에 액세스할 수 있습니다. 합니다 `File` 클래스를 사용 하면 생성, 삭제 및 파일을 읽 및 `Directory` 클래스 만들기, 삭제 또는 디렉터리의 내용을 열거할 수 있습니다. 사용할 수도 있습니다 `Stream` 뛰어난 파일 작업 (예: 파일 내의 압축 또는 위치 검색)에 대 한 제어를 제공할 수 있는 하위 클래스입니다.

iOS 응용 프로그램에서 악성 앱 사용자를 보호 하 고 응용 프로그램 데이터의 보안을 유지 하기 위해 파일 시스템을 사용 하 여 수행할 수 있는 몇 가지 제한 사항이 적용 합니다. 이러한 제한의 일부인를 *샌드박스 응용 프로그램* – 파일, 기본 설정, 네트워크 리소스, 하드웨어 등 응용 프로그램의 액세스를 제한 하는 규칙의 집합입니다. 응용 프로그램은 홈 디렉터리 (설치 된 위치) 내에서 파일 읽기 및 쓰기 제한 다른 응용 프로그램의 파일 액세스할 수 없습니다.

iOS 파일 시스템 관련 기능도 있습니다: 특정 디렉터리에는 백업 및 업그레이드 관련 하 여 특수 한 처리가 필요 하 고 응용 프로그램은 iTunes 통해 파일을 공유할 수도 있습니다.

기능에 설명 및 iOS의 제한 사항으로 말하면, 파일 시스템 및 Xamarin.iOS를 사용 하 여 몇 가지 간단한 파일 시스템 작업을 실행 하는 방법을 보여 주는 샘플 응용 프로그램을 포함 합니다.

 [![](file-system-images/05-sampleapp.png "몇 가지 간단한 파일 시스템 작업을 실행 하는 ios 샘플")](file-system-images/05-sampleapp.png#lightbox)

 <a name="General_File_Access" />

## <a name="general-file-access"></a>일반 파일 액세스

Xamarin.iOS를 사용 하면.NET을 사용할 수 있습니다 `System.IO` ios 파일 시스템 작업에 대 한 클래스입니다.

다음 코드 조각에서는 몇 가지 일반적인 파일 작업을 보여 줍니다. 찾을 수에서 아래 모든 해당는 `SampleCode.cs` 이 문서에 대 한 샘플 응용 프로그램 파일.

<a name="Working_with_directories" />

### <a name="working-with-directories"></a>디렉터리 사용

현재 디렉터리에 하위 디렉터리를 열거 하는이 코드 (지정 된는 ". /" 매개 변수), 응용 프로그램 실행 파일의 위치입니다.
출력에는 모든 파일 및 폴더 (디버깅 하는 동안 콘솔 창에 표시 됨) 응용 프로그램과 함께 배포 되는 목록이 됩니다.

```csharp
var directories = Directory.EnumerateDirectories("./");
foreach (var directory in directories) {
      Console.WriteLine(directory);
}
```

 <a name="Reading_files" />


### <a name="reading-files"></a>파일 읽기

텍스트 파일을 읽으려면 하기만 하면 코드를 전혀 됩니다. 이 예제에서는 응용 프로그램 출력 창에 텍스트 파일의 내용을 표시 됩니다.

```csharp
var text = File.ReadAllText("TestData/ReadMe.txt");
Console.WriteLine(text);
```

 <a name="XML_Serialization" />


### <a name="xml-serialization"></a>XML Serialization

전체 작업 하지만 `System.Xml` 네임 스페이스는이 문서의 범위를 벗어납니다, 다음과 같은 StreamReader를 사용 하 여는 파일 시스템에서 XML 문서를 쉽게 역직렬화 할 수 있습니다.

```csharp
using (TextReader reader = new StreamReader("./TestData/test.xml")) {
      XmlSerializer serializer = new XmlSerializer(typeof(MyObject));
      var xml = (MyObject)serializer.Deserialize(reader);
}
```

에 대 한 MSDN 설명서를 참조 합니다 [System.Xml](http://msdn.microsoft.com/library/system.xml.aspx) 네임 스페이스에 대 한 자세한 내용은 [serialization](http://msdn.microsoft.com/library/system.xml.serialization.aspx)합니다. 도 검토 해야 합니다 [Xamarin.iOS 설명서](~/ios/deploy-test/linker.md) 링커 –에서 일반적으로 추가 해야는 `[Preserve]` serialize 하려는 클래스에 특성입니다.

 <a name="Creating_Files_and_Directories" />


### <a name="creating-files-and-directories"></a>파일 및 디렉터리 만들기

이 샘플에 사용 하는 방법을 보여 줍니다는 `Environment` 파일 및 디렉터리 만들기 수에서는 문서 폴더에 액세스 하는 클래스입니다.

```csharp
var documents =
 Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments); 
var filename = Path.Combine (documents, "Write.txt");
File.WriteAllText(filename, "Write this text into a file");
```

디렉터리를 만들고 유사한 프로세스입니다.

```csharp
var documents =
 Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var directoryname = Path.Combine (documents, "NewDirectory");
Directory.CreateDirectory(directoryname);
```

System.IO 네임 스페이스에 대 한 자세한 내용은 참조는 [MSDN 설명서](http://msdn.microsoft.com/library/system.io.aspx)합니다.


### <a name="serializing-json"></a>Json 직렬화

Json을 사용 하 여 작업 Xamarin.iOS 응용 프로그램에서 데이터를 쉽게 사용 하는 [Json.NET](http://www.newtonsoft.com/json) .NET NuGet 패키지에 대 한 고성능 JSON 프레임 워크. 응용 프로그램의 프로젝트에 NuGet 패키지를 추가 하기만 하면 됩니다. 

[![](file-system-images/json01.png "응용 프로그램 프로젝트에 NuGet 패키지 추가")](file-system-images/json01.png#lightbox)

다음으로 직렬화/역직렬화에 대 한 데이터 모델로 사용할 클래스를 추가 (이 경우 `Account.cs`):

```csharp
using System;
using System.Collections.Generic;
using Foundation; // for Preserve attribute, which helps serialization with Linking enabled

namespace FileSystem
{
    [Preserve]
    public class Account
    {
        #region Computed Properties
        public string Email { get; set; }
        public bool Active { get; set; }
        public DateTime CreatedDate { get; set; }
        public List<string> Roles { get; set; }
        #endregion

        #region Constructors
        public Account() {

        }
        #endregion
    }
}
```

마지막으로 인스턴스를 만들기는 `Account` 클래스를 json 데이터로 serialize 하 고 파일에 쓰기:

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
Json.NET의 가리킵니다 [설명서](http://www.newtonsoft.com/json/help) .NET 응용 프로그램에서 json 데이터로 작업 하는 방법에 대 한 자세한 내용은 합니다.

<a name="Special_Considerations" />

## <a name="special-considerations"></a>특별 고려 사항

Xamarin.iOS 및.NET 파일 작업, iOS 및 Xamarin.iOS 간의 유사점에도 불구 하 고 몇 가지 중요 한 방법으로.NET에서 다릅니다.

 <a name="runtimeaccessible" />


### <a name="making-project-files-accessible-at-runtime"></a>프로젝트 파일을 런타임에 액세스할 수 있도록

기본적으로 프로젝트에 파일을 추가 하는 경우 최종 어셈블리에 포함 되지 않습니다 하 고 응용 프로그램에 사용할 수 없습니다. 어셈블리의 파일을 포함 하기 위해 콘텐츠 라는 특수 빌드 작업을 사용 하 여 표시 해야 합니다.

포함에 대 한 파일을 표시 하려면 파일을 마우스 오른쪽 단추로 클릭 하 고 선택 **빌드 작업 &gt; 콘텐츠** mac 용 Visual Studio에서 변경할 수도 있습니다는 **빌드 작업** 파일의 **속성** 시트입니다.

 <a name="Case_Sensitivity" />


### <a name="case-sensitivity"></a>대/소문자 구분

IOS 파일 시스템 인지 알아야 할 것 *대/소문자 구분*합니다. 이 사용자 파일 및 디렉터리 이름은 정확히 일치 해야 – README.txt 및 readme.txt 것은 다른 파일 이름 의미 합니다.

.NET 개발자 인 Windows 파일 시스템에 더 익숙한 사용자에 대 한 혼동을 줄 수 있습니다 *대/소문자 구분*– "Files", "파일" 및 "files" 모두 동일한 디렉터리를 참조할 수 있습니다.

따라서 iOS 장치는 대/소문자 구분을 염두에서에 코드를 작성 하지만 iOS 시뮬레이터는 하지 대/소문자 구분 기본적으로 합니다. 즉, 실제 장치에서 실패할 것이 없는 코드가 시뮬레이터에서 작동 여전히에 파일 이름을 대/소문자 구분 파일 자체 코드에서이에 대 한 참조와 다른 경우. 초기 및 iOS 개발 하는 동안에 자주 실제 장치에 배포 하는 중요 한 이유는 이유 중 하나입니다.

 <a name="Path_Separator" />


### <a name="path-separator"></a>경로 구분 기호

슬래시를 사용 하는 iOS '/' 경로 구분 기호로 (백슬래시를 사용 하는 Windows와에서는 달리 ' \').

사용 하는 것이 좋습니다는 것이 혼동 차이 때문에 `System.IO.Path.Combine` 하드 코드 하지 않고 현재 플랫폼에 대 한 특정 경로 구분 기호를 조정 하는 메서드. 이 코드 이식성 다른 플랫폼에 간단한 단계입니다.

 <a name="Application_Sandbox" />


## <a name="application-sandbox"></a>샌드박스 응용 프로그램

파일 시스템 (및 네트워크 및 하드웨어 기능 등의 다른 리소스)에 대 한 응용 프로그램의 액세스 보안상의 이유로 제한 됩니다. 이 제한 이라고 합니다 *응용 프로그램 샌드박스*합니다. 파일 시스템 측면에서 응용 프로그램 만들기 및 해당 홈 디렉터리의 파일 및 디렉터리 삭제로 제한 됩니다.

홈 디렉터리는 응용 프로그램 및 모든 데이터가 저장 되는 위치는 파일 시스템에서 고유 위치. 선택 (하거나 변경할 수) 응용 프로그램에 대 한 홈 디렉터리의 위치 그러나 iOS 및 Xamarin.iOS 속성 및 파일 및 디렉터리 내에서 관리 하는 메서드를 제공 합니다.

 <a name="The_Application_Bundle" />


## <a name="the-application-bundle"></a>응용 프로그램 번들

합니다 *응용 프로그램 번들* 응용 프로그램이 포함 된 폴더입니다.
.App 접미사 디렉터리 이름에 추가 하 여 다른 폴더에서 구분 됩니다. 실행 파일 및 모든 콘텐츠 (파일, 이미지 등) 프로젝트에 필요한 사용자 응용 프로그램 번들에 포함 되어 있습니다.

Mac os에서에 응용 프로그램 번들을 찾아볼 때 표시 됩니다 다른 아이콘을 사용 하 여 다른 디렉터리에 표시 합니다 (및.app 접미사 숨겨져); 보다 그러나 일반 디렉터리 운영 체제를 다르게 표시 하는 것입니다.

샘플 코드에 대 한 응용 프로그램 번들을 보려면 마우스 오른쪽 단추를 Visual Studio에서 프로젝트를 선택한 Mac **상위 폴더 열기**합니다. 다음 이동할 **bin/Debug/** 여기서 찾아야 응용 프로그램 아이콘 (아래 스크린샷과 유사).

 [![](file-system-images/40-bundle.png "응용 프로그램 아이콘을이 스크린샷과 유사 하 게 찾으려면 bin/Debug로 이동")](file-system-images/40-bundle.png#lightbox)

이 아이콘을 마우스 오른쪽 단추로 클릭 하 고 선택 **패키지 내용 보기** 응용 프로그램 번들 디렉터리의 내용을 탐색할 수 있습니다. 내용을 다음과 같이 일반 디렉터리의 내용을 처럼 표시 됩니다.

 [![](file-system-images/45-bundle.png "앱 번들의 콘텐츠")](file-system-images/45-bundle.png#lightbox)

응용 프로그램 번들 설치 된 기능 또는 장치 시뮬레이터에서 테스트 중 이며 궁극적으로 앱 스토어에 포함 하기 위해 Apple에 제출 되 어떤 합니다.

 <a name="Application_Directories" />


## <a name="application-directories"></a>응용 프로그램 디렉터리

응용 프로그램을 장치에 설치 될 때 운영 체제는 홈 디렉터리를 만들고 내부 응용 프로그램 번들에 배치 합니다. 코드는 데이터를 응용 프로그램 번들을 액세스할 수 있지만, nothing에 써야 해당 루트 디렉터리와 서명 된 모든 수정 응용 프로그램을 무효화 되며 시작 하지 못하도록 방지 합니다.

루트 디렉터리에 쓸 수는 아무 것도 있지만 따라서 <b>iOS 7 및 이전 버전에서</b> 다양 한 용도로 사용할 수 있는 응용 프로그램 루트 디렉터리 내에서 디렉터리를 만듭니다. <b>사용자에 액세스할 수 있는 디렉터리에 iOS 8 <a href="https://developer.apple.com/library/ios/technotes/tn2406/_index.html" target="_blank">있지</a> 응용 프로그램 루트 내의</b>합니다.

이러한 디렉터리 및 해당 용도 다음과 같습니다.

&nbsp;

|디렉터리|설명|
|---|---|
|[ApplicationName].app/|**IOS 7 및 이전 버전의** 이것이 `ApplicationBundle` 응용 프로그램 실행 파일 저장 되는 디렉터리입니다. 앱에서 만든 디렉터리 구조 (예: 이미지 및 Mac 프로젝트에 대 한 Visual Studio에서 리소스로 표시 하는 다른 파일 형식)이이 디렉터리에 있습니다.<br /><br />이 디렉터리의 경로를 통해 사용할 수는 응용 프로그램 번들에 포함 된 콘텐츠 파일에 액세스 해야 할 경우는 `NSBundle.MainBundle.BundlePath` 속성입니다.|
|문서 /|이 디렉터리를 사용 하 여 사용자 문서 및 응용 프로그램 데이터 파일을 저장 합니다.<br /><br />이 디렉터리의 내용은 사용할 수는 사용자에 게 iTunes 파일 공유 (하지만이 기본적으로 사용 안 함)을 통해. 추가 된 `UIFileSharingEnabled` 이러한 파일에 액세스할 수 있도록 하려면 Info.plist 파일에 부울 키입니다.<br /><br />응용 프로그램 하지 즉시 파일 공유를 사용 하도록 하는 경우에이 디렉터리에 사용자의 숨겨진 파일을 배치 하지 않아야 (예: 데이터베이스 파일을 공유 하려는 경우가 아니면). 중요 한 파일이 남아 숨겨진, 이러한 파일 하지 노출 (되며 수정 또는 삭제 iTunes에서 잠재적으로 이동) 나중 버전에서 파일 공유를 사용 하는 경우.<br /><br /> 사용할 수는 `Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments)` 메서드를 응용 프로그램에 대 한 문서 디렉터리의 경로를 가져옵니다.<br /><br />이 디렉터리의 내용은 iTunes에서 백업 됩니다.|
|라이브러리 /|라이브러리 디렉터리는 데이터베이스 또는 다른 응용 프로그램에서 생성 된 파일과 같은 사용자가 직접 생성 되지 않은 파일을 저장 하는 것이 좋습니다. 이 디렉터리의 내용은 iTunes 통해 사용자에 게 노출 되지 않습니다.<br /><br />Library;에서 고유한 하위 디렉터리를 만들 수 있습니다. 그러나 이미 일부 시스템에서 만든 디렉터리가 있으면 여기의 기본 설정 및 캐시를 포함 하 여 알고 있어야 합니다.<br /><br />(제외 캐시 하위 디렉터리)이이 디렉터리의 내용은 iTunes에서 백업 됩니다. 라이브러리에서 만든 사용자 지정 디렉터리 백업 됩니다.|
|라이브러리/기본 /|응용 프로그램별 기본 설정 파일은이 디렉터리에 저장 됩니다. 이러한 파일을 직접 만들지 않습니다. 대신는 `NSUserDefaults` 클래스입니다.<br /><br />이 디렉터리의 내용은 iTunes에서 백업 됩니다.|
|라이브러리/캐시 /|캐시 디렉터리가 응용 프로그램에 도움이 되는 데이터 파일을 저장 하는 것이 좋습니다 실행 되지만 필요한 경우 쉽게 다시 만들 수는 있습니다. 응용 프로그램 만들기 및 필요에 따라 이러한 파일을 삭제 해야 다시 필요한 경우 이러한 파일을 만들 수 있습니다. iOS 5 (매우 짧은 저장소의 경우)에서 이러한 파일을 삭제할 수도 있지만 수행은 응용 프로그램이 실행 되는 동안에 하지 것입니다.<br /><br />즉, 사용자 장치를 복원 하는 경우 표시 되지 않으므로 iTunes에서이 디렉터리의 콘텐츠를 백업 하지 않습니다 하 고 응용 프로그램의 업데이트 된 버전 설치 된 후에 나타날 수 있습니다.<br /><br />예를 들어, 응용 프로그램 네트워크에 연결할 수 없는 경우 데이터 또는 오프 라인 좋은 환경을 제공 하기 위해 파일을 저장 하는 캐시 디렉터리를 사용할 수 있습니다. 응용 프로그램에서 저장 하 고 네트워크 응답을 기다리는 동안이 데이터를 신속 하 게 검색할 수 있지만 백업할 필요가 없습니다 및 수 쉽게 수 복구 하거나 복원 또는 버전을 업데이트 한 후 다시 만들어집니다.|
|tmp/|응용 프로그램이이 디렉터리의 짧은 기간 동안에만 필요한 임시 파일을 저장할 수 있습니다. 공간 절약을 위해 더 이상 필요 없는 경우 파일이 삭제 됩니다. 응용 프로그램이 실행 중이지 않을 때 운영 체제가이 디렉터리에서 파일을 삭제할 수도 수 있습니다.<br /><br />이 디렉터리의 내용은 iTunes에서 백업 되지 않습니다.<br /><br />예를 들어, tmp 디렉터리 데 사용할 수 있습니다 (예: Twitter 아바타 또는 전자 메일 첨부 파일)을 사용자에 게 표시 하기 위해 다운로드 하는 되지만 했습니다 되었습니다 볼 (있으며 나중에 필요한 경우 다시 다운로드 되 면 삭제할 수 없습니다는 임시 파일을 저장 ).|

이 스크린샷에서 Finder 창에서 디렉터리 구조를 보여 줍니다.

 [![](file-system-images/08-library-directory.png "이 스크린샷은 Finder 창에서 디렉터리 구조를 보여 줍니다.")](file-system-images/08-library-directory.png#lightbox)

 <a name="Accessing_Other_Directories_Programmatically" />

### <a name="accessing-other-directories-programmatically"></a>다른 디렉터리를 프로그래밍 방식으로 액세스

에 액세스 하는 이전 디렉터리 및 파일 예제는 `Documents` 디렉터리입니다. 쓸 다른 디렉터리를 사용 하 여 경로 생성 해야 합니다는 "..." 구문을 다음과 같이 합니다.

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var library = Path.Combine (documents, "..", "Library");
var filename = Path.Combine (library, "WriteToLibrary.txt");
File.WriteAllText(filename, "Write this text into a file in Library");
```

디렉터리를 만들 하는 것은 매우 유사 합니다.

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var library = Path.Combine (documents, "..", "Library");
var directoryname = Path.Combine (library, "NewLibraryDirectory");
Directory.CreateDirectory(directoryname);
```

경로 `Caches` 및 `tmp` 디렉터리 같이 생성할 수 있습니다.

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var cache = Path.Combine (documents, "..", "Library", "Caches");
var tmp = Path.Combine (documents, "..", "tmp");
```

 <a name="Sharing_Files_with_the_User_through_iTunes" />


## <a name="sharing-files-with-the-user-through-itunes"></a>ITunes 통해 사용자를 사용 하 여 파일을 공유합니다.

사용자를 편집 하 여 응용 프로그램의 문서 디렉터리의 파일에 액세스할 수 있습니다 `Info.plist` 만들고는 **응용 프로그램이 iTunes 공유를 지원** (`UIFileSharingEnabled`)에서 항목을 **원본** 보기도 다음과 같습니다.

 [![](file-system-images/09-uifilesharingenabled-plist.png "속성을 공유 하는 iTunes를 지 원하는 응용 프로그램 추가")](file-system-images/09-uifilesharingenabled-plist.png#lightbox)

장치가 연결 되어 있고 사용자가 선택 하는 경우 iTunes에서 이러한 파일에 액세스할 수 있습니다는 `Apps` 탭 합니다. 예를 들어 다음 스크린샷은 iTunes를 통해 공유 하는 선택한 앱의 파일을 보여줍니다.

 [![](file-system-images/10-itunes-file-sharing.png "이 스크린샷에서 iTunes를 통해 공유 하는 선택한 앱의 파일을 표시")](file-system-images/10-itunes-file-sharing.png#lightbox)

사용자는 iTunes 통해이 디렉터리의 최상위 항목만 액세스할 수 있습니다. (하지만 해당 컴퓨터에 복사 하거나 삭제할 수 있습니다) 모든 하위 디렉터리의 내용을 볼 수 없습니다. 예를 들어 GoodReader를 사용 하 여 PDF 파일과 EPUB 공유할 수 있습니다 응용 프로그램을 사용 하 여 사용자가 iOS 장치에서 읽을 수 있도록.

사용자가 자신의 문서 폴더의 내용을 수정 주의 하지 않을 경우 문제가 발생할 수 있습니다. 응용 프로그램이 고려 하 고 Documents 폴더의 안전 하지 않은 업데이트에 탄력적으로 대처 해야 합니다.

이 문서에 대 한 샘플 코드 Documents 폴더에 파일 및 폴더를 만듭니다 (에서 **SampleCode.cs**)에서 파일 공유를 사용 하도록 설정 하 고는 **Info.plist** 파일입니다. 이 스크린샷에서 이러한 iTunes에서 표시 하는 방법을 보여 줍니다.

 [![](file-system-images/15-itunes-file-sharing-example.png "이 스크린 샷에서 iTunes에서 파일을 표시 하는 방법을 보여 줍니다.")](file-system-images/15-itunes-file-sharing-example.png#lightbox)

참조를 [이미지를 사용 하 여 작업](~/ios/app-fundamentals/images-icons/index.md) 만든 응용 프로그램 및 모든 사용자 지정 문서 유형에 대 한 아이콘을 설정 하는 방법에 대 한 정보에 대 한 문서.

경우는 `UIFileSharingEnabled` 키가 잘못 된 경고나 없는 파일 공유를 기본적으로 사용 하지 않도록 설정 하 고 프로그램 Documentsdirectory 상호 작용할 수 없습니다.

 <a name="Backup_and_Restore" />

## <a name="backup-and-restore"></a>백업 및 복원

장치를 iTunes에서 백업 응용 프로그램의 홈 디렉터리에 생성 된 모든 디렉터리는 다음을 제외한 저장 됩니다.

-   **[ApplicationName].app** – The 응용 프로그램 번들 *않습니다* 백업, 번들 (응용 프로그램 업데이트를 설치 하면 예를 들어) 변경 된 경우에만 합니다. 이므로 설치가 변경 되지 않은 상태로 유지 해야 서명 되어 있으므로이 디렉터리를 어쨌든 수정 해서는 안 됩니다. 
-   **라이브러리/캐시** – 캐시 디렉터리를 백업할 필요가 없는 작업 파일에 대 한 것입니다. 
-   **tmp** –이 디렉터리는 생성 되 고 더 이상 필요를 삭제 하는 임시 파일에 사용 하거나 파일에 대 한 해당 iOS 공간이 더 필요한 경우을 삭제 합니다. 


많은 양의 데이터를 백업 하면 시간이 오래 걸릴 수 있습니다. 문서 및 라이브러리를 응용 프로그램 에서만 사용 해야 있는 특정 문서 또는 데이터를 백업 해야 할 경우이 대 한 폴더입니다. 임시 데이터 또는 네트워크에서 쉽게 검색할 수 있는 파일에 대 한 캐시 또는 tmp 디렉터리를 사용 합니다.

iOS는 '정리' 파일 시스템 장치 디스크 공간이 매우 부족 실행 될 때 주의 합니다. 이 프로세스는 tmp 라이브러리/캐시에서 모든 파일을 제거 하는 현재 실행 되지 않는 응용 프로그램의 폴더입니다.

 <a name="Complying_with_iOS5_iCloud_Backup_Restrictions" />

## <a name="complying-with-ios5-icloud-backup-restrictions"></a>IOS5 iCloud 백업 제한 사항 준수

Apple 도입 *iCloud 백업* iOS 5 사용 하 여 기능 합니다. ICloud 백업 사용 되는 경우, 응용 프로그램의 홈 디렉터리에 있는 모든 파일 (일반적으로 백업 되지, 예를 들어, 앱 번들에는 디렉터리를 제외 `Caches` 및 `tmp`)는 백업 iCloud 서버. 이 기능은 장치 분실, 도난 또는 손상 된 경우 전체 백업 사용 하 여 사용자를 제공 합니다.

ICloud만을 제공 하므로 '무료' 공간이 5gb 각 사용자에 게 및 대역폭을 불필요 하 게 사용 하지 않으려면 Apple 응용 프로그램을만 백업 필수 사용자 생성 데이터는 필요 합니다. IOS 데이터 저장소 지침을 준수 하기 다음 항목을 준수 함으로써 백업 하는 데이터 양을 제한 해야 합니다.

-  사용자 생성 데이터 또는 그렇지 않은 경우 (즉, 백업) 문서 디렉터리에 다시 만들 수 없는 데이터에만 저장 합니다. 
-  쉽게 다시 생성 또는 다시에서 다운로드할 수 있는 다른 모든 데이터를 저장할 `Library/Caches` 또는 `tmp` (하는 백업 되지 및 수 '정리'). 
-  에 대 한 적절 한 수 파일이 있는 경우는 `Library/Caches` 또는 `tmp` 폴더 있지만 하지 않으려는 '정리', 저장할 다른 위치 (같은 `Library/YourData` ) 적용는 ' 백업 하지 않으면 등록 ' iCloud 백업 ba를 사용 하 여 파일을 방지 하기 위해 특성 ndwidth 및 저장소 공간입니다. 이 데이터는 여전히 장치에 공간을 사용 신중 하 게 관리 하 고 가능한 경우 삭제 해야 합니다. 

' 백업 하지 않으면 등록 ' 특성은 사용 하 여 설정 됩니다는 `NSFileManager` 클래스입니다. 클래스를 확인 하십시오 `using Foundation` 호출 `SetSkipBackupAttribute` 같이:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var filename = Path.Combine (documents, "LocalOnly.txt");
File.WriteAllText(filename, "This file will never get backed-up. It would need to be re-created after a restore or re-install");
NSFileManager.SetSkipBackupAttribute (filename, true); // backup will be skipped for this file
```

때 `SetSkipBackupAttribute` 됩니다 `true` 파일을 백업에서 저장 된 디렉터리에 관계 없이 됩니다 (도 `Documents` 디렉터리). 사용 하 여 특성을 쿼리할 수 있습니다 합니다 `GetSkipBackupAttribute` 메서드를 다시 설정할 수를 호출 하 여 합니다 `SetSkipBackupAttribute` 메서드를 `false`, 다음과 같은:

```csharp
NSFileManager.SetSkipBackupAttribute (filename, false); // file will be backed-up
```

## <a name="sharing-data-between-ios-apps-and-app-extensions"></a>공유 데이터 간의 iOS 앱 및 앱 확장

앱 확장을 포함 하는 앱) (달리 호스트 응용 프로그램의 일부로 실행 되므로 데이터를 공유 하지 않습니다 자동 포함 되므로 추가 작업이 필요 하지 않습니다. 앱 그룹 메커니즘 iOS 앱 데이터를 공유할 수 있도록 사용 됩니다. 응용 프로그램은 제대로 올바른 권한 부여를 사용 하 여 구성 및 프로 비전 되었으면, 해당 일반 iOS 샌드박스 외부에서 공유 디렉터리를 액세스할 수 있습니다.

### <a name="configure-an-app-group"></a>앱 그룹 구성

공유 위치를 사용 하 여 구성 되어는 [앱 그룹](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19)에서 구성 된 합니다 **인증서, 식별자 및 프로필** 섹션에서 [iOS 개발자 센터](https://developer.apple.com/devcenter/ios/)합니다. 이 값을 참조 하 여 각 프로젝트에도 해야 합니다 **Entitlements.plist**합니다.

앱 그룹을 만들기 및 구성에 대 한 내용은 참조는 [앱 그룹 기능](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) 가이드입니다.

### <a name="files"></a>파일

IOS 앱 및 확장 공통 파일 경로 사용 하 여 파일을 공유할 수도 있습니다 (지정 된 제대로 올바른 권한 부여를 사용 하 여 구성 및 프로 비전 될):

```csharp
var FileManager = new NSFileManager ();
var appGroupContainer =FileManager.GetContainerUrl ("group.com.xamarin.WatchSettings");
var appGroupContainerPath = appGroupContainer.Path

Console.WriteLine ("Group Path: " + appGroupContainerPath);

// use the path to create and update files
...
```
> [!IMPORTANT]
> 그룹 경로 반환 된 `null`자격 및 프로 비전 프로필의 구성을 확인 하 고 그 설정이 정확한 지 확인 합니다.


<a name="Application_Version_Updates" />

## <a name="application-version-updates"></a>응용 프로그램 버전 업데이트

응용 프로그램의 새 버전을 다운로드 하면 iOS는 새 홈 디렉터리를 만들고 새 응용 프로그램 번들이 변수에 저장 합니다. 그런 다음 새 홈 디렉터리에 iOS 응용 프로그램 번들의 이전 버전에서이 다음 폴더를 이동합니다.

-  **문서**
-  **라이브러리**


다른 디렉터리 간에 복사 및 새 홈 디렉터리 아래에 배치 될 수 있습니다 수도 있지만 응용 프로그램이이 시스템 동작에 의존 하지 않아야 하므로 복사할 보장 하지는 합니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 파일 시스템 작업 간단할 Xamarin.iOS를 사용 하 여 다른.NET 응용 프로그램과 보여 주었습니다. 또한 응용 프로그램 샌드박스를 도입 하 고 발생 하는 보안 관련 문제를 검사 합니다. 다음으로, 응용 프로그램 번들 개념이 알아보았습니다. 마지막으로 응용 프로그램에 사용할 수 있는 특수 한 디렉터리를 열거 하 고 응용 프로그램 업그레이드 및 백업 하는 동안 해당 역할을 설명 합니다.


## <a name="related-links"></a>관련 링크

- [FileSystemSampleCode](https://developer.xamarin.com/samples/FileSystemSampleCode/)
- [파일 시스템 프로그래밍 가이드](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/FileSystemProgrammingGUide/Introduction/Introduction.html)
- [사용자 앱에서 지 원하는 형식 파일을 등록](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html#/apple_ref/doc/uid/TP40010411-SW1)
