---
title: 요약 1 장입니다. Xamarin.Forms 포함할 됩니까?
description: 'Xamarin.Forms를 사용 하 여 모바일 앱 만들기: 1 장 요약 합니다. Xamarin.Forms 포함할 됩니까?'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F3F864FF-EE70-49D0-90D1-388889037625
author: charlespetzold
ms.author: chape
ms.date: 07/19/2018
ms.openlocfilehash: abf30f2cd828d67ef6fb04f809fce6235e1add9b
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156485"
---
# <a name="summary-of-chapter-1-how-does-xamarinforms-fit-in"></a>요약 1 장입니다. Xamarin.Forms 포함할 됩니까?

> [!NOTE] 
> 이 페이지에 대 한 참고 사항 Xamarin.Forms 책의 내용을에서 달라졌는지를 위치 하는 영역을 나타냅니다.

해당 플랫폼에는 다른 프로그래밍 언어를 포함 하는 경우에 특히 다른 플랫폼에서 기본 코드를 이식가 프로그래밍에서 가장 원하지 않는 작업 중 하나입니다. 유혹도 리팩터링 하는 코드를 이식 하는 경우 있지만 두 플랫폼 모두 병렬로 유지 해야 하는 경우 다음 두 코드 베이스의 차이점은 어려워질 향후 유지 관리 합니다.

## <a name="cross-platform-mobile-development"></a>크로스 플랫폼 모바일 개발

모바일 플랫폼을 대상으로 하는 경우이 문제가 일반적입니다. 현재 두 가지 주요 모바일 플랫폼, iOS 운영 체제 및 다양 한 휴대폰과 태블릿에서 실행 되는 Android 운영 체제를 실행 하는 Ipad 및 Iphone의 Apple 제품군 존재 합니다. 다른 중요 한 플랫폼은 Microsoft의 Windows 플랫폼 (UWP (유니버설), Windows 10 Mobile 및 Windows 10을 대상으로 한 개의 프로그램을 수 있습니다.

이러한 세 플랫폼을 대상으로 하려는 소프트웨어 공급 업체는 다양 한 사용자 인터페이스 패러다임, 세 가지 다른 개발 환경, 세 개의 서로 다른 프로그래밍 인터페이스를 사용 하 여 처리 해야 하 고&mdash;아마도 가장 이라는&mdash; 세 가지 프로그래밍 언어: Objective-c iPhone 및 iPad, android, Java 및 Windows에 대 한 C#에 대 한 합니다.

## <a name="the-c-and-net-solution"></a>C# 및.NET 솔루션

Objective-c, Java 및 C#은 모두 C 프로그래밍 언어에서 파생 됩니다, 있지만 매우 다른 경로 의해 발전 했습니다. C#은 이러한 언어 중 가장 최근 및 매우 유용한 방식으로 환경의 성숙을 통해 되었습니다. 또한 C#은 밀접 하 게 연결 수학, 디버깅, 리플렉션, 컬렉션, 세계화, 파일 I/O, 네트워킹, 보안, 스레딩, 웹 서비스, 데이터 처리 및 XML에 대 한 지원을 제공 하는.NET 호출 하는 전체 프로그래밍 인프라 및 읽기 및 쓰기 JSON입니다.

현재 Xamarin 네이티브 Mac, iOS 및 Android Api C# 및.NET을 사용 하 여 대상으로 하는 도구를 제공 합니다. 이러한 도구는 Xamarin.Mac, Xamarin.iOS 및 Xamarin.Android에서 Xamarin 플랫폼으로 통칭 라고 합니다. 이들은, 라이브러리 및.NET 관용구를 사용 하 여 이러한 플랫폼의 네이티브 Api를 표현 하는 바인딩입니다.

개발자는 해당 대상 Mac, iOS 또는 Android에서 C# 응용 프로그램을 작성 하려면 Xamarin 플랫폼을 사용할 수 있습니다. 둘 이상의 플랫폼을 대상으로 하는 경우 것은 아니지만 일부 대상 플랫폼 간에 코드를 공유 하는 것이 많이 있습니다. 플랫폼별 코드 (일반적으로 포함 되는 사용자 인터페이스) 및 일반적으로 기본.NET framework만 해야 하는 플랫폼 독립적인 코드를 프로그램을 분리 해야 합니다. 이 플랫폼 독립적인 코드 이식 가능한 클래스 라이브러리 (PCL), 또는 공유 프로젝트의 경우 자주 공유 자산 프로젝트 또는 SAP를 호출 하거나 있을 수 있습니다.

> [!NOTE] 
> 이식 가능한 클래스 라이브러리는.NET Standard 라이브러리로 바뀌었습니다. 이 책에서 모든 샘플 코드를.NET 표준 라이브러리를 사용 하도록 변환 되었습니다.

## <a name="introducing-xamarinforms"></a>Xamarin.Forms 소개

여러 모바일 플랫폼을 대상으로 하는 경우 Xamarin.Forms 더 많은 코드 공유를 허용 합니다. Xamarin.Forms 용으로 작성 된 단일 프로그램 5 개의 고유한 플랫폼 대상으로 지정할 수 있습니다.:

- iPhone, iPad 및 iPod touch에서 실행 되는 프로그램에 대 한 iOS
- Android 휴대폰 및 태블릿에서 실행 되는 프로그램에 대 한 android
- 유니버설 Windows 플랫폼을 대상 Windows 10 및 Windows 10 Mobile
- Windows 8.1 Windows 런타임 API
- Windows Phone 8.1의 Windows 런타임 API

> [!NOTE] 
> Xamarin.Forms는 Windows 8.1, Windows Phone 8.1 또는 Windows 10 Mobile 더 이상 지원 하지만 Windows 10 데스크톱에서 Xamarin.Forms 응용 프로그램을 실행 합니다. 에 대 한 미리 보기 지원도 합니다 [Mac](~/xamarin-forms/platform/mac.md)를 [WPF](~/xamarin-forms/platform/wpf.md)를 [GTK #](~/xamarin-forms/platform/gtk.md), 및 [Tizen](/xamarin-forms/platform/tizen.md) 플랫폼입니다.

Xamarin.Forms 프로그램 대량의 라이브러리 또는 SAP에 있습니다. 각 플랫폼은이 공유 코드를 호출 하는 작은 응용 프로그램 스텁을 구성 됩니다. 

Xamarin.Forms Api는 각 플랫폼에 해당 특성 모양과 느낌 유지 되도록 각 플랫폼에서 네이티브 컨트롤에 매핑됩니다.

[![공유 플랫폼 시각 효과의 삼중 스크린 샷](images/ch01fg03-small.png "각 플랫폼에서 Xamarin.Forms 컨트롤")](images/ch01fg03-large.png#lightbox "각 플랫폼에서 Xamarin.Forms 컨트롤")

왼쪽에서 오른쪽 스크린샷을 iPhone, Android 휴대폰 및 Windows 10 Mobile phone 보여 줍니다. 

> [!NOTE] 
> Xamarin.Forms는 더 이상 Windows 10 Mobile 지원합니다.

각 화면에서 페이지에는 Xamarin.Forms [ `Label` ](xref:Xamarin.Forms.Label) 텍스트를 표시를 [ `Button` ](xref:Xamarin.Forms.Button) 작업을 시작 하기 위한을 [ `Switch` ](xref:Xamarin.Forms.Switch) 에 대 한 설정/해제 값 및 [ `Slider` ](xref:Xamarin.Forms.Slider) 연속 범위 내에서 값을 지정 하는 것에 대 한 합니다. 해당 보기의 모든 4의 자식인를 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) 에 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)합니다.

구성 된 여러 Xamarin.Forms 도구 모음에는 페이지에도 연결 [ `ToolbarItem` ](xref:Xamarin.Forms.ToolbarItem) 개체입니다. 이 iOS 및 Android 화면 맨 위에 있는 및 Windows 10 Mobile 화면 아래쪽에 아이콘으로 표시 됩니다.

여러 응용 프로그램 플랫폼에 대 한 Microsoft에서 개발 된 Extensible Application Markup Language, Xamarin.Forms XAML을도 지원 합니다. 위에 표시 된 프로그램의 모든 시각적 개체에 설명 된 대로 XAML에 정의 된 합니다 [ **PlatformVisuals** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01/PlatformVisuals) 샘플입니다.

Xamarin.Forms 프로그램에서 실행 중인 플랫폼을 결정할 하 고 그에 따라 서로 다른 코드를 실행할 수 있습니다. 더를 아주 개발자는 다양 한 플랫폼에 대 한 사용자 지정 코드를 작성 하 고 플랫폼 독립적인 방식으로 Xamarin.Forms 프로그램에서 해당 코드를 실행할 수 있습니다. 또한 개발자 각 플랫폼에 대 한 렌더러를 작성 하 여 추가 컨트롤을 만들 수 있습니다.

Xamarin.Forms 빠른 개념 증명 데모를 보려면 치거나 프로토타입 생성 또는 기간 업무 응용 프로그램에 대 한 적합 한 솔루션 이지만, 벡터 그래픽 이나 복잡 한 터치 상호 작용을 필요로 하는 응용 프로그램에 대 한 작은 이상적입니다.

## <a name="your-development-environment"></a>개발 환경

개발 환경을 대상으로 지정할 플랫폼 및 새로운 컴퓨터 원하는 사용에 따라 달라 집니다.

대상 iOS 하려는 경우 Mac Xcode 및 Xamarin 플랫폼을 설치 해야 합니다. Android 에서도 지원 Java 및 필요한 Sdk를 설치 하는 작업 필요 합니다. 그런 다음 iOS 및 mac 용 Visual Studio를 사용 하 여 Android를 대상 지정할 수 있습니다.

Visual Studio를 설치 하면 PC에 iOS, Android 및 Windows 플랫폼을 대상으로 합니다. 그러나 Xcode 및 설치 하는 Xamarin 플랫폼을 사용 하 여 Mac을 위해서는 여전히 Visual Studio에서 iOS를 대상으로 합니다.

컴퓨터에 USB로 연결 중 하나는 실제 장치 또는 시뮬레이터에서 프로그램을 테스트할 수 있습니다.

## <a name="installation"></a>설치

만들고 Xamarin.Forms 응용 프로그램을 작성 하기 전에 만들고 개별적으로 iOS 응용 프로그램, Android 응용 프로그램 및 UWP 응용 프로그램을 대상 및 개발 환경을 하려는 플랫폼에 따라 하려고 해야 합니다.

이 작업을 수행 하는 방법에 대 한 정보를 포함 하는 Xamarin 및 Microsoft 웹 사이트:

- [IOS 시작](~/ios/get-started/index.md)
- [Android 시작](~/android/get-started/index.md)
- [Windows 개발자 센터](http://dev.windows.com)

한 번 만들고 이러한 개별 플랫폼을 위한 프로젝트를 실행 하는 문제가 없습니다 만들고 Xamarin.Forms 응용 프로그램을 실행 합니다.

## <a name="related-links"></a>관련 링크

- [1 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch01-Apr2016.pdf)
- [1 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01)
