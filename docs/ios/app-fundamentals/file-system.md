---
title: "파일 시스템 작업"
description: "Xamarin.iOS 모든.NET 응용 프로그램에서 사용할 수 있는 iOS의 파일 및 디렉터리를 사용 하는 동일한 System.IO 클래스를 사용할 수 있습니다. 그러나 친숙 한 클래스 및 메서드를 불구 하 고 iOS 생성 하거나 액세스할 수 있는 파일에 몇 가지 제한 사항이 구현 및 특별 한 기능도 제공 특정 디렉터리. 이 문서에서는 이러한 제한 사항 및 기능에 설명 하 고 파일 액세스 Xamarin.iOS 응용 프로그램에서 작동 하는 방법을 보여 줍니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 37DF2F38-901E-8F8E-269A-5EE0CCD28C08
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: a2c3ce9e19340067d77a8bc131b5a247806ecfa1
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-the-file-system"></a>파일 시스템 작업

_Xamarin.iOS 모든.NET 응용 프로그램에서 사용할 수 있는 iOS의 파일 및 디렉터리를 사용 하는 동일한 System.IO 클래스를 사용할 수 있습니다. 그러나 친숙 한 클래스 및 메서드를 불구 하 고 iOS 생성 하거나 액세스할 수 있는 파일에 몇 가지 제한 사항이 구현 및 특별 한 기능도 제공 특정 디렉터리. 이 문서에서는 이러한 제한 사항 및 기능에 설명 하 고 파일 액세스 Xamarin.iOS 응용 프로그램에서 작동 하는 방법을 보여 줍니다._

Xamarin.iOS를 사용할 수 있습니다 및 `System.IO` 에 있는 클래스는 *.NET 클래스 라이브러리 BCL (기본)* iOS 파일 시스템에 액세스할 수 있습니다. `File` 클래스 사용 생성, 삭제 및 파일을 읽을 수 있습니다 및 `Directory` 클래스 만들기, 삭제 또는 디렉터리의 내용을 열거할 수 있습니다. 사용할 수도 있습니다 `Stream` 높은 수준의 파일 작업 (예: 파일 내에서 압축 또는 위치 검색)에 대 한 제어를 제공할 수 있는 하위 클래스입니다.

응용 프로그램 응용 프로그램의 데이터의 보안을 유지 하 고 악성 응용 프로그램에서 사용자가 보호 하는 파일 시스템으로 수행할 수 있는 몇 가지 제한을 적용 하는 iOS입니다. 이러한 제한은의 일부는 *샌드박스 응용 프로그램* -환경 설정, 네트워크 리소스, 하드웨어, 등 파일에 대 한 응용 프로그램의 액세스를 제한 하는 규칙의 집합입니다. 응용 프로그램 (설치 된 위치); 홈 디렉터리 내에서 파일 읽기 및 쓰기로 제한 됩니다. 다른 응용 프로그램의 파일에 액세스할 수 없는 것입니다.

iOS 파일 시스템 관련 기능도 일부 있으며: 특정 디렉터리 백업 및 업그레이드와 관련 하 여 특수 한 처리가 필요 하 고 응용 프로그램은 iTunes 통해 파일을 공유할 수도 있습니다.

이 문서는 기능을 설명 및 iOS의 제한 사항 자세히 파일 시스템 고 Xamarin.iOS 몇 가지 간단한 파일 시스템 작업 실행을 사용 하는 방법을 보여 주는 샘플 응용 프로그램을 포함 합니다.

 [ ![](file-system-images/05-sampleapp.png "몇 가지 간단한 파일 시스템 작업을 실행 하는 iOS의 샘플")](file-system-images/05-sampleapp.png)

 <a name="General_File_Access" />


## <a name="general-file-access"></a>일반 파일 액세스

Xamarin.iOS.NET을 사용 하면 `System.IO` iOS에서 파일 시스템 작업에 대 한 클래스입니다.

다음 코드 조각은 몇 가지 일반적인 파일 작업을 보여 줍니다. 찾을 수에서 아래 모든 해당는 `SampleCode.cs` 이 문서에 대 한 샘플 응용 프로그램에서 파일입니다.

 <a name="Working_with_directories" />


### <a name="working-with-directories"></a>디렉터리 작업

현재 디렉터리에 하위 디렉터리를 열거 하는이 코드 (에 지정 된는 ". /" 매개 변수)을 하는 응용 프로그램 실행 파일의 위치입니다.
출력에는 모든 파일 및 폴더 (디버깅 하는 동안 콘솔 창에 표시 됨) 응용 프로그램과 함께 배포 되는 목록이 됩니다.

```csharp
var directories = Directory.EnumerateDirectories("./");
foreach (var directory in directories) {
      Console.WriteLine(directory);
}
```

 <a name="Reading_files" />


### <a name="reading-files"></a>파일 읽기

텍스트 파일을 읽을 수만 하면 코드 한 줄 됩니다. 이 예제에서는 응용 프로그램 출력 창에 텍스트 파일의 내용을 표시 됩니다.

```csharp
var text = File.ReadAllText("TestData/ReadMe.txt");
Console.WriteLine(text);
```

 <a name="XML_Serialization" />


### <a name="xml-serialization"></a>XML Serialization

전체 작업할 수 있지만 `System.Xml` 네임 스페이스는이 문서의 범위를 벗어나는 다음과 같이 StreamReader를 사용 하 여 파일 시스템에서 XML 문서를 쉽게 읽을 수 있습니다.

```csharp
using (TextReader reader = new StreamReader("./TestData/test.xml")) {
      XmlSerializer serializer = new XmlSerializer(typeof(MyObject));
      var xml = (MyObject)serializer.Deserialize(reader);
}
```

에 대 한 MSDN 설명서를 참조는 [System.Xml](http://msdn.microsoft.com/en-us/library/system.xml.aspx) 네임 스페이스에 대 한 자세한 내용은 [serialization](http://msdn.microsoft.com/en-us/library/system.xml.serialization.aspx)합니다. 도 검토 해야는 [Xamarin.iOS 설명서](~/ios/deploy-test/linker.md) 링커 – 일반적으로 해야 합니다 추가 하는 `[Preserve]` serialize 하려는 클래스에 특성입니다.

 <a name="Creating_Files_and_Directories" />


### <a name="creating-files-and-directories"></a>파일 및 디렉터리 만들기

이 예제에서는 사용 하는 방법을 보여 줍니다.는 `Environment` 클래스 파일 및 디렉터리를 만들 수 있습니다 문서 폴더에 액세스할 수 있습니다.

```csharp
var documents =
 Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments); 
var filename = Path.Combine (documents, "Write.txt");
File.WriteAllText(filename, "Write this text into a file");
```

디렉터리를 만드는 작업은 매우 유사한 프로세스:

```csharp
var documents =
 Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var directoryname = Path.Combine (documents, "NewDirectory");
Directory.CreateDirectory(directoryname);
```

System.IO 네임 스페이스에 대 한 자세한 내용은 참조는 [MSDN 설명서](http://msdn.microsoft.com/en-us/library/system.io.aspx)합니다.


### <a name="serializing-json"></a>Json 직렬화

Json 사용 Xamarin.iOS 응용 프로그램에서 데이터를 매우 쉽게 사용 하는 [Json.NET](http://www.newtonsoft.com/json) .NET NuGet 패키지용 고성능 JSON 프레임 워크입니다. 응용 프로그램의 프로젝트에 NuGet 패키지를 추가 합니다. 

[ ![](file-system-images/json01.png "응용 프로그램 프로젝트에 NuGet 패키지 추가")](file-system-images/json01.png)

다음에 serialization/deserialization에 대 한 데이터 모델로 사용할 클래스를 추가 (이 경우 `Account.cs`):

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

마지막으로,의 인스턴스를 만듭니다는 `Account` 클래스, json 데이터를 serialize 하 고 파일에 기록 합니다.

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
Json.NET의를 참조 [설명서](http://www.newtonsoft.com/json/help) .NET 응용 프로그램에서 json 데이터로 작업 하는 방법에 대 한 자세한 내용은 합니다.

<a name="Special_Considerations" />

## <a name="special-considerations"></a>특별 고려 사항

Xamarin.iOS 및.NET 파일 작업, iOS 및 Xamarin.iOS 간의 유사성을 충분히 불구 하 고 몇 가지 중요 한 방식에서.NET에서 다릅니다.

 <a name="runtimeaccessible" />


### <a name="making-project-files-accessible-at-runtime"></a>프로젝트 파일을 런타임에 액세스할 수 있도록 설정

기본적으로 파일을 프로젝트에 추가 하는 경우 최종 어셈블리에 포함 되지 않습니다 고 따라서 응용 프로그램에 사용할 수 없습니다. 어셈블리의 파일을 포함 하기 위해 콘텐츠를 호출 하면 특수 빌드 작업으로 표시 해야 합니다.

포함에 대 한 파일을 표시 하려면 파일을 마우스 오른쪽 단추로 클릭 하 고 선택 **빌드 작업 &gt; 콘텐츠** Mac.에 대 한 Visual Studio에서 변경할 수 있습니다는 **빌드 작업** 파일의 **속성** 시트입니다.

 <a name="Case_Sensitivity" />


### <a name="case-sensitivity"></a>대/소문자 구분

IOS 파일 시스템 받음을 이해 해야 *대/소문자 구분*합니다. 즉, 파일 및 디렉터리 이름은 정확히 일치 해야 – README.txt 및 readme.txt 다른 파일 이름 간주 됩니다.

이 Windows 파일 시스템에 더 익숙한.NET 개발자를 위한 혼동 될 수 없습니다 *대/소문자 구분*– "Files", "FILES"와 "files"는 모두 동일한 디렉터리를 참조 합니다.

따라서 iOS 장치는 대/소문자 구분 있으며 염두에서에 코드를 작성, iOS 시뮬레이터는 하지 대/소문자 구분 기본적으로 합니다. 이 실제 장치에서 실패 하 게 하지만 해당 코드가 시뮬레이터에서 작동 여전히 파일 자체와 코드에서 참조를 문자가 다른 프로그램 파일 이름 대/소문자를 의미 합니다. 이 초기 바인딩과 iOS 개발 하는 동안에 자주 실제 장치에 배포 하는 중요 한 이유는 여러 가지 이유 중 하나입니다.

 <a name="Path_Separator" />


### <a name="path-separator"></a>경로 구분 기호

iOS 슬래시를 사용 하 여 '/' 경로 구분 기호로 (백슬래시를 사용 하 여 Windows와에서는 달리 ' \').

이러한 혼동 차이로 인해 것이 좋습니다 사용 하는 `System.IO.Path.Combine` 하드 코드 하지 않고 현재 플랫폼에 대 한 특정 경로 구분 기호를 조정 하는 메서드. 다른 플랫폼에 게 코드 이식성 있는 간단한 단계입니다.

 <a name="Application_Sandbox" />


## <a name="application-sandbox"></a>샌드박스 응용 프로그램

파일 시스템 (및 네트워크 및 하드웨어 기능 등의 다른 리소스)에 대 한 응용 프로그램의 액세스 보안상의 이유로 제한 됩니다. 이 제한은 라고는 *응용 프로그램 샌드박스*합니다. 파일 시스템의 관점에서 응용 프로그램을 만들고 홈 디렉터리의 파일 및 디렉터리 삭제로 제한 됩니다.

홈 디렉터리는 고유한 응용 프로그램 및 모든 데이터가 저장 되는 파일 시스템에 있습니다. 선택 (하거나 변경할 수); 응용 프로그램에 대 한 홈 디렉터리의 위치 그러나 iOS 및 Xamarin.iOS 속성 및 내부 디렉터리와 파일을 관리 하는 메서드를 제공 합니다.

 <a name="The_Application_Bundle" />


## <a name="the-application-bundle"></a>응용 프로그램 번들

*응용 프로그램 번들* 응용 프로그램을 포함 하는 폴더입니다.
.App 접미사 디렉터리 이름에 추가 하 여 다른 폴더에서 구분 됩니다. 실행 파일 및 모든 콘텐츠 (파일, 이미지 등) 프로젝트에 필요한 응용 프로그램 번들에 포함 되어 있습니다.

Mac OS에서 응용 프로그램 번들에를 탐색할 때 표시 아이콘은 보다 다른 디렉터리에서 참조 (및.app 접미사 숨겨져); 그러나 되기 일반 디렉터리 운영 체제를 다르게 표시입니다.

샘플 코드에 대 한 응용 프로그램 번들을 보려면 마우스 오른쪽 단추로 클릭 Visual Studio에서 프로젝트를 선택 하 고 Mac에 대 한 **상위 폴더 열기**합니다. 다음로 이동 **bin/Debug/** 위치를 찾아야 응용 프로그램 아이콘 (아래 스크린샷에서 유사).

 [ ![](file-system-images/40-bundle.png "Bin/Debug이이 스크린샷과 유사 하 게 응용 프로그램 아이콘을 찾을 수로 이동")](file-system-images/40-bundle.png)

이 아이콘을 마우스 오른쪽 단추로 클릭 하 고 선택 **패키지 내용 보기** 응용 프로그램 번들 디렉터리의 내용을 찾아볼 수 있습니다. 내용을 다음과 같이 일반 디렉터리의 내용을와 동일 하 게 표시 됩니다.

 [ ![](file-system-images/45-bundle.png "앱 번들의 내용")](file-system-images/45-bundle.png)

응용 프로그램 번들 시뮬레이터 또는 장치에서 테스트 중에 설치 되어 지정 되며 최종적으로 Apple 앱 스토어에 포함할에 제출 내용입니다.

 <a name="Application_Directories" />


## <a name="application-directories"></a>응용 프로그램 디렉터리

응용 프로그램이 장치에 설치 되 면 운영 체제는 홈 디렉터리를 만들고 내부 응용 프로그램 번들에 배치 합니다. 코드는 데이터를 읽으려면 응용 프로그램 번들을 액세스할 수 있지만 아무 쓸지 해당 루트 디렉터리에 반드시 서명 되도록 하 고 모든 수정 되며 무효화 응용 프로그램에서 시작 하는 것을 방지 합니다.

루트 디렉터리를 작성 해야 하는 아무 것도 따라서 <b>iOS 7 및 이전 버전에서</b> 다양 한 용도로 사용할 수 있는 응용 프로그램 루트 디렉터리에서 디렉터리를 만듭니다. <b>사용자를 액세스할 수 있는 디렉터리에는 iOS 8에서에서 <a href="https://developer.apple.com/library/ios/technotes/tn2406/_index.html" target="_blank">없는</a> 응용 프로그램 루트</b>합니다.

이 디렉터리 및 해당 용도 다음과 같습니다.

&nbsp;

<table>
  <tbody>
    <tr>
      <td>
디렉터리 </td>
      <td>
설명 </td>
    </tr>
    <tr>
      <td>
        <p>[ApplicationName].app/</p>
      </td>
      <td>
        <p><b>IOS 7 및 이전 버전에서</b> 이것이 <code>ApplicationBundle</code> 응용 프로그램 실행 파일 저장 된 디렉터리입니다. 앱에서 만든 디렉터리 구조 (예: 이미지 및 Mac 프로젝트에 대 한 Visual Studio에서 리소스 그룹으로 표시 하는 다른 파일 형식)이이 디렉터리에 있습니다.</p>
        <p>이 디렉터리의 경로를 통해 사용할 수는 응용 프로그램 번들 내부에 있는 콘텐츠 파일에 액세스 해야 할 경우는 <code>NSBundle.MainBundle.BundlePath</code> 속성입니다.</p>
      </td>
    </tr>
    <tr>
      <td>
        <p>문서 /</p>
      </td>
      <td>
        <p>이 디렉터리를 사용 하 여 사용자 문서 및 응용 프로그램 데이터 파일을 저장 합니다.</p>
        <p>이 디렉터리의 내용은 만들 수 있습니다 사용할 수 있는 사용자에 게 iTunes 파일 공유 (하지만이 기본적으로 해제)을 통해. 추가 <code>UIFileSharingEnabled</code> 부울 키이 파일에 액세스할 수 있도록 하려면 Info.plist 파일입니다.</p>
        <p>응용 프로그램 파일 공유를 사용 즉시 하지 않는 경우에이 디렉터리의 사용자가 숨겨진 파일을 저장 하지 마십시오 (데이터베이스 파일을 같은 것을 공유 하려는 경우가 아니면). 중요 한 파일을 숨겨진 유지,으로 이러한 파일 되지 노출 (되며 수정 되거나 삭제 iTunes에서 잠재적으로 이동) 나중 버전에서 파일 공유를 사용 합니다.</p>
        <p>사용할 수는 <code>Environment.GetFolderPath
(Environment.SpecialFolder.MyDocuments)</code> 를 응용 프로그램에 대 한 문서 디렉터리의 경로를 가져오는 메서드입니다.</p>
        <p>이 디렉터리의 내용은 iTunes에서 백업 됩니다.</p>
      </td>
    </tr>
    <tr>
      <td>
        <p>라이브러리 /</p>
      </td>
      <td>
        <p>라이브러리 디렉터리는 데이터베이스 또는 다른 응용 프로그램에서 생성 된 파일 같은 사용자가 직접 생성 되지 않은 파일을 저장 하는 것이 좋습니다.
이 디렉터리의 내용은 iTunes 통해 사용자에 노출 되지 않습니다.</p>
        <p>Library;에서 고유한 하위 디렉터리를 만들 수 있습니다. 그러나 이미 일부 시스템에서 만든 디렉터리가 있으면 여기의 기본 설정 및 캐시를 포함 하 여 인식 되어야 합니다.</p>
        <p>(제외 캐시 하위 디렉터리)이이 디렉터리의 내용은 iTunes에서 백업 됩니다. 라이브러리에서 만든 사용자 지정 디렉터리 백업 됩니다.</p>
      </td>
    </tr>
    <tr>
      <td>
        <p>라이브러리/기본 /</p>
      </td>
      <td>
        <p>응용 프로그램별 기본 설정 파일은이 디렉터리에 저장 됩니다. 이러한 파일을 직접 만들지 않습니다. 대신를 사용 하 여는 <code>NSUserDefaults</code> 클래스입니다.</p>
        <p>이 디렉터리의 내용은 iTunes에서 백업 됩니다.</p>
      </td>
    </tr>
    <tr>
      <td>
        <p>라이브러리/캐시 /</p>
      </td>
      <td>
        <p>캐시 디렉터리 응용 프로그램 수 있는 데이터 파일을 저장 하려면 먼저 실행 중이지만 필요한 경우 쉽게 다시 만들 수 있는 합니다. 응용 프로그램 및 필요에 따라 이러한 파일은 삭제를 만들고 필요한 경우 이러한 파일을 다시 만들 수 있습니다. 하지만 iOS 5 (매우 낮은 저장소의 경우)에서 이러한 파일을 삭제 수행은 응용 프로그램이 실행 되는 동안에 하지 것입니다.</p>
        <p>이 디렉터리의 내용을 즉, 사용자는 장치를 복원 하는 경우 표시 됩니다, iTunes에서 백업 되지 않습니다 및 응용 프로그램의 업데이트 된 버전을 설치한 후에 있을 수 있습니다.</p>
        <p>예를 들어, 응용 프로그램 네트워크에 연결할 수 없는 경우에 데이터 또는 오프 라인 좋은 환경을 제공 하기 위해 파일을 저장 하는 캐시 디렉터리를 사용할 수 있습니다. 응용 프로그램에서 저장 하 고 네트워크 응답을 기다리는 동안이 데이터를 신속 하 게 검색할 수 있지만 백업할 필요가 없습니다 및 수 쉽게 될 복구 하거나 복원 또는 버전을 업데이트 한 다음 다시 생성 됩니다.</p>
      </td>
    </tr>
    <tr>
      <td>
        <p>tmp/</p>
      </td>
      <td>
        <p>응용 프로그램이이 디렉터리에 짧은 기간 동안에만 필요한 임시 파일을 저장할 수 있습니다. 공간을 절약 하려면 더 이상 필요 없는 경우 파일을 삭제 해야 합니다. 응용 프로그램이 실행 중이지 않을 때 운영 체제가이 디렉터리에서 파일을 삭제할 수도 수 있습니다.</p>
        <p>이 디렉터리의 내용은 iTunes에서 백업 되지 않습니다.</p>
        <p>예를 들어 tmp 디렉터리 데 사용할 수 있습니다 (예: Twitter 아바타 또는 전자 메일 첨부 파일)를 사용자에 게 표시 하기 위해 다운로드 하는 되지만 삭제할 수 있습니다 했으므로 되 고 되 면 볼 (필요한 경우 나중에 다시 다운로드 하는 임시 파일을 저장 ).</p>
      </td>
    </tr>
  </tbody>
</table>

이 스크린샷에서 Finder 창의 디렉터리 구조를 보여 줍니다.

 [ ![](file-system-images/08-library-directory.png "이 스크린 샷 Finder 창의 디렉터리 구조를 보여 줍니다.")](file-system-images/08-library-directory.png)

 <a name="Accessing_Other_Directories_Programmatically" />

### <a name="accessing-other-directories-programmatically"></a>프로그래밍 방식으로 다른 디렉터리에 액세스

에 액세스 하는 이전 디렉터리 및 파일 예제는 `Documents` 디렉터리입니다. 사용 하 여 경로 먼저 생성 해야 다른 디렉터리에 쓸 수는 ".." 다음과 같이 구문:

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

에 대 한 경로 `Caches` 및 `tmp` 디렉터리는 다음과 같은 생성 될 수 있습니다.

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var cache = Path.Combine (documents, "..", "Library", "Caches");
var tmp = Path.Combine (documents, "..", "tmp");
```

 <a name="Sharing_Files_with_the_User_through_iTunes" />


## <a name="sharing-files-with-the-user-through-itunes"></a>ITunes 통해 사용자와 파일 공유

사용자가 편집 하 여 응용 프로그램의 문서 디렉터리에 파일에 액세스할 수 `Info.plist` 및 만들기는 **iTunes 공유 응용 프로그램이 지원** (`UIFileSharingEnabled`) 항목에는 **소스** 보기도 다음과 같습니다.

 [ ![](file-system-images/09-uifilesharingenabled-plist.png "속성을 공유 하는 iTunes를 지 원하는 응용 프로그램 추가")](file-system-images/09-uifilesharingenabled-plist.png)

장치가 연결 되어 있고 사용자가 선택 하는 경우 iTunes에서 이러한 파일을 액세스할 수 있습니다는 `Apps` 탭 합니다. 예를 들어 다음 스크린 샷에서 iTunes 통해 공유 하는 선택 된 응용 프로그램에서는 파일:

 [ ![](file-system-images/10-itunes-file-sharing.png "이 스크린 샷 iTunes 통해 공유 하는 선택 된 응용 프로그램의 파일을 보여")](file-system-images/10-itunes-file-sharing.png)

사용자는 iTunes 통해이 디렉터리의 최상위 항목만 액세스할 수 있습니다. (하지만 자신의 컴퓨터에 복사 하거나 삭제할 수 있습니다) 하위 디렉터리의 내용을 볼 수 없습니다. 예를 들어와 GoodReader, PDF 및 EPUB 파일 공유할 수 있습니다 응용 프로그램과 사용자가 iOS 장치에서 읽을 수 있도록.

자신의 문서 폴더의 내용을 수정 하는 사용자는 신중 하 게 없는 경우 문제가 발생할 수 있습니다. 응용 프로그램 고려해이 고 Documents 폴더의 파괴적 업데이트에 유연 하 게 해야 합니다.

이 문서에 대 한 샘플 코드 Documents 폴더에는 파일과 폴더를 만듭니다 (에서 **SampleCode.cs**)에서 파일 공유를 사용 하도록 설정 하 고는 **Info.plist** 파일입니다. 이 스크린샷에서 이러한 iTunes에서 어떻게 나타나는지 보여 줍니다.

 [ ![](file-system-images/15-itunes-file-sharing-example.png "이 스크린샷은 iTunes에 파일을 표시 하는 방법을 보여 줍니다.")](file-system-images/15-itunes-file-sharing-example.png)

참조는 [이미지 작업](~/ios/app-fundamentals/images-icons/index.md) 만들면 모든 사용자 지정 문서 유형에 대 한 응용 프로그램에 대 한 아이콘이 설정 하는 방법에 대 한 내용은 합니다.

경우는 `UIFileSharingEnabled` 키가 false 이거나 없으면이 파일 공유는 기본적으로 사용 하지 않도록 설정 하 고 사용자 프로그램 Documentsdirectory와 상호 작용할 수 없습니다.

 <a name="Backup_and_Restore" />

## <a name="backup-and-restore"></a>백업 및 복원

장치 iTunes에서 백업 될 때 응용 프로그램의 홈 디렉터리에서 만든 모든 디렉터리에서 다음을 제외한 저장 됩니다.

-   **[ApplicationName].app** – The 응용 프로그램 번들 *않습니다* 백업, 있지만 번들 (응용 프로그램 업데이트 설치 된 경우, 예를 들어) 변경 된 경우에 합니다. 서명 되어 있으므로 되 고 설치 후에 변경 되지 않고 유지 해야 하므로이 디렉터리를 누르고, 수정할 하지 않아야 합니다. 
-   **라이브러리/캐시** – 캐시 디렉터리를 백업 하려면 필요 하지 않은 작업 파일 위한 기능입니다. 
-   **tmp** –이 디렉터리는 임시 파일을 만들고 더 이상 필요 하면 삭제 사용 또는 파일에 대 한 해당 iOS 공간이 더 필요한 경우를 삭제 합니다. 


많은 양의 데이터를 백업 하는 시간이 오래 걸릴 수 있습니다. 문서 및 라이브러리 응용 프로그램 에서만 사용 해야 있는 특정 문서 또는 데이터를 백업 해야 할 경우이 대 한 폴더입니다. 임시 데이터 또는 네트워크에서 쉽게 검색할 수 있는 파일에 대 한 캐시 또는 tmp 디렉터리를 사용 합니다.

iOS 됩니다 '정리' 파일 시스템 장치에 디스크 공간이 매우 부족 실행 될 때 유의 합니다. 이 프로세스는 tmp 라이브러리/캐시에서 모든 파일을 제거 하는 현재 실행 하지 않는 응용 프로그램의 폴더입니다.

 <a name="Complying_with_iOS5_iCloud_Backup_Restrictions" />

## <a name="complying-with-ios5-icloud-backup-restrictions"></a>IOS5 iCloud 백업 제한 사항 준수

Apple 도입 *iCloud 백업* iOS 5 포함 된 기능입니다. ICloud 백업을 사용 하도록 설정한 경우, 응용 프로그램의 홈 디렉터리에 있는 모든 파일 (예: 앱 번들을 일반적으로 백업 하지 않습니다 디렉터리를 제외 `Caches` 및 `tmp`)은 백업 iCloud 서버에 있습니다. 이 기능은 장치 분실, 도난 또는 손상 된 경우에 전체 백업 사용 하 여 사용자를 제공 합니다.

ICloud만를 제공 하므로 '무료' 공간이 5gb 불필요 하 게 대역폭을 사용 하지 않으려면 및 각 사용자에 게 Apple 응용 프로그램을 백업 필수 사용자에서 생성 된 데이터만 필요 합니다. IOS 데이터 저장소 지침을 준수 하기에 다음 항목을 준수 하 여 백업 되는 데이터 양을 제한 해야 합니다.

-  만 사용자에서 생성 된 데이터 또는 그렇지 않은 경우 (즉, 백업) Documents 디렉터리에 다시 생성 될 수 없는 데이터를 저장 합니다. 
-  다른 모든 데이터를 쉽게 다시 만들거나 다시 다운로드에 저장 `Library/Caches` 또는 `tmp` (되지 백업, 하며 수 없습니다 '정리'). 
-  에 대 한 적합 한 파일이 있는 경우는 `Library/Caches` 또는 `tmp` 폴더 하지 않으려는 '정리', 다른 위치에 저장 (같은 `Library/YourData` ) 적용 하 고는 ' 백업 하지 않으면를 ' iCloud 백업 ba를 사용 하 여 파일을 방지 하기 위해 특성 ndwidth 및 저장소 공간입니다. 이 데이터는 여전히 장치에 공간을 사용 하므로 신중 하 게 관리 하 고 가능한 경우 삭제 해야 합니다. 

' 백업 하지 않으면를 ' 특성은 사용 하 여 설정 된 `NSFileManager` 클래스. 클래스는 확인 `using Foundation` 호출 `SetSkipBackupAttribute` 다음과 같이 합니다.

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var filename = Path.Combine (documents, "LocalOnly.txt");
File.WriteAllText(filename, "This file will never get backed-up. It would need to be re-created after a restore or re-install");
NSFileManager.SetSkipBackupAttribute (filename, true); // backup will be skipped for this file
```

때 `SetSkipBackupAttribute` 은 `true` 파일 백업에 저장 된 디렉터리에 상관 없이 됩니다 (도 `Documents` 디렉터리). 사용 하 여 특성을 쿼리할 수 있습니다는 `GetSkipBackupAttribute` 메서드를 다시 설정할 수를 호출 하 여는 `SetSkipBackupAttribute` 메서드를 `false`, 다음과 같이 합니다.

```csharp
NSFileManager.SetSkipBackupAttribute (filename, false); // file will be backed-up
```

## <a name="sharing-data-between-ios-apps-and-app-extensions"></a>공유 데이터 간의 iOS 앱 및 앱 확장

응용 프로그램 확장도 포함 하는 앱) (반대로 호스트 응용 프로그램의 일부로 실행 데이터를 공유 되지 않습니다 자동 포함 되므로 추가 작업이 필요 합니다. 응용 프로그램 그룹은 메커니즘 iOS 사용 하 여 다른 앱에서 데이터를 공유 하도록 허용 합니다. 응용 프로그램은 올바른 자격으로 올바르게 구성 된 경우 프로 비전, 공유 디렉터리의 일반 iOS 샌드박스 외부에서 액세스할 수 있습니다.

### <a name="configure-an-app-group"></a>앱 그룹을 구성

공유 위치를 사용 하 여 구성 된는 [App 그룹](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19)에 구성 되어 있는 **인증서, 식별자 및 프로필** 섹션에서 [iOS Dev Center](https://developer.apple.com/devcenter/ios/)합니다. 이 값이 각 프로젝트에서 참조 해야 **Entitlements.plist**합니다.

앱 그룹 만들기 및 구성에 대 한 내용은를 참조는 [앱 그룹 기능](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) 가이드입니다.

### <a name="files"></a>파일

IOS 앱 및 확장 공통 파일 경로 사용 하 여 파일을 공유할 수도 있습니다 (지정 된 제대로 올바른 자격과 구성 및 프로 비전 있다가):

```csharp
var FileManager = new NSFileManager ();
var appGroupContainer =FileManager.GetContainerUrl ("group.com.xamarin.WatchSettings");
var appGroupContainerPath = appGroupContainer.Path

Console.WriteLine ("Group Path: " + appGroupContainerPath);

// use the path to create and update files
...
```
> [!IMPORTANT]
> **참고:** 그룹 경로 반환 되 면 `null`자격과 프로 비전 프로필의 구성을 확인 하 고 정확한 지 확인 합니다.


<a name="Application_Version_Updates" />

## <a name="application-version-updates"></a>업데이트 된 응용 프로그램 버전

응용 프로그램의 새 버전을 다운로드 하는 iOS 새 홈 디렉터리를 만들고 새 응용 프로그램 번들이 변수에 저장 합니다. 그런 다음 새 홈 디렉터리에 iOS 응용 프로그램 번들의 이전 버전에서이 다음 폴더를 이동합니다.

-  **문서**
-  **라이브러리**


다른 디렉터리에서 복사 및 새 홈 디렉터리에서 관리 될 수도 있지만 응용 프로그램이이 시스템 동작에 의존 하지 않아야 하므로 복사 될 보장 되지 하 합니다.

<a name="Summary" />

## <a name="summary"></a>요약

파일 시스템 작업은 간단할 Xamarin.iOS와 다른 모든.NET 응용 프로그램에서와 마찬가지로이 문서에 살펴보았습니다. 또한 응용 프로그램 샌드박스를 도입 하 고 발생 하는 보안 관련 문제를 검사 합니다. 다음으로 응용 프로그램 번들의 개념을 탐색 합니다. 마지막으로, 응용 프로그램에 사용할 수 있는 특별 한 디렉터리를 열거 하 고 응용 프로그램 업그레이드 및 백업 하는 동안 해당 역할을 설명 합니다.


## <a name="related-links"></a>관련 링크

- [FileSystemSampleCode](https://developer.xamarin.com/samples/FileSystemSampleCode/)
- [파일 시스템 프로그래밍 가이드](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/FileSystemProgrammingGUide/Introduction/Introduction.html)
- [사용자 응용 프로그램이 지 원하는 형식 파일을 등록](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html#/apple_ref/doc/uid/TP40010411-SW1)
