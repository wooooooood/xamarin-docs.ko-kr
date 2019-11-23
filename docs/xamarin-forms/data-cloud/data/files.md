---
title: Xamarin.Forms의 파일 처리
description: Xamarin.Forms를 통한 파일 처리는 .NET Standard 라이브러리의 코드를 사용하거나 포함된 리소스를 사용하여 수행할 수 있습니다.
ms.prod: xamarin
ms.assetid: 9987C3F6-5F04-403B-BBB4-ECB024EA6CC8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/21/2018
ms.openlocfilehash: fb3bbda3caee9fdbd490aaea7e119baf470eedd1
ms.sourcegitcommit: 4cf434b126eb7df6b2fd9bb1d71613bf2b6aac0e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/07/2019
ms.locfileid: "71997161"
---
# <a name="file-handling-in-xamarinforms"></a>Xamarin.Forms의 파일 처리

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfiles)

_Xamarin.Forms를 통한 파일 처리는 .NET Standard 라이브러리의 코드를 사용하거나 포함 리소스를 사용하여 수행할 수 있습니다._

## <a name="overview"></a>개요

Xamarin.Forms 코드는 각자 자체적인 파일 시스템을 지닌 여러 개의 플랫폼에서 실행됩니다. 이전에는 각 플랫폼에서 원시 파일 API를 사용하여 파일을 읽고 쓰는 것이 가장 쉽게 수행되었다는 것을 의미합니다. 또는 포함 리소스가 앱을 사용하여 데이터 파일을 배포하는 더 간단한 솔루션입니다. 그러나 .NET Standard 2.0을 사용하면 .NET Standard 라이브러리에서 파일 액세스 코드를 공유할 수 있습니다.

이미지 파일 처리에 대한 자세한 내용은 [이미지 작업](~/xamarin-forms/user-interface/images.md) 페이지를 참조하세요.

<a name="Loading_and_Saving_Files" />

## <a name="saving-and-loading-files"></a>파일 저장 및 로드

`System.IO` 클래스는 각 플랫폼의 파일 시스템에 액세스하는 데 사용할 수 있습니다. `File` 클래스를 사용하여 파일을 만들고, 삭제하고, 삭제할 수 있으며, `Directory` 클래스를 사용하여 디렉터리의 콘텐츠를 만들거나, 삭제하거나, 열거할 수 있습니다. 또한 파일 작업(예: 파일 내의 압축 또는 위치 검색)을 훨씬 더 효율적으로 제어할 수 있는 `Stream` 하위 클래스를 사용할 수도 있습니다.

텍스트 파일은 `File.WriteAllText` 메서드를 사용하여 쓸 수 있습니다.

```csharp
File.WriteAllText(fileName, text);
```

텍스트 파일은 `File.ReadAllText` 메서드를 사용하여 읽을 수 있습니다.

```csharp
string text = File.ReadAllText(fileName);
```

또한 `File.Exists` 메서드는 지정된 파일이 있는지 여부를 확인합니다.

```csharp
bool doesExist = File.Exists(fileName);
```

각 플랫폼의 파일 경로는 [`Environment.SpecialFolder`](xref:System.Environment.SpecialFolder) 열거형의 값을 `Environment.GetFolderPath` 메서드의 첫 번째 인수로 사용하여 .NET Standard 라이브러리에서 확인할 수 있습니다. 그런 다음, `Path.Combine` 메서드를 사용하여 파일 이름과 결합할 수 있습니다.

```csharp
string fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "temp.txt");
```

이러한 작업은 텍스트 저장하고 로드하는 페이지가 포함된 샘플 앱에서 보여 줍니다.

[![텍스트 저장 및 로드](files-images/saveandload-sml.png "앱에서 파일 저장 및 로드")](files-images/saveandload.png#lightbox "앱에서 파일 저장 및 로드")

<a name="Loading_Files_Embedded_as_Resources" />

## <a name="loading-files-embedded-as-resources"></a>리소스로 포함된 파일 로드

**.NET Standard** 어셈블리에 파일을 포함시키려면 파일을 만들거나 추가하고 **빌드 작업: EmbeddedResource**를 확인합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![포함 리소스 빌드 작업 구성](files-images/vs-embeddedresource-sml.png "EmbeddedResource") 빌드](files-images/vs-embeddedresource.png#lightbox "EmbeddedResource 빌드 설정")

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![.Net 표준 라이브러리에 포함 된 텍스트 파일, 포함 리소스 빌드 작업 구성](files-images/xs-embeddedresource-sml.png "EmbeddedResource") 빌드](files-images/xs-embeddedresource.png#lightbox "EmbeddedResource 빌드 설정")

-----

`GetManifestResourceStream`은 **리소스 ID**를 사용하여 포함된 파일에 액세스하는 데 사용합니다. 기본적으로 리소스 ID는 포함 된 프로젝트의 기본 네임 스페이스가 접두사로 붙는 파일 이름입니다 .이 경우 어셈블리는 WorkingWithFiles이 고 파일 이름은 **입니다. 따라서**리소스 id는 `WorkingWithFiles.LibTextResource.txt`됩니다.

```csharp
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(LoadResourceText)).Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.LibTextResource.txt");
string text = "";
using (var reader = new System.IO.StreamReader (stream))
{  
    text = reader.ReadToEnd ();
}
```

그러면 `text` 변수를 사용하여 텍스트를 표시하거나 그렇지 않은 경우 코드에서 사용할 수 있습니다. [샘플 앱](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfiles)의 다음 스크린샷에서는 `Label` 컨트롤에서 렌더링된 텍스트를 보여 줍니다.

 [(files-images/pcltext-sml.png "앱에 표시 되는 .NET Standard 라이브러리에 포함 된 텍스트") 파일을 ![.net standard 라이브러리에 포함 된 텍스트 파일]](files-images/pcltext.png#lightbox "앱에 표시 되는 .NET Standard 라이브러리의 포함 텍스트 파일")

XML을 로드하고 역직렬화하는 것도 마찬가지로 간단합니다. 다음 코드에서는 리소스에서 로드 및 역직렬화된 다음, 표시할 `ListView`에 바인딩되는 XML 파일을 보여 줍니다. XML 파일에는 `Monkey` 개체의 배열이 포함되어 있습니다(클래스는 샘플 코드에서 정의됨).

```csharp
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(LoadResourceText)).Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.LibXmlResource.xml");
List<Monkey> monkeys;
using (var reader = new System.IO.StreamReader (stream)) {
    var serializer = new XmlSerializer(typeof(List<Monkey>));
    monkeys = (List<Monkey>)serializer.Deserialize(reader);
}
var listView = new ListView ();
listView.ItemsSource = monkeys;
```

 [![Xml 파일이 .net standard 라이브러리에 포함 되어]있습니다. Listview(files-images/pclxml-sml.png "에 표시 되는 .net 표준 라이브러리의 Listview embedded Xml 파일") 에 표시 됩니다.](files-images/pclxml.png#lightbox "ListView에 표시 되는 .NET 표준 라이브러리의 포함 XML 파일")

<a name="Embedding_in_Shared_Projects" />

## <a name="embedding-in-shared-projects"></a>공유 프로젝트에 포함

공유 프로젝트는 파일을 포함 리소스로 포함할 수도 있지만, 공유 프로젝트의 콘텐츠가 참조 프로젝트로 컴파일되므로 포함된 파일 리소스 ID에 사용되는 접두사가 변경될 수 있습니다. 즉 포함된 파일 각각에 대한 리소스 ID가 플랫폼마다 다를 수 있습니다.

공유 프로젝트와 관련된 이 문제에 대한 두 가지 해결 방법은 다음과 같습니다.

- **프로젝트 동기화** - **동일한** 어셈블리 이름과 기본 네임스페이스를 사용하도록 각 플랫폼에 대한 프로젝트 속성을 편집합니다. 이 값은 공유 프로젝트의 포함 리소스 ID에 대한 접두사로 "하드 코드"될 수 있습니다.
- **#if 컴파일러 지시문** - 컴파일러 지시문을 사용하여 올바른 리소스 ID 접두사를 설정하고, 해당 값을 사용하여 올바른 리소스 ID를 동적으로 구성합니다.

두 번째 옵션을 설명하는 코드는 아래와 같습니다. 컴파일러 지시문은 하드 코드된 리소스 접두사(일반적으로 참조 프로젝트에 대한 기본 네임스페이스와 동일함)를 선택하는 데 사용됩니다. 그런 다음, `resourcePrefix` 변수가 포함 리소스 파일 이름과 연결하여 올바른 리소스 ID를 만드는 데 사용됩니다.

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

위의 예제에서는 파일이 .NET Standard 라이브러리 프로젝트의 루트에 포함되어 있다고 가정합니다. 이 경우 리소스 ID는 **Namespace.Filename.Extension** 형식입니다(예: `WorkingWithFiles.LibTextResource.txt` 및 `WorkingWithFiles.iOS.SharedTextResource.txt`).

포함 리소스는 폴더에 구성할 수 있습니다. 포함 리소스가 폴더에 배치되면 폴더 이름이 리소스 ID의 일부가 되며(마침표로 구분됨), 리소스 ID 형식은 **Namespace.Folder.Filename.Extension**이 됩니다. 샘플 앱에 사용된 파일이 **MyFolder** 폴더에 배치되면 해당 리소스 ID(`WorkingWithFiles.MyFolder.LibTextResource.txt` 및 `WorkingWithFiles.iOS.MyFolder.SharedTextResource.txt`)가 됩니다.

<a name="Debugging_Embedded_Resources" />

### <a name="debugging-embedded-resources"></a>포함 리소스 디버깅

특정 리소스가 로드되지 않는 이유를 이해하기 어려운 경우가 있으므로 다음 디버그 코드를 애플리케이션에 임시로 추가하여 리소스가 올바르게 구성되었는지 확인할 수 있습니다. 지정된 어셈블리에 포함된 모든 알려진 리소스를 **Errors**(오류) 패드로 출력하여 리소스 로드 문제를 디버그하는 데 도움을 줍니다.

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

이 문서에서는 텍스트를 디바이스에 저장 및 로드하고, 포함 리소스를 로드하는 몇 가지 간단한 파일 작업을 보여 주었습니다. .NET Standard 2.0을 사용하면 .NET Standard 라이브러리에서 파일 액세스 코드를 공유할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [FilesSample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfiles)
- [Xamarin.Forms 샘플](https://github.com/xamarin/xamarin-forms-samples)
- [Xamarin.iOS에서 파일 시스템 작업](~/ios/app-fundamentals/file-system.md)
