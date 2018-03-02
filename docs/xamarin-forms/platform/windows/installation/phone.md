---
title: "Windows Phone 응용 프로그램 추가"
ms.topic: article
ms.prod: xamarin
ms.assetid: B598FA9D-6818-4CC9-8191-838C156DB9DA
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/16/2016
ms.openlocfilehash: bb3b7e101dad98a76ac6b8f55d190514d1bd599d
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="adding-a-windows-phone-app"></a>Windows Phone 응용 프로그램 추가


먼저 PCL Xamarin.Forms 템플릿을 사용 하는 경우, [프로필을 업데이트](~/xamarin-forms/platform/windows/installation/index.md), 아래 지침을 따릅니다.

1. **솔루션을 마우스 오른쪽 단추로 클릭 > 추가 > 새 프로젝트...**  추가 **비어 있는 앱 (Windows Phone)**

  ![](phone-images/add-wp81.png "새 프로젝트 추가 대화 상자")

2. **새로 만든된 프로젝트를 마우스 오른쪽 단추로 클릭 > NuGet 패키지 관리...**  추가 **Xamarin.Forms** 패키지 합니다.

3. **프로젝트를 마우스 오른쪽 단추로 클릭 > 추가 > 참조** 공유 Xamarin.Forms 응용 프로그램 프로젝트에 대 한 프로젝트를 만듭니다.

  ![](phone-images/addref.png "참조 관리자 대화 상자")

4. 편집 **App.xaml.cs** 포함 하도록는 `Init()` 메서드 호출의 `OnLaunched` 줄 67 메서드:

```csharp
// add this line
Xamarin.Forms.Forms.Init (e); // requires LaunchActivatedEventArgs
// above this existing line
if (e.PreviousExecutionState == ApplicationExecutionState.Terminated) {}
```

 5 . 편집 **MainPage.xaml** -루트 요소를 변경할 `<Page` 를 `<forms:WindowsPhonePage` *및* 정의 `xmlns:forms` 를 사용 하 여:

```xaml
<forms:WindowsPhonePage
...
   xmlns:forms="using:Xamarin.Forms.Platform.WinRT"
...
</forms:WindowsPhonePage>
```

 6 . 편집 **MainPage.xaml.cs** 제거 하는 `: PhonePage` 클래스 이름에 대 한 상속 지정자입니다.

```csharp
public sealed partial class MainPage  // REMOVE ": PhonePage"
```

 7 . 여전히 **MainPage.xaml.cs**, 추가 `LoadApplication` 호출는 `MainPage` 줄 28) (주위 생성자 Xamarin.Forms 응용 프로그램을 시작:

```csharp
// below this existing line
this.InitializeComponent();
// add this line
LoadApplication(new YOUR_NAMESPACE.App());
```

8 . 두 번 클릭 **Package.appxmanifest** 필요한 경우가 많습니다 이러한 기능을 설정 하려면:

  * 인터넷(클라이언트 및 서버)

9 . 마지막으로, 로컬 리소스를 않음 (예: 추가 이미지 파일)는 필요한 기존 플랫폼 프로젝트에서.

