---
title: Xamarin.forms에서 파일 처리
description: .NET 표준 라이브러리 또는 포함 된 리소스를 사용 하 여 코드를 사용 하 여 파일 xamarin.forms 처리를 구현할 수 있습니다.
ms.prod: xamarin
ms.assetid: 9987C3F6-5F04-403B-BBB4-ECB024EA6CC8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/21/2018
ms.openlocfilehash: 0be441a7be9777698212e690aca95fdd75e5050f
ms.sourcegitcommit: eac092f84b603958c761df305f015ff84e0fad44
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36310155"
---
# <a name="file-handling-in-xamarinforms"></a>Xamarin.forms에서 파일 처리

_.NET 표준 라이브러리 또는 포함 된 리소스를 사용 하 여 코드를 사용 하 여 파일 xamarin.forms 처리를 구현할 수 있습니다._

## <a name="overview"></a>개요

Xamarin.Forms 코드는 각자 자체적인 파일 시스템을 지닌 여러 개의 플랫폼에서 실행됩니다. 이전에이 네이티브 파일 Api를 사용 하 여 각 플랫폼에는 파일 읽기 및 쓰기를 가장 쉽게 수행 의미 합니다. 또는 포함 된 리소스는 응용 프로그램과 데이터 파일을 배포 하는 보다 간단한 솔루션입니다. 그러나 표준.NET 2.0과 함께 되기.NET 표준 라이브러리에 파일 액세스 코드를 공유할 수 있습니다.

이미지 파일을 처리 하는 대 한 자세한 내용은 참조는 [이미지 작업](~/xamarin-forms/user-interface/images.md) 페이지.

<a name="Loading_and_Saving_Files" />

## <a name="saving-and-loading-files"></a>저장 하 고 파일 로드

`System.IO` 각 플랫폼에서 파일 시스템에 액세스 하는 클래스를 사용할 수 있습니다. `File` 클래스 사용 생성, 삭제 및 파일을 읽을 수 있습니다 및 `Directory` 클래스 만들기, 삭제 또는 디렉터리의 내용을 열거할 수 있습니다. 사용할 수도 있습니다는 `Stream` 높은 수준의 파일 작업 (예: 파일 내에서 압축 또는 위치 검색)에 대 한 제어를 제공할 수 있는 하위 클래스입니다.

사용 하 여 텍스트 파일을 쓸 수는 `File.WriteAllText` 메서드:

```csharp
File.WriteAllText(fileName, text);
```

사용 하 여 텍스트 파일을 읽을 수는 `File.ReadAllText` 메서드:

```csharp
string text = File.ReadAllText(fileName);
```

또한는 `File.Exists` 메서드는 지정 된 파일이 있는지 확인 합니다.

```csharp
bool doesExist = File.Exists(fileName);
```

값을 사용 하 여.NET 표준 라이브러리에서 각 플랫폼에 있는 파일의 경로 확인할 수 있습니다는 [ `Environment.SpecialFolder` ](xref:System.Environment.SpecialFolder) 열거형에는 첫 번째 인수는 `Environment.GetFolderPath` 메서드. 사용 하는 파일와 결합한 다음이 고 `Path.Combine` 메서드:

```csharp
string fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "temp.txt");
```

이러한 작업을 저장 하 고 텍스트를 로드 하는 페이지를 포함 하는 샘플 응용 프로그램을 보여 줍니다.

[![저장 및 로드 텍스트](files-images/saveandload-sml.png "저장 및 응용 프로그램에서 파일 로드")](files-images/saveandload.png#lightbox "저장 및 응용 프로그램에서 파일 로드")

<a name="Loading_Files_Embedded_as_Resources" />

## <a name="loading-files-embedded-as-resources"></a>리소스로 포함 하는 파일 로드

파일에 포함 하는 **.NET 표준** 어셈블리 만들기 또는 파일을 추가 및 확인 **빌드 작업: 포함 리소스**합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![구성 리소스 빌드 작업을 포함](files-images/vs-embeddedresource-sml.png "설정을 포함 리소스 빌드 작업")](files-images/vs-embeddedresource.png#lightbox "설정을 포함 리소스 빌드 작업")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![포함된 리소스 빌드 작업을 구성 하는 PCL에 포함 된 텍스트 파일](files-images/xs-embeddedresource-sml.png "설정을 포함 리소스 빌드 작업")](files-images/xs-embeddedresource.png#lightbox "설정을 포함 리소스 빌드 작업")

-----

`GetManifestResourceStream` 사용 하 여 포함 된 파일에 액세스 하는 데 사용 되는 **리소스 ID**합니다. 리소스 ID는 파일 이름-에 포함 된 프로젝트에 대 한 기본 네임 스페이스 접두사로 기본적으로이 경우 어셈블리는 **WorkingWithFiles** 파일 이름이 고 **PCLTextResource.txt**, 리소스 ID가 있으므로 `WorkingWithFiles.PCLTextResource.txt`합니다.

```csharp
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(LoadResourceText)).Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.PCLTextResource.txt");
string text = "";
using (var reader = new System.IO.StreamReader (stream)) {
    text = reader.ReadToEnd ();
}
```

`text` 변수는 텍스트를 표시 하거나 그렇지 않으면 코드에서 사용 하 여 사용할 수 있습니다. 이 스크린샷은 [샘플 응용 프로그램](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/) 에 렌더링 하면 라는 텍스트가 표시는 `Label` 제어 합니다.

 [![PCL에 포함 된 텍스트 파일](files-images/pcltext-sml.png "앱에 표시 되는 PCL에 포함 된 텍스트 파일")](files-images/pcltext.png#lightbox "앱에 표시 되는 PCL에 포함 된 텍스트 파일")

로드 하 고 XML을 역직렬화도 매우 간단 합니다. 다음 코드에 표시 된 XML 파일의 로드 하 고 리소스에에서 deserialize 된 다음에 바인딩된는 `ListView` 표시 합니다. XML 파일의 배열을 포함 `Monkey` 개체 (클래스 샘플 코드에 정의 됨).

```csharp
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(LoadResourceText)).Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.PCLXmlResource.xml");
List<Monkey> monkeys;
using (var reader = new System.IO.StreamReader (stream)) {
    var serializer = new XmlSerializer(typeof(List<Monkey>));
    monkeys = (List<Monkey>)serializer.Deserialize(reader);
}
var listView = new ListView ();
listView.ItemsSource = monkeys;
```

 [![ListView에 표시 되는 PCL에 포함 된 Xml 파일](files-images/pclxml-sml.png "ListView에 표시 되는 PCL에 포함 된 XML 파일")](files-images/pclxml.png#lightbox "ListView에 표시 되는 PCL에 포함 된 XML 파일")

<a name="Embedding_in_Shared_Projects" />

### <a name="embedding-in-shared-projects"></a>공유 프로젝트에 포함

공유 프로젝트 공유 프로젝트의 내용을 참조 하는 프로젝트에 컴파일되므로에 사용 되는 접두사 포함 파일 Id를 변경할 수 있지만 파일 포함 리소스로 포함할 수도 있습니다. 즉, 포함 된 각 파일에 대 한 리소스 ID는 각 플랫폼에 대해 다를 수 있습니다.

공유 프로젝트와 함께이 문제에 대 한 두 가지 솔루션 가지가 있습니다.

-  **프로젝트는 동기화** -사용 하는 각 플랫폼에 대 한 프로젝트 속성을 편집 하는 **동일한** 어셈블리 이름 및 기본 네임 스페이스입니다. 다음이 값이 포함 된 리소스 공유 프로젝트에는 Id에 대 한 접두사로 "하드 코드 된"를 수 있습니다.
-  **컴파일러 지시문을 #if** -컴파일러 지시문을 사용 하 여 올바른 리소스 ID를 설정 하 고 올바른 리소스 id입니다.를 동적으로 생성 하려면 해당 값을 사용 하려면


두 번째 옵션을 설명 하는 코드는 다음과 같습니다. 컴파일러 지시문 (이 일반적으로 참조 하는 프로젝트에 대 한 기본 네임 스페이스와 동일) 하드 코드 된 리소스 접두사를 선택 하는 데 사용 됩니다. `resourcePrefix` 포함된 리소스 파일 이름으로 연결 하 여 올바른 리소스 ID를 만들려면 변수가 사용 됩니다.

```csharp
#if __IOS__
var resourcePrefix = "WorkingWithFiles.iOS.";
#endif
#if __ANDROID__
var resourcePrefix = "WorkingWithFiles.Droid.";
#endif

Debug.WriteLine("Using this resource prefix: " + resourcePrefix);
// note that the prefix includes the trailing period '.' that is required
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(SharedPage)).Assembly;
Stream stream = assembly.GetManifestResourceStream
    (resourcePrefix + "SharedTextResource.txt");
```

<a name="Organizing_Resources" />

### <a name="organizing-resources"></a>리소스 구성

위의 예에서는 파일 형식의 경우 리소스 ID는 표준.NET 라이브러리 프로젝트의 루트에 포함 되어 있는 것으로 가정 **Namespace.Filename.Extension**와 같은 `WorkingWithFiles.PCLTextResource.txt` 및 `WorkingWithFiles.iOS.SharedTextResource.txt`합니다.

폴더에 포함 된 리소스를 구성 하는 것이 불가능 합니다. 포함된 리소스는 폴더에에서 저장 되며, 폴더 이름의 일부가 됩니다 (마침표로 구분 됨) 리소스 ID, 리소스 ID 형식이 되도록 **Namespace.Folder.Filename.Extension**합니다. 폴더에 샘플 앱에 사용 되는 파일을 배치 **MyFolder** 해당 리소스 Id 하 게 만드는 `WorkingWithFiles.MyFolder.PCLTextResource.txt` 및 `WorkingWithFiles.iOS.MyFolder.SharedTextResource.txt`합니다.

<a name="Debugging_Embedded_Resources" />

### <a name="debugging-embedded-resources"></a>포함된 리소스 디버깅

특정 리소스 로드 되 고 있지 않습니다 이유를 이해 하기 어려운 경우도 이기 때문에 올바르게 구성 하는 리소스를 확인할 수 있도록 응용 프로그램에 일시적으로 다음 디버그 코드를 추가할 수 있습니다. 지정된 된 어셈블리에 포함 된 알려진된 모든 리소스를 출력 합니다는 **오류** 패드 문제를 로드 하는 리소스를 디버그할 수 있도록 합니다.

```csharp
using System.Reflection;
// ...
// use for debugging, not in released app code!
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(SharedPage)).Assembly;
foreach (var res in assembly.GetManifestResourceNames()) {
    System.Diagnostics.Debug.WriteLine("found resource: " + res);
}
```

## <a name="summary"></a>요약

이 문서는 포함된 리소스를 로드 및 저장 하 고 장치에 텍스트를 로드에 대 한 몇 가지 간단한 파일 작업을 보여 주었습니다. 표준.NET 2.0과 함께.NET 표준 라이브러리에 파일 액세스 코드를 공유 하는 것이 같습니다.

## <a name="related-links"></a>관련 링크

- [FilesSample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/)
- [Xamarin.Forms 샘플](https://github.com/xamarin/xamarin-forms-samples)
- [Xamarin.iOS에 파일 시스템 작업](~/ios/app-fundamentals/file-system.md)
- [통합 문서 파일](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/files/files.workbook)
