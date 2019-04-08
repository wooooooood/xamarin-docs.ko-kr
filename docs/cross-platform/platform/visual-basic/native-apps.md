---
title: Visual Basic.NET Xamarin iOS 및 Android
description: 이 연습에서는 Visual Basic.net으로 작성 된 비즈니스 논리를 사용 하는 네이티브 Xamarin.iOS 및 Xamarin.Android 앱을 빌드하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 455fda67-3879-4299-8036-b12840e6a498
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: f62d3cb076019ba49303f2c82f009975d9fbdc50
ms.sourcegitcommit: 650458de1d362cd7de174cacef7838f0e74426f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/15/2019
ms.locfileid: "58070971"
---
# <a name="visual-basicnet-in-xamarin-ios-and-android"></a>Visual Basic.NET Xamarin iOS 및 Android

합니다 [TaskyPortableVB](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyPortableVB) 샘플 응용 프로그램 Xamarin을 사용 하 여 이식 가능한 클래스 라이브러리로 컴파일된 Visual Basic 코드를 사용할 수 있는 방법을 보여 줍니다. IOS, Android 및 Windows Phone 실행 되는 결과 앱의 일부 스크린 샷은 다음과 같습니다.

 [![](native-apps-images/image5.png "iOS, Android 및 Windows phone Visual Basic을 사용 하 여 빌드한 앱 실행")](native-apps-images/image5.png#lightbox)

IOS, Android 및 Windows Phone 예제에서 프로젝트 모두에 기록 됩니다 C#입니다. 각 응용 프로그램에 대 한 사용자 인터페이스를 기본 기술로 빌드됩니다 (스토리 보드, Xml 및 Xaml 각각), 하는 동안 합니다 `TodoItem` Visual Basic 이식 가능한 클래스 라이브러리에서 관리를 제공를 사용 하 여는 `IXmlStorage` 구현에서 제공 네이티브 프로젝트입니다.

## <a name="sample-walkthrough"></a>샘플 연습

이 가이드에서 Visual Basic 구현 되는 방법을 설명 합니다 [TaskyPortableVB](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyPortableVB) iOS 및 Android 용 Xamarin 샘플입니다.

> [!NOTE]
> 지침을 검토 [이식 가능한 Visual Basic.NET](index.md) 이 가이드를 사용 하 여 계속 합니다.

## <a name="visualbasicportablelibrary"></a>VisualBasicPortableLibrary

Visual Basic 이식 가능한 클래스 라이브러리는 Visual Studio에서 만들 수만 있습니다.
예제 라이브러리에는 네 가지 Visual Basic 파일에서 응용 프로그램의 기본 사항을 포함 됩니다.

-  IXmlStorage.vb
-  TodoItem.vb
-  TodoItemManager.vb
-  TodoItemRepositoryXML.vb


### <a name="ixmlstoragevb"></a>IXmlStorage.vb

이식 가능한 클래스 라이브러리를 제공 하지 않습니다 파일 액세스 동작 플랫폼 간에 따라서 크게 다르므로 `System.IO` 파일 저장소 Api에서 모든 프로필입니다. 즉, 이식 가능한 코드에 파일 시스템와 직접 상호 작용 하려고 하는 경우 해야 각 플랫폼에서 네이티브 프로젝트를 다시 호출 합니다.  구현 될 수 있는 간단한 인터페이스에 대 한 Visual Basic 코드를 작성 하 여 C# 각 플랫폼에서 공유할 수 있는 Visual Basic 코드 파일 시스템에 액세스할 수 있는 가질 수 있습니다.

두 메서드가 포함 된이 매우 간단한 인터페이스를 사용 하는 샘플 코드: 읽기 및 직렬화 된 Xml 파일을 쓰기입니다.

```vb
Public Interface IXmlStorage
    Function ReadXml(filename As String) As List(Of TodoItem)
    Sub WriteXml(tasks As List(Of TodoItem), filename As String)
End Interface
```

iOS, Android 및 Windows Phone 구현에서는이 인터페이스에 대 한이 가이드의 뒷부분에 표시 됩니다.

### <a name="todoitemvb"></a>TodoItem.vb

이 클래스는 응용 프로그램 전체에서 사용할 비즈니스 개체를 포함 합니다. Visual Basic에서 정의 됩니다 하 고 iOS, Android 및 Windows Phone 사용 하 여 공유 프로젝트에서 작성 된 C#입니다.

클래스 정의 다음과 같습니다.

```vb
Public Class TodoItem
    Property ID() As Integer
    Property Name() As String
    Property Notes() As String
    Property Done() As Boolean
End Class
```

샘플은 XML 직렬화 및 역직렬화를 사용 하 여 로드 하 고 TodoItem 개체를 저장 합니다.

### <a name="todoitemmanagervb"></a>TodoItemManager.vb

Manager 클래스는 이식 가능한 코드에 대 한 'API'를 표시합니다. 에 대 한 기본적인 CRUD 작업을 제공 하는 `TodoItem` 하지만 이러한 작업을 구현 하지.

```vb
Public Class TodoItemManager
    Private _repository As TodoItemRepositoryXML
    Public Sub New(filename As String, storage As IXmlStorage)
        _repository = New TodoItemRepositoryXML(filename, storage)
    End Sub
    Public Function GetTask(id As Integer) As TodoItem
        Return _repository.GetTask(id)
    End Function
    Public Function GetTasks() As List(Of TodoItem)
        Return New List(Of TodoItem)(_repository.GetTasks())
    End Function
    Public Function SaveTask(item As TodoItem) As Integer
        Return _repository.SaveTask(item)
    End Function
    Public Function DeleteTask(item As TodoItem) As Integer
        Return _repository.DeleteTask(item.ID)
    End Function
End Class
```

생성자는 매개 변수로 IXmlStorage의 인스턴스를 사용 합니다. 따라서 각 플랫폼을에 이식 가능한 코드를 공유할 수 있는 다른 기능을 설명 하면서 자체 작업 구현을 제공할 수 있습니다.

### <a name="todoitemrepositoryvb"></a>TodoItemRepository.vb

저장소 클래스는 TodoItem 개체 목록을 관리 하기 위한 논리를 포함 합니다. 전체 코드를 추가 되 고 컬렉션에서 제거할 TodoItems에서 고유한 ID 값을 관리 하는 주로 논리 존재 하는 아래 표시 됩니다.

```vb
Public Class TodoItemRepositoryXML
    Private _filename As String
    Private _storage As IXmlStorage
    Private _tasks As List(Of TodoItem)

    ''' <summary>Constructor</summary>
    Public Sub New(filename As String, storage As IXmlStorage)
        _filename = filename
        _storage = storage
        _tasks = _storage.ReadXml(filename)
    End Sub
    ''' <summary>Inefficient search for a Task by ID</summary>
    Public Function GetTask(id As Integer) As TodoItem
        For t As Integer = 0 To _tasks.Count - 1
            If _tasks(t).ID = id Then
                Return _tasks(t)
            End If
        Next
        Return New TodoItem() With {.ID = id}
    End Function
    ''' <summary>List all the Tasks</summary>
    Public Function GetTasks() As IEnumerable(Of TodoItem)
        Return _tasks
    End Function
    ''' <summary>Save a Task to the Xml file
    ''' Calculates the ID as the max of existing IDs</summary>
    Public Function SaveTask(item As TodoItem) As Integer
        Dim max As Integer = 0
        If _tasks.Count > 0 Then
            max = _tasks.Max(Function(t As TodoItem) t.ID)
        End If
        If item.ID = 0 Then
            item.ID = ++max
            _tasks.Add(item)
        Else
            Dim j = _tasks.Where(Function(t) t.ID = item.ID).First()
            j = item
        End If
        _storage.WriteXml(_tasks, _filename)
        Return max
    End Function
    ''' <summary>Removes the task from the XMl file</summary>
    Public Function DeleteTask(id As Integer) As Integer
        For t As Integer = 0 To _tasks.Count - 1
            If _tasks(t).ID = id Then
                _tasks.RemoveAt(t)
                _storage.WriteXml(_tasks, _filename)
                Return 1
            End If
        Next
        Return -1
    End Function
End Class
```

> [!NOTE]
> 이 코드는 매우 기본적인 데이터 저장소 메커니즘의 예입니다.
> 이식 가능한 클래스 라이브러리 (이 예제의 경우에 로드 하 고 Xml 파일을 저장할) 플랫폼 특정 기능에 액세스 하는 인터페이스에 대 한 코드를 작성할 수는 방법을 보여 주기 위해 제공 됩니다. 이 해당 클래스일 수 없습니다 프로덕션 품질 데이터베이스 대체 합니다.

## <a name="ios-android-and-windows-phone-application-projects"></a>iOS, Android 및 Windows Phone 응용 프로그램 프로젝트

이 섹션에서는 IXmlStorage 인터페이스에 대 한 플랫폼별 구현을 포함 하 고 각 응용 프로그램에서 사용 하는 방법을 보여 줍니다. 응용 프로그램 프로젝트 모두 기록 됩니다 C#입니다.

### <a name="ios-and-android-ixmlstorage"></a>iOS 및 Android IXmlStorage

Xamarin.iOS 및 Xamarin.Android 전체 제공 `System.IO` 기능 손쉽게 로드 하 고 다음 클래스를 사용 하 여 Xml 파일을 저장할 수 있습니다.

```csharp
public class XmlStorageImplementation : IXmlStorage
{
    public XmlStorageImplementation(){}
    public List<TodoItem> ReadXml(string filename)
    {
        if (File.Exists(filename))
        {
            var serializer = new XmlSerializer(typeof(List<TodoItem>));
            using (var stream = new FileStream(filename, FileMode.Open))
            {
                return (List<TodoItem>)serializer.Deserialize(stream);
            }
        }
        return new List<TodoItem>();
    }
    public void WriteXml(List<TodoItem> tasks, string filename)
    {
        var serializer = new XmlSerializer(typeof(List<TodoItem>));
        using (var writer = new StreamWriter(filename))
        {
            serializer.Serialize(writer, tasks);
        }
    }
}
```

IOS 응용 프로그램에는 `TodoItemManager` 및 `XmlStorageImplementation` 만들어집니다 합니다 **AppDelegate.cs** 이 코드 조각에 나와 있는 것 처럼 파일. 처음 네 줄은 데이터를 저장할; 파일의 경로를 빌드만 마지막 두 줄은 인스턴스화할 액션이 두 클래스를 보여 줍니다.

```csharp
var xmlFilename = "TodoList.xml";
string documentsPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); // Documents folder
string libraryPath = Path.Combine(documentsPath, "..", "Library"); // Library folder
var path = Path.Combine(libraryPath, xmlFilename);
var xmlStorage = new XmlStorageImplementation();
TaskMgr = new TodoItemManager(path, xmlStorage);
```

Android 응용 프로그램에는 `TodoItemManager` 및 `XmlStorageImplementation` 만들어집니다 합니다 **Application.cs** 이 코드 조각에 나와 있는 것 처럼 파일. 첫 세 줄은 데이터를 저장할; 파일의 경로를 빌드할 것 마지막 두 줄은 인스턴스화할 액션이 두 클래스를 보여 줍니다.

```csharp
var xmlFilename = "TodoList.xml";
string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal);
var path = Path.Combine(libraryPath, xmlFilename);
var xmlStorage = new AndroidTodo.XmlStorageImplementation();
TaskMgr = new TodoItemManager(path, xmlStorage);
```

응용 프로그램 코드의 나머지 부분은 주로 사용자 인터페이스를 사용 하 여 우려 하 고 사용 하 여 합니다 `TaskMgr` 클래스를 로드 및 저장 `TodoItem` 클래스입니다.

### <a name="windows-phone-ixmlstorage"></a>Windows Phone IXmlStorage

Windows Phone에서는 장치의 파일 시스템에 대 한 전체 액세스 대신 IsolatedStorage API를 노출 합니다. `IXmlStorage` Windows Phone 대 한 구현은 다음과 같습니다.

```csharp
public class XmlStorageImplementation : IXmlStorage
{
    public XmlStorageImplementation(){}
    public List<TodoItem> ReadXml(string filename)
    {
        IsolatedStorageFile fileStorage = IsolatedStorageFile.GetUserStoreForApplication();
        if (fileStorage.FileExists(filename))
        {
            var serializer = new XmlSerializer(typeof(List<TodoItem>));
            using (var stream = new StreamReader(new IsolatedStorageFileStream(filename, FileMode.Open, fileStorage)))
            {
                return (List<TodoItem>)serializer.Deserialize(stream);
            }
        }
        return new List<TodoItem>();
    }
    public void WriteXml(List<TodoItem> tasks, string filename)
    {
        IsolatedStorageFile fileStorage = IsolatedStorageFile.GetUserStoreForApplication();
        var serializer = new XmlSerializer(typeof(List<TodoItem>));
        using (var writer = new StreamWriter(new IsolatedStorageFileStream(filename, FileMode.OpenOrCreate, fileStorage)))
        {
            serializer.Serialize(writer, tasks);
        }
    }
}
```

합니다 `TodoItemManager` 및 `XmlStorageImplementation` 만들어집니다 합니다 **App.xaml.cs** 이 코드 조각에 나와 있는 것 처럼 파일.

```csharp
var filename = "TodoList.xml";
var xmlStorage = new XmlStorageImplementation();
TodoMgr = new TodoItemManager(filename, xmlStorage);
```

Xaml의 Windows Phone 응용 프로그램의 나머지 부분으로 구성 됩니다 하 고 C# 사용자 인터페이스를 만들고 사용 하는 `TodoMgr` 클래스를 로드 및 저장 `TodoItem` 개체입니다.

## <a name="visual-basic-pcl-in-visual-studio-for-mac"></a>Visual Basic Mac 용 Visual Studio에서 PCL

Mac 용 visual Studio는 Visual Basic 언어를 지원 하지 않습니다 – 만들거나 mac 용 Visual Studio를 사용 하 여 Visual Basic 프로젝트를 컴파일할 수 없습니다.

이식 가능한 클래스 라이브러리에 대 한 지원 Mac 용 visual Studio는 Visual Basic에서 작성 된 PCL 어셈블리를 참조 하는 것을 의미 합니다.

이 섹션에서는 Visual Studio에서 PCL 어셈블리를 컴파일하는 것을 버전 제어 시스템에 저장 되며 다른 프로젝트에서 참조를 확인 하는 방법을 설명 합니다.

### <a name="keeping-the-pcl-output-from-visual-studio"></a>Visual Studio에서 PCL 출력 유지

기본적으로 대부분의 버전 제어 시스템 (TFS 및 Git 포함)를 무시 하도록 구성할 수는 **/bin/** 즉, 컴파일된 PCL 어셈블리 디렉터리에 저장 되지 것입니다. 즉,이에 대 한 참조를 추가 하는 Mac 용 Visual Studio를 실행 하는 모든 컴퓨터에 수동으로 복사 해야 합니다.

버전 제어 시스템 PCL 어셈블리 출력을 저장할 수 있도록 프로젝트 루트에 복사 하는 빌드 후 스크립트를 만들 수 있습니다. 이 빌드 후 단계는 어셈블리를 쉽게 소스 제어에 추가 하 고 다른 프로젝트와 공유할 수 있는지 확인 하는 데 도움이 됩니다.

#### <a name="visual-studio-2017"></a>Visual Studio 2017

1. 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 합니다 **속성 > 빌드 이벤트** 섹션입니다.

2. 추가 _post-build_ 프로젝트 루트 디렉터리에이 프로젝트에서 출력 DLL을 복사 하는 스크립트 (인 외부 **/bin/**). 버전 제어 구성에 따라 DLL 이제 있어야 소스 제어에 추가 합니다.

  [![](native-apps-images/image6-vs-sml.png "빌드 이벤트 사후 빌드 스크립트 VB DLL을 복사 하려면")](native-apps-images/image6-vs.png#lightbox)

다음에 프로젝트를 빌드하면, 이식 가능한 클래스 라이브러리 어셈블리 확인에서 / 커밋/푸시 DLL 변경 해야 하는 경우 및 프로젝트 루트에 복사 됩니다 (되도록 Mac 용 Visual Studio를 사용 하 여 Mac에 다운로드할 수 있습니다)를 저장 합니다.

  [![](native-apps-images/image8-sml.png "Visual Basic 어셈블리 출력 파일 위치")](native-apps-images/image8.png#lightbox)


이 어셈블리 다음 추가할 수 있습니다 Visual Studio에서 Xamarin 프로젝트에 Mac 용 Visual Basic 언어 자체 Xamarin iOS 또는 Android 프로젝트에서 지원 되지 않습니다도.

### <a name="referencing-the-pcl-in-visual-studio-for-mac"></a>Mac 용 Visual Studio에서 PCL 참조

Xamarin은 Visual Basic을 지원 하지 않으므로이 스크린샷에 표시 된 대로 PCL 프로젝트 (또는 Windows Phone 앱) 로드할 수 없습니다.

 [![](native-apps-images/image9.png "솔루션 Mac 용 visual Studio")](native-apps-images/image9.png#lightbox)

여전히 Xamarin.iOS 및 Xamarin.Android 프로젝트에서 Visual Basic PCL 어셈블리 DLL을 포함할 수 있습니다 했습니다.

1.  마우스 오른쪽 단추로 클릭 합니다 **참조가** 노드와 선택 **참조 편집...**

    [![](native-apps-images/image10.png "프로젝트 편집 참조 메뉴")](native-apps-images/image10.png#lightbox)

1.  선택 된 **.NET 어셈블리** 탭 및 Visual Basic 프로젝트 디렉터리에 출력 DLL로 이동 합니다. Mac 용 Visual Studio에서 프로젝트를 열 수 없는 경우에 모든 파일이 있어야 소스 제어에서 합니다. 클릭 **추가** 한 다음 **확인** iOS 및 Android 응용 프로그램에이 어셈블리를 추가 합니다.

    [![](native-apps-images/image11-sml.png "클릭 한 다음 추가 확인이이 어셈블리는 iOS 및 Android 응용 프로그램을 추가 하려면")](native-apps-images/image11.png#lightbox)

1.  IOS 및 Android 응용 프로그램에는 Visual Basic 이식 가능한 클래스 라이브러리에서 제공 하는 응용 프로그램 논리를 포함할 이제 수 있습니다. 이 스크린샷은 Visual Basic PCL 참조 및 해당 라이브러리에서 기능을 사용 하는 코드에는 iOS 응용 프로그램을 보여 줍니다.

    [![](native-apps-images/image12-sml.png "편집 참조에는.NET 어셈블리 창 추가")](native-apps-images/image12.png#lightbox)


Visual Studio에서 Visual Basic 프로젝트에 프로젝트를 빌드, 소스 제어에서 결과 어셈블리 DLL을 저장 해야 하 고 Mac 용 Visual Studio 빌드 수 있도록 Mac에 소스 제어에서 새 DLL을 끌어올 수 변경 되 면 최신 포함 기능입니다.


## <a name="summary"></a>요약

이 문서에 명시 되어 Visual Studio 및 이식 가능한 클래스 라이브러리를 사용 하 여 Xamarin 응용 프로그램에서 Visual Basic 코드를 사용 하는 방법입니다. Xamarin Visual Basic을 직접 지원 하지 않습니다, 경우에 Visual Basic PCL로 컴파일하는 iOS 및 Android 앱에 포함 될 Visual Basic로 작성 된 코드 수 있습니다.

## <a name="related-links"></a>관련 링크

- [TaskyPortableVB (샘플)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyPortableVB)
- [.NET Framework (Microsoft)를 사용 하 여 플랫폼 간 개발](https://msdn.microsoft.com/library/gg597391(v=vs.110).aspx)
