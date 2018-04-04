---
title: Chapter 1의 요약입니다. Xamarin.Forms 포함할 됩니까?
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F3F864FF-EE70-49D0-90D1-388889037625
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 534c36a16acdc10ffb6f6b6703296a672875286e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-1-how-does-xamarinforms-fit-in"></a>Chapter 1의 요약입니다. Xamarin.Forms 포함할 됩니까?

해당 플랫폼 다른 프로그래밍 언어를 포함 하는 경우에 특히 다른 플랫폼에서 기본 코드를 이식은 프로그래밍에서 가장 원하지 않는 작업 중 하나입니다. 유혹도 리팩터링 코드를 이식할 때 있지만 두 플랫폼은 모두 동시에 존재 해야 하는 경우 다음 두 코드 베이스 간의 차이점은 향후 유지 관리 더 어렵게 만듭니다.

## <a name="cross-platform-mobile-development"></a>플랫폼 간 모바일 개발

이 문제는 모바일 플랫폼을 대상으로 할 때 자주 발생 합니다. 현재 두 가지 주요 모바일 플랫폼, Apple Iphone 및 Ipad iOS 운영 체제를 실행 하는 다양 한 휴대폰 및 태블릿에 실행 되는 Android 운영 체제 제품군은 존재 합니다. 또 다른 중요 한 플랫폼은 Microsoft 웹 플랫폼 UWP (유니버설 Windows), Windows 10 및 Windows 10 Mobile 모두 대상으로 하는 단일 프로그램 있습니다.

하지 않고자 한다면 이러한 세 가지 플랫폼을 대상으로 하는 소프트웨어 공급 업체는 다양 한 사용자 인터페이스 패러다임, 세 가지 개발 환경, 세 개의 서로 다른 프로그래밍 인터페이스를 처리 해야 하 고&mdash;아마도 가장 이라는&mdash; 세 가지 프로그래밍 언어: Objective-c iPhone 및 iPad, android의 경우 Java와 C# windows에 대 한 합니다.

## <a name="the-c-and-net-solution"></a>C# 및.NET 솔루션

Objective-c, Java 및 C# C 프로그래밍 언어에서 파생 모두, 있지만 매우 다른 경로 의해 발전가 있습니다. C# 이러한 언어 중 가장 최근의 고 매우 유용한 방식으로 발전 되었습니다. 또한 C#은 관련.NET math, 디버깅, 리플렉션, 컬렉션, 세계화, 파일 I/O, 네트워킹, 보안, 스레딩, 웹 서비스, 데이터 처리 및 XML에 대 한 지원을 제공 하는 호출 하는 전체 프로그래밍 인프라 및 읽기 및 쓰기 JSON입니다.

Xamarin에는 현재 네이티브 Mac, iOS 및 Android Api C# 및.NET을 사용 하 여 대상으로 하는 도구를 제공 합니다. 이러한 도구는 Xamarin.Mac, Xamarin.iOS 및 Xamarin.Android, Xamarin 플랫폼으로 통칭 라고 합니다. 이들은, 라이브러리 및.NET 관용구와 이러한 플랫폼의 기본 Api를 표현 하는 바인딩입니다.

개발자는 응용 프로그램을 작성할 C#에서 해당 대상 Mac, iOS 또는 Android Xamarin 플랫폼을 사용할 수 있습니다. 되지만 둘 이상의 플랫폼을 대상으로 지정 하는 경우 대상 플랫폼 간 코드를 공유 하는 바람직합니다. 이 플랫폼별 코드 (일반적으로 관련 된 사용자 인터페이스) 및 플랫폼 독립적인 코드를 일반적으로 기본.NET framework로 프로그램을 분리 해야 합니다. 이 플랫폼 독립적인 코드를 PCL 이식 가능한 클래스 라이브러리 (), 또는 공유 프로젝트의 경우 공유 자산 프로젝트 또는 SAP 라고도 하거나 있을 수 있습니다.

## <a name="introducing-xamarinforms"></a>Xamarin.Forms 소개

여러 모바일 플랫폼을 대상으로 지정 하는 경우 Xamarin.Forms 더 많은 코드 공유할 수 있도록 허용 합니다. Xamarin.Forms에 대해 작성 된 단일 프로그램 5 개의 서로 다른 플랫폼 대상으로 지정할 수 있습니다.:

- iPhone, iPad 및 iPod touch에서 실행 되는 프로그램에 대 한 iOS
- Android 휴대폰 및 태블릿에 실행 되는 프로그램에 대 한 android
- 대상 Windows 10 및 Windows 10 Mobile에 유니버설 Windows 플랫폼
- Windows 8.1 Windows 런타임 API
- Windows Phone 8.1의 Windows 런타임 API

현재 Xamarin.Forms 솔루션 템플릿은 Windows 8.1 및 Windows Phone 8.1 플랫폼에 대 한 프로젝트 템플릿이 포함 하지 않습니다.

Xamarin.Forms 프로그램의 대부분 PCL 또는 SAP에 있습니다. 각 플랫폼 PCL를 호출 하는 작은 응용 프로그램 스텁 이루어져 있습니다. Xamarin.Forms Api는 각 플랫폼의 문자 모양 및 느낌을 유지 되도록 각 플랫폼에 네이티브 컨트롤에 매핑됩니다.

[![공유 플랫폼 시각적 개체의 삼중 스크린 샷](images/ch01fg03-small.png "각 플랫폼에서 컨트롤 Xamarin.Forms")](images/ch01fg03-large.png#lightbox "각 플랫폼에서 Xamarin.Forms 컨트롤")

왼쪽에서 오른쪽 스크린샷을 iPhone, Android 휴대폰, 및 Windows 10 Mobile phone 보여 줍니다. 각 화면에서 페이지에는 Xamarin.Forms [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 텍스트를 표시 하기 위한는 [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) 동작을 시작 하는 것에 대 한 한 [ `Switch` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/) 에 대 한 켜기/끄기 값을 선택 및 [ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) 연속 범위 내의 값을 지정 하는 데 있습니다. 보기의 모든 4 개의 자식 요소인는 [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) 에 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)합니다.

구성 된 여러 Xamarin.Forms 도구 모음에는 페이지에도 연결 [ `ToolbarItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/) 개체입니다. 이 iOS 및 Android 화면 맨 위에 있는 및 Windows 10 Mobile 화면 아래쪽에 아이콘으로 표시 됩니다.

Xamarin.Forms는 XAML에서는, 여러 응용 프로그램 플랫폼에 대 한 microsoft Extensible Application Markup Language를 개발 합니다. 위에 표시 된 프로그램의 모든 시각적 개체에서와 같이 XAML에 정의 된 [ **PlatformVisuals** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01/PlatformVisuals) 샘플.

Xamarin.Forms 프로그램 어떤 플랫폼에서 실행 중인를 확인 하 고 적절 하 게 서로 다른 코드를 실행 합니다. 더를 매우 개발자가 다양 한 플랫폼에 대 한 사용자 지정 코드를 작성 하 고 플랫폼에 관계 없이 Xamarin.Forms 프로그램에서 해당 코드를 실행할 수 있습니다. 개발자는 각 플랫폼에 대 한 렌더러를 작성 하 여 추가 컨트롤을 만들 수도 있습니다.

Xamarin.Forms는 빠른 개념 증명 데모 치거나 프로토타입용으로 또는 기간 업무 응용 프로그램에 대 한 적합 한 솔루션을 하는 동안에 벡터 그래픽 또는 복잡 한 터치 상호 작용 해야 하는 응용 프로그램 덜 적합 합니다.

## <a name="your-development-environment"></a>개발 환경

개발 환경에 대상으로 지정할 플랫폼 및 컴퓨터 있습니다 사용할 따라 달라 집니다.

대상 iOS 하려는 경우 Mac Xcode 및 Xamarin 플랫폼을 설치 해야 합니다. Android도 지원 Java와 필요한 Sdk를 설치 해야 합니다. IOS 및 Android Mac.에 대 한 Visual Studio를 사용 하 여 다음 대상

Visual Studio를 설치 하면 PC에서 iOS, Android 및 모든 Windows 플랫폼을 대상으로 합니다. 그러나 Xcode 및 Xamarin 플랫폼 설치 된 Mac을 위해서는 여전히 Visual Studio에서 iOS를 대상으로 지정 합니다.

컴퓨터에 USB로 연결 중 하나는 실제 장치 또는 시뮬레이터에서 앱에서 프로그램을 테스트할 수 있습니다.

## <a name="installation"></a>설치

를 만들고 Xamarin.Forms 응용 프로그램을 작성 하기 전에 만들고 iOS 응용 프로그램, Android 응용 프로그램 및 개발 환경 및 대상으로 지정할 플랫폼에 따라 UWP 응용 프로그램을 개별적으로 구축 하려고 해야 합니다.

이 작업을 수행 하는 방법에 대 한 정보를 포함 하는 Xamarin 및 Microsoft 웹 사이트:

- [IOS를 시작 하기](~/ios/get-started/index.md)
- [Android 시작](~/android/get-started/index.md)
- [Windows 개발자 센터](http://dev.windows.com)

한 번 만들 수 있습니다 및 이러한 각 플랫폼에 대 한 프로젝트를 실행 하는 문제가 없습니다 만들고 Xamarin.Forms 응용 프로그램을 실행 합니다.



## <a name="related-links"></a>관련 링크

- [1 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch01-Apr2016.pdf)
- [1 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01)
