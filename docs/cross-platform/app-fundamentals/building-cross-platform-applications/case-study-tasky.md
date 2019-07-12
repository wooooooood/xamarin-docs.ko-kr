---
title: '플랫폼 간 앱 사례 연구: Tasky'
description: 이 문서에서는 Tasky Portable 샘플 응용 프로그램 설계 되 고 플랫폼 간 모바일 응용 프로그램을 구축 하는 방법을 설명 합니다. 앱의 요구 사항, 인터페이스, 데이터 모델, 핵심 기능, 구현 및 자세히 설명합니다.
ms.prod: xamarin
ms.assetid: B581B2D0-9890-C383-C654-0B0E12DAD5A6
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 7bea80c22f6931858d0629382f6882203dfd374f
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67829947"
---
# <a name="cross-platform-app-case-study-tasky"></a>플랫폼 간 앱 사례 연구: Tasky

*Tasky* *휴대용* 는 간단한 할 일 목록 응용 프로그램입니다. 이 문서에서는 어떻게 디자인 되 고 다음 지침을 작성 합니다 [플랫폼 간 응용 프로그램 빌드](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) 문서. 설명을 다음 영역을 다룹니다.

<a name="Design_Process" />

## <a name="design-process"></a>디자인 프로세스

만드는 것이 좋습니다는 코딩을 시작 하기 전에 수행 하려는 항목에 대 한 로드맵을입니다. 이 여러 가지 방법으로 노출 될 기능을 빌드할 때 플랫폼 간 개발 하는 경우 특히 그렇습니다. 무엇을 빌드 하는지가 저장 시간과 개발 주기에서 나중에 대 한 명확한 아이디어부터 시작 합니다.

 <a name="Requirements" />

### <a name="requirements"></a>요구 사항

응용 프로그램을 디자인 하는 첫 번째 단계는 원하는 기능을 식별 하는 것입니다. 가장 중요 한 목표 수 또는 사용 사례를 자세히 설명 합니다. Tasky에 간단 하 게 기능 요구 사항이 있습니다.

-  작업의 목록을 보려면
-  추가, 편집 및 삭제 작업
-  '실행' 작업의 상태를 설정 합니다.

플랫폼별 기능 사용을 고려해 야 합니다.  Tasky 활용 지 오 펜싱 iOS 또는 Windows Phone 라이브 타일? 첫 번째 버전에서 플랫폼별 기능을 사용 하지 않는 경우에 사용자의 비즈니스 및 데이터 계층을 수용할 수 있도록 미리 계획 해야 합니다.

 <a name="User_Interface_Design" />

### <a name="user-interface-design"></a>사용자 인터페이스 설계

대상 플랫폼에서 구현할 수 있는 전반적인 디자인을 시작 합니다. 참고 플랫폼 특정 UI 제약 조건에 주의 해야 합니다. 예를 들어, 한 `TabBarController` iOS에서 표시할 수 있습니다 5 개가 넘는 단추, 반면에 해당 하는 Windows Phone 최대 4를 표시할 수 있습니다.
(용지 작동) 선택한 도구를 사용 하 여 화면-흐름을 그립니다.

 [![](case-study-tasky-images/taskydesign.png "에 선택한 문서 작업의 도구를 사용 하 여 화면-흐름 그리기")](case-study-tasky-images/taskydesign.png#lightbox)

 <a name="Data_Model" />

### <a name="data-model"></a>데이터 모델

데이터를 저장 해야 합니다. 알고 사용 하는 지 속성 메커니즘을 결정 하는 데 도움이 됩니다. 참조 [플랫폼 간 데이터 액세스](~/cross-platform/app-fundamentals/index.md) 서로 결정 데 도움이 되 고 사용 가능한 저장소 메커니즘에 대 한 정보에 대 한 합니다. 이 프로젝트에 대 한 SQLite.NET 사용 합니다.

Tasky 각 'TaskItem'에 대 한 세 가지 속성을 저장 해야 합니다.

- **이름** – 문자열
- **메모** – 문자열
- **완료** -부울

 <a name="Core_Functionality" />

### <a name="core-functionality"></a>핵심 기능

API를 사용자 인터페이스 요구 사항에 맞게 사용 해야 하는 것이 좋습니다. 할 일 목록에는 다음 기능을 필요합니다.

- **모든 태스크를 나열** – 사용 가능한 모든 작업의 주 화면 목록 표시
- **하나의 작업이** – 작업 행을 연결 하는 경우
- **하나 이상의 태스크 저장** – 작업을 편집 하는 경우
- **하나 이상의 태스크 삭제** – 작업 삭제
- **빈 작업 만들기** 새 작업이 만들어지면 –

코드 재사용을 위해서,이 API에서 구현 하는 한 번 합니다 *이식 가능한 클래스 라이브러리*합니다.

 <a name="Implementation" />

### <a name="implementation"></a>구현

응용 프로그램 디자인 면으로 동의 후에 어떻게 플랫폼 간 응용 프로그램으로 구현할 수 있습니다 하는 것이 좋습니다. 이 응용 프로그램의 아키텍처 됩니다. 지침에 따라 합니다 [플랫폼 간 응용 프로그램 빌드](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) 문서, 응용 프로그램 코드는 중단 이어야 합니다는 다음 부분으로 아래쪽:

- **일반적인 코드** – 작업 데이터를 저장 하는 코드를 다시 사용 가능한 포함 되며 모델 클래스 및 저장 하는 관리 하기 위한 API를 노출 하는 일반적인 프로젝트 및 데이터 로드 합니다.
- **플랫폼 특정 코드** – '백 엔드' 공통 코드를 활용 하 여 각 운영 체제에 대 한 네이티브 UI를 구현 하는 플랫폼별 프로젝트입니다.

[![](case-study-tasky-images/taskypro-architecture.png "플랫폼별 프로젝트 백 엔드로 공통 코드를 활용 하 여 각 운영 체제에 대 한 네이티브 UI 구현")](case-study-tasky-images/taskypro-architecture.png#lightbox)

이 두 부분은 다음 섹션에 설명 되어 있습니다.

 <a name="Common_(PCL)_Code" />

## <a name="common-pcl-code"></a>공용 (PCL) 코드

Tasky Portable 공통 코드를 공유 하는 것에 대 한 이식 가능한 클래스 라이브러리는 전략을 사용 합니다. 참조 된 [코드 공유 옵션](~/cross-platform/app-fundamentals/code-sharing.md) 코드 공유 옵션에 대 한 문서.

데이터 액세스 계층, 데이터베이스 코드 및 계약을 비롯 한 모든 일반 코드에서 라이브러리 프로젝트에 배치 됩니다.

전체 PCL 프로젝트 아래에서 설명 됩니다. 모든 이식 가능한 라이브러리의 코드는 각 대상된 플랫폼 호환 됩니다. 배포 하는 경우 각 네이티브 앱 해당 라이브러리를 참조 합니다.

![](case-study-tasky-images/portable-project.png "각 네이티브 앱 배포 하는 경우 해당 라이브러리를 참조 합니다.")

클래스 다이어그램 아래 계층에 의해 분류 된 클래스를 보여 줍니다. `SQLiteConnection` 클래스 Sqlite NET 패키지의 상용구 코드입니다. 클래스의 나머지는 Tasky에 대 한 사용자 지정 코드입니다. 합니다 `TaskItemManager` 및 `TaskItem` 클래스는 플랫폼별으로 응용 프로그램에 노출 되는 API를 나타냅니다.

 [![](case-study-tasky-images/classdiagram-core.png "TaskItemManager 및 TaskItem 클래스를 나타내는 플랫폼 관련 응용 프로그램에 노출 되는 API")](case-study-tasky-images/classdiagram-core.png#lightbox)

네임 스페이스를 사용 하 여 계층을 구분 하려면 각 계층 간의 참조를 관리 하는 데 도움이 됩니다. 플랫폼별 프로젝트에만 포함 해야는 `using` 비즈니스 계층에 대 한 문입니다. 데이터 액세스 계층 및 데이터 계층에서 노출 되는 API에서 캡슐화 해야 `TaskItemManager` 비즈니스 계층에 있습니다.

 <a name="References" />

### <a name="references"></a>참조

이식 가능한 클래스 라이브러리는 각각 다양 한 플랫폼 및 프레임 워크 기능에 대 한 지원 수준을 여러 플랫폼을 통해 사용 가능 해야 합니다. 따라서는 프레임 워크 라이브러리 및 패키지 수는 제한이 있습니다. 예를 들어 Xamarin.iOS 지원 하지 않습니다 c# `dynamic` 키워드를 하므로 이러한 코드는 Android에서 작동 하는 경우에 이식 가능한 클래스 라이브러리에서 동적 코드에 의존 하는 모든 패키지를 사용할 수 없습니다. Mac 용 visual Studio에서 호환 되지 않는 패키지 및 참조를 추가 하지 것입니다 하지만 제한 문제를 방지 하려면 나중에 주의 해야 합니다.

참고: 프로젝트에 사용 하지 않은 프레임 워크 라이브러리를 참조 하는지 볼 수 있습니다. 이러한 참조는 Xamarin 프로젝트 템플릿의 일부로 포함 됩니다. 연결 프로세스 참조 되지 않은 코드 하지만 제거 앱 컴파일되면 `System.Xml` 되었습니다 참조는 수에 포함 되지 않으므로 최종 응용 프로그램 Xml 함수를 사용 하지 때문에 있습니다.

 <a name="Data_Layer_(DL)" />

### <a name="data-layer-dl"></a>데이터 계층 (DL)

데이터 계층 데이터의 실제 저장소는 데이터베이스, 플랫 파일 또는 다른 메커니즘을 수행 하는 코드를 포함 합니다. 두 부분으로 구성 Tasky 데이터 계층: SQLite NET 라이브러리 및 사용자 지정 코드를 연결 하는 것에 추가 합니다.

Tasky는 (Frank Kreuger에서 게시) Sqlite net nuget 패키지를 데이터베이스 개체-관계형 매핑 (ORM) 인터페이스를 제공 된 SQLite NET 코드를 포함 합니다. 합니다 `TaskItemDatabase` 클래스에서 상속 `SQLiteConnection` 의 필요한 만들기, 읽기, 업데이트, SQLite에 데이터를 읽고 삭제 (CRUD) 메서드를 추가 합니다. 이 다른 프로젝트에서 다시 사용할 수 있는 일반 CRUD 메서드의 단순 상용구 구현입니다.

`TaskItemDatabase` 가 단일 동일한 인스턴스에 대해 모든 액세스 발생 하는지 확인 합니다. 여러 스레드에서 동시 액세스를 방지 하기 위해 잠금이 사용 됩니다.

 <a name="SQLite_on_WIndows_Phone" />

#### <a name="sqlite-on-windows-phone"></a>Windows Phone SQLite

IOS 및 운영 체제의 일부로 SQLite 사용 하 여 Android 모두 제공 하는 동안 Windows Phone는 호환 되는 데이터베이스 엔진을 포함 하지 않습니다. 세 플랫폼 모두에서 코드를 공유 하는 Windows phone 전용 SQLite의 버전이 필요 합니다. 참조 [로컬 데이터베이스를 사용 하 여 작업](~/xamarin-forms/data-cloud/data/databases.md) Sqlite에 대 한 Windows Phone 프로젝트를 설정 하는 방법에 대 한 자세한 내용은 합니다.

 <a name="Using_an_Interface_to_Generalize_Data_Access" />

#### <a name="using-an-interface-to-generalize-data-access"></a>인터페이스를 사용 하 여 데이터 액세스를 일반화 합니다.

데이터 계층에 종속 `BL.Contracts.IBusinessIdentity` 기본 키를 필요로 하는 추상 데이터 액세스 메서드를 구현할 수 있도록 합니다. 데이터 계층에 유지한 다음 인터페이스를 구현 하는 비즈니스 계층 클래스를 사용할 수 있습니다.

인터페이스에는 기본 키로 작동 하는 정수 속성을 지정 합니다.

```csharp
public interface IBusinessEntity {
    int ID { get; set; }
}
```

기본 클래스 인터페이스를 구현 하 고 자동 증분 기본 키로 표시 하기 위해 SQLite NET 특성을 추가 합니다. 이 기본 클래스를 구현 하는 비즈니스 계층의 모든 클래스 데이터 계층의 다음 유지 될 수 있습니다.

```csharp
public abstract class BusinessEntityBase : IBusinessEntity {
    public BusinessEntityBase () {}
    [PrimaryKey, AutoIncrement]
    public int ID { get; set; }
}
```

이 인터페이스를 사용 하는 데이터 계층에서 제네릭 메서드의 예가 `GetItem<T>` 메서드:

```csharp
public T GetItem<T> (int id) where T : BL.Contracts.IBusinessEntity, new ()
{
    lock (locker) {
        return Table<T>().FirstOrDefault(x => x.ID == id);
    }
}
```

 <a name="Locking_to_prevent_Concurrent_Access" />

#### <a name="locking-to-prevent-concurrent-access"></a>동시 액세스를 방지 하기 위해 잠금

A [잠금을](https://msdn.microsoft.com/library/c5kehkcz(v=vs.100).aspx) 내에서 구현 되는 `TaskItemDatabase` 데이터베이스에 대 한 동시 액세스를 방지 하기 위해 클래스입니다. 이렇게 하면 서로 다른 여러 스레드에서 동시 액세스는 직렬화 됩니다 (그렇지 않으면 UI 구성 요소 하려고 백그라운드 스레드에서 업데이트를 동시에 데이터베이스를 읽기). 잠금을 구현 하는 방법에 대 한 예는 다음과 같습니다.

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

다른 프로젝트에 대부분의 데이터 계층 코드를 다시 사용할 수 있습니다. 계층의 응용 프로그램별 코드를 `CreateTable<TaskItem>` 에서 호출을 `TaskItemDatabase` 생성자입니다.

 <a name="Data_Access_Layer_(DAL)" />

### <a name="data-access-layer-dal"></a>DAL (데이터 액세스 계층)

합니다 `TaskItemRepository` 클래스를 허용 하는 강력한 형식의 API 사용 하 여 데이터 저장소 메커니즘을 캡슐화 `TaskItem` 삭제, 검색 및 업데이트 된 개체를 만들 수 있습니다.

 <a name="Using_Conditional_Compilation" />

#### <a name="using-conditional-compilation"></a>조건부 컴파일을 사용

클래스는 파일 위치를 설정 하려면 조건부 컴파일을 사용-플랫폼 확산을 구현 하는 예제입니다. 속성 경로 반환 하는 각 플랫폼에서 다른 코드를 컴파일합니다. 코드 및 플랫폼별 컴파일러 지시문은 다음과 같습니다.

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

플랫폼에 따라서는 출력 됩니다. "<app
path>/Library/TaskDB.db3" ios의 경우 "<app
path>/Documents/TaskDB.db3" Android에만 "TaskDB.db3" Windows Phone 대 한 합니다.

### <a name="business-layer-bl"></a>비즈니스 계층 (BL)

비즈니스 계층에는 모델 클래스 및 관리 하기 위해 외관을 구현 합니다.
모델의 Tasky 합니다 `TaskItem` 클래스 및 `TaskItemManager` 관리 하기 위한 API를 제공 하는 외관 패턴을 구현 `TaskItems`합니다.

 <a name="Façade" />

#### <a name="faade"></a>Façade

 `TaskItemManager` 래핑하는 `DAL.TaskItemRepository` Get, 저장 및 제공 응용 프로그램 및 UI 계층에서 참조 되는 메서드를 삭제 합니다.

비즈니스 규칙 및 논리가 위치할 여기 필요한 – 예를 들어 유효성 검사 규칙 개체를 저장 하기 전에 충족 해야 하는 경우.

 <a name="API_for_Platform-Specific_Code" />

### <a name="api-for-platform-specific-code"></a>플랫폼 특정 코드에 대 한 API

일반적인 코드를 작성 한 후 수집 하 고 여기에서 제공 하는 데이터를 표시할 사용자 인터페이스를 빌드해야 합니다. `TaskItemManager` 클래스에 액세스 하려면 응용 프로그램 코드에 대 한 간단한 API를 제공 하는 외관 패턴을 구현 합니다.

각 플랫폼 특정 프로젝트에서 작성 한 코드는 일반적으로 밀접 하 게 결합 장치의 네이티브 SDK를 및 정의한 API를 사용 하는 일반적인 코드에만 액세스를 `TaskItemManager`입니다. 여기에 메서드 및 비즈니스 클래스 것을 노출 같은 `TaskItem`합니다.

이미지는 플랫폼 간에 공유 되지 않았지만 각 프로젝트에 독립적으로 추가 합니다. 이 각 플랫폼 이미지를 다르게 처리를 다른 파일 이름, 디렉터리 및 해상도 사용 하 여 중요 합니다.

나머지 섹션에서는 Tasky ui 플랫폼 전용 구현 세부 정보를 설명합니다.

 <a name="iOS_App" />

## <a name="ios-app"></a>iOS 앱

IOS 일반 PCL 프로젝트를 사용 하 여 저장 하 고 데이터를 검색 하는 Tasky 응용 프로그램을 구현 하는 데 필요한 클래스의 일부만이 있습니다. 전체 iOS Xamarin.iOS 프로젝트는 다음과 같습니다.

 ![](case-study-tasky-images/taskyios-solution.png "iOS 프로젝트는 다음과 같습니다.")

클래스는 레이어로 그룹화 합니다.이 다이어그램에 표시 됩니다.

 [![](case-study-tasky-images/classdiagram-android.png "클래스를 레이어로 그룹화 합니다.이 다이어그램에 표시 됩니다.")](case-study-tasky-images/classdiagram-android.png#lightbox)

 <a name="References" />

### <a name="references"></a>참조

IOS 앱 예는 플랫폼별 SDK 라이브러리를 참조합니다. Xamarin.iOS 및 MonoTouch.Dialog-1입니다.

또한 참조 해야 합니다는 `TaskyPortableLibrary` PCL 프로젝트.
참조 목록은 다음과 같습니다.

 ![](case-study-tasky-images/taskyios-references.png "참조 목록은 다음과 같습니다.")

응용 프로그램 계층 및 사용자 인터페이스 레이어는 이러한 참조를 사용 하 여이 프로젝트에서 구현 됩니다.

 <a name="Application_Layer_(AL)" />

### <a name="application-layer-al"></a>응용 프로그램 계층 (AL)

응용 프로그램 계층 PCL에 UI에 의해 노출 되는 개체를 '바인딩' 하는 데 필요한 플랫폼 관련 클래스를 포함 합니다. IOS-관련 응용 프로그램 작업을 표시 하는 데 두 클래스에 있습니다.

- **EditingSource** –이 클래스는 사용자 인터페이스에 작업 목록을 바인딩하는 데 있습니다. 때문에 `MonoTouch.Dialog` 사용한에서 안쪽으로 살짝 밀어-삭제 기능을 사용 하도록 설정 하려면이 도우미를 구현 해야 작업 목록에는 `UITableView` 합니다. 삭제 하려면 안쪽으로 살짝 밀어 iOS 있지만 Android 또는 Windows Phone 일반적 이므로 특정 iOS 프로젝트는 구현 하는 유일한 합니다.
- **TaskDialog** –이 클래스는 단일 태스크 UI를 바인딩할 사용 됩니다. 사용 된 `MonoTouch.Dialog` '래핑' 하는 리플렉션 API는 `TaskItem` 입력된 화면 서식을 올바르게 지정할 수 있도록 올바른 특성을 포함 하는 클래스를 사용 하 여 개체.

합니다 `TaskDialog` 클래스는 `MonoTouch.Dialog` 클래스의 속성을 기반으로 하는 화면을 만들려면 특성입니다. 클래스는 다음과 같습니다.

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

통지를 `OnTap` 특성 메서드 이름이 필요 – 이러한 메서드는 클래스에 있어야 합니다. 여기서는 `MonoTouch.Dialog.BindingContext` 만들어집니다 (이 경우는 `HomeScreen` 다음 섹션에서 설명 하는 클래스).

 <a name="User_Interface_Layer_(UI)" />

### <a name="user-interface-layer-ui"></a>사용자 인터페이스 계층 (UI)

사용자 인터페이스 계층 다음 클래스로 구성 됩니다.

1.   **AppDelegate** -글꼴 및 응용 프로그램에서 사용 되는 색 스타일 모양을 API 호출을 포함 합니다. Tasky 이므로 간단한 응용 프로그램에서 실행 중인 다른 초기화 작업 `FinishedLaunching` 합니다.
2.   **화면** –의 서브 클래스 `UIViewController` 각 화면 및 해당 동작을 정의 하는 합니다. 응용 프로그램 계층 클래스를 사용 하 여 UI 및 공용 API 화면을 함께 연결할 ( `TaskItemManager` ). 이 예제의 화면 코드에서 생성 되지만 이러한 수 있도록 고안 되었습니다 Xcode의 Interface Builder 또는 스토리 보드 디자이너를 사용 하 여 합니다.
3.   **이미지** – 시각적 요소는 모든 응용 프로그램의 중요 한 부분입니다. Tasky에 iOS에 대 한 일반 및 레 티 나 해상도에서 제공 해야 하는 시작 화면과 아이콘 이미지가 있습니다.

 <a name="Home_Screen" />

#### <a name="home-screen"></a>홈 화면

홈 화면의 `MonoTouch.Dialog` 화면에 SQLite 데이터베이스에서 작업 목록이 표시 됩니다. 상속 `DialogViewController` 하 고 설정 하는 코드를 구현 합니다 `Root` 의 컬렉션을 포함 하도록 `TaskItem` 표시에 대 한 개체입니다.

 [![](case-study-tasky-images/ios-taskylist.png "DialogViewController에서 상속 되 고 표시 하기 위해 TaskItem 개체의 컬렉션을 포함 하는 루트를 설정 하는 코드를 구현 합니다.")](case-study-tasky-images/ios-taskylist.png#lightbox)

작업 목록을 사용 하 여 상호 작용 및 표시와 관련 된 두 가지 주요 방법은 다음과 같습니다.

1.   **PopulateTable** – 비즈니스 계층을 사용 하 여 `TaskManager.GetTasks` 의 컬렉션을 검색 하는 메서드 `TaskItem` 표시할 개체입니다.
2.   **선택한** – 행을 연결 하는 경우 새 화면에서 태스크를 표시 합니다.

 <a name="Task_Details_Screen" />

#### <a name="task-details-screen"></a>작업 세부 정보 화면

작업 세부 정보에는 편집 하거나 삭제 하는 작업을 허용 하는 입력된 화면입니다.

Tasky 사용 `MonoTouch.Dialog`의 리플렉션 API 화면에 표시할 거 군요 없습니다 `UIViewController` 구현 합니다. 대신 합니다 `HomeScreen` 클래스를 인스턴스화하고 표시를 `DialogViewController` 사용 하 여를 `TaskDialog` 응용 프로그램 계층에서 클래스입니다.

보여주는 빈 화면을 보여 주는이 스크린샷 합니다 `Entry` 특성 워터 마크 텍스트를 설정 합니다 **이름** 및 **정보** 필드:

 [![](case-study-tasky-images/ios-taskydetail.png "이름 및 메모 필드에 워터 마크 텍스트를 설정 하는 항목 특성을 보여 주는 빈 화면을 보여 주는이 스크린샷")](case-study-tasky-images/ios-taskydetail.png#lightbox)

기능의 **작업 세부 정보** (예: 저장 또는 삭제 작업) 화면에서 구현 해야 합니다 `HomeScreen` 위치 이기 때문에 클래스를 `MonoTouch.Dialog.BindingContext` 만들어집니다. 다음 `HomeScreen` 메서드 작업 세부 정보 화면을 지원 합니다.

1.   **ShowTaskDetails** – 만듭니다는 `MonoTouch.Dialog.BindingContext` 화면을 렌더링 합니다. 속성 이름을 검색 하기 위해 리플렉션을 사용 하 여 입력된 화면을 만들고에서 형식을 `TaskDialog` 클래스입니다. 입력된 상자에 대 한 워터 마크 텍스트와 같은 추가 정보는 속성의 특성을 사용 하 여 구현 됩니다.
2.   **SaveTask** –에서 참조 하는이 메서드는 `TaskDialog` 를 통해 클래스는 `OnTap` 특성입니다. 될 때 호출 됩니다 **저장** 를 사용 하는 `MonoTouch.Dialog.BindingContext` 사용 하 여 변경 내용을 저장 하기 전에 사용자가 입력 한 데이터를 검색할 `TaskItemManager` 합니다.
3.   **DeleteTask** –에서 참조 하는이 메서드는 `TaskDialog` 를 통해 클래스는 `OnTap` 특성입니다. 사용 하 여 `TaskItemManager` 기본 키 (ID 속성)를 사용 하 여 데이터를 삭제 합니다.

 <a name="Android_App" />

## <a name="android-app"></a>Android 앱

전체 Xamarin.Android 프로젝트는 아래 그림은:

 ![](case-study-tasky-images/taskyandroid-solution.png "Android 프로젝트는 다음과 같은")

계층을 그룹화 하는 클래스를 사용 하 여 클래스 다이어그램:

 [![](case-study-tasky-images/classdiagram-android.png "계층을 그룹화 하는 클래스를 사용 하 여 클래스 다이어그램")](case-study-tasky-images/classdiagram-android.png#lightbox)

 <a name="References" />

### <a name="references"></a>참조

Android 앱 프로젝트는 Android SDK의 클래스에 액세스 하려면 플랫폼별 Xamarin.Android 어셈블리를 참조 해야 합니다.

PCL 프로젝트 (예: 참조도 해야 합니다. TaskyPortableLibrary) 일반적인 데이터 및 비즈니스 계층 코드에 액세스할 수 있습니다.

 ![](case-study-tasky-images/taskyandroid-references.png "일반적인 데이터 및 비즈니스 계층 코드에 액세스 하려면 TaskyPortableLibrary")

 <a name="Application_Layer_(AL)" />

### <a name="application-layer-al"></a>응용 프로그램 계층 (AL)

IOS 버전에 살펴보았습니다 비슷합니다 이전에 Android 버전의 응용 프로그램 계층 클래스를 포함 플랫폼별 핵심 UI에 의해 노출 되는 개체를 '바인딩' 하는 데 필요한 합니다.

 **TaskListAdapter** – 목록을 표시 하려면<T> 에서 사용자 지정 개체를 표시 하려면 어댑터가 구현 해야 하는 개체는 `ListView`합니다. 어댑터는 레이아웃 목록의 각 항목에 대해 사용 되는 제어 – 코드를 기본 제공 Android 레이아웃을 사용 하는 예제의 `SimpleListItemChecked`합니다.

 <a name="User_Interface_(UI)" />

### <a name="user-interface-ui"></a>사용자 인터페이스 (UI)

Android 앱의 사용자 인터페이스 계층에는 코드와 XML 태그의 조합입니다.

- **리소스/레이아웃** – 화면 레이아웃 및 행 셀 AXML 파일로 구현 디자인 합니다. 를 직접 작성 또는 레이아웃이 시각적으로 Android 용 Xamarin UI 디자이너를 사용 하 여 AXML 수 있습니다.
- **리소스/Drawable** – 이미지 (아이콘) 및 사용자 지정 단추입니다.
- **화면** – 각 화면 및 해당 동작을 정의 하는 활동 서브 클래스입니다. 응용 프로그램 계층 클래스를 사용 하 여 UI 및 공용 API를 함께 연결 (`TaskItemManager`).

 <a name="Home_Screen" />

#### <a name="home-screen"></a>홈 화면

활동 서브 클래스의 홈 화면으로 구성 됩니다 `HomeScreen` 하며 `HomeScreen.axml` (단추 및 작업 목록의 위치) 레이아웃을 정의 하는 파일입니다. 화면이 다음과 유사합니다.

 [![](case-study-tasky-images/android-taskylist.png "화면이 다음과 같이 표시 됩니다.")](case-study-tasky-images/android-taskylist.png#lightbox)

홈 화면 코드는 단추를 클릭 하 고 목록에서 항목을 클릭 하면 뿐만 아니라 목록 채우기에 대 한 처리기를 정의 합니다 `OnResume` 메서드 (작업 세부 정보 화면에서 변경한 내용을 반영 하는 해당 it). 비즈니스 계층을 사용 하 여 데이터를 로드 `TaskItemManager` 하며 `TaskListAdapter` 응용 프로그램 계층에서 합니다.

 <a name="Task_Details_Screen" />

#### <a name="task-details-screen"></a>작업 세부 정보 화면

구성도 작업 세부 정보 화면을 `Activity` 하위 클래스 및 AXML 레이아웃 파일입니다. 입력된 컨트롤의 위치를 결정 하는 레이아웃 및 C# 클래스를 로드 하 고 저장 동작을 정의 `TaskItem` 개체입니다.

 [![](case-study-tasky-images/android-taskydetail.png "TaskItem 개체를 저장 및 로드 동작을 정의 하는 클래스")](case-study-tasky-images/android-taskydetail.png#lightbox)

통해 PCL 라이브러리에 대 한 모든 참조 되는 `TaskItemManager` 클래스입니다.

 <a name="Windows_Phone_App" />

## <a name="windows-phone-app"></a>Windows Phone 앱
전체 Windows Phone 프로젝트:

 ![](case-study-tasky-images/taskywp7-solution.png "Windows Phone 앱 전체 Windows Phone 프로젝트")

아래 다이어그램에는 계층으로 그룹화 하는 클래스를 제공 합니다.

 [![](case-study-tasky-images/classdiagram-wp7.png "이 다이어그램 레이어로 분류 된 클래스를 표시 합니다.")](case-study-tasky-images/classdiagram-wp7.png#lightbox)

 <a name="References" />

### <a name="references"></a>참조

플랫폼별 프로젝트에서 필요한 플랫폼별 라이브러리를 참조 해야 합니다 (같은 `Microsoft.Phone` 고 `System.Windows`) 올바른 Windows Phone 응용 프로그램을 만드는 합니다.

PCL 프로젝트 (예: 참조도 해야 합니다. `TaskyPortableLibrary`)를 활용 하 여 `TaskItem` 클래스 및 데이터베이스입니다.

 ![](case-study-tasky-images/taskywp7-references.png "TaskItem 클래스 및 데이터베이스를 활용 하는 TaskyPortableLibrary")

 <a name="Application_Layer_(AL)" />

### <a name="application-layer-al"></a>응용 프로그램 계층 (AL)

마찬가지로 iOS 및 Android 버전의 경우와 마찬가지로 응용 프로그램 계층 비시각적 요소로 구성 사용자 인터페이스에 데이터를 바인딩할 수 있도록 합니다.

 <a name="ViewModels" />

#### <a name="viewmodels"></a>ViewModels

Viewmodel PCL에서 데이터를 래핑합니다 ( `TaskItemManager`) 하 고 Silverlight/XAML 데이터 바인딩에서 사용할 수 있는 방식으로 제공 합니다. 이 플랫폼 특정 동작의 예 (플랫폼 간 응용 프로그램 문서에서 설명).

 <a name="User_Interface_(UI)" />

### <a name="user-interface-ui"></a>사용자 인터페이스 (UI)

XAML 태그에서 선언 될 수 있으며 개체를 표시 하는 데 필요한 코드의 양을 줄일는 고유 데이터 바인딩 기능에 있습니다.

1.   **페이지** – XAML 파일과 해당 코드 숨김 사용자 인터페이스를 정의 하 고 Viewmodel 및 PCL 프로젝트를 표시 하 고 데이터 수집을 참조 합니다.
2.   **이미지** – 시작 화면, 배경 및 아이콘 이미지는 사용자 인터페이스의 핵심 부분입니다.

 <a name="MainPage" />

#### <a name="mainpage"></a>MainPage

MainPage 클래스에서 사용 하는 `TaskListViewModel` XAML의 데이터 바인딩 기능을 사용 하 여 데이터를 표시 합니다. 페이지의 `DataContext` 비동기적으로 채워지는 보기 모델에 설정 됩니다. `{Binding}` 는 XAML 구문을 데이터 표시 방법을 결정 합니다.

 <a name="TaskDetailsPage" />

#### <a name="taskdetailspage"></a>TaskDetailsPage

각 작업에 바인딩하여 표시 됩니다는 `TaskViewModel` 는 TaskDetailsPage.xaml에 정의 된 XAML을 합니다. 작업 데이터를 통해 검색 된 `TaskItemManager` 비즈니스 계층에서.

 <a name="Results" />

## <a name="results"></a>결과

응용 프로그램 같이 각 플랫폼에서.

 <a name="iOS" />

#### <a name="ios"></a>iOS

'추가' 단추를 같은 iOS 표준 사용자 인터페이스 디자인을 사용 하 여 응용 프로그램의 탐색 모음에 배치 되 고 기본 제공을 사용 하 여 **더하기 (+)** 아이콘입니다. 또한 사용 하 여 기본 `UINavigationController` '뒤로' 동작과 ' 안쪽으로 살짝 밀어 삭제 ' 테이블에서 지원 단추입니다.

 [![](case-study-tasky-images/ios-taskylist.png "또한 기본 UINavigationController 뒤로 단추 동작을 사용 하 고 테이블에서의 통과 / 삭제를 지원")](case-study-tasky-images/ios-taskylist.png#lightbox) [![](case-study-tasky-images/ios-taskylist.png "UINavigationController 기본값 사용 단추 동작을 백업 하 고 표에 통과 / 삭제를 지원 합니다.")](case-study-tasky-images/ios-taskylist.png#lightbox)

 <a name="Android" />

#### <a name="android"></a>Android

Android 앱의 기본 제공 레이아웃 표시 '틱' 필요로 하는 행을 포함 하는 기본 제공 컨트롤을 사용 합니다. 하드웨어/시스템 백 동작을 화면의 뒤로 단추 외에도 지원 됩니다.

 [![](case-study-tasky-images/android-taskylist.png "하드웨어/시스템 백 동작을 화면의 뒤로 단추 외에도 지원 됩니다")](case-study-tasky-images/android-taskylist.png#lightbox)[![](case-study-tasky-images/android-taskylist.png "하드웨어/시스템 백 동작 외에 사용할 수는 화면 뒤로 단추")](case-study-tasky-images/android-taskylist.png#lightbox)

 <a name="Windows_Phone" />

#### <a name="windows-phone"></a>Windows Phone

Windows Phone 앱의 위쪽 탐색 모음 대신 화면 맨 아래에 있는 앱 바 채우기 표준 레이아웃을 사용 합니다.

 [![](case-study-tasky-images/wp-taskylist.png "위쪽 탐색 모음 대신 화면 맨 아래에 있는 앱 표시줄을 채울 표준 레이아웃을 사용 하 여 Windows Phone 앱")](case-study-tasky-images/wp-taskylist.png#lightbox) [![](case-study-tasky-images/wp-taskylist.png "The Windows Phone 앱의 표준 사용 하 여 위쪽 탐색 모음 대신 화면 맨 아래에 있는 앱 표시줄을 채울 레이아웃")](case-study-tasky-images/wp-taskylist.png#lightbox)

 <a name="Summary" />

## <a name="summary"></a>요약

이 문서는 세 가지 모바일 플랫폼 간에 코드를 다시 사용을 용이 하 게 하는 간단한 응용 프로그램에 적용 된 계층화 된 응용 프로그램 디자인의 원칙 하는 방법에 대해 상세히 제공한: iOS, Android 및 Windows Phone입니다.

에 사용 하 여 응용 프로그램 계층을 디자인 하는 프로세스를 설명 하 고 코드 설명 &amp; 기능 각 계층에서 구현 되었습니다.

코드에서 다운로드할 수 있습니다 [github](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)합니다.

## <a name="related-links"></a>관련 링크

- [플랫폼 간 응용 프로그램 빌드 (본문)](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [Tasky Portable 샘플 앱 (github)](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)
