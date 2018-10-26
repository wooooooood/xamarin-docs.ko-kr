---
title: Xamarin.Forms에 대 한 XAML 미리 보기
description: 이 문서에 입력할 때 렌더링 하 여 Xamarin.Forms 레이아웃을 확인 하려면 XAML 미리 보기를 사용 하는 방법을 설명 합니다. XAML 미리 보기는 Visual Studio 2017 및 mac 용 Visual Studio에서 사용할 수 있습니다.
ms.prod: xamarin
ms.assetid: 84769ff1-72fd-4c44-8251-dd6d5bf8c7b2
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/31/2018
ms.openlocfilehash: def7a7a3bdd9e165252c5ad1928b89ec654e5d74
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108088"
---
# <a name="xaml-previewer-for-xamarinforms"></a>Xamarin.Forms에 대 한 XAML 미리 보기

_입력할 때 렌더링 하 여 Xamarin.Forms 레이아웃을 확인 하세요._

## <a name="requirements"></a>요구 사항

프로젝트 작업을 XAML 미리 보기에 대 한 최신 Xamarin.Forms NuGet 패키지를 해야 합니다. 필요한 Android 앱을 미리 보기 [JDK 1.8 x64](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)합니다.

자세한 정보는 [릴리스](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#Xamarin_Forms_Previewer)합니다.

## <a name="getting-started"></a>시작

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

XAML 미리 보기는 기본적으로 및에서 제어할 수 있습니다 합니다 **도구 > 옵션 > Xamarin > Forms 미리 보기** 대화 합니다. 이 대화 상자에서 기본 문서 보기 및 분할 방향을 선택할 수 있습니다.

[![Visual Studio에서 ListView 컨트롤 미리 보기](xaml-previewer-images/xamlp-options-vs.png "Visual Studio의 Forms 미리 보기 옵션")](xaml-previewer-images/xamlp-options-vs.png#lightbox "Visual Studio의 Forms 미리 보기 옵션")

때 선택한 설정에 따라 편집기 분할 XAML 페이지를 열고 합니다 **도구 > 옵션 > Xamarin > Forms 미리 보기** 대화 합니다. 그러나 이러한 기본 설정 편집기 창에서 변경할 수 있습니다.

## <a name="xaml-preview-controls"></a>XAML 미리 보기 컨트롤

편집기 창의 위쪽에는 창 위쪽 단추 디자인 창으로 전환 및 소스 창으로 전환 하 고 아래쪽 단추를 사용 하 여 사용 중인 선택 단추가 있습니다. 가운데 단추 창 순서를 바꿉니다.

[![Visual Studio에서 ListView 컨트롤 미리 보기](xaml-previewer-images/xamlp-controls-vs.png "Visual Studio에서 컨트롤을 폼 미리 보기 창")](xaml-previewer-images/xamlp-controls-vs.png#lightbox "Visual Studio의 Forms 미리 보기 창 제어")

편집기 창의 아래쪽에 가로 및 세로로 분할 창, 현재 하위 창을 확장 하거나 축소 하는 단추가 있습니다.

[![Visual Studio에서 ListView 컨트롤 미리 보기](xaml-previewer-images/xamlp-controls2-vs.png "Visual Studio에서 컨트롤을 폼 미리 보기 창")](xaml-previewer-images/xamlp-controls2-vs.png#lightbox "Visual Studio의 Forms 미리 보기 창 제어")

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

합니다 **미리 보기** 단추가 XAML 페이지를 열면 편집기에 표시 됩니다. 미리 보기 창에 표시 하거나 키를 눌러 숨길 수는 **미리 보기** 모든 XAML 문서 창의 오른쪽 위 모서리에 있는 단추:

[![Mac 용 Visual Studio에서 ListView 컨트롤 미리](xaml-previewer-images/xamlp-list-sml.png "Mac 용 Visual Studio의 Forms 미리 보기")](xaml-previewer-images/xamlp-list.png#lightbox "Mac 용 Visual Studio의 Forms 미리 보기")

-----

## <a name="xaml-preview-options"></a>XAML 미리 보기 옵션

미리 보기 창 맨 위에 있는 옵션은 다음과 같습니다.

* **Phone** – phone 크기 화면에 렌더링 합니다.
* **태블릿** – 태블릿 크기 화면에 렌더링 (확대/축소 컨트롤 창의 오른쪽 아래에 있는 참고)
* **Android** – Android 버전의 화면 표시
* **iOS** – iOS 버전의 화면 표시
* Portait (아이콘)-미리 보기에 세로 방향을 사용합니다
* 가로 (아이콘) – 사용 하 여 가로 방향 미리 보기

## <a name="adding-design-time-data"></a>디자인 타임 데이터를 추가합니다.

일부 레이아웃 없이 사용자 인터페이스 컨트롤에 바인딩된 모든 데이터를 시각화 하기 어려울 수 있습니다. 미리 보기를 더 유용 하 게 하려면 일부 정적 데이터 컨트롤에 할당 하드 코딩 하 여 바인딩 컨텍스트 (코드 숨김 또는 XAML을 사용 하 여 중 하나).

James Montemagno의 가리킵니다 [디자인 타임 데이터를 추가 하는 방법에 대 한 블로그 게시물](http://motzcod.es/post/143702671962/xamarinforms-xaml-previewer-design-time-data) XAML에서 정적 ViewModel에 바인딩하는 방법을 확인 합니다.

## <a name="detecting-design-mode"></a>디자인 모드를 검색합니다.

정적 [ `DesignMode.IsDesignModeEnabled` ](xref:Xamarin.Forms.DesignMode.IsDesignModeEnabled) 미리 보기에서 응용 프로그램을 실행 중인지 여부를 확인 하려면 속성을 검사할 수 있습니다. 이 옵션을 사용 하면 응용 프로그램 미리 보기에서 실행 중일 때만 실행 하는 코드를 지정할 수 있습니다.

```csharp
if (DesignMode.IsDesignModeEnabled)
{
  // Previewer only code  
}
```

## <a name="troubleshooting"></a>문제 해결

아래 문제를 확인 하며 [Xamarin 포럼](https://forums.xamarin.com/categories/xamarin-forms)문제가 있는 경우.

### <a name="xaml-preview-isnt-showing"></a>XAML 미리 보기 표시 되지 않으면

다음을 확인하십시오.

* 프로젝트를 빌드해야 (컴파일된) XAML 파일 미리 보기를 시도 하기 전에 합니다.
* 디자이너 에이전트 설치를 XAML 파일을 미리 볼 처음 여야 합니다-이 준비 될 때까지 진행률 메시지와 함께 미리 보기에서 진행률 표시기가 나타납니다.
* Try 닫고 다시 열어 XAML 파일입니다.
* 확인 프로그램 `App` 클래스는 매개 변수가 없는 생성자가 있습니다.

### <a name="invalid-xaml-the-android-project-needs-to-built-before-preview-can-be-created"></a>잘못 된 XAML: Android 프로젝트에 추가 해야 미리 보기를 만들기 전에 작성

XAML 미리 보기 페이지를 렌더링 하기 전에 해당 프로젝트가 빌드 수 있어야 합니다.
미리 보기 창의 맨 아래 오류 있으면 다시 응용 프로그램을 빌드하고 다시 시도 하세요.

![오류 메시지: 프로젝트 우선 빌드해야](xaml-previewer-images/error-not-built-sml.png "오류 메시지: 프로젝트를 다시 작성")
