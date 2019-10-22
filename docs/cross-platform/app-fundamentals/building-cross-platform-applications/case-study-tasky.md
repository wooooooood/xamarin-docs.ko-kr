---
title: '플랫폼 간 앱 사례 연구: Tasky'
description: 이 문서에서는 Tasky 이식 가능한 샘플 응용 프로그램을 플랫폼 간 모바일 응용 프로그램으로 설계 및 구축 하는 방법을 설명 합니다. 응용 프로그램의 요구 사항, 인터페이스, 데이터 모델, 핵심 기능, 구현 등에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: B581B2D0-9890-C383-C654-0B0E12DAD5A6
author: conceptdev
ms.author: crdun
ms.date: 03/23/2017
ms.openlocfilehash: 246ee002404fdf6fe1120c19701aceb3c2dee7db
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2019
ms.locfileid: "71249782"
---
# <a name="cross-platform-app-case-study-tasky"></a>플랫폼 간 앱 사례 연구: Tasky

*Tasky* *이식 가능* 은 간단한 할 일 목록 응용 프로그램입니다. 이 문서에서는 [플랫폼 간 응용 프로그램 빌드](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) 문서의 지침에 따라 설계 및 구축 된 방법을 설명 합니다. 토론은 다음 영역을 다룹니다.

<a name="Design_Process" />

## <a name="design-process"></a>디자인 프로세스

코딩을 시작 하기 전에 달성할 항목에 대 한도로 지도를 만드는 것이 좋습니다. 여러 가지 방법으로 노출 되는 기능을 구축 하는 플랫폼 간 개발의 경우 특히 그렇습니다. 빌드를 명확 하 게 파악 하 여 개발 주기에서 나중에 시간과 노력을 절감할 수 있습니다.

 <a name="Requirements" />

### <a name="requirements"></a>요구 사항

응용 프로그램을 디자인 하는 첫 번째 단계는 원하는 기능을 식별 하는 것입니다. 이는 높은 수준의 목표 또는 자세한 사용 사례 일 수 있습니다. Tasky에는 다음과 같은 간단한 기능 요구 사항이 있습니다.

- 작업 목록 보기
- 작업 추가, 편집 및 삭제
- 작업 상태를 ' 완료 '로 설정 합니다.

플랫폼별 기능의 사용을 고려해 야 합니다.  Tasky는 iOS 지 오 펜싱 또는 Windows Phone 라이브 타일을 활용할 수 있나요? 플랫폼 특정 기능을 첫 번째 버전에서 사용 하지 않는 경우에도 비즈니스 & 데이터 계층이 이러한 기능을 수용할 수 있도록 미리 계획 해야 합니다.

 <a name="User_Interface_Design" />

### <a name="user-interface-design"></a>사용자 인터페이스 설계

대상 플랫폼에서 구현할 수 있는 높은 수준의 디자인으로 시작 합니다. 플랫폼 특정 UI 제약 조건을 주의 해야 합니다. 예를 들어 iOS의 `TabBarController`은 5 개 이상의 단추를 표시할 수 있지만 Windows Phone 해당 하는 항목은 최대 4 개까지 표시할 수 있습니다.
선택한 도구를 사용 하 여 화면 흐름을 그립니다 (paper works).

 [![](case-study-tasky-images/taskydesign.png "Draw the screen-flow using the tool of your choice paper works")](case-study-tasky-images/taskydesign.png#lightbox)

 <a name="Data_Model" />

### <a name="data-model"></a>데이터 모델

저장 해야 하는 데이터를 알면 사용할 지 속성 메커니즘을 결정 하는 데 도움이 됩니다. 사용 가능한 저장소 메커니즘에 대 한 자세한 내용은 [플랫폼 간 데이터 액세스](~/cross-platform/app-fundamentals/index.md) 를 참조 하 고이를 결정 하는 데 도움을 줍니다. 이 프로젝트의 경우 SQLite.NET를 사용 합니다.

Tasky는 각 ' TaskItem '에 대 한 세 가지 속성을 저장 해야 합니다.

- **이름** – 문자열
- **참고** – 문자열
- **Done** – 부울

 <a name="Core_Functionality" />

### <a name="core-functionality"></a>핵심 기능

사용자 인터페이스에서 요구 사항을 충족 하기 위해 사용 해야 하는 API를 고려 합니다. 할 일 목록에는 다음 함수가 필요 합니다.

- **모든 작업 나열** – 사용 가능한 모든 작업의 기본 화면 목록을 표시 하려면
- 작업 행이 작업 될 때 **하나의 작업을 가져옵니다** .
- 작업 **1 개 저장** – 작업을 편집 하는 경우
- 작업 **삭제** – 태스크가 삭제 된 경우
- **빈 작업 만들기** – 새 작업을 만든 경우

코드 재사용을 위해이 API는 *이식 가능한 클래스 라이브러리*에서 한 번 구현 되어야 합니다.

 <a name="Implementation" />

### <a name="implementation"></a>구현

응용 프로그램 디자인을 동의한 후에는 플랫폼 간 응용 프로그램으로 구현 하는 방법을 고려해 야 합니다. 이는 응용 프로그램의 아키텍처가 됩니다. [플랫폼 간 응용 프로그램 빌드](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) 문서의 지침에 따라 응용 프로그램 코드는 다음 부분으로 분류 됩니다.

- **일반 코드** – 작업 데이터를 저장 하는 데 재사용 가능한 코드를 포함 하는 공용 프로젝트입니다. 모델 클래스 및 API를 노출 하 여 데이터의 저장 및 로드를 관리 합니다.
- **플랫폼별 코드** – 일반적인 코드를 ' 백 엔드 '로 활용 하 여 각 운영 체제에 대 한 네이티브 UI를 구현 하는 플랫폼별 프로젝트입니다.

[![](case-study-tasky-images/taskypro-architecture.png "Platform-specific projects implement a native UI for each operating system, utilizing the common code as the back end")](case-study-tasky-images/taskypro-architecture.png#lightbox)

이러한 두 부분에 대해서는 다음 섹션에서 설명 합니다.

 <a name="Common_(PCL)_Code" />

## <a name="common-pcl-code"></a>일반 (PCL) 코드

Tasky 이식 가능한 클래스 라이브러리 전략을 사용 하 여 공통 코드를 공유 합니다. 코드 공유 옵션에 대 한 설명은 [코드 공유 옵션](~/cross-platform/app-fundamentals/code-sharing.md) 문서를 참조 하세요.

데이터 액세스 계층, 데이터베이스 코드 및 계약을 비롯 한 모든 공통 코드는 라이브러리 프로젝트에 배치 됩니다.

전체 PCL 프로젝트가 아래에 나와 있습니다. 이식 가능한 라이브러리의 모든 코드는 각 대상 플랫폼과 호환 됩니다. 배포 되 면 각 네이티브 앱이 해당 라이브러리를 참조 합니다.

![](case-study-tasky-images/portable-project.png "When deployed, each native app will reference that library")

아래 클래스 다이어그램에서는 계층 별로 그룹화 된 클래스를 보여 줍니다. @No__t_0 클래스는 Sqlite-NET 패키지의 상용구 코드입니다. 나머지 클래스는 Tasky의 사용자 지정 코드입니다. @No__t_0 및 `TaskItem` 클래스는 플랫폼별 응용 프로그램에 노출 되는 API를 나타냅니다.

 [![](case-study-tasky-images/classdiagram-core.png "The TaskItemManager and TaskItem classes represent the API that is exposed to the platform-specific applications")](case-study-tasky-images/classdiagram-core.png#lightbox)

네임 스페이스를 사용 하 여 레이어를 분리 하면 각 계층 간의 참조를 관리할 수 있습니다. 플랫폼별 프로젝트는 비즈니스 계층에 대 한 `using` 문만 포함 해야 합니다. 비즈니스 계층에서 `TaskItemManager`에 의해 노출 되는 API를 통해 데이터 액세스 계층과 데이터 계층을 캡슐화 해야 합니다.

 <a name="References" />

### <a name="references"></a>참조 항목

이식 가능한 클래스 라이브러리는 플랫폼 및 프레임 워크 기능에 대해 다양 한 수준의 지원을 제공 하는 여러 플랫폼에서 사용할 수 있어야 합니다. 따라서 사용할 수 있는 패키지 및 프레임 워크 라이브러리에 대 한 제한 사항이 있습니다. 예를 들어 Xamarin.ios는 c # `dynamic` 키워드를 지원 하지 않으므로 이식 가능한 클래스 라이브러리는 Android에서 작동 하는 경우에도 동적 코드에 종속 된 패키지를 사용할 수 없습니다. Mac용 Visual Studio를 사용 하면 호환 되지 않는 패키지 및 참조를 추가할 수 없지만 나중에 발생 하는 것을 방지 하기 위해 제한을 염두에 두어야 합니다.

참고: 프로젝트에서 사용 하지 않은 프레임 워크 라이브러리를 참조 하는 것을 볼 수 있습니다. 이러한 참조는 Xamarin 프로젝트 템플릿의 일부로 포함 됩니다. 앱이 컴파일되면 링크 프로세스에서 참조 되지 않은 코드를 제거 하므로 `System.Xml` 참조 된 경우에도 Xml 함수를 사용 하지 않으므로 최종 응용 프로그램에 포함 되지 않습니다.

 <a name="Data_Layer_(DL)" />

### <a name="data-layer-dl"></a>데이터 계층 (DL)

데이터 계층에는 데이터베이스, 플랫 파일 또는 기타 메커니즘과 관련 된 데이터의 실제 저장소를 수행 하는 코드가 포함 되어 있습니다. Tasky 데이터 계층은 SQLite 네트워크 라이브러리와 연결에 추가 된 사용자 지정 코드의 두 부분으로 구성 됩니다.

Tasky는 Frank Kreuger에서 게시 한 Sqlite-net nuget 패키지를 사용 하 여 ORM (개체-관계형 매핑) 데이터베이스 인터페이스를 제공 하는 SQLite-NET 코드를 포함 합니다. @No__t_0 클래스는 `SQLiteConnection`에서 상속 되며 SQLite에 데이터를 읽고 쓰는 데 필요한 만들기, 읽기, 업데이트, 삭제 (CRUD) 메서드를 추가 합니다. 다른 프로젝트에서 다시 사용할 수 있는 일반적인 CRUD 메서드의 간단한 상용구 구현입니다.

@No__t_0는 단일 항목 이므로 동일한 인스턴스에 대 한 모든 액세스가 발생 합니다. 잠금은 여러 스레드의 동시 액세스를 방지 하는 데 사용 됩니다.

 <a name="SQLite_on_WIndows_Phone" />

#### <a name="sqlite-on-windows-phone"></a>Windows Phone의 SQLite

IOS와 Android는 모두 SQLite를 운영 체제의 일부로 제공 하지만 Windows Phone에는 호환 되는 데이터베이스 엔진이 포함 되지 않습니다. 세 플랫폼 모두에서 코드를 공유 하려면 Windows phone 네이티브 버전 SQLite가 필요 합니다. Sqlite의 Windows Phone 프로젝트를 설정 하는 방법에 대 한 자세한 내용은 [로컬 데이터베이스 작업](~/xamarin-forms/data-cloud/data/databases.md) 을 참조 하세요.

 <a name="Using_an_Interface_to_Generalize_Data_Access" />

#### <a name="using-an-interface-to-generalize-data-access"></a>인터페이스를 사용 하 여 데이터 액세스 일반화

데이터 계층은 기본 키가 필요한 추상 데이터 액세스 메서드를 구현할 수 있도록 `BL.Contracts.IBusinessIdentity`에 대 한 종속성을 사용 합니다. 그런 다음 인터페이스를 구현 하는 모든 비즈니스 계층 클래스를 데이터 계층에 유지할 수 있습니다.

인터페이스는 기본 키 역할을 할 정수 속성만 지정 합니다.

```csharp
public interface IBusinessEntity {
    int ID { get; set; }
}
```

기본 클래스는 인터페이스를 구현 하 고 SQLite-NET 특성을 추가 하 여 자동 증가 기본 키로 표시 합니다. 이 기본 클래스를 구현 하는 비즈니스 계층의 모든 클래스는 데이터 계층에 유지 될 수 있습니다.

```csharp
public abstract class BusinessEntityBase : IBusinessEntity {
    public BusinessEntityBase () {}
    [PrimaryKey, AutoIncrement]
    public int ID { get; set; }
}
```

인터페이스를 사용 하는 데이터 계층의 제네릭 메서드 예는이 `GetItem<T>` 메서드입니다.

```csharp
public T GetItem<T> (int id) where T : BL.Contracts.IBusinessEntity, new ()
{
    lock (locker) {
        return Table<T>().FirstOrDefault(x => x.ID == id);
    }
}
```

 <a name="Locking_to_prevent_Concurrent_Access" />

#### <a name="locking-to-prevent-concurrent-access"></a>동시 액세스를 방지 하기 위한 잠금

데이터베이스에 대한 동시 액세스를 방지하기 위해 `TaskItemDatabase` 클래스 내에서 [잠금](https://msdn.microsoft.com/library/c5kehkcz(v=vs.100).aspx)이 구현됩니다. 이는 다른 스레드의 동시 액세스가 serialize 되도록 하기 위한 것입니다. 그렇지 않으면 UI 구성 요소에서 백그라운드 스레드가 업데이트 하는 동시에 데이터베이스를 읽으려고 시도할 수 있습니다. 잠금이 구현 되는 방법의 예는 다음과 같습니다.

```csharp
static object locker = new object ();
public IEnumerable<T> GetItems<T> () where T : BL.Contracts.IBusinessEntity, new ()
{
    lock (locker) {
        return (from i in Table<T> () select i).ToList ();
    }
}
public T GetItem<T> (int id) where T : BL.Contracts.IBusinessEntity, new ()
{
    lock (locker) {
        return Table<T>().FirstOrDefault(x => x.ID == id);
    }
}
```

대부분의 데이터 계층 코드는 다른 프로젝트에서 다시 사용할 수 있습니다. 계층의 응용 프로그램별 코드는 `TaskItemDatabase` 생성자의 `CreateTable<TaskItem>` 호출 뿐입니다.

 <a name="Data_Access_Layer_(DAL)" />

### <a name="data-access-layer-dal"></a>DAL (데이터 액세스 계층)

@No__t_0 클래스는 `TaskItem` 개체를 만들고, 삭제 하 고, 검색 하 고, 업데이트할 수 있는 강력한 형식의 API를 사용 하 여 데이터 저장소 메커니즘을 캡슐화 합니다.

 <a name="Using_Conditional_Compilation" />

#### <a name="using-conditional-compilation"></a>조건부 컴파일 사용

클래스는 조건부 컴파일을 사용 하 여 파일 위치를 설정 합니다 .이는 플랫폼 확산을 구현 하는 예제입니다. 경로를 반환 하는 속성은 각 플랫폼에서 다른 코드로 컴파일됩니다. 코드 및 플랫폼별 컴파일러 지시문이 다음과 같이 표시 됩니다.

```csharp
public static string DatabaseFilePath {
    get {
        var sqliteFilename = "TaskDB.db3";
#if SILVERLIGHT
        // Windows Phone expects a local path, not absolute
        var path = sqliteFilename;
#else
#if __ANDROID__
        // Just use whatever directory SpecialFolder.Personal returns
        string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); ;
#else
        // we need to put in /Library/ on iOS5.1+ to meet Apple's iCloud terms
        // (they don't want non-user-generated data in Documents)
        string documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal); // Documents folder
        string libraryPath = Path.Combine (documentsPath, "..", "Library"); // Library folder
#endif
        var path = Path.Combine (libraryPath, sqliteFilename);
                #endif
                return path;
    }
}
```

플랫폼에 따라 출력은 iOS의 경우 "<app
path>/라이브러리/r o d e r/r e n t 3"이 고, Android의 경우 "<app
path>/pbbdb3"이 고 Windows Phone의 경우 "TaskDB." 뿐입니다.

### <a name="business-layer-bl"></a>비즈니스 계층 (BL)

비즈니스 계층은 모델 클래스와이를 관리 하기 위한 외관을 구현 합니다.
Tasky에서 모델은 `TaskItem` 클래스 이며 `TaskItemManager` `TaskItems`을 관리 하기 위한 API를 제공 하는 외관 패턴을 구현 합니다.

 <a name="Façade" />

#### <a name="faade"></a>Façade

 `TaskItemManager`는 응용 프로그램 및 UI 계층에서 참조 되는 Get, Save 및 Delete 메서드를 제공 하도록 `DAL.TaskItemRepository`를 래핑합니다.

필요한 경우 비즈니스 규칙 및 논리가 여기에 배치 됩니다. 예를 들어 개체를 저장 하기 전에 충족 해야 하는 유효성 검사 규칙을 사용할 수 있습니다.

 <a name="API_for_Platform-Specific_Code" />

### <a name="api-for-platform-specific-code"></a>플랫폼별 코드에 대 한 API

공용 코드를 작성 한 후에는 사용자 인터페이스를 빌드하여이를 통해 노출 되는 데이터를 수집 하 고 표시 해야 합니다. @No__t_0 클래스는 응용 프로그램 코드에서 액세스할 수 있는 간단한 API를 제공 하는 외관 패턴을 구현 합니다.

각 플랫폼별 프로젝트에서 작성 된 코드는 일반적으로 해당 장치의 네이티브 SDK와 긴밀 하 게 결합 되며 `TaskItemManager`에서 정의한 API를 사용 하 여 공통 코드에만 액세스할 수 있습니다. 여기에는 `TaskItem` 같이 노출 하는 메서드 및 비즈니스 클래스가 포함 됩니다.

이미지는 플랫폼 간에 공유 되지 않지만 각 프로젝트에 독립적으로 추가 됩니다. 이는 각 플랫폼에서 서로 다른 파일 이름, 디렉터리 및 해상도를 사용 하 여 이미지를 다르게 처리 하기 때문에 중요 합니다.

나머지 섹션에서는 Tasky UI의 플랫폼별 구현 세부 정보에 대해 설명 합니다.

 <a name="iOS_App" />

## <a name="ios-app"></a>iOS 앱

데이터를 저장 하 고 검색 하기 위해 일반적인 PCL 프로젝트를 사용 하 여 iOS Tasky 응용 프로그램을 구현 하는 데 필요한 클래스는 몇 가지 뿐입니다. 전체 iOS Xamarin.ios 프로젝트는 다음과 같습니다.

 ![](case-study-tasky-images/taskyios-solution.png "iOS project is shown here")

클래스는 계층으로 그룹화 된이 다이어그램에 표시 됩니다.

 [![](case-study-tasky-images/classdiagram-android.png "The classes are shown in this diagram, grouped into layers")](case-study-tasky-images/classdiagram-android.png#lightbox)

 <a name="References" />

### <a name="references"></a>참조 항목

IOS 앱은 플랫폼별 SDK 라이브러리를 참조 합니다 (예:). Xamarin.ios 및 Monotouch.dialog-1.

@No__t_0 PCL 프로젝트도 참조 해야 합니다.
참조 목록이 여기에 표시 됩니다.

 ![](case-study-tasky-images/taskyios-references.png "The references list is shown here")

응용 프로그램 계층 및 사용자 인터페이스 계층은 이러한 참조를 사용 하 여이 프로젝트에서 구현 됩니다.

 <a name="Application_Layer_(AL)" />

### <a name="application-layer-al"></a>응용 프로그램 계층 (AL)

응용 프로그램 계층에는 PCL에서 노출 한 개체를 UI에 ' 바인딩 ' 하는 데 필요한 플랫폼별 클래스가 포함 되어 있습니다. IOS 관련 응용 프로그램에는 작업을 표시 하는 데 도움이 되는 두 가지 클래스가 있습니다.

- **EditingSource** –이 클래스는 작업 목록을 사용자 인터페이스에 바인딩하는 데 사용 됩니다. 작업 목록에 `MonoTouch.Dialog`를 사용 했기 때문에 `UITableView`에서 살짝 밀기-삭제 기능을 사용 하려면이 도우미를 구현 해야 합니다. 살짝 밀기-삭제는 iOS에서 일반적 이지만 Android 또는 Windows Phone에는 적용 되지 않으므로 iOS 관련 프로젝트는이를 구현 하는 유일한 프로젝트입니다.
- **Taskdialog** –이 클래스는 단일 작업을 UI에 바인딩하는 데 사용 됩니다. @No__t_0 리플렉션 API를 사용 하 여 입력 화면의 형식이 올바르게 지정 될 수 있도록 올바른 특성이 포함 된 클래스를 사용 하 여 `TaskItem` 개체를 ' 래핑 ' 합니다.

@No__t_0 클래스는 `MonoTouch.Dialog` 특성을 사용 하 여 클래스의 속성을 기반으로 화면을 만듭니다. 클래스는 다음과 같습니다.

```csharp
public class TaskDialog {
    public TaskDialog (TaskItem task)
    {
        Name = task.Name;
        Notes = task.Notes;
        Done = task.Done;
    }
    [Entry("task name")]
    public string Name { get; set; }
    [Entry("other task info")]
    public string Notes { get; set; }
    [Entry("Done")]
    public bool Done { get; set; }
    [Section ("")]
    [OnTap ("SaveTask")]    // method in HomeScreen
    [Alignment (UITextAlignment.Center)]
    public string Save;
    [Section ("")]
    [OnTap ("DeleteTask")]  // method in HomeScreen
    [Alignment (UITextAlignment.Center)]
    public string Delete;
}
```

@No__t_0 특성에는 메서드 이름이 필요 합니다. 이러한 메서드는 `MonoTouch.Dialog.BindingContext`를 만든 클래스 (이 경우 다음 섹션에서 설명 하는 `HomeScreen` 클래스)에 있어야 합니다.

 <a name="User_Interface_Layer_(UI)" />

### <a name="user-interface-layer-ui"></a>UI (사용자 인터페이스 계층)

사용자 인터페이스 계층은 다음 클래스로 구성 됩니다.

1. **AppDelegate** – 응용 프로그램에 사용 되는 글꼴 및 색의 스타일을 표시 하는 모양 API에 대 한 호출을 포함 합니다. Tasky는 간단한 응용 프로그램 이므로 `FinishedLaunching`에서 실행 되는 다른 초기화 작업이 없습니다.
2. Screen **– 각** 화면 및 해당 동작을 정의 하는 `UIViewController`의 서브 클래스입니다. 응용 프로그램 계층 클래스 및 공용 API (`TaskItemManager`)를 사용 하 여 UI를 함께 연결 합니다. 이 예제에서는 코드에서 화면을 만들지만 Xcode의 Interface Builder 또는 스토리 보드 디자이너를 사용 하 여 디자인할 수 있습니다.
3. **이미지** – 시각적 요소는 모든 응용 프로그램에서 중요 한 부분입니다. Tasky에는 iOS 용 시작 화면 및 아이콘 이미지가 있고,이 이미지는 regular 및 레 티 나 resolution에서 제공 되어야 합니다.

 <a name="Home_Screen" />

#### <a name="home-screen"></a>홈 화면

홈 화면은 SQLite 데이터베이스의 작업 목록을 표시 하는 `MonoTouch.Dialog` 화면입니다. @No__t_0에서 상속 하 고 `Root`를 표시 하기 위해 `TaskItem` 개체의 컬렉션을 포함 하도록 설정 하는 코드를 구현 합니다.

 [![](case-study-tasky-images/ios-taskylist.png "It inherits from DialogViewController and implements code to set the Root to contain a collection of TaskItem objects for display")](case-study-tasky-images/ios-taskylist.png#lightbox)

작업 목록과 상호 작용을 표시 하 고 상호 작용 하는 데 관련 된 두 가지 주요 메서드는 다음과 같습니다.

1. **PopulateTable** – 비즈니스 계층의 `TaskManager.GetTasks` 메서드를 사용 하 여 표시할 `TaskItem` 개체의 컬렉션을 검색 합니다.
2. **선택 됨** – 행이 작업 될 때 새 화면에 작업을 표시 합니다.

 <a name="Task_Details_Screen" />

#### <a name="task-details-screen"></a>작업 세부 정보 화면

작업 세부 정보는 작업을 편집 하거나 삭제할 수 있는 입력 화면입니다.

Tasky는 `MonoTouch.Dialog`의 리플렉션 API를 사용 하 여 화면을 표시 하므로 `UIViewController` 구현이 없습니다. 대신, `HomeScreen` 클래스는 응용 프로그램 계층에서 `TaskDialog` 클래스를 사용 하 여 `DialogViewController`을 인스턴스화하고 표시 합니다.

이 스크린샷에서는 **이름** 및 **메모** 필드의 워터 마크 텍스트를 설정 하는 `Entry` 특성을 보여 주는 빈 화면을 보여 줍니다.

 [![](case-study-tasky-images/ios-taskydetail.png "This screenshot shows an empty screen that demonstrates the Entry attribute setting the watermark text in the Name and Notes fields")](case-study-tasky-images/ios-taskydetail.png#lightbox)

작업 **정보** 화면의 기능 (예: 작업 저장 또는 삭제)은 `HomeScreen` 클래스에서 구현 해야 합니다 .이는 `MonoTouch.Dialog.BindingContext`를 만든 위치 이기 때문입니다. 다음 `HomeScreen` 메서드는 작업 세부 정보 화면을 지원 합니다.

1. **Showtaskdetails** – 화면을 렌더링 하는 `MonoTouch.Dialog.BindingContext`을 만듭니다. 리플렉션을 사용 하 여 `TaskDialog` 클래스에서 속성 이름 및 형식을 검색 하는 입력 화면을 만듭니다. 입력란의 워터 마크 텍스트와 같은 추가 정보는 속성에 대 한 특성으로 구현 됩니다.
2. **Savetask** -이 메서드는 `OnTap` 특성을 통해 `TaskDialog` 클래스에서 참조 됩니다. **저장** 을 누를 때 호출 되며 `MonoTouch.Dialog.BindingContext`를 사용 하 여 `TaskItemManager`를 사용 하 여 변경 내용을 저장 하기 전에 사용자가 입력 한 데이터를 검색 합니다.
3. **Deletetask** -이 메서드는 `OnTap` 특성을 통해 `TaskDialog` 클래스에서 참조 됩니다. @No__t_0를 사용 하 여 기본 키 (ID 속성)를 사용 하 여 데이터를 삭제 합니다.

 <a name="Android_App" />

## <a name="android-app"></a>Android 앱

전체 Xamarin Android 프로젝트는 아래 그림에 나와 있습니다.

 ![](case-study-tasky-images/taskyandroid-solution.png "Android project is pictured here")

계층 별로 그룹화 된 클래스를 포함 하는 클래스 다이어그램:

 [![](case-study-tasky-images/classdiagram-android.png "The class diagram, with classes grouped by layer")](case-study-tasky-images/classdiagram-android.png#lightbox)

 <a name="References" />

### <a name="references"></a>참조 항목

Android 앱 프로젝트는 Android SDK의 클래스에 액세스 하기 위해 플랫폼별 Xamarin Android 어셈블리를 참조 해야 합니다.

또한 PCL 프로젝트를 참조 해야 합니다 (예: TaskyPortableLibrary)를 사용 하 여 공통 데이터 및 비즈니스 계층 코드에 액세스할 수 있습니다.

 ![](case-study-tasky-images/taskyandroid-references.png "TaskyPortableLibrary to access the common data and business layer code")

 <a name="Application_Layer_(AL)" />

### <a name="application-layer-al"></a>응용 프로그램 계층 (AL)

이전에 살펴본 iOS 버전과 마찬가지로, Android 버전의 응용 프로그램 계층에는 핵심에 의해 노출 되는 개체를 UI에 ' 바인딩 ' 하는 데 필요한 플랫폼별 클래스가 포함 되어 있습니다.

 **Tasklistadapter** – \<T > 개체의 목록을 표시 하려면 `ListView` 사용자 지정 개체를 표시 하는 어댑터를 구현 해야 합니다. 어댑터는 목록의 각 항목에 사용 되는 레이아웃을 제어 합니다 .이 경우 코드는 Android 기본 제공 레이아웃 `SimpleListItemChecked`를 사용 합니다.

 <a name="User_Interface_(UI)" />

### <a name="user-interface-ui"></a>UI (사용자 인터페이스)

Android 앱의 사용자 인터페이스 계층은 코드 및 XML 태그의 조합입니다.

- **리소스/레이아웃** – 화면 레이아웃 및 행 셀 디자인은 xml 파일로 구현 됩니다. AXML은 Android 용 Xamarin UI 디자이너를 사용 하 여 직접 작성 하거나 시각적으로 레이아웃할 수 있습니다.
- **리소스/그릴** 모양 – 이미지 (아이콘) 및 사용자 지정 단추.
- Screen **– 각** 화면 및 해당 동작을 정의 하는 동작 서브 클래스입니다. 응용 프로그램 계층 클래스 및 공용 API (`TaskItemManager`)와 UI를 함께 연결 합니다.

 <a name="Home_Screen" />

#### <a name="home-screen"></a>홈 화면

홈 화면은 작업 하위 클래스 `HomeScreen` 및 레이아웃 (단추 및 작업 목록의 위치)을 정의 하는 `HomeScreen.axml` 파일로 구성 됩니다. 화면은 다음과 같습니다.

 [![](case-study-tasky-images/android-taskylist.png "The screen looks like this")](case-study-tasky-images/android-taskylist.png#lightbox)

홈 화면 코드는 단추를 클릭 하 고 목록에서 항목을 클릭 하 고 작업 세부 정보 화면에서 변경한 내용을 반영 하도록 `OnResume` 메서드의 목록을 채우는 데 필요한 처리기를 정의 합니다. 데이터는 비즈니스 계층의 `TaskItemManager` 및 응용 프로그램 계층의 `TaskListAdapter`를 사용 하 여 로드 됩니다.

 <a name="Task_Details_Screen" />

#### <a name="task-details-screen"></a>작업 세부 정보 화면

작업 세부 정보 화면은 `Activity` 하위 클래스와 AXML 레이아웃 파일로 구성 되어 있습니다. 레이아웃은 입력 컨트롤의 위치를 결정 하 고 클래스 C# 는 `TaskItem` 개체를 로드 하 고 저장 하는 동작을 정의 합니다.

 [![](case-study-tasky-images/android-taskydetail.png "The class defines the behavior to load and save TaskItem objects")](case-study-tasky-images/android-taskydetail.png#lightbox)

PCL 라이브러리에 대 한 모든 참조는 `TaskItemManager` 클래스를 통해 진행 됩니다.

 <a name="Windows_Phone_App" />

## <a name="windows-phone-app"></a>Windows Phone 앱
전체 Windows Phone 프로젝트:

 ![](case-study-tasky-images/taskywp7-solution.png "Windows Phone App The complete Windows Phone project")

아래 다이어그램에서는 계층으로 그룹화 된 클래스를 보여 줍니다.

 [![](case-study-tasky-images/classdiagram-wp7.png "This diagram presents the classes grouped into layers")](case-study-tasky-images/classdiagram-wp7.png#lightbox)

 <a name="References" />

### <a name="references"></a>참조 항목

플랫폼별 프로젝트는 올바른 Windows Phone 응용 프로그램을 만들기 위해 필요한 플랫폼별 라이브러리 (예: `Microsoft.Phone` 및 `System.Windows`)를 참조 해야 합니다.

또한 PCL 프로젝트를 참조 해야 합니다 (예: `TaskyPortableLibrary`)를 사용 하 여 `TaskItem` 클래스와 데이터베이스를 활용할 수 있습니다.

 ![](case-study-tasky-images/taskywp7-references.png "TaskyPortableLibrary to utilize the TaskItem class and database")

 <a name="Application_Layer_(AL)" />

### <a name="application-layer-al"></a>응용 프로그램 계층 (AL)

또한 iOS 및 Android 버전과 마찬가지로, 응용 프로그램 계층은 사용자 인터페이스에 데이터를 바인딩하는 데 도움이 되는 비시각적 요소로 구성 됩니다.

 <a name="ViewModels" />

#### <a name="viewmodels"></a>ViewModels

ViewModels PCL (`TaskItemManager`)에서 데이터를 래핑하고 Silverlight/XAML 데이터 바인딩에서 사용할 수 있는 방식으로 표시 합니다. 플랫폼 특정 동작의 예입니다 (플랫폼 간 응용 프로그램 문서에서 설명).

 <a name="User_Interface_(UI)" />

### <a name="user-interface-ui"></a>UI (사용자 인터페이스)

XAML에는 태그에서 선언할 수 있는 고유한 데이터 바인딩 기능이 있으며 개체를 표시 하는 데 필요한 코드의 양을 줄일 수 있습니다.

1. **페이지** – XAML 파일 및 해당 코드 숨김은 사용자 인터페이스를 정의 하 고 VIEWMODELS PCL 프로젝트를 참조 하 여 데이터를 표시 하 고 수집 합니다.
2. **이미지** – 시작 화면, 배경 및 아이콘 이미지는 사용자 인터페이스의 핵심 부분입니다.

 <a name="MainPage" />

#### <a name="mainpage"></a>MainPage

MainPage 클래스는 `TaskListViewModel`을 사용 하 여 XAML의 데이터 바인딩 기능을 사용 하 여 데이터를 표시 합니다. 페이지의 `DataContext`은 비동기적으로 채워지는 뷰 모델로 설정 됩니다. XAML의 `{Binding}` 구문은 데이터가 표시 되는 방법을 결정 합니다.

 <a name="TaskDetailsPage" />

#### <a name="taskdetailspage"></a>TaskDetailsPage

TaskDetailsPage에 정의 된 XAML에 `TaskViewModel`를 바인딩하여 각 작업을 표시 합니다. 작업 데이터는 비즈니스 계층의 `TaskItemManager`을 통해 검색 됩니다.

 <a name="Results" />

## <a name="results"></a>결과

결과 응용 프로그램은 각 플랫폼에서 다음과 같이 표시 됩니다.

 <a name="iOS" />

### <a name="ios"></a>iOS

응용 프로그램은 탐색 모음에 배치 되는 ' 추가 ' 단추와 기본 제공 **더하기 (+)** 아이콘을 사용 하 여 iOS 표준 사용자 인터페이스 디자인을 사용 합니다. 또한 기본 `UINavigationController` ' 뒤로 ' 단추 동작을 사용 하 고 테이블에서 ' 살짝 밀기-삭제 '를 지원 합니다.

 [![](case-study-tasky-images/ios-taskylist.png "또한 기본 UINavigationController 뒤로 단추 동작을 사용 하 고 테이블의 살짝 밀기 삭제를 지원 합니다.")](case-study-tasky-images/ios-taskylist.png#lightbox)[![](case-study-tasky-images/ios-taskylist.png "또한 기본 UINavigationController 뒤로 단추 동작을 사용 하 고 테이블의 살짝 밀기 삭제를 지원 합니다.")](case-study-tasky-images/ios-taskylist.png#lightbox)

 <a name="Android" />

### <a name="android"></a>Android

Android 앱은 ' 틱 '이 표시 되어야 하는 행의 기본 제공 레이아웃을 포함 하는 기본 제공 컨트롤을 사용 합니다. 하드웨어/시스템 뒤로 동작은 화상 뒤로 단추 외에도 지원 됩니다.

 [![](case-study-tasky-images/android-taskylist.png "The hardware/system back behavior is supported in addition to an on-screen back button")](case-study-tasky-images/android-taskylist.png#lightbox)[![](case-study-tasky-images/android-taskylist.png "The hardware/system back behavior is supported in addition to an on-screen back button")](case-study-tasky-images/android-taskylist.png#lightbox)

 <a name="Windows_Phone" />

### <a name="windows-phone"></a>Windows Phone

Windows Phone 앱은 표준 레이아웃을 사용 하 여 위쪽의 탐색 모음 대신 화면 아래쪽에 있는 앱 표시줄을 채웁니다.

 [![](case-study-tasky-images/wp-taskylist.png "Windows Phone 앱은 표준 레이아웃을 사용 하 고 맨 위에 탐색 모음 대신 화면 아래쪽에 있는 앱 표시줄을 채웁니다.")](case-study-tasky-images/wp-taskylist.png#lightbox)[![](case-study-tasky-images/wp-taskylist.png "Windows Phone 앱은 표준 레이아웃을 사용 하 고 맨 위에 탐색 모음 대신 화면 아래쪽에 있는 앱 표시줄을 채웁니다.")](case-study-tasky-images/wp-taskylist.png#lightbox)

 <a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 다양 한 모바일 플랫폼 (iOS, Android 및 Windows Phone)에서 코드 재사용을 용이 하 게 하기 위해 계층화 된 응용 프로그램 디자인의 원리를 간단한 응용 프로그램에 적용 하는 방법에 대 한 자세한 설명을 제공 했습니다.

응용 프로그램 계층을 설계 하는 데 사용 되는 프로세스를 설명 하 고 각 계층에서 구현 된 코드 &amp; 기능에 대해 설명 했습니다.

[Github](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)에서 코드를 다운로드할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [플랫폼 간 응용 프로그램 빌드 (주 문서)](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [Tasky 이식 가능한 샘플 앱 (github)](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)
