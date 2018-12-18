---
title: Xamarin.Forms 용 XAML 미리 보기
description: 이 문서에는 XAML 미리 보기를 사용하여 표시되는 Xamarin.Forms 레이아웃을 확인하는 방법을 설명합니다. XAML 미리 보기는 Visual Studio 2017 및 Visual Studio for Mac에서 사용할 수 있습니다.
ms.prod: xamarin
ms.assetid: 84769ff1-72fd-4c44-8251-dd6d5bf8c7b2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: d29186681f79a2f7591978411b5d15fd9f90f807
ms.sourcegitcommit: 03dfb4a2c20ad68515875b415e7d84ee9b0a8cb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51563474"
---
# <a name="xaml-previewer-for-xamarinforms"></a>Xamarin.Forms 용 XAML 미리 보기

_입력하면서 표시되는 Xamarin.Forms 레이아웃을 확인 하세요._

## <a name="requirements"></a>요구 사항

프로젝트를 수행하려면 XAML 미리 보기에 대한 최신 Xamarin.Forms NuGet 패키지가 필요 합니다. Android 앱을 미리 보기하려면 [JDK 1.8 x64](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)가 필요합니다.

[릴리스 정보](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#Xamarin_Forms_Previewer)에 더 많은 정보가 있습니다.

## <a name="getting-started"></a>시작하기

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

XAML 미리 보기는 기본적으로 기본적으로 켜져 있으며 **도구 > 옵션 > Xamarin > Forms Previewer** 대화 상자에서 제어 할 수 있습니다. 해당 대화 상자에서 기본 문서 보기(Default document view) 및 분할 방향(Split Orientation)을 선택할 수 있습니다.

[![Visual Studio의 ListView 컨트롤 미리 보기](xaml-previewer-images/xamlp-options-vs.png "Visual Studio의 Forms Previewer 옵션")](xaml-previewer-images/xamlp-options-vs.png#lightbox "Visual Studio의 Forms Previewer 옵션")

XAML 페이지를 열면 편집기는 **도구 > 옵션 > Xamarin > Forms Previewer** 대화 상자에서 선택한 설정에 따라 분할됩니다. 그러나 이러한 기본 설정은 편집기 창에서 변경할 수 있습니다.

## <a name="xaml-preview-controls"></a>XAML 미리 보기 제어

편집기 창의 맨 위에는 사용 중인 창을 선택하는 단추가 있으며, 맨 위에 있는 단추는 디자인 창으로 전환되고, 하단의 단추는 소스 창으로 전환됩니다. 가운데 단추는 창 순서를 바꿉니다.

[![Visual Studio의 ListView 컨트롤 미리 보기](xaml-previewer-images/xamlp-controls-vs.png "Visual Studio의 Forms Previewer 창 제어")](xaml-previewer-images/xamlp-controls-vs.png#lightbox "Visual Studio의 Forms Previewer 창 제어")

편집기 창의 아래쪽에는 창을 세로 및 가로로 분할하고 현재 창을 확장하거나 축소 할 수 있는 단추가 있습니다.

[![Visual Studio의 ListView 컨트롤 미리 보기](xaml-previewer-images/xamlp-controls2-vs.png "Visual Studio의 Forms Previewer 창 제어")](xaml-previewer-images/xamlp-controls2-vs.png#lightbox "Visual Studio의 Forms Previewer 창 제어")

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

XAML 페이지를 열면 편집기에 **미리 보기** 버튼이 표시 됩니다. 미리 보기 창은 다음과 같이 XAML 문서 창의 오른쪽 위 모서리에 있는 **미리 보기** 버튼을 눌러 표시하거나 숨길 수 있습니다.

[![Visual Studio for Mac의 ListView 컨트롤 미리 보기](xaml-previewer-images/xamlp-list-sml.png "Visual Studio for Mac의 Forms Previewer")](xaml-previewer-images/xamlp-list.png#lightbox "Visual Studio for Mac의 Forms Previewer")

-----

## <a name="xaml-preview-options"></a>XAML 미리 보기 옵션

미리 보기 창 맨 위에 있는 옵션은 다음과 같습니다.

* **Phone** – 폰 크기 화면에 표시
* **Tablet** – 태블릿 크기 화면에 표시 (창의 우측 하단에 확대/축소 컨트롤이 있다는 것을 참고)
* **Android** – Android 버전의 화면 표시
* **iOS** – iOS 버전의 화면 표시
* 세로 (아이콘)-미리 보기에 세로 방향을 사용
* 가로 (아이콘) – 미리 보기에 가로 방향을 사용

## <a name="adding-design-time-data"></a>디자인 시 데이터 추가

일부 레이아웃은 사용자 인터페이스 컨트롤에 바인딩 된 아무런 데이터가 없으면 시각화하기 어려울 수 있습니다. 미리 보기를 더 유용하게 만들려면 바인딩 컨텍스트(코드 비하인드 또는 XAML 사용)를 하드 코딩하여 컨트롤에 일부 정적 데이터를 할당하십시오.

XAML에서 정적 뷰모델에 바인딩하는 방법을 보려면 James Montemagno의 [디자인 시 데이터 추가에 대한 블로그 게시물](http://motzcod.es/post/143702671962/xamarinforms-xaml-previewer-design-time-data)을 참조하십시오.

## <a name="detecting-design-mode"></a>디자인 모드 인식

정적 [ `DesignMode.IsDesignModeEnabled` ](xref:Xamarin.Forms.DesignMode.IsDesignModeEnabled) 속성을 검사하여 응용 프로그램이 미리 보기에서 실행 중인지 여부를 확인할 수 있습니다. 해당 옵션을 사용하면 다음과 같이 미리 보기에서 응용 프로그램이 실행될 때만 실행되는 코드를 지정할 수 있습니다.

```csharp
if (DesignMode.IsDesignModeEnabled)
{
  // Previewer only code  
}
```

## <a name="troubleshooting"></a>문제 해결

아래 문제를 확인하고 문제가 발생하면 [Xamarin 포럼](https://forums.xamarin.com/categories/xamarin-forms)을 확인하십시오.

### <a name="xaml-preview-isnt-showing"></a>XAML 미리 보기가 표시 되지 않으면

다음을 확인하십시오.

* XAML 파일 미리 보기를 시도 하기 전에 프로젝트를 빌드(컴파일)해야 합니다.
* 디자이너 에이전트는 처음 XAML 파일 미리 보기 할 때 설정해야 합니다. 준비가 완료 될 때까지 진행률 표시기가 진행률 메시지와 함께 미리보기에 나타납니다.
* XAML 파일을 닫았다가 다시 열기 해 봅니다.
* `App` 클래스가 매개 변수 없는 생성자를 가지고 있는지 확인합니다.

### <a name="invalid-xaml-the-android-project-needs-to-built-before-preview-can-be-created"></a>잘못 된 XAML: 미리 보기를 만들기 전에 Android 프로젝트를 빌드해야 함

XAML 미리 보기를 사용하려면 페이지를 표시하기 전에 해당 프로젝트를 빌드해야 합니다.
미리 보기 창 상단 아래에 오류가 나타나면 응용 프로그램을 다시 빌드하고 시도 하십시오.

![오류 메시지: 프로젝트를 먼저 빌드해야 합니다](xaml-previewer-images/error-not-built-sml.png "오류 메시지: 프로젝트 다시 작성")
