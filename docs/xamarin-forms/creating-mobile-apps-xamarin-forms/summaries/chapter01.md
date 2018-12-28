---
title: 1장 요약. Xamarin.Forms는 어떻게 우리의 요구사항을 충족시켜줄까요?
description: Xamarin.Forms를 사용 하 여 모바일 앱을 만듭니다. 1장 요약. Xamarin.Forms는 어떻게 우리의 요구사항을 충족시켜줄까요?
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F3F864FF-EE70-49D0-90D1-388889037625
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: 40f319a67ecc2ca81243c8ac7c415266c1ea0b5c
ms.sourcegitcommit: 9492e417f739772bf264f5944d6bae056e130480
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53746858"
---
# <a name="summary-of-chapter-1-how-does-xamarinforms-fit-in"></a>1장 요약. Xamarin.Forms는 어떻게 우리의 요구사항을 충족시켜줄까요?

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01)

> [!NOTE]
> 이 페이지에 대 한 참고 사항 Xamarin.Forms 책의 내용을에서 달라졌는지를 위치 하는 영역을 나타냅니다.

프로그래밍에서 가장 즐겁지 않은 작업 중 하나는 다른 플랫폼으로 코드를 포팅하는 작업입니다. 특히 다른 프로그래밍 언어로 포팅하는 작업은 쉽지 않은 작업입니다. 포팅 과정에서 리팩토링 충동을 느낄 수 있으며 포팅된 코드와 기존 코드는 동시에 유지보수 되어야 하기 때문에 두 코드베이스 간의 차이점들은 앞으로의 유지보수 작업을 더욱 힘들게 할 것입니다.

## <a name="cross-platform-mobile-development"></a>크로스 플랫폼 모바일 개발

이 문제는 모바일 플렛폼을 타겟으로 개발할 때 일반적으로 발생합니다. 현재 두개의 메이저 모바일 플렛폼이 존재합니다. iOS 운영체제를 사용하는 아이폰, 아이패드 애플 제품군과 안드로이드 운영체제를 사용하는 수많은 스마트폰과 태블릿이 있습니다. 다른 중요 한 플랫폼은 Microsoft의 Windows 플랫폼 (UWP (유니버설)을 모두 Windows 10을 대상으로 한 개의 프로그램을 수 있습니다.

이러한 플랫폼을 대상으로 하려는 소프트웨어 공급 업체는 다양 한 사용자 인터페이스 패러다임, 세 가지 다른 개발 환경, 세 개의 서로 다른 프로그래밍 인터페이스를 사용 하 여 처리 해야 하 고&mdash;아마도 가장 이라는&mdash;3 다른 프로그래밍 언어: IPhone 및 iPad, Android 용 Java 용 Objective-c 및 C# Windows에 대 한 합니다.

## <a name="the-c-and-net-solution"></a>C#과 .NET 솔루션

Objective-C, Java 및 C#은 모두 C 프로그래밍 언어에서 파생되었지만, 서로 다른 방향으로 발전하였습니다. C#은 이들 언어 중 가장 최신 언어이며 매우 유용한 방향으로 성장해 왔습니다. 또한 C#은 .NET이라는 프로그래밍 인프라와 밀접하게 관련되어 있습니다. .NET은 수학, 디버깅, 리플렉션, 컬렉션, 세계화, 파일 I/O, 네트워킹, 보안, 스레딩, 웹 서비스, 데이터 처리 및 XML과 JSON 읽기와 쓰기를 지원합니다.

현재 Xamarin은 네이티브 Mac, iOS 및 Android API를 지원하는 C#, .NET 도구들을 제공합니다. 이러한 도구는 Xamarin.Mac, Xamarin.iOS 및 Xamarin.Android라고 불리며 Xamarin 플랫폼이라고 통칭합니다. Xamarin은 각 플랫폼의 Native API를 .NET 관용어구로 표현하는 라이브러리와 바인딩을 제공합니다.

개발자는 Mac, iOS 또는 Android 어플리케이션을 개발하기 위해 Xamarin 플랫폼을 사용할 수 있으며 C#언어로 개발할 수 있습니다. 하나 이상의 플랫폼을 타겟으로 개발할 때는 플랫폼 간에 일부 코드를 공유하는 것이 합리적입니다. 따라서 프로그램 코드를 플랫폼 의존적인 코드(주로 사용자 인터페이스와 관계된 코드)와 플랫폼 중립적인 코드(일반적으로 .NET 프레임워크만을 필요로 하는 코드)로 분리해야 합니다. 플랫폼 중립적인 코드는 Portable Class Library(PCL) 또는 공유 프로젝트(Shared Asset Project 또는 SAP)로 관리할 수 있습니다.

> [!NOTE]
> Portable Class Library는 .NET Standard 라이브러리로 변경되었습니다. 이 책에서 모든 샘플 코드는 .NET 표준 라이브러리를 사용하도록 변경되었습니다.

## <a name="introducing-xamarinforms"></a>Xamarin.Forms 소개

여러 모바일 플랫폼을 대상으로 하는 경우 Xamarin.Forms는 더 많은 코드 공유를 허용합니다. Xamarin.Forms 용으로 작성 된 단일 프로그램 이러한 플랫폼을 대상 수 있습니다.

- iPhone, iPad 및 iPod touch에서 실행되는 프로그램을 지원하는 iOS
- Android 휴대폰 및 태블릿에서 실행되는 프로그램을 위한 Android
- 대상 Windows 10 유니버설 Windows 플랫폼

> [!NOTE]
> Xamarin.Forms는 더 이상 Windows 8.1, Windows Phone 8.1 또는 Windows 10 Mobile을 지원하지 않지만 Windows 10 데스크톱에서 실행은 지원합니다. 또한 [Mac](~/xamarin-forms/platform/mac.md), [WPF](~/xamarin-forms/platform/wpf.md), [GTK #](~/xamarin-forms/platform/gtk.md), [Tizen](~/xamarin-forms/platform/tizen.md) 플랫폼에 대한 Preview Support도 제공합니다.

대부분의 Xamarin.Forms 프로그램의 공유 코드는 단일 라이브러리나 SAP에 위치하게 됩니다. 각 플랫폼들은 이러한 공유 코드를 호출하는 작은 프로그램 조각으로 구성됩니다.

Xamarin.Forms API는 각 플랫폼의 네이티브 컨트롤에 매핑되며 그 결과 각 플랫폼의 고유한 형태와 느낌을 유지하게 됩니다.

[![공유 플랫폼 시각 효과의 삼중 스크린 샷](images/ch01fg03-small.png "각 플랫폼에서 Xamarin.Forms 컨트롤")](images/ch01fg03-large.png#lightbox "각 플랫폼에서 Xamarin.Forms 컨트롤")

왼쪽에서 오른쪽 스크린샷을 iPhone 및 Android 휴대폰을 보여 줍니다.

각 화면에서 페이지는 텍스트를 표시하는 Xamarin.Forms [ `Label` ](xref:Xamarin.Forms.Label), 작업을 시작하기 위한 [ `Button` ](xref:Xamarin.Forms.Button), On/Off를 선택하는 [ `Switch` ](xref:Xamarin.Forms.Switch), 범위 내에서 값을 선택할 수 있는 [ `Slider` ](xref:Xamarin.Forms.Slider)를 포함하고 있습니다. 이 4개의 View는 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 내에 포함된 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)의 자식들입니다.

또한 몇 개의 [ `ToolbarItem` ](xref:Xamarin.Forms.ToolbarItem)로 구성된 Xamarin.Forms 도구 모음이 페이지에 연결되어 있습니다. 도구 모음은 iOS 및 Android 화면에서는 맨 위에 나타나 있으며 Windows 10 Mobile에서는 화면 아래쪽에 아이콘으로 표시되어 있습니다.

또한 Xamarin.Forms는 XAML(마이크로소프트에서 개발한 Extensible Application Markup Language)을 지원합니다. 위에 나타난 프로그램들의 시각적인 부분은 XAML로 정의되어 있으며 [ **PlatformVisuals** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01/PlatformVisuals) 샘플에서 확인하실 수 있습니다.

Xamarin.Forms 프로그램은 실행 중인 플랫폼이 어떤 플랫폼인지 식별할 수 있으며, 플랫폼에 따라 서로 다른 코드를 실행할 수 있습니다. 또한 개발자는 각 플랫폼을 위한 커스텀 코드들을 작성할 수 있으며 이렇게 작성한 코드들을 Xamarin.Forms에서 플랫폼 중립적인 방법으로 호출할 수 있습니다. 개발자는 각 플랫폼을 위한 렌더러를 작성함으로써 추가적인 컨트롤을 작성할 수 있습니다.

Xamarin.Forms는 비지니스 어플리케이션을 개발하거나 빠른 개념 증명 데모를 만들거나 프로토타입 어플리케이션에 적합한 솔루션이지만, 벡터 그래픽이나 복잡한 터치 상호 작용을 필요로 하는 응용 프로그램 개발에 대해서는 덜 적합합니다.

## <a name="your-development-environment"></a>개발 환경

개발 환경은 개발하려는 플렛폼이나 개발에 사용되는 컴퓨터에 따라 달라집니다.

iOS 어플리케이션을 개발하려는 경우 XCode와 Xamarin 플랫폼이 설치된 Mac이 필요합니다. 또한 Android를 지원하려면 Java와 필요 SDK가 필요합니다. 그런 다음 Visual Studio for Mac을 사용하면 iOS와 Android 어플리케이션을 모두 개발할 수 있습니다.

PC에 Visual Studio를 설치하면 iOS, Android 및 Windows 플랫폼을 대상으로 개발할 수 있습니다. 그러나 이 경우에도 iOS 개발을 위해서는 XCode와 Xamarin 플랫폼이 설치된 Mac이 필요합니다.

컴퓨터에 USB로 연결된 실제 장치나 시뮬레이터를 통해 프로그램을 테스트할 수 있습니다.

## <a name="installation"></a>설치

Xamarin.Forms 어플리케이션을 생성하고 빌드하기 전에 개발하고자 하는 플렛폼과 개발환경에 맞추어 iOS, Android, UWP 어플리케이션을 각각 분리하여 만들어보는 것이 좋습니다.

이 작업을 수행하는 방법에 대한 정보를 제공하는 Xamarin과 Microsoft 웹 사이트:

- [IOS 시작](~/ios/get-started/index.md)
- [Android 시작](~/android/get-started/index.md)
- [Windows 개발자 센터](http://dev.windows.com)

일단 플랫폼별로 분리하여 프로젝트를 생성하고 실행하는 데 성공했다면 Xamarin.Forms 애플리케이션 개발에도 전혀 문제가 없을 것입니다.

## <a name="related-links"></a>관련 링크

- [1 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch01-Apr2016.pdf)
- [1 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01)
