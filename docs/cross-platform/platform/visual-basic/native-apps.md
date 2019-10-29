---
title: Xamarin Android 및 Xamarin.ios의 Visual Basic
description: 이 연습에서는 Visual Basic.NET으로 작성 된 비즈니스 논리를 사용 하는 네이티브 Xamarin.ios 및 Xamarin Android 앱을 빌드하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 455fda67-3879-4299-8036-b12840e6a498
author: davidortinau
ms.author: daortin
ms.date: 04/24/2019
ms.openlocfilehash: 9f227f51596a4ed93fd830c3f3495a90c1f7f722
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73014561"
---
# <a name="visual-basic-in-xamarin-android-and-ios"></a>Xamarin Android 및 iOS의 Visual Basic

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/mobile-samples/visualbasic-taskyvb/)

[Taskyvb](https://docs.microsoft.com/samples/xamarin/mobile-samples/visualbasic-taskyvb/) 샘플 응용 프로그램은 .NET Standard 라이브러리로 컴파일되는 Visual Basic 코드를 Xamarin과 함께 사용할 수 있는 방법을 보여 줍니다. 다음은 Android 및 iOS에서 실행 되는 응용 프로그램의 몇 가지 스크린샷입니다.

 [Visual Basic로 빌드된 앱을 실행 하는 Android 및 iOS![](native-apps-images/simulators-sml.png)](native-apps-images/simulators.png#lightbox)

예제의 Android 및 iOS 프로젝트는 모두로 C#작성 됩니다. 각 응용 프로그램에 대 한 사용자 인터페이스는 기본 기술을 사용 하 여 작성 되지만 `TodoItem` 관리는 XML 파일을 사용 하 여 Visual Basic .NET Standard 라이브러리에서 제공 됩니다 (전체 데이터베이스가 아닌 데모용).

## <a name="sample-walkthrough"></a>샘플 연습

이 가이드는 iOS 및 Android 용 [Taskyvb](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyVB) Xamarin 샘플에서 Visual Basic를 구현 하는 방법에 대해 설명 합니다.

> [!NOTE]
> 이 가이드를 계속 진행 하기 전에 [Visual Basic 및 .NET Standard](index.md) 에 대 한 지침을 검토 합니다.
>
> 공유 사용자 인터페이스 Visual Basic 코드를 사용 하 여 앱을 빌드하는 방법에 대 한 자세한 내용은 [Visual Basic를 사용 하 여 xamarin.ios](xamarin-forms.md) 를 참조 하세요.

## <a name="visualbasicnetstandard"></a>VisualBasicNetStandard

Visual Basic .NET Standard 라이브러리는 Windows의 Visual Studio 에서만 만들 수 있습니다.
예제 라이브러리에는 이러한 Visual Basic 파일의 응용 프로그램에 대 한 기본 사항이 포함 되어 있습니다.

- TodoItem
- TodoItemManager
- TodoItemRepositoryXML
- XmlStorage .vb

### <a name="todoitemvb"></a>TodoItem

이 클래스는 응용 프로그램 전체에서 사용할 비즈니스 개체를 포함 합니다. Visual Basic에서 정의 되 고로 작성 된 Android 및 iOS 프로젝트와 공유 됩니다 C#.

클래스 정의는 다음과 같습니다.

```vb
Public Class TodoItem
    Property ID() As Integer
    Property Name() As String
    Property Notes() As String
    Property Done() As Boolean
End Class
```

이 샘플에서는 XML 직렬화 및 역직렬화를 사용 하 여 TodoItem 개체를 로드 하 고 저장 합니다.

### <a name="todoitemmanagervb"></a>TodoItemManager

관리자 클래스는 이식 가능한 코드에 대해 ' API '를 표시 합니다. `TodoItem` 클래스에 대 한 기본 CRUD 작업을 제공 하지만 이러한 작업을 구현 하지는 않습니다.

```vb
Public Class TodoItemManager
    Private _repository As TodoItemRepositoryXML
    Public Sub New(filename As String)
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

생성자는 IXmlStorage의 인스턴스를 매개 변수로 사용 합니다. 이렇게 하면 각 플랫폼에서 자체적으로 작동 하는 구현을 제공할 수 있으며, 이식 가능한 코드에서 공유할 수 있는 다른 기능을 설명할 수 있습니다.

### <a name="todoitemrepositoryvb"></a>TodoItemRepository

리포지토리 클래스에는 TodoItem 개체 목록을 관리 하는 논리가 포함 되어 있습니다. 전체 코드는 아래와 같습니다. 논리는 주로 컬렉션에서 추가 및 제거 될 때 TodoItems에서 고유한 ID 값을 관리 하는 데 있습니다.

```vb
Public Class TodoItemRepositoryXML
    Private _filename As String
    Private _storage As IXmlStorage
    Private _tasks As List(Of TodoItem)

    ''' <summary>Constructor</summary>
    Public Sub New(filename As String)
        _filename = filename
        _storage = New XmlStorage
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
> .NET Standard 라이브러리가 인터페이스에 대해 코딩 하 여 플랫폼별 기능에 액세스 하는 방법을 보여 주기 위해 제공 됩니다 (이 경우 XML 파일 로드 및 저장). 프로덕션 품질 데이터베이스를 대체 하기 위한 것이 아닙니다.

## <a name="android-and-ios-application-projects"></a>Android 및 iOS 응용 프로그램 프로젝트

### <a name="ios"></a>iOS

IOS 응용 프로그램에서 `TodoItemManager`와 `XmlStorageImplementation`는이 코드 조각에 표시 된 것 처럼 **AppDelegate.cs** 파일에 생성 됩니다. 처음 네 줄은 데이터가 저장 되는 파일에 대 한 경로를 작성 하는 것입니다. 마지막 두 줄에서는 인스턴스화되는 두 개의 클래스를 보여 줍니다.

```csharp
var xmlFilename = "TodoList.xml";
string documentsPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); // Documents folder
string libraryPath = Path.Combine(documentsPath, "..", "Library"); // Library folder
var path = Path.Combine(libraryPath, xmlFilename);

TaskMgr = new TodoItemManager(path);
```

### <a name="android"></a>Android

Android 응용 프로그램에서 `XmlStorageImplementation` `TodoItemManager`는이 코드 조각에 표시 된 것 처럼 **Application.cs** 파일에 생성 됩니다. 처음 세 줄은 데이터가 저장 되는 파일에 대 한 경로를 작성 하는 것입니다. 마지막 두 줄에서는 인스턴스화되는 두 개의 클래스를 보여 줍니다.

```csharp
var xmlFilename = "TodoList.xml";
string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal);
var path = Path.Combine(libraryPath, xmlFilename);

TaskMgr = new TodoItemManager(path);
```

응용 프로그램 코드의 나머지 부분은 주로 사용자 인터페이스와 `TaskMgr` 클래스를 사용 하 여 `TodoItem` 클래스를 로드 하 고 저장 하는 것과 관련이 있습니다.

## <a name="visual-studio-2019-for-mac"></a>Mac용 Visual Studio 2019

> [!WARNING]
> Mac용 Visual Studio는 Visual Basic 언어 편집을 지원 하지 않습니다. Visual Basic 프로젝트 또는 파일을 만드는 데 사용할 수 있는 메뉴 항목이 없습니다. **.Vb** 를 열면 언어 구문 강조 표시, 자동 완성 또는 IntelliSense가 없습니다.

Mac 용 visual Studio 2019는 Windows에서 만든 Visual Studio .NET Standard 프로젝트를 컴파일할 _수_ 있으므로 iOS 앱에서 해당 프로젝트를 참조할 수 있습니다.

Visual Studio 2017은 Visual Basic 프로젝트를 빌드할 _수 없습니다_ .

## <a name="summary"></a>요약

이 문서에서는 Visual Studio 및 .NET Standard 라이브러리를 사용 하 여 Xamarin 응용 프로그램에서 Visual Basic 코드를 사용 하는 방법을 보여 주었습니다. Xamarin은 Visual Basic를 직접 지원 하지 않지만 Visual Basic를 .NET Standard 라이브러리로 컴파일하면 Visual Basic로 작성 된 코드를 iOS 및 Android 앱에 포함할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [TaskyVB (.NET Standard 샘플)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyVB)
- [.NET Standard의 새로운 기능](https://docs.microsoft.com/dotnet/standard/whats-new/whats-new-in-dotnet-standard?tabs=csharp)
