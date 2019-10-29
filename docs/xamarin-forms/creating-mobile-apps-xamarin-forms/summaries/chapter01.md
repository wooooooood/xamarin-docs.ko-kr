---
title: 1 장 요약 Xamarin.ios는 어떻게 적합 한가요?
description: 'Xamarin.ios를 사용 하 여 Mobile Apps 만들기: 1 장 요약 Xamarin.ios는 어떻게 적합 한가요?'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F3F864FF-EE70-49D0-90D1-388889037625
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: 6dfa473bdfb4c1dd88ca833dbf5011a0bbdec42a
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032893"
---
# <a name="summary-of-chapter-1-how-does-xamarinforms-fit-in"></a>1 장 요약 Xamarin.ios는 어떻게 적합 한가요?

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01)

> [!NOTE]
> 이 페이지의 정보는 Xamarin.ios가 책에 제공 된 자료에서 달라져서 있는 영역을 표시 합니다.

프로그래밍에서 가장 불쾌 작업 중 하나는 플랫폼 간에 코드 베이스를 이식 하는 것입니다. 특히 해당 플랫폼에서 다른 프로그래밍 언어를 필요로 하는 경우에 해당 합니다. 코드를 리팩터링 하는 데 코드를 이식할 때 하려는 유혹 있지만 두 플랫폼을 동시에 유지 해야 하는 경우에는 두 코드 베이스 간의 차이로 인해 향후 유지 관리가 어려워집니다.

## <a name="cross-platform-mobile-development"></a>플랫폼 간 모바일 개발

이 문제는 모바일 플랫폼을 대상으로 지정할 때 일반적입니다. 현재 iOS 운영 체제를 실행 하는 Apple Iphone 및 Ipad의 두 가지 주요 모바일 플랫폼과 다양 한 휴대폰 및 태블릿에서 실행 되는 Android 운영 체제가 있습니다. 또 다른 중요 한 플랫폼은 단일 프로그램이 Windows 10을 대상으로 할 수 있도록 하는 Microsoft UWP (유니버설 Windows 플랫폼)입니다.

이러한 플랫폼을 대상으로 하는 소프트웨어 공급 업체는 다양 한 사용자 인터페이스 패러다임, 3 개의 다른 개발 환경, 3 개의 다른 프로그래밍 인터페이스 및&mdash;가장 awkwardly&mdash;세 가지를 처리 해야 합니다. 프로그래밍 언어: iPhone 및 iPad 용 목적-C, Android 용 Java 및 C# Windows 용입니다.

## <a name="the-c-and-net-solution"></a>C# 및 .net 솔루션

목적-C, Java 및 C# 은 모두 c 프로그래밍 언어에서 파생 되지만 매우 다른 경로로 발전 했습니다. C#는 이러한 언어 중 최신 버전 이며 매우 유용한 방법으로 없겠지만 되었습니다. 또한 C# 는 수학, 디버깅, 리플렉션, 컬렉션, 세계화, 파일 i/o, 네트워킹, 보안, 스레딩, 웹 서비스, 데이터 처리를 지 원하는 .net 이라는 전체 프로그래밍 인프라와 밀접 하 게 연관 되어 있습니다. 및 XML 및 JSON 읽기/쓰기

Xamarin은 현재 및 .NET을 사용 하 여 C# 네이티브 Mac, IOS 및 Android api를 대상으로 하는 도구를 제공 합니다. 이러한 도구는 xamarin 플랫폼 이라고 통칭 된 xamarin.ios, Xamarin.ios 및 Xamarin 이라고 합니다. 이러한 플랫폼은 .NET 관용구를 사용 하 여 이러한 플랫폼의 네이티브 Api를 표현 하는 라이브러리 및 바인딩입니다.

개발자는 Xamarin 플랫폼을 사용 하 여 Mac, C# IOS 또는 Android를 대상으로 하는 응용 프로그램을 작성할 수 있습니다. 그러나 두 개 이상의 플랫폼을 대상으로 지정 하는 경우 대상 플랫폼에서 일부 코드를 공유 하는 것이 매우 적합 합니다. 여기에는 프로그램을 플랫폼에 종속 된 코드 (일반적으로 사용자 인터페이스 포함)와 플랫폼 독립적인 코드 (일반적으로 기본 .NET framework만 필요 함)로 구분 하는 작업이 포함 됩니다. 이 플랫폼 독립적인 코드는 PCL (이식 가능한 클래스 라이브러리)에 상주할 수도 있고 공유 자산 프로젝트 또는 SAP 라고도 하는 공유 프로젝트에 상주할 수도 있습니다.

> [!NOTE]
> 이식 가능한 클래스 라이브러리는 .NET Standard 라이브러리로 대체 되었습니다. 이 책의 모든 샘플 코드는 .NET standard 라이브러리를 사용 하도록 변환 되었습니다.

## <a name="introducing-xamarinforms"></a>Xamarin.ios 소개

여러 모바일 플랫폼을 대상으로 하는 경우 Xamarin을 사용 하면 더 많은 코드를 공유할 수 있습니다. Xamarin.ios 용으로 작성 된 단일 프로그램은 다음 플랫폼을 대상으로 할 수 있습니다.

- iPhone, iPad 및 iPod touch에서 실행 되는 프로그램을 위한 iOS
- Android 휴대폰 및 태블릿에서 실행 되는 android for 프로그램
- Windows 10을 대상으로 하는 유니버설 Windows 플랫폼

> [!NOTE]
> Xamarin.ios는 Windows 8.1, Windows Phone 8.1 또는 Windows 10 Mobile을 더 이상 지원 하지 않지만 Xamarin.ios 응용 프로그램은 Windows 10 desktop에서 실행 됩니다. [Mac](~/xamarin-forms/platform/other/mac.md), [WPF](~/xamarin-forms/platform/other/wpf.md), [GTK #](~/xamarin-forms/platform/other/gtk.md)및 [Tizen](~/xamarin-forms/platform/other/tizen.md) 플랫폼에 대 한 미리 보기 지원도 있습니다.

Xamarin Forms 프로그램의 대부분은 라이브러리나 SAP에 있습니다. 각 플랫폼은이 공유 코드를 호출 하는 작은 응용 프로그램 스텁으로 구성 됩니다.

Xamarin.ios Api는 각 플랫폼의 네이티브 컨트롤에 매핑되므로 각 플랫폼에서 해당 특성의 모양과 느낌을 유지 합니다.

[![플랫폼 시각적 개체 공유의 삼중 스크린샷](images/ch01fg03-small.png "각 플랫폼의 Xamarin Forms 컨트롤")](images/ch01fg03-large.png#lightbox "각 플랫폼의 Xamarin Forms 컨트롤")

왼쪽에서 오른쪽으로 iPhone 및 Android 휴대폰을 표시 합니다.

각 화면에서 페이지에는 텍스트를 표시 하기 위한 Xamarin [`Label`](xref:Xamarin.Forms.Label) , 작업 시작에 대 한 [`Button`](xref:Xamarin.Forms.Button) , 설정/해제 값을 선택 하는 [`Switch`](xref:Xamarin.Forms.Switch) 및 연속 범위 내에 값을 지정 하는 [`Slider`](xref:Xamarin.Forms.Slider) 이 포함 됩니다. . 이러한 뷰 중 4 개는 [`ContentPage`](xref:Xamarin.Forms.ContentPage)에서 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 의 자식입니다.

또한 페이지에 연결 된는 여러 [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) 개체로 구성 된 xamarin.ios 도구 모음입니다. 이러한 도구는 iOS 및 Android 화면 위쪽 및 Windows 10 Mobile 화면 아래쪽에 아이콘으로 표시 됩니다.

Xamarin.ios는 여러 응용 프로그램 플랫폼에 대해 Microsoft에서 개발한 Extensible Application Markup Language XAML도 지원 합니다. 위에 표시 된 프로그램의 모든 시각적 개체는 [**Platformvisuals 개체**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01/PlatformVisuals) 샘플에서 설명한 대로 XAML로 정의 됩니다.

Xamarin.ios 프로그램은 실행 중인 플랫폼을 확인 하 고 그에 따라 다른 코드를 실행할 수 있습니다. 개발자는 다양 한 플랫폼에 대 한 사용자 지정 코드를 작성 하 고 아주 프로그램에서 플랫폼 독립적 방식으로 해당 코드를 실행할 수 있습니다. 또한 개발자는 각 플랫폼에 대 한 렌더러를 작성 하 여 추가 컨트롤을 만들 수 있습니다.

Xamarin.ios는 lob (기간 업무) 응용 프로그램에 적합 한 솔루션 이거나, 프로토타입 또는 개념 증명 데모를 수행 하는 데 적합 한 솔루션 이지만 벡터 그래픽이 나 복잡 한 터치 조작이 필요한 응용 프로그램에는 적합 하지 않습니다.

## <a name="your-development-environment"></a>개발 환경

개발 환경은 대상으로 지정할 플랫폼과 사용 하려는 컴퓨터에 따라 달라 집니다.

IOS를 대상으로 하려면 Xcode 및 Xamarin 플랫폼이 설치 된 Mac이 필요 합니다. Android도 지원 하려면 Java 및 필수 Sdk를 설치 해야 합니다. 그런 다음 Mac용 Visual Studio를 사용 하 여 iOS 및 Android를 대상으로 지정할 수 있습니다.

Visual Studio를 설치 하면 PC에서 iOS, Android 및 모든 Windows 플랫폼을 대상으로 지정할 수 있습니다. 그러나 Visual Studio에서 iOS를 대상으로 하는 경우에도 Xcode 및 Xamarin 플랫폼이 설치 된 Mac이 필요 합니다.

USB를 통해 컴퓨터 또는 시뮬레이터에 연결 된 실제 장치에서 프로그램을 테스트할 수 있습니다.

## <a name="installation"></a>설치

Xamarin Forms 응용 프로그램을 만들고 빌드하기 전에 대상으로 지정 하려는 플랫폼과 개발 환경에 따라 iOS 응용 프로그램, Android 응용 프로그램 및 UWP 응용 프로그램을 별도로 만들고 빌드해야 합니다.

Xamarin 및 Microsoft 웹 사이트에는이 작업을 수행 하는 방법에 대 한 정보가 포함 되어 있습니다.

- [IOS 시작](~/ios/get-started/index.md)
- [Android 시작](~/android/get-started/index.md)
- [Windows 개발자 센터](https://dev.windows.com)

이러한 개별 플랫폼에 대 한 프로젝트를 만들고 실행할 수 있는 경우에는 Xamarin.ios 응용 프로그램을 만들고 실행 하는 데 문제가 없어야 합니다.

## <a name="related-links"></a>관련 링크

- [1 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch01-Apr2016.pdf)
- [1 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01)
