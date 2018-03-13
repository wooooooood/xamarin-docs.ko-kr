---
title: "파일"
description: "포함된 리소스를 사용 하 여 또는 네이티브 파일 시스템 Api에 대 한 쓰기 파일 xamarin.forms 처리를 수행할 수 있습니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 9987C3F6-5F04-403B-BBB4-ECB024EA6CC8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/22/2017
ms.openlocfilehash: c6d10025ccc038ba160fe3c09f6ce92e97d916d2
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2018
---
# <a name="files"></a>파일

_포함된 리소스를 사용 하 여 또는 네이티브 파일 시스템 Api에 대 한 쓰기 파일 xamarin.forms 처리를 수행할 수 있습니다._

## <a name="overview"></a>개요

Xamarin.Forms 코드는 각자 자체적인 파일 시스템을 지닌 여러 개의 플랫폼에서 실행됩니다. 따라서 파일 읽기 및 쓰기 대부분을 쉽게 실행 취소할 네이티브 파일 Api를 사용 하 여 각 플랫폼에 있습니다. 또는 포함 된 리소스는 응용 프로그램과 데이터 파일을 배포 하는 보다 간단한 솔루션입니다.

이 문서에서는 다음과 같은 일반적인 파일 시나리오를 처리를 다룹니다.

-  [ **리소스 그룹으로 포함 된 파일** ](#Loading_Files_Embedded_as_Resources) -는 응용 프로그램 및 로드를 사용 하 여 리플렉션 API의 일부로 파일을 전달할 수 있습니다.
-  [ **저장 하 고 파일을 로드** ](#Loading_and_Saving_Files) -쓰기 가능한 저장소 사용자 수 고유 하 게 구현 하 고 다음 사용 하 여 액세스는 `DependencyService` 합니다.


타사 구성 요소 호출 **PCLStorage** 에서 읽기 및 쓰기 파일을 액세스할 수 있는 저장소 사용자 PCL 코드를 사용할 수도 있습니다.

이미지 파일을 처리 하는 대 한 자세한 내용은 참조는 [이미지 작업](~/xamarin-forms/user-interface/images.md) 페이지.

<a name="Loading_Files_Embedded_as_Resources" />

## <a name="loading-files-embedded-as-resources"></a>리소스로 포함 하는 파일 로드

파일에 포함 하는 **PCL** 어셈블리 만들기 또는 파일을 추가 및 확인 **빌드 작업: 포함 리소스**합니다.

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
#if WINDOWS_PHONE
var resourcePrefix = "WorkingWithFiles.WinPhone.";
#endif

Debug.WriteLine("Using this resource prefix: " + resourcePrefix);
// note that the prefix includes the trailing period '.' that is required
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(SharedPage)).Assembly;
Stream stream = assembly.GetManifestResourceStream
    (resourcePrefix + "SharedTextResource.txt");
```

<a name="Organizing_Resources" />

### <a name="organizing-resources"></a>리소스 구성

위의 예에서는 파일 형식의 경우 리소스 ID는 PCL 프로젝트의 루트에 포함 되어 있는 것으로 가정 **Namespace.Filename.Extension**와 같은 `WorkingWithFiles.PCLTextResource.txt` 및 `WorkingWithFiles.iOS.SharedTextResource.txt`합니다.

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

<a name="Loading_and_Saving_Files" />

## <a name="saving-and-loading-files"></a>저장 하 고 파일 로드

Xamarin.Forms는 각각 자체 파일 시스템에 있는 여러 플랫폼에서 실행 되므로를 로드 하 고 사용자가 만든 파일을 저장 한 방법은 없습니다. 저장 하 고 샘플 응용 프로그램에 저장 하 고 일부 사용자 입력-로드 화면을 포함 하는 텍스트 파일을 로드 하는 방법을 보여 주는 완성 된 화면은 다음과 같습니다.

 [![저장 및 로드 텍스트](files-images/saveandload-sml.png "저장 및 응용 프로그램에서 파일 로드")](files-images/saveandload.png#lightbox "저장 및 응용 프로그램에서 파일 로드")

각 플랫폼에는 약간 다른 디렉터리 구조 및 다른 파일 시스템 기능-예를 들어 Xamarin.iOS 및 Xamarin.Android 지원 대부분 `System.IO` 기능 하지만 Windows Phone 지원 `IsolatedStorage` 및 [ `Windows.Storage` ](http://msdn.microsoft.com/library/windowsphone/develop/jj681698(v=vs.105).aspx) Api입니다.

이 문제를 해결 하려면 샘플 응용 프로그램 로드 하 고 파일을 저장 하는 Xamarin.Forms PCL에 인터페이스를 정의 합니다. 장치에 저장 될 로드 하 고 텍스트 파일을 저장 하는 단순 API를 제공 합니다.

```csharp
public interface ISaveAndLoad {
    void SaveText (string filename, string text);
    string LoadText (string filename);
}
```

PCL 다음 코드는 [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md) 인터페이스의 native 구현에 대 한 참조를 얻으려고 합니다. 따라서 이식 가능한 코드를을 로드 하 고 각 플랫폼별 프로젝트에 작성 된 클래스 파일의 저장을 위임할 수 있습니다. 이 샘플에서는 **저장** 및 **부하** 단추와 같이 작성 됩니다.

```csharp
var saveButton = new Button {Text = "Save"};
saveButton.Clicked += (sender, e) => {
    DependencyService.Get<ISaveAndLoad>().SaveText("temp.txt", input.Text);
};
var loadButton = new Button {Text = "Load"};
loadButton.Clicked += (sender, e) => {
    output.Text = DependencyService.Get<ISaveAndLoad>().LoadText("temp.txt");
};
```

구현을 다음 파일을 저장 및 로드할 실제로 수 전에 각 플랫폼별 프로젝트에 추가 해야 합니다.

### <a name="ios-and-android"></a>iOS 및 Android

Xamarin.iOS 및 Xamarin.Android 프로젝트에 대 한 인터페이스의 구현을 동일할 수 있습니다. 포함 하 여 아래 코드는 표시는 `[assembly: Dependency (typeof (SaveAndLoad))]` 에 필요한 특성은 `DependencyService` 에서 실행 되도록 합니다.

```csharp
[assembly: Dependency (typeof (SaveAndLoad))]
namespace WorkingWithFiles {
    public class SaveAndLoad : ISaveAndLoad {
        public void SaveText (string filename, string text) {
            var documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal);
            var filePath = Path.Combine (documentsPath, filename);
            System.IO.File.WriteAllText (filePath, text);
        }
        public string LoadText (string filename) {
            var documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal);
            var filePath = Path.Combine (documentsPath, filename);
            return System.IO.File.ReadAllText (filePath);
        }
    }
}
```

### <a name="universal-windows-platform-uwp-windows-81-and-windows-phone-81"></a>유니버설 Windows 플랫폼 (UWP), Windows 8.1 및 Windows Phone 8.1

이러한 플랫폼은 다른 파일 시스템 API- [ `Windows.Storage` ](/windows/uwp/files/quickstart-reading-and-writing-files/) – 즉 저장 하 고 파일을 로드 하는 데 사용 합니다.
`ISaveAndLoad` 아래와 같이 인터페이스를 구현할 수 있습니다.

```csharp
using System;
using System.Threading.Tasks;
using Windows.Storage;
using WinApp;
using WorkingWithFiles;
using Xamarin.Forms;

[assembly: Dependency(typeof(SaveAndLoad_WinApp))]

namespace WindowsApp
{
    // https://msdn.microsoft.com/library/windows/apps/xaml/hh758325.aspx
    public class SaveAndLoad_WinApp : ISaveAndLoad
    {
        public async Task SaveTextAsync(string filename, string text)
        {
            StorageFolder localFolder = ApplicationData.Current.LocalFolder;
            StorageFile sampleFile = await localFolder.CreateFileAsync(filename, CreationCollisionOption.ReplaceExisting);
            await FileIO.WriteTextAsync(sampleFile, text);
        }
        public async Task<string> LoadTextAsync(string filename)
        {
            StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
            StorageFile sampleFile = await storageFolder.GetFileAsync(filename);
            string text = await Windows.Storage.FileIO.ReadTextAsync(sampleFile);
            return text;
        }
    }
}
```


<a name="Saving_and_Loading_in_Shared_Projects" />

### <a name="saving-and-loading-in-shared-projects"></a>저장 및 공유 프로젝트에 로드

공유 프로젝트 내에 단일 클래스 파일에 모든 플랫폼별 코드를 포함 시킬 수는 공유 프로젝트 컴파일러 지시문을 지원 하기 때문에 (사용 하지 않고는 `DependencyService`).
단일 `SaveAndLoad` 위의 두 구현 모두 들어 있는 같은 컴파일러 지시문을 사용 하 여 참조 프로젝트에 선택적으로 컴파일된 클래스를 작성할 수 `#if WINDOWS_PHONE`, `#if __IOS__`, 및 `#if __ANDROID__`합니다.

## <a name="additional-information"></a>추가 정보

PCL 기반 Xamarin.Forms 프로젝트도 활용할 수는 [PCLStorage NuGet](http://www.nuget.org/packages/pclstorage) ([코드 &amp; 설명서](https://pclstorage.codeplex.com/)) 플랫폼 간 방식으로 파일 작업을 구현할 수 있도록 합니다.


## <a name="summary"></a>요약

이 문서는 포함된 리소스를 로드 및 저장 하 고, 장치에 텍스트를 로드 하기 위한 단순한 파일 작업을 보여 주었습니다. 개발자가 자신의 네이티브 파일 Api를 사용 하 여 구현할 수는 `DependencyService`, 파일 조작 요구 사항을 처리 하는 데 필요한 만큼 복잡 하 합니다.


## <a name="related-links"></a>관련 링크

- [FilesSample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/)
- [만들기, 쓰기 및 (UWP) 파일 읽기](/windows/uwp/files/quickstart-reading-and-writing-files/)
- [Xamarin.Forms 샘플](https://github.com/xamarin/xamarin-forms-samples)
- [통합 문서 파일](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/files/files.workbook)
