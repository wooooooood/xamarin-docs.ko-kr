---
title: Xamarin.Forms 플랫폼 기능
description: 이 가이드에는 다양 한 기술 사용 하 여 Xamarin.Forms 응용 프로그램에 플랫폼 특정 기능을 활용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 2C6CE42C-E380-4BB9-90CC-D0F4E60C4C03
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2018
ms.openlocfilehash: 9bac53f71178ac321dea162d346295556a8f7adb
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998757"
---
# <a name="xamarinforms-platform-features"></a>Xamarin.Forms 플랫폼 기능

Xamarin.Forms는 확장 가능 하며 사용 하 여 플랫폼 특정 기능을 통합 하면 [효과](~/xamarin-forms/app-fundamentals/effects/index.md), [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)서 [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md), 합니다 [MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md), 등입니다.

## <a name="androidandroidindexmd"></a>[Android](android/index.md)

이 가이드에서는 기존 Xamarin.Forms Android 앱을 업데이트 하 여 재질 디자인을 구현 하는 방법을 설명 합니다.

## <a name="application-indexing-and-deep-linkingdeep-linkingmd"></a>[응용 프로그램 인덱싱 및 딥 링크 설정](deep-linking.md)

응용 프로그램 인덱싱 몇 가지 사용 하 여 검색 결과에 표시 하 여 인재를 후 잊어버린 그렇지 않은 경우는 응용 프로그램을 수 있습니다. 딥 링크 설정 응용 프로그램을 딥 링크에서 참조 하는 페이지로 이동 하 여 일반적으로 응용 프로그램 데이터를 포함 하는 검색 결과에 응답할 수 있습니다.

## <a name="device-classdevicemd"></a>[장치 클래스](device.md)

사용 하는 방법의 `Device` 공유 코드 (XAML을 사용 하는 등) 사용자 인터페이스에 플랫폼 특정 동작을 만드는 클래스입니다. 또한 다룹니다 `BeginInvokeOnMainThread` 백그라운드 스레드에서 UI 컨트롤을 수정할 때 필수적입니다.

## <a name="iosiosindexmd"></a>[iOS](ios/index.md)

일부 iOS 스타일 지정을 통해 수행할 수 있습니다 **Info.plist** 고 `UIAppearance` API. 이 가이드는 iOS 9 기능이 핵심 스포트라이트 검색을 포함 하 여 Xamarin.Forms 솔루션에 iOS 앱에 포함 하는 방법의 예제를 포함 합니다.

## <a name="gtkgtkmd"></a>[GTK](gtk.md)

Xamarin.Forms는 GTK # 앱에 대 한 미리 보기 지원이 되었습니다.

## <a name="macmacmd"></a>[Mac](mac.md)

Xamarin.Forms는 macOS 앱에 대 한 미리 보기 지원이 되었습니다.

## <a name="native-formsnative-formsmd"></a>[네이티브 양식](native-forms.md)

네이티브 양식 허용 Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-Xamarin.iOS, Xamarin.Android 및 유니버설 Windows 플랫폼 (UWP)에 대 한 네이티브 프로젝트에서 사용할 수 있도록 페이지를 파생 합니다.

## <a name="native-viewsnative-viewsindexmd"></a>[네이티브 뷰](native-views/index.md)

Xamarin.Forms에서 iOS, Android 및 유니버설 Windows 플랫폼의 네이티브 뷰를 직접 참조할 수 있습니다. 기본 보기에서 속성 및 이벤트 처리기를 설정할 수 있습니다 및 Xamarin.Forms 뷰를 사용 하 여 상호 작용할 수 있습니다.

## <a name="platform-specificsplatform-specificsindexmd"></a>[플랫폼별](platform-specifics/index.md)

플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 없이 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다.

## <a name="pluginspluginsmd"></a>[플러그 인](plugins.md)

Xamarin.Forms 앱을 확장할 수 있도록 하는 Github, Nuget 및 Xamarin Component Store에서 사용할 수 있는 다양 한 오픈 소스 플러그 인 됩니다.

## <a name="tizentizenmd"></a>[Tizen](tizen.md)

Tizen.NET을 사용 하면 Xamarin.Forms 및 Tizen.NET framework를 사용 하 여.NET 응용 프로그램을 빌드할 수 있습니다.

## <a name="windowswindowsindexmd"></a>[Windows](windows/index.md)

Xamarin.Forms는 Windows 10에서 유니버설 Windows 플랫폼 (UWP)에 대 한 지원 합니다. 이 문서에 추가 하는 방법에 설명 된 기존 Xamarin.Forms 솔루션에 UWP 프로젝트입니다.

## <a name="wpfwpfmd"></a>[WPF](wpf.md)

Xamarin.Forms에는 Windows Presentation Foundation (WPF) 앱에 대 한 미리 보기 지원이 되었습니다.
