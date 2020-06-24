---
title: 요약 - 1장. Xamarin.Forms가 왜 적합한가요?
description: 'Xamarin.Forms로 모바일 앱 만들기: 요약 - 1장. Xamarin.Forms가 왜 적합한가요?'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F3F864FF-EE70-49D0-90D1-388889037625
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 48b2fb429d206f6582886c94d4d99839d790dc8d
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136931"
---
# <a name="summary-of-chapter-1-how-does-xamarinforms-fit-in"></a>요약 - 1장. Xamarin.Forms가 왜 적합한가요?

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01)

> [!NOTE]
> 이 페이지의 정보는 Xamarin.Forms가 이 책에 제공된 자료와는 다르게 사용되는 경우를 설명합니다.

프로그래밍에서 가장 달갑지 않은 작업 중 하나는 플랫폼 간에 코드베이스를 이식하는 것입니다. 해당 플랫폼에 다른 프로그래밍 언어가 필요한 경우에는 특히 그렇습니다. 코드를 이식할 때 리팩터링도 하려는 유혹이 있지만 두 플랫폼을 동시에 유지해야 하는 경우에는 두 코드베이스 간의 차이로 인해 이후 유지 관리가 어려워집니다.

## <a name="cross-platform-mobile-development"></a>플랫폼 간 모바일 개발

이 문제는 모바일 플랫폼을 대상으로 지정할 때 일반적입니다. 현재 Apple iPhone 및 iPad 제품군에서 실행되는 iOS 운영 체제와 다양한 휴대폰 및 태블릿에서 실행되는 Android 운영 체제라는 두 개의 주요 모바일 플랫폼이 있습니다. 또 다른 중요한 플랫폼은 Microsoft UWP(유니버설 Windows 플랫폼)로, 단일 프로그램이 Windows 10을 대상으로 할 수 있게 해줍니다.

이러한 플랫폼을 대상으로 하는 소프트웨어 공급업체는 다양한 사용자 인터페이스 패러다임, 세 가지 개발 환경, 세 가지 프로그래밍 인터페이스, 그리고 (아마도 가장 곤란한 부분인) 세 가지 프로그래밍 언어, 즉 Objective-C(iPhone 및 iPad), Java(Android), C#(Windows)을 다뤄야 합니다.

## <a name="the-c-and-net-solution"></a>C# 및 .NET 솔루션

Objective-C, Java 및 C#는 모두 C 프로그래밍 언어에서 파생되었지만 매우 다른 경로로 발전했습니다. C#는 이러한 언어 중 최신 버전이며 매우 유용한 방법으로 성숙해왔습니다. 또한 C#는 수학, 디버깅, 리플렉션, 컬렉션, 세계화, 파일 I/O, 네트워킹, 보안, 스레딩, 웹 서비스, 데이터 처리, XML 및 JSON 읽기/쓰기에 대한 지원을 제공하는 .NET이라는 전체 프로그래밍 인프라와 밀접하게 연관되어 있습니다.

Xamarin은 현재 C# 및 .NET을 사용하여 네이티브 Mac, iOS 및 Android API를 대상으로 하는 도구를 제공합니다. 이러한 도구는 각각 Xamarin.Mac, Xamarin.iOS 및 Xamarin.Android라고 합니다(총칭하여 Xamarin 플랫폼). 이들은 .NET 관용구를 사용하여 이러한 플랫폼의 네이티브 API를 표현하는 라이브러리 및 바인딩입니다.

개발자는 Xamarin 플랫폼을 사용하여 Mac, iOS 또는 Android를 대상으로 하는 애플리케이션을 C#로 작성할 수 있습니다. 그러나 두 개 이상의 플랫폼을 대상으로 지정하는 경우 대상 플랫폼에서 일부 코드를 공유하는 것이 매우 적절합니다. 여기에는 프로그램을 플랫폼 종속적 코드(일반적으로 사용자 인터페이스와 관련)와 플랫폼 독립적 코드(일반적으로 기본 .NET 프레임워크만 필요)로 구분하는 작업이 포함됩니다. 이 플랫폼 독립적 코드는 PCL(이식 가능한 클래스 라이브러리)에 상주할 수도 있고 SAP(공유 자산 프로젝트)라고도 하는 공유 프로젝트에 상주할 수도 있습니다.

> [!NOTE]
> 이식 가능한 클래스 라이브러리는 .NET Standard 라이브러리로 대체되었습니다. 이 책의 모든 샘플 코드는 .NET Standard 라이브러리를 사용하도록 변환되었습니다.

## <a name="introducing-xamarinforms"></a>Xamarin.Forms 소개

여러 모바일 플랫폼을 대상으로 하는 경우 Xamarin.Forms를 사용하면 더 많은 코드를 공유할 수 있습니다. Xamarin.Forms용으로 작성된 단일 프로그램은 다음 플랫폼을 대상으로 할 수 있습니다.

- iOS - iPhone, iPad 및 iPod touch에서 실행되는 프로그램
- Android - Android 휴대폰 및 태블릿에서 실행되는 프로그램
- 유니버설 Windows 플랫폼 - Windows 10을 대상으로 함

> [!NOTE]
> Xamarin.Forms는 Windows 8.1, Windows Phone 8.1 또는 Windows 10 Mobile을 더 이상 지원하지 않지만 Xamarin.Forms 애플리케이션은 Windows 10 데스크톱에서 실행됩니다. [Mac](~/xamarin-forms/platform/other/mac.md), [WPF](~/xamarin-forms/platform/other/wpf.md), [GTK#](~/xamarin-forms/platform/other/gtk.md) 및 [Tizen](~/xamarin-forms/platform/other/tizen.md) 플랫폼에 대한 미리 보기 지원도 있습니다.

Xamarin.Forms 프로그램은 대부분 라이브러리 또는 SAP에 있습니다. 각 플랫폼은 이 공유 코드를 호출하는 작은 애플리케이션 스텁으로 구성됩니다.

Xamarin.Forms API는 각 플랫폼의 네이티브 컨트롤에 매핑되므로 각 플랫폼은 특징적인 모양과 느낌을 유지합니다.

[![플랫폼 시각적 개체 공유의 삼중 스크린샷](images/ch01fg03-small.png "각 플랫폼의 Xamarin.Forms 컨트롤")](images/ch01fg03-large.png#lightbox "각 플랫폼의 Xamarin.Forms 컨트롤")

스크린샷에서는 iPhone(왼쪽) 및 Android 휴대폰(오른쪽)을 보여 줍니다.

각 화면에서 페이지에는 Xamarin.Forms [`Label`](xref:Xamarin.Forms.Label)(텍스트 표시), [`Button`](xref:Xamarin.Forms.Button)(작업 시작), [`Switch`](xref:Xamarin.Forms.Switch)(on/off 값 선택) 및 [`Slider`](xref:Xamarin.Forms.Slider)(연속 범위에서 값 지정)가 포함되어 있습니다. 이러한 보기 4개는 모두 [`ContentPage`](xref:Xamarin.Forms.ContentPage)에서 [`StackLayout`](xref:Xamarin.Forms.StackLayout)의 자식입니다.

또한 페이지에는 여러 [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) 개체로 구성된 Xamarin.Forms 도구 모음이 연결되어 있습니다. 이러한 도구는 iOS 및 Android 화면 위쪽과 Windows 10 Mobile 화면 아래쪽에 아이콘으로 표시됩니다.

Xamarin.Forms는 여러 애플리케이션 플랫폼에서 Microsoft가 개발한 XAML(Extensible Application Markup Language)도 지원합니다. 위에 표시된 프로그램의 모든 시각적 개체는 [**PlatformVisuals**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01/PlatformVisuals) 샘플과 같이 XAML로 정의되어 있습니다.

Xamarin.Forms 프로그램은 실행 중인 플랫폼을 확인하고 그 결과에 따라 다른 코드를 실행할 수 있습니다. 개발자는 다양한 플랫폼에 대한 사용자 지정 코드를 작성하고 플랫폼 독립적 방식으로 Xamarin.Forms 프로그램의 해당 코드를 실행할 수 있습니다. 또한 개발자는 각 플랫폼에 대한 렌더러를 작성하여 추가 컨트롤을 만들 수 있습니다.

Xamarin.Forms는 LOB(기간 업무) 애플리케이션, 프로토타입 또는 신속한 개념 증명 데모에 적합한 솔루션이지만, 벡터 그래픽 또는 복잡한 터치 상호 작용이 필요한 애플리케이션에는 그다지 적합하지 않습니다.

## <a name="your-development-environment"></a>개발 환경

개발 환경은 대상으로 지정할 플랫폼과 사용할 컴퓨터에 따라 달라집니다.

iOS를 대상으로 하려면 Xcode 및 Xamarin 플랫폼이 설치된 Mac이 필요합니다. Android도 지원하려면 Java 및 필수 SDK를 설치해야 합니다. 그런 다음 Mac용 Visual Studio를 사용하여 iOS 및 Android를 모두 대상으로 지정할 수 있습니다.

Visual Studio를 설치하면 PC에서 iOS, Android 및 모든 Windows 플랫폼을 대상으로 지정할 수 있습니다. 그러나 Visual Studio에서 iOS를 대상으로 하는 경우 여전히 Xcode 및 Xamarin 플랫폼이 설치된 Mac이 필요합니다.

USB를 통해 컴퓨터에 연결된 실제 디바이스에서 또는 시뮬레이터에서 프로그램을 테스트할 수 있습니다.

## <a name="installation"></a>설치

Xamarin.Forms 애플리케이션을 만들고 빌드하기 전에 대상으로 지정하려는 플랫폼과 개발 환경에 따라 iOS 애플리케이션, Android 애플리케이션 및 UWP 애플리케이션을 별도로 만들고 빌드해 보아야 합니다.

Xamarin 및 Microsoft 웹 사이트에는 이 작업을 수행하는 방법에 대한 정보가 포함되어 있습니다.

- [iOS 시작](~/ios/get-started/index.md)
- [Android 시작](~/android/get-started/index.md)
- [Windows 개발자 센터](https://dev.windows.com)

이러한 개별 플랫폼에서 프로젝트를 만들고 실행할 수 있으면 Xamarin.Forms 애플리케이션을 만들고 실행하는 데 문제가 없을 것입니다.

## <a name="related-links"></a>관련 링크

- [1장 전체 텍스트(PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch01-Apr2016.pdf)
- [1장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01)
