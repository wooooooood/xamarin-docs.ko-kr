---
title: Windows 앱 추가
ms.prod: xamarin
ms.assetid: ADF99B78-F1BC-48DF-9128-01B93C4411C1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/16/2016
ms.openlocfilehash: 0d2ef44896c9352776443c2fec318d40d27d7539
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="adding-a-windows-app"></a>Windows 앱 추가


1. **솔루션을 마우스 오른쪽 단추로 클릭 > 추가 > 새 프로젝트...**  추가 **비어 있는 앱 (Windows)**

 ![](tablet-images/add-wu.png "새 프로젝트 추가 대화 상자")

2. **프로젝트를 마우스 오른쪽 단추로 클릭 > NuGet 패키지 관리...**  추가 **Xamarin.Forms** 패키지 합니다.

3. **프로젝트를 마우스 오른쪽 단추로 클릭 > 추가 > 참조** 공유 Xamarin.Forms 응용 프로그램 프로젝트에 대 한 프로젝트를 만듭니다.

  ![](tablet-images/addref.png "참조 관리자 대화 상자")

4. 편집 **App.xaml.cs** 포함 하도록는 `Init()` 메서드 호출 내에서 `OnLaunched` 줄 65 메서드

```csharp
// add this line
Xamarin.Forms.Forms.Init (e); // requires LaunchActivatedEventArgs
// above this existing line
if (e.PreviousExecutionState == ApplicationExecutionState.Terminated) {}
```

 5 . 편집 **MainPage.xaml** -루트 요소를 변경할 `<Page` 를 `<forms:WindowsPage` *및* 정의 `xmlns:forms` 를 사용 하 여:

```xaml
<forms:WindowsPage
...
   xmlns:forms="using:Xamarin.Forms.Platform.WinRT"
...
</forms:WindowsPage>
```


 6 . 편집 **MainPage.xaml.cs** 제거 하는 `: Page` 클래스 이름에 대 한 상속 지정자입니다.

```csharp
public sealed partial class MainPage  // REMOVE ": Page"
```

 7 . 여전히 **MainPage.xaml.cs**, 추가 `LoadApplication` 호출는 `MainPage` 줄 28) (주위 생성자 Xamarin.Forms 응용 프로그램을 시작:

```csharp
// below this existing line
this.InitializeComponent();
// add this line
LoadApplication(new YOUR_NAMESPACE.App());
```

8 . 두 번 클릭 **Package.appxmanifest** 필요한 경우가 많습니다 이러한 기능을 설정 하려면:

  기능을 설정합니다.

  * 인터넷(클라이언트)
  * 위치

9 . 마지막으로, 로컬 리소스를 않음 (예: 추가 이미지 파일)는 필요한 기존 플랫폼 프로젝트에서.

