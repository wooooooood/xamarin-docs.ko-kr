---
title: Xamarin.Forms의 파일 처리
description: Xamarin.Forms를 사용 하 여 처리 하는 파일은.NET Standard 라이브러리에서 또는 포함 된 리소스를 사용 하 여 코드를 사용 하 여 수행할 수 있습니다.
ms.prod: xamarin
ms.assetid: 9987C3F6-5F04-403B-BBB4-ECB024EA6CC8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/21/2018
ms.openlocfilehash: e31bb46569ed96d514ec87eacaf9f3912dcf3237
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675161"
---
# <a name="file-handling-in-xamarinforms"></a>Xamarin.Forms의 파일 처리

_Xamarin.Forms를 사용 하 여 처리 하는 파일은.NET Standard 라이브러리에서 또는 포함 된 리소스를 사용 하 여 코드를 사용 하 여 수행할 수 있습니다._

## <a name="overview"></a>개요

Xamarin.Forms 코드는 각자 자체적인 파일 시스템을 지닌 여러 개의 플랫폼에서 실행됩니다. 이전에이 네이티브 파일 Api를 사용 하 여 각 플랫폼에는 파일 읽기 및 쓰기를 가장 쉽게 수행 의미 합니다. 또는 포함 된 리소스는 앱을 사용 하 여 데이터 파일을 배포 하는 간단한 솔루션입니다. 그러나.NET Standard 2.0을 사용 하 여.NET Standard 라이브러리에서 파일 액세스 코드를 공유 하는 것이 같습니다.

이미지 파일 처리에 대 한 내용은 참조는 [이미지를 사용 하 여 작업](~/xamarin-forms/user-interface/images.md) 페이지입니다.

<a name="Loading_and_Saving_Files" />

## <a name="saving-and-loading-files"></a>저장 하 고 파일 로드

`System.IO` 각 플랫폼에서 파일 시스템에 액세스 하는 클래스를 사용할 수 있습니다. 합니다 `File` 클래스를 사용 하면 생성, 삭제 및 파일을 읽 및 `Directory` 클래스 만들기, 삭제 또는 디렉터리의 내용을 열거할 수 있습니다. 사용할 수도 있습니다는 `Stream` 뛰어난 파일 작업 (예: 파일 내의 압축 또는 위치 검색)에 대 한 제어를 제공할 수 있는 하위 클래스입니다.

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

값을 사용 하 여.NET Standard 라이브러리에서 각 플랫폼에 있는 파일의 경로 확인할 수 있습니다 합니다 [ `Environment.SpecialFolder` ](xref:System.Environment.SpecialFolder) 첫 번째 인수로 열거형은 `Environment.GetFolderPath` 메서드. 사용 하는 파일을 사용 하 여 결합한 다음이 `Path.Combine` 메서드:

```csharp
string fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "temp.txt");
```

이러한 작업을 저장 하 고 텍스트를 로드 하는 페이지를 포함 하는 샘플 앱에서 보여 줍니다.

[![저장 하 고 텍스트를 로드](files-images/saveandload-sml.png "저장 및 앱에서 파일 로드")](files-images/saveandload.png#lightbox "저장 및 앱에서 파일 로드")

<a name="Loading_Files_Embedded_as_Resources" />

## <a name="loading-files-embedded-as-resources"></a>리소스로 포함 하는 파일 로드

파일을 포함 하는 **.NET Standard** 어셈블리 만들기 또는 파일을 추가 및 확인 **빌드 작업: EmbeddedResource**합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![구성 리소스 빌드 작업을 포함](files-images/vs-embeddedresource-sml.png "설정을 EmbeddedResource BuildAction")](files-images/vs-embeddedresource.png#lightbox "설정 포함 리소스 빌드 작업")

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![PCL, 포함된 리소스 빌드 작업 구성에 포함 된 텍스트 파일](files-images/xs-embeddedresource-sml.png "설정을 EmbeddedResource BuildAction")](files-images/xs-embeddedresource.png#lightbox "설정 포함 리소스 빌드 작업")

-----

`GetManifestResourceStream` 사용 하 여 포함 된 파일에 액세스 하는 데 사용 되는 **리소스 ID**합니다. 어셈블리는이 경우 기본적으로 리소스 ID은 파일 이름에 포함 된 프로젝트에 대 한 기본 네임 스페이스 접두사로 **WorkingWithFiles** 파일 및 **PCLTextResource.txt**, 리소스 ID 이므로 `WorkingWithFiles.PCLTextResource.txt`합니다.

```csharp
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(LoadResourceText)).Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.PCLTextResource.txt");
string text = "";
using (var reader = new System.IO.StreamReader (stream)) {
    text = reader.ReadToEnd ();
}
```

`text` 변수 텍스트를 표시 하거나 그렇지 않으면 코드에서 사용 하 여 사용할 수 있습니다. 이 스크린샷을 [샘플 앱](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/) 렌더링할 텍스트를 보여 줍니다는 `Label` 컨트롤입니다.

 [![PCL에 포함 된 텍스트 파일로](files-images/pcltext-sml.png "앱에 표시 되는 PCL에 포함 된 텍스트 파일로")](files-images/pcltext.png#lightbox "앱에 표시 되는 PCL에 포함 된 텍스트 파일")

로드 하 고 XML을 역직렬화 하는 작업도 똑같이 간단 합니다. 다음 코드는 XML 파일 로드에서 리소스를 deserialize 하 고 바인딩할 표시는 `ListView` 표시 합니다. XML 파일의 배열을 포함 `Monkey` 개체 (클래스는 샘플 코드에 정의 됨).

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

 [![PCL을 ListView에서 표시에 포함 된 Xml 파일](files-images/pclxml-sml.png "ListView에 표시 되는 PCL에 포함 된 XML 파일")](files-images/pclxml.png#lightbox "ListView에 표시 되는 PCL에 포함 된 XML 파일")

<a name="Embedding_in_Shared_Projects" />

## <a name="embedding-in-shared-projects"></a>공유 프로젝트에 포함

하지만 공유 프로젝트에 사용 되는 접두사 포함 파일 리소스 Id를 변경할 수는 공유 프로젝트의 내용을 참조 프로젝트로 컴파일되므로 파일이 포함 리소스로 포함할 수도 있습니다. 즉, 포함 된 각 파일에 대 한 리소스 ID는 각 플랫폼에 대해 다를 수 있습니다.

공유 프로젝트를 사용 하 여이 문제에 대 한 솔루션 두 가지가 있습니다.

-  **프로젝트 동기화** -사용 하려면 각 플랫폼에 대 한 프로젝트 속성을 편집 합니다 **동일한** 어셈블리 이름 및 기본 네임 스페이스입니다. 이 값이 포함 된 리소스 공유 프로젝트의 Id에 대 한 접두사로 "하드 코드 된" 수 있습니다.
-  **컴파일러 지시문이 #if** -컴파일러 지시문을 사용 하 여 올바른 리소스 ID 접두사를 설정 하 고 해당 값을 사용 하 여 올바른 리소스 ID를 동적으로 생성 하려면


두 번째 옵션을 보여 주는 코드는 다음과 같습니다. 컴파일러 지시문은 하드 코드 된 리소스 접두사 (이 일반적으로 참조 하는 프로젝트에 대 한 기본 네임 스페이스와 동일)을 선택 하는 데 사용 됩니다. `resourcePrefix` 변수는 포함된 리소스 파일 이름으로 연결 하 여 올바른 리소스 ID를 만드는 데 사용 됩니다.

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

위의 예제에서는 파일 형식의 경우 리소스 ID는.NET Standard 라이브러리 프로젝트의 루트에 포함 되어 있음을 가정 **Namespace.Filename.Extension**와 같은 `WorkingWithFiles.PCLTextResource.txt` 고 `WorkingWithFiles.iOS.SharedTextResource.txt`입니다.

폴더에 포함 된 리소스를 구성 하는 것이 가능 합니다. 포함된 리소스 폴더에 넣으면 폴더 이름이 일부가 (마침표로 구분 됨) 리소스 ID의 리소스 ID 형식은 되도록 **Namespace.Folder.Filename.Extension**합니다. 폴더에 샘플 앱에서 사용 되는 파일 배치 **MyFolder** 해당 하는 리소스 Id를 확인 하는 `WorkingWithFiles.MyFolder.PCLTextResource.txt` 고 `WorkingWithFiles.iOS.MyFolder.SharedTextResource.txt`입니다.

<a name="Debugging_Embedded_Resources" />

### <a name="debugging-embedded-resources"></a>포함된 리소스 디버깅

특정 리소스 로드 되지 이유를 이해 하기 어려운 경우가 있기 때문에 리소스를 올바르게 구성 되었는지 확인 하는 데 응용 프로그램에 일시적으로 다음 디버그 코드를 추가할 수 있습니다. 지정된 된 어셈블리에 포함 된 알려진된 모든 리소스를 출력 합니다 **오류** 패드 리소스 로드 문제를 디버깅 하는 데 있습니다.

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

이 문서에서는 포함 된 리소스를 로드 및 저장 하 고 장치에 텍스트를 로드에 대 한 몇 가지 간단한 파일 작업을 보여 주었습니다. .NET Standard 2.0을 사용 하 여.NET Standard 라이브러리에서 파일 액세스 코드를 공유 하는 것이 같습니다.

## <a name="related-links"></a>관련 링크

- [FilesSample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/)
- [Xamarin.Forms 샘플](https://github.com/xamarin/xamarin-forms-samples)
- [Xamarin.iOS에서 파일 시스템 작업](~/ios/app-fundamentals/file-system.md)

